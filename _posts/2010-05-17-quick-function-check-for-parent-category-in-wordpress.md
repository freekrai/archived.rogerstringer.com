---
layout: "post"
title: "Quick Function: Check for parent category in wordpress"
tags: 
- "code"
date: "2010-05-17 15:18:23"
ogtype: "article"
bodyclass: "post"
---

When I was working on foodjumper, there were times when I wanted to check the parent category, for example, if you’re in the *Dinner* category or viewing a post in that category, I want to display the proper layout for posts in the *Recipes* category. One function that works, is to do a check like this:


    function parent_category( $cats, $_post = null ){
        if( is_string($cats) )  $cats = get_cat_ID($cats);
        foreach ( (array) $cats as $cat ) {
            $descendants = get_term_children( (int) $cat, 'category');
            if ( $descendants && in_category( $descendants, $_id ) ) return true;
        }
        return false;
    }
    


WordPress published a similar function on their site, but it used the actual category ID in the function. I found it easier to let you enter the slug instead. Now, when you want to check if a post is in a category that is a child, you can do a check like this:


    if(in_category('recipes') || parent_category('recipes') ): 
        // display this post as a recipe
    else: 
        // display this post as a normal post
    endif;
    


This can be adapted to almost any type of similar treatment, I just used recipes above as an example since it’s where I just used this function.