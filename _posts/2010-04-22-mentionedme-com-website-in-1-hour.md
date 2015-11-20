---
layout: "post"
title: "Mentionedme.com: Website in 1 hour"
tags: 
- "articles"
date: "2010-04-22 17:52:12"
ogtype: "article"
bodyclass: "post"
---

![mentionedmecom-website-1-hour.txt](http://cdn.rogerstringer.com/wp-content/uploads/2010/04/mentionedme.png)

[MentionedMe.com](http://www.mentionedme.com) was another website I recently built with the idea of providing a very quick solution. I wanted to offer a site that would text you if you got a new mention on twitter, pretty simple process, and one that has uses for people monitoring their username mentions. I decided to once again use the SMS API form [twilio.com](http://twilio.com) as a starting point since I’ve been using it quite a lot in recent projects, and then used YQL to perform twitter searches on usernames that are added. The end result is that every 15 minutes, it does a check, and if there is a recent mention of your name, then you get sent text message saying “*you have [insert number here] new mentions for @[insert your name here]*”. I used the YUI framework to quickly throw together the site design, and it’s been working well.