---
layout:     post
title:      Python mysql and unicode
---


The top google result for the search terms mysql, python, and unicode is a [blog post](http://tahpot.blogspot.com/2005/06/mysql-and-python-and-unicode.html) that recommends using the init_command option to the construct a MySQLdb connection to run the SQL 'SET NAMES utf8'. A better way is to set the charset option to 'utf8'. This way MySQLdb realizes the connection is using utf8 and encodes things properly. For more explanation see [MySQLDB's user guide](http://mysql-python.sourceforge.net/MySQLdb.html).




