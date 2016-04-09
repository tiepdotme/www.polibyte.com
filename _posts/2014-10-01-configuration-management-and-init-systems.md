---
layout:     post
title:      Configuration management and init systems
---


I recently performed a [fixup](https///github.com/lusis/chef-logstash/pull/354) of the service management aspects of the logstash chef cookbook. This took more effort than I expected. Ohai doesn't provide a working means of [detecting the init system](https///github.com/opscode/ohai/pull/301) (I checked puppet's facter and it doesn't handle this either), so any cross-distro cookbook has to include a lot of nested conditionals to choose the right init provider. Much of this logic was wrong, so I fixed it up and added tests. Once I had it writing configurations for the right init system, I had to tweak those service configurations to make them valid.

Towards the end of doing this I discovered Jordan Sissel's [pleaserun](https///github.com/jordansissel/pleaserun) project, which aims to generate service configurations for all the popular init systems. My first reaction was that this was a manifestation of [XKCD 927](https///xkcd.com/927/), but it seems focused on handling the simple cases rather than being a universal standard. The only thing I think I would add is a way to set environment variables.

I'm warming up to the idea that configuration management systems should offer a pleaserun-like abstraction of service configuration themselves. For many cookbooks/modules, its capabilities would be good enough and it would allow a major reduction in complexity. Paul Czar's [chef-pleaserun](chttps///github.com/paulczar/chef-pleaserun) is an attempt to offer this for chef.





