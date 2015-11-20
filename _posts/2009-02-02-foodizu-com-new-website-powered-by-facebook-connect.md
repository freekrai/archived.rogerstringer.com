---
layout: "post"
title: "Foodizu.com - new website, powered by facebook connect"
tags: 
- "articles"
date: "2009-02-02 10:50:05"
ogtype: "article"
bodyclass: "post"
---

![foodizucom-powered-by-facebook-connect.txt](http://rogerstringer.com/thumbs/foodizu.jpg)

I recently started work on a new website called [foodizu.com](http://www.foodizu.com), this site was built mostly as an experiment in using Facebook Connect to power a website. In this case, this website was a site where you could add recipes, comment on recipes and bookmark recipes that others have added. I decided to make use of a couple different pieces of available technologies:

1. *Facebook Connect* for user logins
2. *Disqus.com *for comments (which also accepts Facebook Connect)
3. *AddThis.com *for bookmarking off the home page
4. *ShareThis* for bookmarking recipes. My reason for doing this was mostly to see how it all handled together. I’m generally the bootstrap type guy, who builds everything from scratch. But for the purpose of this project, I decided to just go with the existing services.

Setting up facebook connect was pretty straight forward. You can build the site itself pretty quickly and they have lots of tips for handling user connections. I also included the ability for people to sign up who didn’t want to use Facebook. One advantage to using facebook, is that you can have facebook notify your friends that you’ve just added a new recipe on the website, by posting it on your newsfeed. This is optional, and entirely up to the user.

In displaying the recipes, I decided to use disqus.com to handle comments, as I could also have that site use facebook connect. I also placed ShareThis on that page to take care of people wanting to bookmark certain recipes.

I probably put in about 20 hours development time on the site. Between the disqus setup, testing the connections to facebook. Configuring user feeds, handling how recipes were posted and displayed, setting up RSS feeds, setting up Pings to feedburner (google now) to update the RSS feeds. So what’s next? Well, I haven’t decided yet, but it should be interesting.

*Ok,  as an update, setting disqus to use facebook connect on a site powered by facebook connect just doesn’t work. The two sites actually cancel each other out, so that logging into one, makes the other instead of connect think you are logged into that one, and yet, you are not which means you get issues. End result, is that if you use facebook connect, it’s better to stick to your own comments system.**Lesson learned on that one.*