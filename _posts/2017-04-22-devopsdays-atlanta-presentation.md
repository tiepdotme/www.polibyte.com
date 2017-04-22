---
layout:     post
title:      DevOpsDays Atlanta 2017 Presentation
---

At [DevOpsDays Atlanta](https://www.devopsdays.org/events/2017-atlanta/welcome/), I presented an ignite talk on our recently-completed VPC migration at Eventbrite. This was my first time speaking under the constraints on the ignite format -  20 slides, automatically advancing every 15 seconds, and it was quite nerve-wracking!

You can view my slides as a PDF [here](/presentations/vpc_lightning_talk.pdf). My speaker notes for each slide are below.

1 Hi, I’m Brian from Eventbrite and I want to tell you about lessons my team learned while migrating our infrastructure from Amazon’s Classic compute service to their virtual private cloud, or VPC.

2 I work on the systems engineering team. We're 5 people out of a 150+ person engineering department. We do a mix of traditional ops and sre-style work, and we’re solely responsible for management of all of our infrastructure in EC2, top to bottom.

3 We typically try to divide our time 50% between internal projects and 50% helping the rest of engineering. As you’d imagine with such a relatively small team, we're typically each working independently to handle the workload.

4 We faced a problem: all our infrastructure was running in EC2 Classic. Classic was clearly on the way out, and we were starting to miss out on new instances types and other features.

5 We needed to move from Classic to VPC, but as we started and initially failed to make progress on the migration it became clear we needed to make some changes. First, let's talk about the easy part: the technical side.

6 In Classic we’d managed all configuration like security groups through homegrown python scripts. These made changes simple, but required duplicate work across environments and led to configuration drift and confusion.

7 With the extra complexity of VPC, this wasn’t going to cut it. We evaluated a few tools and settled on Terraform. The learning curve was steep, but we ended with a base configuration as code that we can apply and evolve across all environments.

8 On our instances, we’d long been using puppet for configuration management. Gowever, we had a lot of duplication of configuration files, and they were chock-full of hardcoded hostnames. Continuing this approach during the migration would be a mess.

9 Instead, we invested in Consul for service discovery and consul-template to populate many configuration files. This let us reduce rather than expand the copies we managed and start replacing instances without constantly editing config.

10 Now, onto the cultural side. Some tasks, like designing the VPC architecture, blocked other migration tasks and required lots of focus from one or two people. We found we weren’t making progress because they were distracted by other responsibilities.

11 We had to learn to shield our teammates. This meant those of us who weren’t focused on those tasks taking on more external projects and handling more interrupts. This increased stress but we knew it was temporary and should pay off.

12 When we got to the actual instance migrations, we had to make a decision. If we stuck with our usual approach, we’d have one person working on the migration and the other 4 of us working on 4 other internal projects.

13 That way the migration would take forever, and the person working on it would get really bored! Instead we declared it a priority to the exclusion of other internal projects and divided up the work to tackle in parallel.

14 To do this we designed an issue template through which someone would would fill out to propose a migration process for a service, a peer review process, and a standard approach to rolling out and testing across environments

15 To keep things rolling along we made a decision to limit the scope to just replacing instances. No rearchitecting services, no upgrading software versions, etc. This was tough but key to avoid getting sidetracked and losing momentum.

16 Doing this project required reshaping our relationship with the rest of engineering. They weren’t used to us taking on a single project that required so much time and resources, particularly one with no obvious direct benefit to the product.

17 We had to learn how to communicate the value of the migration to the business, how to develop realistic timeline and milestones, and how to deliver reports with metrics showing the progress we were making.

18 After finishing the migration we are seeing technical benefits, like performance improvements from newer instance types and enhanced networking, saner VPN configurations, happier security engineers, etc.

19 But even better we’re seeing improvements in how we work together as contributors within our team and as a team within engineering. The lessons we learned are shaping how we approach future projects.

20 Thanks for listening, I hope you learned something from our story, please find me if you have questions or want to share your own experiences. I’ll post the slides online at the address above.
