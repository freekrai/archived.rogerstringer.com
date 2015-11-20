---
layout: "post"
title: "Quick Function: iPad Detection"
tags: 
- "code"
date: "2010-04-28 16:13:37"
ogtype: "article"
bodyclass: "post"
---

Of course, the iPad is a pretty large screen and a fully capable browser, so most websites dont need to have iPad specific versions of them. But if you need to, you can detect for it with .htaccess


    RewriteCond %{HTTP_USER_AGENT} ^.*iPad.*$
    RewriteRule ^(.*)$ http://ipad.yourdomain.com [R=301]
    


This will redirect iPad users to a URL you specify.