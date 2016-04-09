---
layout:     post
title:      Dokuwiki httpclient fix
---


For a while now, I had an annoying error every time I created a new page in DokuWiki.

Warning: Invalid argument supplied for foreach() in inc/HTTPClient.php on line 427
Warning: Cannot modify header information - headers already sent by (output started at inc/HTTPClient.php:427) in inc/actions.php on line 296

The problem may have been caused by the blog plugin. The fix is to cast $data to an array in line 427 of HTTPCLient.php like [so](http://forum.dokuwiki.org/thread/1446)

	:::php
	foreach((array) $data as $key => $val){


Tags: {{tag>DokuWiki}}

