---
layout:     post
title:      Iscsiadm wrapper
---


When I first started to use icsci on RHEL5, isciadm seemed like a very complicated command. This turned out not to be the case, but
the syntax is a bit verbose and can be hard to remember if you don't use it frequently. I put together a wrapper script to make it easier to remember.

	
	$ iscsi help
	You must provide the operation and, if applicable, the target
	Examples:
	iscsi discovery
	iscsi session
	iscsi login iqn.2001-05.com.equallogic:0-aaaaaa-bbbbbbbbb-cccccccccccccccc-example
	iscsi logout iqn.2001-05.com.equallogic:0-aaaaaa-bbbbbbbbb-cccccccccccccccc-example
	iscsi rescan iqn.2001-05.com.equallogic:0-aaaaaa-bbbbbbbbb-cccccccccccccccc-example
	iscsi delete iqn.2001-05.com.equallogic:0-aaaaaa-bbbbbbbbb-cccccccccccccccc-example


You can find the code [here](https://github.com/sciurus/splatbang/blob/master/iscsi) on github.




