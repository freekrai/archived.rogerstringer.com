---
layout: "post"
title: "OverStock II"
tags: 
- "articles"
date: "2005-11-05 18:01:38"
ogtype: "article"
bodyclass: "post"
---

Yesterday I mentioned about doing some sort of web services client / server type thing for OverStock.com datafeed sites.

I set up a subdomain to contain the database stuff and from there I set up an XML-RPC server to read the DB and forward it to the client per request.

The system works nicely. Just been doing testing so far, next step is to set up some actual sites that use the system.

It’s set up in a way that means that I could technically use the site as a gateway between overstock affiliates and myself if I wanted to. Hmmm, might be worth considering down the road since I don’t store the affiliate IDs on the server side, only on the client side.

Wonder if that is against their regulations to have an XML-RPC gateway set up and sell access to it to other affiliates?

Ok, time to shut my mouth with these ideas.