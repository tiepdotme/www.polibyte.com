---
layout:     post
title:      Why powertop is awesome
---


I recently got a new, off-brand replacement battery for my Inspiron 1420 laptop. I used it for the first time today and was shocked to see the charge go from 100% to 10% in about 50 minutes while I was writing some emails. No noteworthy processor or disk activity was evident in my system monitor applet. Just as I was about to write a negative review on Amazon, it crossed my mind to check [PowerTOP](http://www.lesswatts.org/projects/powertop/). PowerTOP is a utility for viewing how much time an Intel processor is spending in various power-saving states and what processes are waking it up from idle. I discovered I had left a virtual machine running. It was stuck trying to PXE boot; while its actual use of processor time was minimal, it was causing around 3000 wakeups a second! Shutting this virtual machine down brought wakeups down to a more reasonable 250 per second. This highlights how important the power savings from idling are on a modern processor and how powertop can help diagnose problems that would be difficult to tackle with other tools.




