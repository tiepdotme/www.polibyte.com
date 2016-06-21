---
layout:     post
title:      Learning lita at hack nashville
---


I spent last weekend at [Hack Nashville](http://hacknashville.com/), which as always was a blast. [Lee Jones](https://leejones.github.io/) and I decided to spend the weekend exploring a chat bot framework called [Lita](https://www.lita.io/). [Lonely Planet](http://engineering.lonelyplanet.com/) uses [Slack](https://slack.com/) heavily, and we already have an instance of [Hubot](https://hubot.github.com/) running that we use for things like [interacting with PagerDuty](https://www.npmjs.org/package/hubot-pager-me) and [viewing pugs](https://www.npmjs.org/package/hubot-pugme). Lita caught my interest because of its excellent documentation, its emphasis on testing, and its usage of Ruby, a language many developers at Lonely Planet know well.


We started by getting an instance of Lita running in our infrastructure; we were able to reuse our cloudformation templates for Hubot for this. Next we turned to integrating it with Slack. Along the way we submitted a few PRs to [lita-slack](https://github.com/kenjij/lita-slack) and [lita-slack-handler](https://github.com/kenjij/lita-slack-handler). Once we had this working, we developed a toy plugin to get a feel for the API. You can see this at [lita-vader](https://github.com/lonelyplanet/lita-vader). Happy with this experience, we decided our first useful plugin would be a [Jenkins](http://jenkins-ci.org/) integration. We looked at extending the [existing one](https://github.com/daniely/lita-jenkins) but we not happy with the user interface or its lack of abstraction over the Jenkins API. We spent some time writing up the capabilities we wanted and familiarizing ourselves with [jenkins_api_client](https://github.com/arangamani/jenkins_api_client) before turning to the implementation. During the remaining time at Hack Nashville, we were able to implement half of the features we wanted. We are keeping our plugin in a private repository until the initial feature set is complete and proves itself useful at Lonely Planet.

Working with Lita was very pleasant, and I would recommend other Ruby developers and ChatOps enthusiasts try it out.





