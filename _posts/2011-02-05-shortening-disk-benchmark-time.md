---
layout:     post
title:      Shortening disk benchmark time
---


When benchmarking storage configurations, it's important that your benchmark's set of test data be larger than the amount of memory in the computer where the benchmark is running. Otherwise, the results will be more reflective of your operating system's caching behaviour than your storage system. For example, on a server with 8GB of RAM I would set iozone's *-g* argument to 16GB. Of course, tests that write this much data can take a long time. This can be a problem when you're trying to test many small tweaks to your storage. A way to speed this up is to use the linux kernel's *mem* parameter. For example, in my GRUB configuration I could set *kernel /vmlinuz ro root=/dev/example/root mem=1G*. When I boot with this parameter set the system will only have 1GB of memory available, so I can reduce my test data to 2GB. This makes iterating through many storage configurations much quicker.





