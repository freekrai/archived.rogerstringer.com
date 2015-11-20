---
layout: "post"
title: "Quick Function: Force iPhone redirection"
tags: 
- "code"
date: "2009-07-27 14:46:27"
ogtype: "article"
bodyclass: "post"
---

I use one of my [scripts](http://themeforest.net/item/mymobilesite/49313?ref=freekrai) to display a mobile version of my sites using the RSS feeds, but sometimes I have to redirect people automatically, so I usually use this piece of code somewhere:


    $useragents = array ("iPhone","iPod","blackberry","palm","smartphone","iemobile"); 
    $oniphone = false; 
    foreach ( $useragents as $useragent ) { if (eregi($useragent,$container)){ $oniphone = true; } } 
    if($oniphone){header("Location:/m/");exit;}
    


If I’m on a wordpress site, then the code is placed in my functions.php file for my theme. If I’m on another site, then I usually place it on a file I know will get called. Generally, you can change it to look for a cookie if you choose to let people opt for the full version of your site, but otherwise, this code comes in handy.