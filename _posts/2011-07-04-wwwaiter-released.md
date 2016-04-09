---
layout:     post
title:      Wwwaiter released
---


I've released a small perl script named [wwaiter](https///github.com/sciurus/wwwaiter). wwwaiter is a program for visualizing delays in receiving portions of a document from a web server. It won't be exact due to buffering done by your webserver and operating system, but it should help diagnose slow parts of web applications.

This is the result of being frustrated that Firebug didn't show me how much of a single document had loaded and the realization that there had to be a better way than running curl and watching for the slow parts. I created a project on google code in case anyone else finds it useful and would like to inform me of any bugs or contribute more functionality.

wwwaiter has two modes of use. In either mode, it shows you the lengths of the delays as it encounters them, the total number of delays and time spent waiting, and the total document load time.

The default mode shows limited context before and after each delay. The default amount of context displayed is 3 lines; this can be changed with the -c option. The verbose mode, specified with -v, shows the entire document and switches the output color after each delay.

By default, wwwaiter reports on any delay greater than 10 milliseconds. This threshhold can be changed with the -d option.

If the web server you are testing uses a port other than port 80, you can specify it with the -p option.

Below is an example of using wwwaiter to report all delays of 50 milliseconds or greater when loading this website.

	
	$ ./wwwaiter -d 50 polibyte.com
	
	--------------------------------------------------------------------------------
	`</div>``<div class="Menu" id="level2">``</div>`
			</div> 
			<div class="Content">
	Waited 0.052656 seconds here
					<div class="hfeed">
	`<div class="plugin_include_content" id="plugin_include__blog:libx_maintenance">`
	`<h1>``<a name="libx_maintenance" id="libx_maintenance" href="/blog/libx_maintenance" title="LibX Maintenance">`LibX Maintenance`</a>``</h1>`
	--------------------------------------------------------------------------------
	
	Waited 1 times for 0.052656 seconds
	Total request time of 0.137702 seconds


Update: Moved project from google code to github.




