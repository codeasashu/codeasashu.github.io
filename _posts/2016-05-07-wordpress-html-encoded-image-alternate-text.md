---
id: 144
title: 'WordPress: HTML encoded image alternate text'
date: 2016-05-07T07:20:59+00:00
author: Ashutosh
layout: post
permalink: /wordpress-html-encoded-image-alternate-text/
views:
  - 13
dsq_thread_id:
  - 4806778516
categories:
  - Wordpress
tags:
  - filter
  - image
  - wordpress
---
One problem with wordpress is that it doesn't allow HTML encoded strings in image alternate text. Even if you provide your image alternate text as a HTML string, wordpress will covert it down to plaintext, removing any HTML attributes and the image will then have only plaintext of your alternate text.

i.e even if you write your image alternate section such as this

```html
Hey, I am <i> italic text </i>
```

this would output in frontend as

```
Hey, I am italic text
```

This is not cool. Sometimes, you rely on some plugins which reads image alternate text and display below the images in pretty cool style. 

## Options

One option which came to my mind is to leave alternate text blank and provide the image *caption* in HTML format. For ex, check below image:

![Wp Caption](/assets/wordpess_caption.png)

Wordpres has this feature where if you leave the alternate text blank, it would fill the caption text in alternate text. I thought it would work since I can provide HTML in captions. But again, the same problem. Captions were turned back into plaintext.

Thus, for the above image, the output was something like this:

```html
<img src="http://yourwebsite.com/path/to/image/image.jpg" alt="Hello, this is sample caption. This text is in bold" />
```


### The solution

Luckily, WordPress provides image hooks which you can use to modify image attributes on the fly. The one which I want to use is [wp_get_attachment_image_attributes](https://developer.wordpress.org/reference/hooks/wp_get_attachment_image_attributes/)
   
We can use this hook to see what image attribute we want to change, read its value, format it in HTML and return back our new value. 

Hence, open up your theme's `functions.php` and create a new function where you want to attach this hook. I am calling the function `custom_filter_gallery_img_atts`.

{% highlight php %}
<?php
//Filename: functions.php
function custom_filter_gallery_img_atts( $atts, $attachment ) {
$atts["alt"] = get_post($attachment->ID)->post_excerpt; //This returns image caption (which is in HTML format)
return $atts;
}
  
add_filter( "wp_get_attachment_image_attributes", "custom_filter_gallery_img_atts", 10, 2 );
?>
{% endhighlight %}

What we did above is attaching `wp_get_attachment_image_attributes` to our custom defined function `custom_filter_gallery_img_atts` inside which we read the image caption and stored the value in image alternate text. 

Hope you enjoyed the article. If you have any doubts, please comment below. See you soon. 