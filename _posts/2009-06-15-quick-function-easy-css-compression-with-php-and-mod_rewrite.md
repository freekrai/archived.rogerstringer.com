---
layout: "post"
title: "Quick Function: Easy CSS Compression with PHP and mod_rewrite"
tags: 
- "code"
date: "2009-06-15 13:03:12"
ogtype: "article"
bodyclass: "post"
---

I’m in the process of redoing some aspects of my food site, [foodizu.com](http://www.foodizu.com), it’s a slow process but one of the things I am addressing is speed. I looked into some CSS compression ideas, but I wanted one script that would read a CSS or a javascript file and automatically strip any white spaces out, and return a more compressed CSS file. First, we create a file called **csszip.php**:


    ob_start ("ob_gzhandler");
    if( isset($_REQUEST['file']) ){
        $file = $_REQUEST['file'];
        if( goodfile($file) ){
            $ext = end(explode(".", $file));
            switch($ext){
                case 'css':$contenttype = 'css';break;
                case 'js':$contenttype = 'javascript';break;
                default:die();break;
            }
            header('Content-type: text/'.$contenttype.'; charset: UTF-8');
            header ("cache-control: must-revalidate");
            $offset = 60 * 60;
            $expire = "expires: " . gmdate ("D, d M Y H:i:s", time() + $offset) . " GMT";
            header ($expire);
            $data = file_get_contents($file);
            $data = compress($data);
            echo $data;
        }
    }
    exit;
    function goodfile($file){
        $invalidChars=array("\",""",";",">","<",".php");
        $file=str_replace($invalidChars,"",$file);
        if( file_exists($file) ) return true;
        return false;
    }
    function compress($buffer) {
        $buffer = preg_replace('!/*[^*]**+([^/][^*]**+)*/!', '', $buffer);
        $buffer = str_replace(array("rn", "r", "n", "t", '  ', '    ', '    '), '', $buffer);
        return $buffer;
    }
    


This file will take anything passed via the file argument and compress it. Next, we want to make this process automatic, so we open up .htaccess and add this line:


    RewriteEngine On
    RewriteRule ^(.*).css$ /csszip.php?file=$1.css [L]
    


Obviously, if you already have a RerwriteEngine On line, then you can leave it out and place the RewriteRule on the line directly below it. This will tell the website to compress any css file you attempt to load. You can also have this work on javascript files by adding:


    RewriteRule ^(.*).js$ /csszip.php?file=$1.js [L]
    


This will load and compress any javascript file as well. This method works well for reducing your bandwidth usage and cutting back on page load time, which are always nice bonuses.