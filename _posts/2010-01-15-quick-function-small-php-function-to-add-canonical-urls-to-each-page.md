---
layout: "post"
title: "Quick Function: small PHP function to add canonical URLs to each page"
tags: 
- "code"
date: "2010-01-15 17:35:10"
ogtype: "article"
bodyclass: "post"
---

When I redid Foodizu a few months ago, I added canonical URLs to each page. The easiest to do this was with a quick PHP function:


    function canonical_link(){
        $url = 'http';
        if ($_SERVER["HTTPS"] == "on") {$url .= "s";}
        $url .= "://";
        if ($_SERVER["SERVER_PORT"] != "80") {
            $url .= $_SERVER["SERVER_NAME"].":".$_SERVER["SERVER_PORT"];
        } else {
            $url .= $_SERVER["SERVER_NAME"];
        }
        if( isset($_SERVER['REQUEST_URI']) ){
            $url = $url . $_SERVER['REQUEST_URI'];
        }
        if($url){
            echo '<link rel="canonical" href="'.$url.'" />'."n";
        }
    }
    


to call this function you simply add in your header somewhere:


    <?php canonical_link(); ?>
    


and it will display the canonical URL.