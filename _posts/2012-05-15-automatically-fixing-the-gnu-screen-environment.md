---
layout:     post
title:      Automatically fixing the gnu screen environment
---


Stale environment variables, particularly those related to SSH agent and X11 forwarding, are a well-known problem for GNU Screen users. Search google and you'll find many solutions to this problem (including [one](http://samrowe.com/wordpress/ssh-agent-and-gnu-screen/) from my fellow Athenian Sam Rowe), but I wasn't happy with having to manually run a command to fix each screen window. Thus, I whipped up a script I call [rescreen](https://github.com/sciurus/splatbang/blob/master/rescreen).

Rescreen depends on two tricks to automate fixing the environment variables to match your new SSH connection. First, it uses screen's setenv command to set the environment for any new screen windows you create. Second, it uses a signal handler to have bash processes running in your existing screen windows alter their environments.

To start using rescreen, you need to [download it](https://raw.github.com/sciurus/splatbang/master/rescreen) into your $PATH and then place the following in your bashrc.

	:::bash
	trap "if [ -f "$HOME/.rescreen_environment" ]; then source $HOME/.rescreen_environment; fi" SIGUSR1


Now you can attach a detached screen session by running rescreen with a single argument, the session name. SSH_CLIENT, SSH_TTY, SSH_AUTH_SOCK, SSH_CONNECTION, and DISPLAY will be reloaded if necessary.  If you need to change more variables, just add to the list starting in line 11. If you left a screen session attached, you'll have to detach it with *screen -d sessionname* before using rescreen. You could alter rescreen to do this automatically, but I prefer to think about whether I really want to "call the Dr".





