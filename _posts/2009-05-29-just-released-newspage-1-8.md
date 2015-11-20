---
layout: "post"
title: "Just released: newsPage 1.8"
tags: 
- "articles"
date: "2009-05-29 20:07:36"
ogtype: "article"
bodyclass: "post"
---

A couple days ago, I added some news features to newsPage and fixed a couple of bugs. Bugs fixed: Mostly, the main bug was a display one, now, if you mouse over a story, it will only show the CSS-controlled summary instead of also showing the URL’s title on top of that. New features: We’ve added some more shortcode functionality, so you can now go:


    [newspage limit=5 topic="web design"]
    


and have it show all topics under the web design topic, and show 5 stories by page. Web design is just an example here, you can use it for which ever topic you have. Also, you can simply call


    [newspage limit=10]
    


And you will display all links, 10 stories per link just as always. This was one of the most requested features from newsPage’s users and now that things have finally settled a little around here, I was able to get the time I needed to add it. At the same time, all other methods of displaying newsPages still work as they did before, I just added onto the functionality a little. There are some more features coming shortly, I just have to get Friendz finished first (the delays have been unfortunate and not my fault, and I promise everyone will see the final version very soon).

[download files](http://wordpress.org/extend/plugins/newspage/)