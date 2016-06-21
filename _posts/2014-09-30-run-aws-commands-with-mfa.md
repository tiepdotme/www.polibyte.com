---
layout:     post
title:      Run aws commands with mfa
---


There are a number of programs I use frequently that interact with AWS, such as [aws-cli](https://aws.amazon.com/cli/) and [test-kitchen](http://kitchen.ci/). I haven't found any programs that natively support [multi-factor authentication with AWS](https://aws.amazon.com/iam/details/mfa/). Thankfully most programs will check the right environment variables, but you have to do a song-and-dance with [STS](http://docs.aws.amazon.com/STS/latest/UsingSTS/Welcome.html) to set them up. I've just made a public [a program called aws-mfa](https://github.com/lonelyplanet/aws-mfa) that I created for work to simplify this process. Enjoy!




