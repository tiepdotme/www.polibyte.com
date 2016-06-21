---
layout:     post
title:      Vagrant plugins
---


Earlier this fall I started using [vagrant-libvirt](https://github.com/pradels/vagrant-libvirt), a plugin for vagrant that enables it to use libvirt instead of virtualbox for managing VMs. Since I started using it I've contributed some bug fixes and some improvements, such as enabling parallel operations on VMs and implementing a *reload* command.

Looking for something more sizable to do, I took on a request to support saving and restoring snapshots of a VM's state. Rather than implementing this in vagrant-libvirt I did it in [sahara](https://github.com/jedi4ever/sahara), another vagrant plugin that provides a nice workflow for snapshots. Before I started, the generic workflow logic was intermingled with virtualbox-specific code. I reworked it to seperate the actions from the provider's implementation, made sure that virtualbox still worked, then added support for libvirt.

Having built my confidence contributing to two existing plugins, I now felt ready to start my own. I was inspired by vagrant-kvm's (a "competitor" to vagrant-libvirt) ability to convert and use a box created for virtualbox. I thought that this sort of conversion would be useful for many providers and that, rather than adding it to each of them, it belonged in its own plugin. Thus [vagrant-mutate](https://github.com/sciurus/vagrant-mutate) was born. It currently support converting from virtualbox to vagrant-libvirt, and I'm working with vagrant-kvm developers to support their project as well.



