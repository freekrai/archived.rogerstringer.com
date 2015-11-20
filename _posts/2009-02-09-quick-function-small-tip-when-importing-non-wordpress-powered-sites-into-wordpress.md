---
layout: "post"
title: "Quick Function: Small Tip When Importing Non-WordPress Powered Sites into WordPress."
tags: 
- "code"
date: "2009-02-09 08:57:57"
ogtype: "article"
bodyclass: "post"
---

Recently,  some clients of mine decided to move a site that was fairly established on it’s own platform to wordpress.

So I was faced with importing all the old articles into wordpress.

For the import, I decided to set up an RSS feed that contained the content, and so I imported all articles to the new system. But then, we found a problem. The old site, used **article.php?id=ARTICLEID**, whereas the new site used **/article-name** so if you went to an article from the old site, you got a 404 error. This caused a problem when the site sent out thousands of emails a day, and was used as a central area of conversation for it’s industry.

We were faced with setting up 301 redirects for over 2000 articles, or finding another solution.

After some research and testing, I noticed that wordpress stores the original article’s address as the new article’s permalink during the import. So we wrote a quick little function that could check for old article references.


    function redirect_fix() {
        global $wpdb;
        if ( isset($_GET['id']) || $_SERVER['QUERY_STRING'] != "" || substr($_SERVER['REQUEST_URI'],-1) == "?") {
            $likestr = "?id=".$_GET['id'];
            $likestra = explode("#",$likestr);
            $likestr = $likestra[0];
            $querystr = "SELECT wposts.* FROM $wpdb->posts AS wposts WHERE wposts.guid LIKE '%".$likestr."'";
            $pageposts = $wpdb->get_results($querystr, OBJECT);
            foreach ($pageposts as $post){
                $url = get_permalink($post->ID);
                if( count($likestra) > 0) $url .= "#".$likestra[1];
                wp_redirect($url,301);
                exit;
            }
        }
    }
    add_action('plugins_loaded','redirect_fix',0);
    


I placed this in the functions.php file of our theme, and set it to work. Now, whenever someone goes to the site using an old URL, they are given a 301 redirect to the new URL. Since we put this fix live over a month ago, we have had no problems with the site.

This tip is just something to show you an idea for handling importing an old established site into wordpress and how to address some of the issues that can come up. This method has worked well for me on the site I used it with, and may help you out later too.

To change it for your needs, change the reference to id to whatever field your site used to use. so that if you used to have a site that called article as the id tag, then you would replace references to “id” with “article”, as shown below:


    function redirect_fix() {
        global $wpdb;
        if ( isset($_GET['article']) || $_SERVER['QUERY_STRING'] != "" || substr($_SERVER['REQUEST_URI'],-1) == "?") {
            $likestr = "?article=".$_GET['article'];
            $likestra = explode("#",$likestr);
            $likestr = $likestra[0];
            $querystr = "SELECT wposts.* FROM $wpdb->posts AS wposts WHERE wposts.guid LIKE '%".$likestr."'";
            $pageposts = $wpdb->get_results($querystr, OBJECT);
            foreach ($pageposts as $post){
                $url = get_permalink($post->ID);
                if( count($likestra) > 0) $url .= "#".$likestra[1];
                wp_redirect($url,301);
                exit;
            }
        }
    }
    add_action('plugins_loaded','redirect_fix',0);
    


It’s that simple to address, and has worked well. Since then I’ve used this trick on my own blog when I merged 360affiliate.com with this blog.