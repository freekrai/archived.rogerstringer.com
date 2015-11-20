---
layout: "post"
title: "Quick Function: Always make sure you close your mysql connections in PHP"
tags: 
- "code"
date: "2009-05-28 12:09:16"
ogtype: "article"
bodyclass: "post"
---

I spend a lot of time working in PHP, and one thing I run into a lot, is garbage collection.

It’s important to make sure you DB connections are closed when the script finishes excution, or it can cause other problems (like memory, resources, etc).

So I handle my DB connection through a very simple class that is set up to do a mysql disconnect at the end of the script’s execution.


    class DbConn {
        public $conn;
        public function __construct($host,$user,$pass,$db) {
            $this->conn = mysql_connect($host, $user, $pass) or die("Couldn't connection to $host");
            mysql_select_db($db,$this->conn);
        }
        public function __destruct(){
            mysql_close($this->conn);
        }   
    }
    $dbconn = new DbConn("localhost","mydbuser","mydbpass","mydbname");
    define( "DBH", $dbconn->conn );
    


Then when you do your database queries, you just make sure to include the DBH handle in the code.


    $result = mysql_query("Select * from people",DBH);
    


When the script ends, the code makes sure it closes your database connections. This can be built on quite a bit to include handling of mysql queries, but I wanted to at least share this simple class that has gotten a lot of use in various projects and has come in handy several times.