---
layout: "post"
title: "Some handy WordPress multisite functions"
link: "https://gist.github.com/1092505"
tags: 
- "code"
date: "2011-07-19 14:37:40"
ogtype: "article"
bodyclass: "post"
---

Quite often when working with WordPress, I end up working with the multisite functionality, and usually the client (which can be myself) wants to show posts on the main site from across the various blogs. These functions make that possible, mainly the main function `multisite_latest_posts` makes that possible. 

The name isn’t entirely true, yes it returns the latest posts, but you can control the sorting, the amount of posts returned, add some pagination and thanks to the recent addition of meta table queries to the function, return only what you need. 

I’ve been using this function for a while and didn’t want to totally rename it: To start, I usually create a file in my theme called `multisite_functions.php`, or I copy the code into functions.php, your choice. 

But if you choose to create the file, then remember to include it in your functions.php file like so:

```php
	include("multisite_functions.php");
```	

Again, there’s also no reason not to just copy these functions directly into your functions.php file if you choose. Anyhow, let’s get onto the code.

```php
	function multisite_latest_post($args) {
		global $wpdb;
		extract($args);
		//  validate
		if( empty($how_many) )		  $how_many = 10;
		if( empty($how_long_days) )	 $how_long_days = 30;
		if( empty($how_many_words) )	$how_many_words = 50;
		if( empty($more_text) )		 $more_text = "[...]";
		if( empty($remove_html) )	   $remove_html = true;
		if( empty($sort_by) )		   $sort_by = "post_date";
		if( empty($post_type) )		 $post_type = "post";
		if( empty($paginate) )		  $paginate = true;
		if( empty($start) )			 $start = 0;
		if( empty($end) )			   $end = 25;
		if( !isset($meta) )			 $meta = array();
		$query = "SELECT blog_id FROM $wpdb->blogs WHERE blog_id !='1'";
		$blogs = $wpdb->get_col($query);
		if ($blogs) {
			foreach ($blogs as $blog) {
				$blogPostsTable = 'wp_'.$blog.'_posts';
				$blogPostMetaTable = 'wp_'.$blog.'_postmeta';
				$db_query = "SELECT sql_calc_found_rows $blogPostsTable.ID,
							$blogPostsTable.post_author,
							$blogPostsTable.post_title,
							$blogPostsTable.guid,
							$blogPostsTable.post_date,
							$blogPostsTable.post_content,
							$blogPostsTable.post_modified,
							$blogPostsTable.comment_count
							FROM $blogPostsTable";
				$i = 1;
				if( count($metas) ){
					foreach($metas as $mname=>$mval){
						$db_query .= " JOIN $blogPostMetaTable m{$i} ON m{$i}.post_id = $blogPostsTable.ID";
						$i++;
					}
					reset($metas);
					$db_query .= " WHERE $blogPostsTable.post_status = 'publish' AND $blogPostsTable.post_type = '".$post_type."'";
					$i = 1;
					foreach($metas as $mname=>$mval){
						$db_query .= " AND m{$i}.meta_key='{$mname}' AND m{$i}.meta_value='{$mval}'";
						$i++;
					}
				}else{
					$db_query .= " WHERE $blogPostsTable.post_status = 'publish' AND $blogPostsTable.post_type = '".$post_type."'";
				}
				if( $paginate ){
					$db_query .= " LIMIT $start,$end";
				}else{
					$db_query .= " AND $blogPostsTable.post_date >= DATE_SUB(CURRENT_DATE(), INTERVAL $how_long_days DAY)";
				}
				$thispos = $wpdb->get_results($db_query);
				foreach($thispos as $thispost) {
					if($sort_by == 'post_date') {
						$order = $thispost->post_date;
					}else{
						$order = $thispost->post_modified;
					}
					$post_dates[]		   = $order;
					$post_guids[$order]	 = $thispost->guid;
					$blog_IDs[$order]	   = $blog;
					$post_IDs[$order]	   = $thispost->ID;
					$post_titles[$order]	= $thispost->post_title;
					$post_authors[$order]   = $thispost->post_author;
					$post_contents[$order]  = $thispost->post_content;
					$comments[$order]	   = $thispost->comment_count;
				}
			}
			rsort($post_dates);
			$union_results  = array_unique($post_dates);
			$ResultArray	= array_slice($union_results, 0, $how_many);
			foreach ($ResultArray as $date) {
				$ID				 = $post_IDs[$date];
				$id_author		  = $post_authors[$date];
				$post_url		   = get_blog_permalink($blog_IDs[$date], $ID);/*$post_guids[$date];*/
				$post_title		 = $post_titles[$date];
				$post_content	   = $post_contents[$date];
				$post_date		  = mysql2date(get_option('date_format'), $date);
				$post_time		  = mysql2date(get_option('time_format'), $date);
				$total_comment	  = $comments[$date];
				$user_info		  = get_userdata($id_author);
				$author_blog_url	= get_blogaddress_by_id($user_info->primary_blog);
				$author_url		 = $user_info->user_url;
				$author_email	   = $user_info->user_email;
				$blog_id			= $blog_IDs[$date];
				if($user_info->first_name) {
					$author_name = $user_info->first_name.' '.$user_info->last_name;
				}else{
					$author_name = $user_info->nickname;
				}
				if($remove_html) {
					$post_content = multisite_cleanup_post($post_content);
				}
				$results = array();
				$results['ID']			  = $ID;
				$results['post_url']		= $post_url;
				$results['post_title']	  = $post_title;
				$results['post_content']	= multisite_cut_article_by_words($post_content, $how_many_words);
				if ($results['post_content'] != $post_content)
					$results['post_content'] .= sprintf('  <a href="%s">%s</a>', $post_url, $more_text);
				$results['author_blog_url'] = $author_blog_url;
				$results['author_url']	  = $author_url;
				$results['author_email']	= $author_email;
				$results['author_name']	 = $author_name;
				$results['post_date']	   = $post_date;
				$results['post_time']	   = $post_time;
				$results['comment_count']   = $total_comment;
				$results['blog_id']		 = $blog_id;
				$returns[] = $results;
			}
			$latest_posts = multisite_bind_array_to_object($returns);
			$total = $wpdb->get_var("SELECT FOUND_ROWS() as total");
			return array("posts"=>$latest_posts,"total"=>$total);
		}
	}
	function multisite_post_meta($blog,$post,$key){
		global $wpdb;
		$blogPostsTable = 'wp_'.$blog.'_postmeta';
		$query = "SELECT meta_value FROM {$blogPostsTable} WHERE post_id ='{$post}' AND meta_key='{$key}'";
		$meta_value = $wpdb->get_var($query);
		return $meta_value;
	}
	function get_blog_domain($blog_id){
		global $wpdb;
		$query = "SELECT domain FROM $wpdb->blogs WHERE blog_id ='{$blog_id}'";
		$domain = $wpdb->get_var($query);
		return $domain;
	}
	function multisite_bind_array_to_object($array) {
		$return = new stdClass();
		foreach ($array as $k => $v) {
			if (is_array($v)) {
				$return->$k = multisite_bind_array_to_object($v);
			}else {
				$return->$k = $v;
			}
		}
		return $return;
	}
	function multisite_cut_article_by_words($original_text, $how_many) {
		$word_cut = strtok($original_text," ");
		$return = '';
		for ($i=1;$i<=$how_many;$i++) {
			$return   .= $word_cut;
			$return   .= (" ");
			$word_cut = strtok(" ");
		}
		$return .= '';
		return $return;
	}
	function multisite_cleanup_post($source) {
		$replace_all_html = strip_tags($source);
		$bbc_tag = array('//is');
		$result = preg_replace($bbc_tag, '', $replace_all_html);
		return $result;
	}
```	


In a file in your theme, you can call the latest_post function like so:

```php
	$posts = multisite_latest_post( array(
		"how_many"=>10,
		"how_long_days"=>30,
		"how_many_words"=>50,
		"more_text"=>"[...]",
		"remove_html"=>true,
		"sort_by"=>"post_date",
		//   if paginating: (otherwise, set paginate to false or don't include it or the start or end variables):
		"paginate"=>true,
		"start"=>0,
		"end"=>25,
		//   query based on meta values: (key / value)
		"metas"=>array(
			"tumblog"=>"articles",
		)
	));
	echo "There were ".$posts['total']." posts found";
	foreach($posts['posts'] as $item){
		echo '<li><a href="'.$item->post_url.'">'.$item->post_title.'</a></li>';
	}
```


And that’s it, you can now get the latest posts from across your sites and display them somewhere. How you choose to display them is up to you, this example just shows you a list of the 10 most recent posts.