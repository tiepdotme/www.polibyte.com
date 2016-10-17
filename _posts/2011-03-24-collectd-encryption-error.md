---
layout:     post
title:      Collectd encryption error
---


If you receive the following messages when starting collectd \\

    network plugin: Option `SecurityLevel' is not allowed here. \\
    network plugin: Option `AuthFile' is not allowed here. \\

It is because you configured collectd to sign or encrypt its communication, but collectd was not compiled with libgcrypt support. This is true of the version in EPEL. \\

    $ yum info collectd | grep Version \\
    Version    : 4.10.2 \\
    $ ldd /usr/lib64/collectd/network.so \\
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00002b620d29d000) \\
        libdl.so.2 => /lib64/libdl.so.2 (0x00002b620d4b8000) \\
        libc.so.6 => /lib64/libc.so.6 (0x00002b620d6bc000) \\
        /lib64/ld-linux-x86-64.so.2 (0x0000003223400000) \\

To solve the problem, either disable signing and encryption or rebuild collectd.
