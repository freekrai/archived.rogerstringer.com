---
layout: "post"
title: "Quick Function: Get a post"
tags: 
- "code"
date: "2010-06-02 16:50:37"
ogtype: "article"
bodyclass: "post"
---

Here’s another little function that tends to come in handy. Recently, on a project I did for a client, they had a sidebar where they wanted to display pages that were children of a main page. I whipped up this function to get the ID of a page’s slug and then perform a query:


    function get_ID_by_slug($page_slug) {
        $page = get_page_by_path($page_slug);
        if ($page) {
            return $page->ID;
        } else {
            return null;
        }
    }
    


So for example, you could then do a query like this in your sidebar:


    $querystr = "SELECT wposts.* FROM $wpdb->posts wposts WHERE wposts.post_parent = '".get_ID_by_slug("white-papers")."' AND wposts.post_status = 'publish' AND wposts.post_type = 'page'";
    


And you would get all pages that are children of “white-papers”, it’s basic I know, but it works, and has a few other uses than just where I used it.