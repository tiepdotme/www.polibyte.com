---
layout:     post
title:      Identifying zombies
---


One on server I adminster, users run workflows that occasionally leave zombie processes behind. A zombie describes a processes that has finished but has not had its exit status read by its parent process. I keep [this shell script](https://github.com/sciurus/splatbang/blob/master/report_zombies) around to generate a report on zombie children and their negligent parents that I can send to the workflow developers.




