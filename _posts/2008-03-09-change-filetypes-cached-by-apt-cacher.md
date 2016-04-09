---
layout:     post
title:      Change filetypes cached by apt-cacher
---


I've been learning how to use debian-installer's preseed functionality in order to automate some of the installations we do at Free IT Athens. Among other things, I wanted to set it to use [apt-cacher](http://packages.debian.org/apt-cacher), our caching proxy server for software, and to install some additional packages, including msttcorefonts. Msttcorefonts downloads each font as an exe file, which isn't in apt-cacher's whitelist of filetypes to accept. If you try, you'll receive a 403 error and the message // Sorry, not allowed to fetch that type of file//. Since apt-cacher is written in perl, this was an easy fix; I modified line 646 to read

	:::perl
	 if ($filename =~ /(\.deb|\.rpm|\.dsc|\.tar\.gz|\.diff\.gz|\.udeb|\.exe)$/) {





