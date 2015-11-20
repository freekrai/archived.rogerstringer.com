---
layout: "post"
title: "Quick Function: Creating Shorter URLs on your wordpress blog"
tags: 
- "code"
date: "2009-03-10 11:02:30"
ogtype: "article"
bodyclass: "post"
---

When I redid this blog, I wanted to make a “tweet this” button, but I didn’t want to send the regular URL to twitter, between my name and some of my articles, you can reach the 140 character limit pretty quick.

I also didn’t want to have to rely on a url shortening service for the shorter URLs, as that can cause it’s own problems since people aren’t seeing your domain name in the URL and what happens if that URL service closes it’s doors or decides to delete all links that are older than x number of days? You have links to your blog that don’t go anywhere.

So I decided to cut out the middle man and create my own functions. There are 2 functions, one handles creating the shorter URLs, and the other function handles the redirect. Place them in your **functions.php** file in your theme:


    function get_shorter_url(){
        global  $wp_query, $post; 
        $post_id = $wp_query->post->ID;
        $tinyURL = get_post_meta($post_id, 'Shorter URL', true);
        if ( $tinyURL == ''){
            $tinyURL =  '/' . base_convert($post_id, 10, 36) . '.goto';
            add_post_meta($post_id, 'Shorter URL', $tinyURL, true); 
        }
        $tinyurl = get_bloginfo('wpurl').$tinyURL;
        $tinyurl = str_replace("www.","",$tinyurl);
        return $tinyurl;
    }
    function redirect_shorter_url() {
        if(is_404()){
            global $wpdb;
            $postmetaquery = "SELECT `post_id`, `meta_value` FROM `$wpdb->postmeta` WHERE meta_key LIKE 'Shorter URL' AND meta_value != ''";
            $postarr = $wpdb->get_results($postmetaquery,ARRAY_A);      
            foreach($postarr as $arr){
                if((get_bloginfo('wpurl') .$arr['meta_value']) == ('http://' . $_SERVER["SERVER_NAME"].$_SERVER["REQUEST_URI"])){
                    $url = get_permalink($arr['post_id']);
                    header("HTTP/1.0 301 Moved Permanently");
                    header ("Location: $url");
                    exit;
                }
            }   
        }
    }
    add_action('template_redirect', 'redirect_shorter_url');    
    


Now, whenever you want to use a shorter url, you just call the function in your theme, like so:


    echo get_shorter_url();
    


So for example, if I wanted to add a tweet this link to the bottom of my article, I would open single.php and add this link:


    <A href="http://twitter.com/home?status=Currently reading <?php echo get_shorter_url() ?>" title="Click to send this page to Twitter!" target="_blank">Tweet this</a>