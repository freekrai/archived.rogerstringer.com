---
layout: "post"
title: "WordPress Function: Deregister widgets that won’t be used."
tags: 
- "code"
date: "2011-07-12 14:39:00"
ogtype: "article"
bodyclass: "post"
---

WordPress’s widgets are handy, they can simplify and also extend your site. But sometimes, you may have other widgets you want to use (for example, if you’ve built a theme that has it’s own custom widgets), or if you’re setting up a client site that uses widgets, maybe you want them to focus on specific widgets instead, so I like to add this to the theme’s functions.php file and simply remove any widgets from the list that you want to allow, or add any that you want to disallow that aren’t already on the list:


    function unregister_default_wp_widgets() {
        unregister_widget('WP_Widget_Pages');
        unregister_widget('WP_Widget_Calendar');
        unregister_widget('WP_Widget_Archives');
        unregister_widget('WP_Widget_Links');
        unregister_widget('WP_Widget_Meta');
        unregister_widget('WP_Widget_Search');
        unregister_widget('WP_Widget_Categories');
        unregister_widget('WP_Widget_Recent_Posts');
        unregister_widget('WP_Widget_Recent_Comments');
        unregister_widget('WP_Widget_Tag_Cloud');
        unregister_widget('WP_Widget_RSS');
        unregister_widget('WP_Widget_Akismet');
    }
    add_action('widgets_init', 'unregister_default_wp_widgets', 1);
    


The end result, is a cleaner widget page so your users can focus exactly on the widgets you want them to use.