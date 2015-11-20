---
layout: "post"
title: "Quick Function: Inserting ads into your RSS feeds in WordPress"
tags: 
- "code"
date: "2010-03-12 03:15:33"
ogtype: "article"
bodyclass: "post"
---

On my main wordpress site (this one your reading now), I like to display a single line below each RSS feed item that tells people quickly about some of my other sites. The ad appears like this:

![](http://cdn.rogerstringer.com/wp-content/uploads/2010/03/adpic.png)

The code for it is pretty straight forward, and can also be used to display a copyright message linking to your site, or anything along those lines. Place the following code in your theme’s functions.php file:

```php
	function insertAds($content) {
		if(is_feed()) {
			srand((float) microtime() * 10000000);
			$banners = array();
			$banners[] = 'Banner number 1. <a href="http://www.somerandomsite/" target="_blank">Some more banner text</a>';
			$banners[] = 'Banner number 2. <a href="http://www.somerandomsite/" target="_blank">Some more banner text</a>';
			$banners[] = 'Banner number 3. <a href="http://www.somerandomsite/" target="_blank">Some more banner text</a>';
			$banners[] = 'Banner number 4. <a href="http://www.somerandomsite/" target="_blank">Some more banner text</a>';
			$rn = array_rand($banners);
			$content = $content.'<hr />'.$banners[$rn];
		}
		return $content;
	}
	add_filter('the_excerpt_rss', 'insertAds');
	add_filter('the_content', 'insertAds');
```	

You could also replace this with banner images. Anyway, what this does is randomly select which text to display on your rss feed items. So for each article, you could have a different ad appearing. If you wanted to use this to display only one message (like a copyright), you’d use this version:

```php
	function insertMsg($content) {
		if(is_feed()) {
			$banner = 'You are reading my blog. <a href="http://www.linktoblog/" target="_blank">Name of your blog</a>';
			$content = $content.'<hr />'.$banner;
		}
		return $content;
	}
	add_filter('the_excerpt_rss', 'insertMsg');
	add_filter('the_content', 'insertMsg');
```

That’s it, like I said, it’s a quick function, but it serves it’s purpose.