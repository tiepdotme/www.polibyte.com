---
layout:     post
title:      Updating dell firmware
---


Dell has several methods for updating their BIOS.

### Working Approach

Out of all the different approaches, this is what I cam up with that successfully let me install the latest BIOS without taking a trip to the data center.

*  Install smbios-utils from [EPEL](http://fedoraproject.org/wiki/EPEL)

*  Download the BIOS package from the Dell website, [here](http://support.dell.com/support/downloads/download.aspx?c=us&cs=555&l=en&s=biz&releaseid=R281099&SystemID=pwe_r610&servicetag=F1WFVH1&os=WNET&osl=en&deviceid=11809&devlib=0&typecnt=0&vercnt=7&catid=-1&impid=-1&formatcnt=0&libid=1&typeid=-1&dateid=-1&formatid=-1&source=-1&fileid=415215) is an example

*  Rather than running what you downloaded, just extract the files, e.g. `./PER610_BIOS_LX_2.1.15.BIN --extract bios_files` would extract the files to a directory named bios_files

*  The actual BIOS image is in the subdirectory named payload. Install it using the [dellBiosUpdate](http://linux.dell.com/libsmbios/main/dellBiosUpdate.html) program, e.g. `dellBiosUpdate -u R610-020115C.hdr`

*  Reboot

### Unified Server Configurator Approach

A GUI that's a part of the Lifecycle Controller. I think you get to it by pressing F10 for System Services during boot.
#### Pros

*  Able to update all firmware on the system

*  Fetches the latest version

*  No dependencies on installed OS
#### Cons

*  Must go to data center since console redirection does not work with it

*  Extensive downtime as updates are downloaded

### Download Approach

Dell's [Drivers and Downloads](http://support.dell.com/support/downloads/index.aspx?c=us&cs=RC956904&l=en&s=hied) page has BIN files you can download and run from within linux to update the BIOS. These are a combined shell script and archive. In theory, you just run them and the new BIOS is installed.
#### Pros

*  Can always get latest version
#### Cons

*  Must manually browse site and identify proper download

*  Only updates the BIOS, not other firmware

*  In my experience, this does not work. A report of the kind of error I ran into and some troubleshooting steps are [here](http://lists.us.dell.com/pipermail/linux-poweredge/2010-January/041030.html).

### Dell "Hardware" Repository Approach

Dell officially maintains and supports a [yum repository](http://linux.dell.com/repo/hardware/latest/) with rpms for their firmware and some tools for installing it.
#### Pros

*  Able to update all firmware on the system
#### Cons

*  Has software that is also packaged in EPEL, meaning you must assign a higher priority to this repository in your yum config

*  The repository is not kept up to date with the latest fimrware. It seems that it's updated in batches every few months.

*  In my experience, this does not work. See [these error reports](http://www.mail-archive.com/search?q=dell_dup_componentid_00159&l=linux-poweredge@dell.com&o=relevance&start=10) over a period of months where the R610 BIOS update fails to install.

### Dell "Firmware" Repository Approach

Matt Domsch from Dell maintains an unsupported [yum repository](http://linux.dell.com/repo/community/) just with BIOS updates. You need to install some other tools from Dell's [community repository](community repository) (or maybe smbios-utils and firmware-addon-dell from EPEL would be enough, haven't tested this yet) to use it.
#### Pros

*  Should always be latest version
#### Cons

*  The script that generates the repository is [fragile and frequently breaks](http://lists.us.dell.com/pipermail/firmware-tools-devel/2010-December/000549.html)

*  Doesn't seem to have all the BIOS I need, e.g. it didn't find any updates for my R610s

**Update**: vwaelchli on reddit points out that you can also update the firmware by [running wsman commands from the lifecycle controller](http://www.reddit.com/r/sysadmin/comments/kamb1/centos_based_livedvd_to_update_firmware_on_dell/c2j23bo).




