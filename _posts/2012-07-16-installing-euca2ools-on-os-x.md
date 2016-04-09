---
layout:     post
title:      Installing euca2ools on os x
---


Download the tag of the latest stable euca2ools release from [github](https///github.com/eucalyptus/euca2ools/tags). Install [virtualenv](http://www.virtualenv.org/en/latest/index.html) with 'sudo easy_install virtualenv' and [homebrew](http://mxcl.github.com/homebrew/) following their instructions if you haven't already.

Using virtualenv and homebrew isn't mandatory. Homebrew makes it much easier to install many tools and libraries like swig, and virtualenv lets you keep isolated python environments.

	
	brew install swig
	
	mkdir -p ~/.virtualenvs/euca2ools
	virtualenv ~/.virtualenvs/euca2ools
	source ~/.virtualenvs/euca2ools/bin/activate
	
	pip install boto
	pip install m2crypto
	
	# the filename will vary depending on what version is the latest
	tar xzf eucalyptus-euca2ools-2.1.0-0-gb5c5889.tar.gz
	cd eucalyptus-euca2ools-c5c7caa
	python setup.py install
	


After this is done, you'll have your euca2ools in '~/.virtualenvs/euca2ools/bin'. In every new shell you start, you'll have to run 'source ~/.virtualenvs/euca2ools/bin/activate' before you can use them.




