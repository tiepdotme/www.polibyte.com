---
layout:     post
title:      Uga proxy bookmarklet
---


Along with my recent Ubuntu upgrade came the Firefox 3 beta, but LibX has not released a version of their extension that works with Firefox 3 yet. If you're like me, all links lead to JSTOR, and you're really missing the ease with which LibX allowed you to reload a page through your institution's proxy. Luckily, this is as easy as adding a bookmark to firefox with the following as the location. If you're not at UGA, you should replace the text inside the quotation marks with the URL of the proxy you use.

	:::javascript
	 javascript:void(location.href="http://proxy-remote.galib.uga.edu:2048/login?url="+location.href);


Now if only del.icio.us would update their extension.




