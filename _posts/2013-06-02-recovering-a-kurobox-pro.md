---
layout:     post
title:      Recovering a kurobox pro
---



I had a Kurobox Pro booting Debian off the internal hard drive as described at http://www.cyrius.com/debian/orion/buffalo/kuroboxpro/index.html. Unfortunately, the internal hard drive died. Here are the steps I took to get it booting from the internal flash again.


If you don't have them already, install java and a tftp server.


Download LS-GL_FW_115.zip from http://www.buffalotech.com/support-and-downloads/download/LS-GL_FW_115.zip. This is the firmware for a Linkstation model that's compatible with the Kurobox Pro.


Download acp_commander.jar from http://downloads.nas-central.org/TOOLS/ALL_LS_KB_ARM9/ACP_COMMANDER/acp_commander.jar. This is a recovery tool; the README is at http://downloads.nas-central.org/TOOLS/ALL_LS_KB_ARM9/ACP_COMMANDER/README


Unzip LS-GL_FW_115.zip. That will create a number of files, including uImage.buffalo and initrd.img. Unzip initrd.img using the password IeY8omJwGlGkIbJm2FH_MV4fLsXE8ieu0gNYwE6Ty. That will create initrd.buffalo. Now you have the kernel (uImage.buffalo) and ramdisk (initrd.buffalo). Move them to the directory your tftp server will serve.


Now disconnect your computer from your network and connect it via an ethernet cable directly to your Kurobox. Power on the Kurobox. At this point, the Kurobox will begin to boot, assign itself the IP address of 192.168.11.150, and try to load uImage.buffalo from 192.168.11.1 via TFTP. It will continue to rety this until it succeeds.


Assign your computer an IP address of 192.168.11.1/24. Start your tftp server. Watch your tftp server logs or sniff your network traffic and you will see your Kurobox loading the kernel. Give it a minute to boot up.


Once it has booted you should run "java -jar acp_commander.jar -t 192.168.11.150 -o". That will connect to a manufacturer backdoor on your kurobox, enable telnet, and clear the root password.


Now telnet to 192.168.11.150 as root and run "/usr/local/sbin/nvram -c set bootcmd 'nboot $(default_kernel_addr) 0 20000; setenv bootargs $(bootargs_base) $(bootargs_root) $(buffalo_ver); bootm $(default_kernel_addr)'"


Stop your tftp server and reboot your Kurobox. After it reboots, you should be able to telnet to 192.168.11.150 as root without a password.




