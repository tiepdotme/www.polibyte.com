---
layout:     post
title:      Monitoring a process via python
---


A friend of mine recently mentioned he was working on the following problem:

>We currently have a set of shell scripts that monitor a problematic procedure. There's a main script "A" which runs a process that has a "progress bar" of sorts that we redirect into a file. Script "B" monitors the file and makes sure that the process hasn't hung up. If it has, script B restarts script A. There's then a third script "C" which monitors script B to make sure it's still working.
>All of these scripts were written (IMNSHO) very poorly and I'd like to rewrite them in a language that has more robust handling of sub-process output etc.

Although I haven't been writing Python for a while, I remembered the subprocess module and thought it would be perfect for this.


First, let's mock up a "problematic procedure". This program's runtime will vary, and it will output the string "bad" or "good" every second until it completes. We'll use "bad" to denote that it is failing and needs to be restarted.

	:::python
	import random, sys, time
	
	while True:
	    r = random.randint(1,6) 
	    if r == 1:
	        sys.exit()
	    elif r == 2 or r == 3:
	        print "bad"
	        sys.stdout.flush()
	    else:
	        print "good"
	        sys.stdout.flush()
	    time.sleep(1)


Now let's write our watchdog. We'll place the logic in a function. The function starts the procedure and then runs a loop until it exits. In the loop we read the output from the procedure. If it's bad we kill the procedure and start our monitoring function again. Now in the body of the program all we have to do is start our function, and it will recursivley call itself until the procedure completes without an error (or we run out of stack frames).

	:::python
	import os, re, signal, subprocess
	
	def monitor():
	    print "Starting procedure"
	
	    p = subprocess.Popen('./problematic_procedure.py',
	                        stdout=subprocess.PIPE
	                        )
	
	    while p.poll() is None:
	        line = p.stdout.readline()
	        print "Procedure output was %s" % line
	        if re.match('bad', line):
	            os.kill(p.pid, signal.SIGTERM)
	            print "Procedure failed so I killed it"
	            monitor()
	
	monitor()
	print 'Procedure exited'


For more uses of subprocess, check out the [PyMOTW entry](http://pymotw.com/2/subprocess/).


