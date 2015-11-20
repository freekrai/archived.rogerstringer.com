---
layout: "post"
title: "WordPress: Using multisite domain mapping on a mediatemple grid server"
tags: 
- "articles"
date: "2011-01-03 19:36:01"
ogtype: "article"
bodyclass: "post"
---

This won’t get into setting up multisite in wordpress, but it will help you set up multisite domain mapping on a grid server from media temple. If you need to see a good intro article on setting up multisite mode in wordpress, [check this article out](http://www.snipe.net/2010/06/upgrading-to-wordpress-3/) or [this article for a good multisite .htaccess intro](http://perishablepress.com/press/2010/07/07/htaccess-code-for-wordpress-multisite/).

First, install [WordPress MU Domain Mapping plugin](http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/), and follow the instructions for setting that up.

Now, to add a new domain to media temple that will show up on your site, the big catch is to redirect this domain to the existing main domain.

For example, originalsite.com is the main root domain, and I am going to create newsite.com.

1. Add the domains you’d like to use (newsite.com in this example) as alternate domains in the AccountCenter.
2. Log into your (gs) via SSH, move into the domains directory and delete the folders for newsite.com with these commands:


    cd domains rm -rf newsite.com
3. Create a symlink named ‘newsite.com’ that points at the originalsite.com folder with this command:


    ln -s originalsite.com newsite.com

This will now have newsite.com pointing to originalsite.com, which thanks to the domain mapping plugin above, you will see the new site show up when you run it. I tried to make this sound as straight forward as I could, so I apologize if it ended up being more complex.