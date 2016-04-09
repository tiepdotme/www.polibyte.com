---
layout:     post
title:      Prying the lid off vagrant
---


When developing software, it's convenient to have a REPL. For Ruby, the best choice I know of is [Pry](http://pryrepl.org/). In addition to facilitating exploratory programming, it can act as a console for exploring a running program's state. It also has a plugin for basic [integration](https///github.com/deivid-rodriguez/pry-byebug) with a [debugger](https///github.com/deivid-rodriguez/byebug). It sounds like it would come in handy for vagrant plugin development, woudn't it?

Vagrant bundles its own ruby, so you don't have access to gems you've installed through normal means. However, it's easy to add gems to your vagrant environment. ''vagrant plugin install'' is primarily a wrapper around Bundler, and it's happy to try to install any gem from a local file or rubygems. Thus, all you need in order to install pry is

	
	vagrant plugin install rb-readline
	vagrant plugin install pry


rb-readline is needed because vagrant's bundled ruby isn't compiled with readline support. Now drop ''binding.pry'' in your plugin's code and the next time you run vagrant you'll be dropped into pry at that line.

Running pry in your vagrant environment without starting vagrant first is only slightly more complicated. By default on linux vagrant's embedded ruby looks for gems in */opt/vagrant/embedded/lib/ruby/2.0.0* (this version number will change as vagrant moves to newer rubies). ''vagrant plugin install'' writes to a different location, *$HOME/.vagrant.d/gems*. However, the binstubs bundler writes there don't look for gems there. You'll need to set the *GEM_PATH* environment variable to that directory before starting pry. Alternately, edit the pry binstub and after the 'require' add

`Gem.path.unshift(File.expand_path('..', File.dirname(__FILE__)))`

After taking either approach, you can run start pry with ''$HOME/.vagrant.d/gems/bin/pry''




