---
layout:     post
title:      Graylog2 path fixes
---


On the off chance that any else is trying to run [Graylog2](http://graylog2.org/)'s web interface at a path other than the root of a URL  (e.g. https://example.com/graylog2/ instead of https://graylog2.example.com/), I have a repository at [sciurus/graylog2-web-interface](https///github.com/sciurus/graylog2-web-interface) that fixes places where absolute URLs crept in to the code. The Graylog2 developers are rewriting the web interface in Java, so I'm not sure if there will be a release in the 0.12 series with these changes.




