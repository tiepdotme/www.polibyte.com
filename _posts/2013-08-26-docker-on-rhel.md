---
layout:     post
title:      Docker on rhel
---


There's been [some interest](https///github.com/dotcloud/docker/issues/172) in running Docker on CentOS and Red Hat, but it currently requries software not available in those distributions, most notable a linux kernel version 3.8 or newer with the AUFS patches. The docker team plans to loosen this dependency soon, but until then I've created spec files for building RPMs of a suitable kernel, the lxc utilities, and docker. The package building instructions are on github at [sciurus/docker-rhel-repo](https///github.com/sciurus/docker-rhel-rpm). The kernel instructions may also be of interested to anyone attempting to run docker on Fedora.

I don't plan to distribute binary packages myself. I tried and failed to get these building on OpenSUSE's Build Service; if anyone who's more familair with that tool has more luck please let me know.




