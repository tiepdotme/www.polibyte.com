---
layout:     post
title:      Massaging ovf for virtualbox
---


[VirtualBox](https://www.virtualbox.org/) and VMware's [ovftool](https://www.vmware.com/support/developer/ovf/) have a different idea of how a storage controller should be described. This prevents exporting a VM from, e.g. VMware Fusion, and then importing it into VirtualBox. However, this can be worked around.

Ovftool produces output like

	
	`<Item>`
	  `<rasd:Address>`0`</rasd:Address>`
	  `<rasd:Description>`SATA Controller`</rasd:Description>`
	  `<rasd:ElementName>`sataController0`</rasd:ElementName>`
	  `<rasd:InstanceID>`3`</rasd:InstanceID>`
	  `<rasd:ResourceSubType>`vmware.sata.ahci`</rasd:ResourceSubType>`
	  `<rasd:ResourceType>`20`</rasd:ResourceType>`
	`</Item>`


but VirtualBox wants a tag named *Caption*, not *ElementName*. It also wants *ResourceSubType* to be just 'AHCI'. See the [code](http://www.virtualbox.org/svn/vbox/trunk/src/VBox/Main/xml/ovfreader.cpp) below

	
	case ResourceType_OtherStorageDevice:        // 20       SATA controller
	{
	    /* `<Item>`
	        `<rasd:Description>`SATA Controller`</rasd:Description>`
	        `<rasd:Caption>`sataController0`</rasd:Caption>`
	        `<rasd:InstanceID>`4`</rasd:InstanceID>`
	        `<rasd:ResourceType>`20`</rasd:ResourceType>`
	        `<rasd:ResourceSubType>`AHCI`</rasd:ResourceSubType>`
	        `<rasd:Address>`0`</rasd:Address>`
	        `<rasd:BusNumber>`0`</rasd:BusNumber>`
	    `</Item>` */
	    if (    i.strCaption.startsWith("sataController", RTCString::CaseInsensitive)
	         && !i.strResourceSubType.compare("AHCI", RTCString::CaseInsensitive)
	       )
	    {
	        HardDiskController hdc;
	        hdc.system = HardDiskController::SATA;
	        hdc.idController = i.ulInstanceID;
	        hdc.strControllerType = i.strResourceSubType;
	
	        vsys.mapControllers[i.ulInstanceID] = hdc;
	    }
	    else
	        throw OVFLogicError(N_("Error reading \"%s\": Host resource of type \"Other Storage Device (%d)\" is supported with SATA AHCI controllers only, line %d"),
	                            m_strPath.c_str(),
	                            ResourceType_OtherStorageDevice,
	                            i.ulLineNumber);
	    break;
	}


If you modify the ovf file to make these changes, the import should succeed. After modfying it, be sure to update the sha1sum of the ovf file in the mf file.





