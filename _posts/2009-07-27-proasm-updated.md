---
layout: "post"
title: "proASM updated…."
tags: 
- "articles"
date: "2009-07-27 08:40:30"
ogtype: "article"
bodyclass: "post"
---

Over the weekend, I updated [proASM](http://www.freekrai.net/products/proasm) to include support for Amazon’s new signed authentication request protocol that they are bringing online August 15th, 2009. Amazon already enforces this protocol with their other AWS services so it was a matter of time til they enforced it with product feeds as well. Basically, every request we send to Amazon, now includes an encrypted version of the URL request encoded with the user’s secret access key. On the Amazon side of things, it compares the certificate you sent with their encoded version and checks for a match. No match means no valid request.

[Read more about proASM here.](http://www.freekrai.net/products/proasm)