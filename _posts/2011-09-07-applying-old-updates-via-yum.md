---
layout:     post
title:      Applying old updates via yum
---


When new software packages are released, you don't want to blindly apply them to all your systems. You should have a test environment, or at least a set of less-critical systems, where you test things first. If you normally stick to a consistent update schedule, such as once a week, you can use the date a package was created to determine whether it should be updated in your production environment. I couldn't find an way to do this directly through yum, so I created a shell script named [update_production](https://github.com/sciurus/splatbang/blob/master/update_production) to do this. As written, it applies all updates, except for packages released to CentOS or EPEL in the last week. It's designed to be easy to tweak the repositories you delay updates from or how long the delay is. Ideally this functionality should be available as a plugin to yum, but that looked much more complicated to develop.





