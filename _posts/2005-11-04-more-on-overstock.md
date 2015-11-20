---
layout: "post"
title: "More on OverStock"
tags: 
- "articles"
date: "2005-11-04 07:04:29"
ogtype: "article"
bodyclass: "post"
---

So, I set up a site testing OverStock.com’s affiliate feeds. First I wanted to try their XML interface, but it wasn’t as good as I wanted it, so instead I opted for the datafeed system. Their datafeed system involves setting up a lot of tables, almost a seperate table for each category to handle the various different fields.

That is working well and downloading new updates on an hourly basis. Each hour, it grabs a new set of data for one category, then the next hour, it grabs another set. Once a day, it unzips all the zip files and uploads them to the database, then deletes the downloaded files.

This can obviously get heavy if you have multiple sites doing this, so I think I will be setting up some sort of a web services interface so that I can have a client script talk to a central server script and serve products to my other planned OverStock affiliate sites that way. Much cleaner, much simpler.

Aside from that, the new cooking site is coming along nicely. It’s a modified version of my CMS as I said before, so the modifications are continuing. Gonna set it up so that people can rate articles, etc.