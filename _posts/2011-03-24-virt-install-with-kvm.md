---
layout:     post
title:      Virt-install with kvm
---


If you are using virt-install to create KVM virtual machines on RHEL or CentOS 5, be sure to specify the '--accelerate' option. If you don't use accelerate, virt-install starts '/usr/bin/qemu-system-x86_64' rather than '/usr/libexec/qemu-kvm'. This isn't what you want, and it will fail with the error message "internal error Domain $YOUR_VM didn't show up". What happened in the background is that libvirt set the machine type to rhel5.4.0, which lets qemu-kvm know it can use virtio, but this machine type is not understood by qemu-system-x86_64. If you check '/var/log/libvirt/qemu/$YOUR_VM.log', you'll see a detailed error like \\ \\

    LC_ALL=C PATH=/sbin:/usr/sbin:/bin:/usr/bin HOME=/ /usr/bin/qemu-system-x86_64 -S -M rhel5.4.0 -no-kqemu -m 512 -smp 1 -name $YOUR_VM -uuid a3e38d9d-d958-0da1-4b3a-57179ee04f29 -monitor pty -pidfile /var/run/libvirt/qemu/$YOUR_VM.pid -no-reboot -boot d -drive file=$YOUR_INSTALLER.iso,if=ide,media=cdrom,index=2 -net nic,macaddr=54:52:00:76:5f:f4,vlan=0 -net tap,fd=36,script=,vlan=0,ifname=vnet10 -serial pty -parallel none -usb -vnc 127.0.0.1:10 -k en-us \\ \\

    Supported machines are: \\
    pc         Standard PC (default) \\
    isapc      ISA-only PC
