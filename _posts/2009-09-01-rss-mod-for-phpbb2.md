---
layout:     post
title:      Rss mod for phpbb2
---


If anyone is using the following mod to a phpbb installation,

	:::php
	/***************************************************************************

	*                                  rss.php
	*                            -------------------
	*   begin                : Monday, July 7, 2003
	*   notes                : This code is based on the work of the original
	*                          developer below.  Portions of this code
	*                          'borrowed' from phpbb_fetch_posts, an
	*                          untitled rdf content syndicator posted at
	*                          phpbb.com, and phpbb itself.
	*   email                : rss@wickedwisdom.com
	*
	*
	*   $Id: rss.php,v 2.0.1 2003/07/17 10:11:00 nschindler Exp $
	*
	*
	***************************************************************************/


I've found that the following patch which splits the query into two greatly increases the performance. MySQL's join plan for the original query starts with the  post text table but we have to sort by the post table, necessitating a temporary table and filesort over a potentially huge number of records. Selecting the id's of the 10 posts we actually care about before loading the related information avoids that.

	:::php
	--- rss.php.original	2009-08-24 20:58:07.000000000 -0400
	+++ rss.php	2009-08-24 22:11:54.000000000 -0400
	@@ -1,4 +1,17 @@
	 <?php
	
	+
	
	+/***************************************************************************
	
	+*				   rss.php
	
	+*			     -------------------
	
	+*   begin		 : Monday, August 24, 2009
	
	+*   notes		 : This code is based on the work of the original
	
	+*			   developers below. The SQL was modified
	
	+*			   to improve performance
	
	+*   email		 : brian@polibyte.com
	
	+*
	
	+*
	
	+***************************************************************************/
	
	+
	
	 /***************************************************************************
	

	 *				   rss.php
	

	 *			     -------------------
	
	@@ -141,6 +154,17 @@
	 //
	
	 // BEGIN SQL statement to fetch active posts of public forums
	
	 //
	
	+$sql = "SELECT post_id FROM " . POSTS_TABLE . " ORDER BY post_time DESC LIMIT $count;";
	
	+$id_query = $db->sql_query($sql);
	
	+$ids_array = array();
	
	+
	
	+while ($id = $db->sql_fetchrow($id_query))
	
	+{
	
	+	$ids_array[] = $id['post_id'];
	
	+}
	
	+
	
	+$ids = implode(',', $ids_array);
	
	+
	
	 $sql = "SELECT f.forum_name, t.topic_title, u.user_id, u.username, u.user_sig, u.user_sig_bbcode_uid, p.post_id, pt.post_text, pt.post_subject, pt.bbcode_uid, p.post_time, t.topic_replies, t.topic_first_post_id
	
	 	FROM " . FORUMS_TABLE . " AS f, " . TOPICS_TABLE . " AS t, " . USERS_TABLE . " AS u, " . POSTS_TABLE . " AS p, " . POSTS_TEXT_TABLE . " as pt
	
	 	WHERE
	
	@@ -151,7 +175,8 @@
	 			AND p.topic_id = t.topic_id
	
	 			$sql_topics_only_where
	
	 			$sql_forum_where
	
	-	ORDER BY p.post_time DESC LIMIT $count";
	
	+			AND p.post_id in ($ids)
	
	+	ORDER BY p.post_time DESC"; // could take this out and reverse the sorting using php instead
	
	 $posts_query = $db->sql_query($sql);
	
	 //
	
	 // END SQL statement to fetch active posts of public forums
	






