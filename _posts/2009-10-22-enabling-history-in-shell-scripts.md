---
layout:     post
title:      Enabling history in shell scripts
---


If you've ever found yourself trying to log the commands ran by your bash scripts (and cursed your failures to quote and escape them properly), then try using bash's built in history functionality instead. Below is in example of how to turn it on within a script.

	:::bash
	#!/bin/bash
	
	HISTFILE="$0.log"
	set -o history
	history -c
	
	echo 'Those who cannot remember the past are condemned to write their own logging functions.'
	
	history -w





