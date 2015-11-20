---
layout: "post"
title: "Recursively remove all .svn files from a folder"
tags: 
- "articles"
date: "2011-01-31 15:56:39"
ogtype: "article"
bodyclass: "post"
---

I use SVN for code control all the time, and sometimes you have to remember to delete the various .svn files that get created when you upload to a website. This little line, executed at the command-line will do just that:


    rm -rf `find . -type d -name .svn`