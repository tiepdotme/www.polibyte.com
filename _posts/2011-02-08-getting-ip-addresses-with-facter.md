---
layout:     post
title:      Getting ip addresses with facter
---


When configuring services with [ Puppet](http://www.puppetlabs.com), you sometimes need to know the IP address of a server. For example, I export nagios host definitions from many of my servers and use them to configure my nagios service. [ Facter](http://www.puppetlabs.com/puppet/related-projects/facter/ ) ships with two sets of network-related facts that can help.

The first is the ipaddress fact. This fact contains the IP address of the first first network interface reported by ifconfig, which outputs them in alphabetical order. The second is a fact for each network interface that has an IP address; unsurprisingly, it contains the interface's IP address.

For example, here are the facts from a server with three network interfaces, eth0, eth1, and eth2. eth1 and eth2 are bonded as the device bond0.

	:::ruby
	ipaddress => 172.16.32.48
	ipaddress_bond0 => 172.16.32.48
	ipaddress_eth0 => 192.0.32.10


In my environment, the device specific facts are too narrow. Although I'm consistent on which interfaces are plugged into which network, some servers use bonding or bridging so I can't assume that the device that has the physical link is what the ip address is assigned to. The private ip might be on eth1 or bond0, for example. Of course, this means the ipaddress fact is too broad. It will be a public IP address on some servers and a private IP address on others, depending on how the interfaces are named.

To improve on this, I've created two custom facts, ipaddress_public and ipaddress_private. Instead of containing the first IP address they find, they contain the first public or private IP address. I also have two facts, on_public and on_private, that report whether a server has any public or private IP address. Here is how my previous example looks with these new facts.

	:::ruby
	ipaddress => 172.16.32.48
	ipaddress_bond0 => 172.16.32.48
	ipaddress_eth0 => 192.0.32.10
	ipaddress_private => 172.16.32.48
	ipaddress_public => 192.0.32.10
	on_private => true
	on_public => true


The code for the facts is below.

	:::ruby
	require 'facter/util/ip'
	
	def has_address(interface)
	  ip = Facter::Util::IP.get_interface_value(interface, 'ipaddress')
	  if ip.nil?
	    false
	  else
	    true
	  end
	end
	
	def is_private(interface)
	  rfc1918 = Regexp.new('^10\.|^172\.(?:1[6-9]|2[0-9]|3[0-1])\.|^192\.168\.')
	  ip = Facter::Util::IP.get_interface_value(interface, 'ipaddress')
	  if rfc1918.match(ip)
	    true
	  else
	    false
	  end
	end
	
	def find_networks
	  found_public = found_private = false
	  Facter::Util::IP.get_interfaces.each do |interface|
	    if has_address(interface)
	      if is_private(interface)
	        found_private = true
	      else
	        found_public = true
	      end
	    end
	  end
	  [found_public, found_private]
	end
	
	# these facts check if any interface is on a public or private network
	# they return the string true or false
	# this fact will always be present
	
	Facter.add(:on_public) do
	  confine :kernel => Facter::Util::IP.supported_platforms
	  setcode do
	    found_public, found_private = find_networks
	    found_public
	  end
	end
	
	Facter.add(:on_private) do
	  confine :kernel => Facter::Util::IP.supported_platforms
	  setcode do
	    found_public, found_private = find_networks
	    found_private
	  end
	end
	
	# these facts return the first public or private ip address found
	# when iterating over the interfaces in alphabetical order
	# if no matching address is found the fact won't be present
	
	Facter.add(:ipaddress_public) do
	  confine :kernel => Facter::Util::IP.supported_platforms
	  setcode do
	    ip=""
	    Facter::Util::IP.get_interfaces.each do |interface|
	      if has_address(interface)
	        if not is_private(interface)
	          ip = Facter::Util::IP.get_interface_value(interface, 'ipaddress')
	          break
	        end
	      end
	    end
	    ip
	  end
	end
	
	Facter.add(:ipaddress_private) do
	  confine :kernel => Facter::Util::IP.supported_platforms
	  setcode do
	    ip=""
	    Facter::Util::IP.get_interfaces.each do |interface|
	      if has_address(interface)
	        if is_private(interface)
	          ip = Facter::Util::IP.get_interface_value(interface, 'ipaddress')
	          break
	        end
	      end
	    end
	    ip
	  end
	end






