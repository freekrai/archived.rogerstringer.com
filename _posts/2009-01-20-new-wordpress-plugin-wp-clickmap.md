---
layout: "post"
title: "New WordPress plugin: WP-Clickmap"
tags: 
- "articles"
date: "2009-01-20 22:08:36"
ogtype: "article"
bodyclass: "post"
---

![wpclickmap.txt](http://cdn.rogerstringer.com/wp-content/uploads/2010/02/cap.png)

Yesterday, over at [css-tricks.com](http://css-tricks.com/tracking-clicks-building-a-clickmap-with-php-and-jquery/),  Chris published an article by [Jay Salvat](http://www.jaysalvat.com) about using PHP and jQuery to create a clickmap on your site. I started playing with that idea and decided to make it into a wordpress plugin. A couple hours of coding, and several versions later, I happily finished WP-Clickmap v1.0 This little plugin will store each click made on pages of your site in a database, and from there, you’ll be able to go to your blog’s admin section and view the various pages. See what is popular with your visitors and what’s not. It’s had some interesting results on the sites I’ve run it on so far, and wanted to share it here for people to download and play around with.

[Download wp-clickmap.zip](http://www.rogerstringer.com/wp-content/uploads/2009/01/wp-clickmap.zip)

For anyone who is wondering about the advantages of clickmaps, clickmaps let you see your site from a usability perspective. Your clickmaps show literally every click a user makes on a particular page. Every click is loaded into the database and over time a visual representation of collective clicks gives you an overview of the hotspots on the page. While it is interesting to see links that are being clicked on, it is even more valuable to see where people are that are not links.

*Why do people keep clicking on the middle of my page?* WP-clickmap is based on the original idea Jay Salvat published on his [blog](http://blog.jaysalvat.com/articles/suivre-et-analyser-les-clics-de-ses-utilisateurs-en-php-et-jquery.php) and on [css-tricks.com](http://css-tricks.com/tracking-clicks-building-a-clickmap-with-php-and-jquery/). Thanks to Jay and Chris for the idea.