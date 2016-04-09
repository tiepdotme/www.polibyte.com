---
layout:     post
title:      How nethogs works
---


There are a lot of underappreciated monitoring utilities for linux. From time to time, I plan to dive into one and write up what it's useful for and how it works. I'll start with [nethogs](http://nethogs.sourceforge.net/[). Unlike most network monitoring tools, nethogs breaks down traffic per-proccess rather than per-host or per-protocol. Its focus is answering the question "What program is using my bandwidth?". It provides a [top-style interface](http://nethogs.sourceforge.net/nethogs.png) for real-time monitoring. So how does it work?

On linux the file /proc/net/tcp lists all established TCP connections. It includes the local and remote socket addresses and the inode number for the local socket. Nethogs uses libpcap to sniff traffic and associate it with its entry in /proc/net/tcp. It takes the inode from there and scans through /proc/*/fd/ looking for the file descripter that has that inode to determine which process has the socket open. Once it finds the process it adds it to a table of inode to process id mappings so it doesn't have to scan through /proc again the second time a packet for that connection comes through.





