---
layout: "post"
title: "Sanitizing PHP request variables quickly"
tags: 
- "code"
date: "2011-01-11 19:37:03"
ogtype: "article"
bodyclass: "post"
---

foreach($_POST as $k=>$v){
        $_POST[$k] = filter_input(INPUT_GET, $k, FILTER_SANITIZE_SPECIAL_CHARS);
    }
    foreach($_GET as $k=>$v){
        $_GET[$k] = filter_input(INPUT_GET, $k, FILTER_SANITIZE_SPECIAL_CHARS);
    }