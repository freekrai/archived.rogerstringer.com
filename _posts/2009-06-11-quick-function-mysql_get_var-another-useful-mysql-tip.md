---
layout: "post"
title: "Quick Function: mysql_get_var: Another useful MySQL tip"
tags: 
- "code"
date: "2009-06-11 07:12:17"
ogtype: "article"
bodyclass: "post"
---

I’m going to show you a small but useful function that is handy to keep in your toolbox today, it’s called **mysql_get_var**. This function lets you run a SQL query, and only return the variable you choose. Here’s the function:


    function mysql_get_var($query,$y=0){
        $res = mysql_query($query);
        $row = mysql_fetch_array($res);
        mysql_free_result($res);
        $rec = $row[$y];
        return $rec;
    }
    


Now, let’s talk about what it does. When you call this function, like for example here:


    $name = mysql_get_var("SELECT name from people where email = 'roger@freekrai.net'");
    


You will return the name field, so what gets returned will be “Roger” (if that was my name in the database). Now, you may notice that this function had a second argument called **$y**, this is so that you can choose which variable to return when your query has multiple fields:


    $city = mysql_get_var("SELECT name,address,city from people where email = 'roger@freekrai.net'",2);
    


In the example above, I told it to return the 2nd argument, which due to PHP starting off arrays with a 0, would actually be the 3rd argument, so it returns the city of the person selected. This function is only for returning 1 field from 1 row to a time, so if there are more rows, this wouldn’t be as useful, but it does work well for grabbing say a user’s name everytime they log in, or some similar function.