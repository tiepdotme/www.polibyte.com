---
layout:     post
title:      Edit file as root from nautilus
---


This is a [script](http://en.wikipedia.org/wiki/Scripting_language) for any users of Linux who'd like a convenient way to begin editing a file as the superuser (i.e root) from Nautilus, the GNOME file manager.

 1.  Copy and paste the code below into a text editor such as gedit.
 2.  Save the text as the file ~/.gnome2/nautilus-scripts/edit_as_root The tilde stands for your home directory. You may need to right click within the save file dialog and check show hidden files before you see .gnome2
 3.  Open a terminal and run the command 'chmod +x ~/.gnome2/nautilus-scripts/edit_as_root' This tells your system that the text file is executable, i.e. that it is a script not a document
 4.  Log out then log back in. Technically, all you must do is restart nautilus.
 5.  In a nautilus window, you can now right click a file or set of files and select script -> edit_as_root.

	:::bash
	#!/bin/sh
	IFS="
	"
	for I in $NAUTILUS_SCRIPT_SELECTED_FILE_PATHS
	do
	  gksu gnome-text-editor $I &
	done
	exit



*  Line 1: The shebang, it says run the program /bin/sh to interpret this script. On Ubuntu this is a soft link to the shell called dash.

*  Lines 2-3: Set the internal field separator to a newline only.

*  Lines 4-5 and 7: Loop through every line in [$NAUTILUS_SCRIPT_SELECTED_FILE_PATHS](https///help.ubuntu.com/community/NautilusScriptsHowto#head-c5b169381fecc7fca6463ddb4dbe712fdd4f9080), call the current line $I, and run Line 6.

*  Line 6, gksu: Get authorization via password then run the next command as root. On Ubuntu the gconf key /apps/gksu/sudo-mode makes gksu act like gksudo.

*  Line 6, gnome-text-editor $I: Open file $I with the default text editor. This editor can be changed wih the command 'update-alternatives update-alternatives --config gnome-text-editor'.

*  Line 6, &: Run the command gnome-text-editor in the background. This means don't wait for it to finish before runing the next command.

*  Line 8: The script is finished.




