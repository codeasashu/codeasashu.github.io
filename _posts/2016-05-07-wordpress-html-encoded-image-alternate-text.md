---
id: 144
title: 'WordPress: HTML encoded image alternate text'
date: 2016-05-07T07:20:59+00:00
author: Ashutosh
layout: post
guid: http://aboutashu.com/blog/?p=144
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
The WordPress media manager allows you to add captions to your images, which can be inserted into your post content wrapped in the **caption** shortcode. However, image alternate text are inserted as plaintext. You can now use HTML encoded image alternate text with few lines of code.

<div id="attachment_145" style="width: 295px" class="wp-caption aligncenter">
  <a href="http://aboutashu.com/blog/wp-content/uploads/2016/05/image_alt.png"><img class="wp-image-145 size-full" title="Image alternate text" src="http://aboutashu.com/blog/wp-content/uploads/2016/05/image_alt.png" alt="Sample gallery image" width="285" height="480" srcset="http://107.170.45.123/blog/wp-content/uploads/2016/05/image_alt.png 285w, http://107.170.45.123/blog/wp-content/uploads/2016/05/image_alt-178x300.png 178w" sizes="(max-width: 285px) 100vw, 285px" /></a>
  
  <p class="wp-caption-text">
    Sample gallery image
  </p>
</div>

As you can see, I&#8217;ve left the &#8220;Alt text&#8221; section blank, while providing the caption for the image. The caption I provided contains HTML string, containing <i> and <br> attributes. You may also want to include links to original author of image, or may want to link to some sources.  Wordpress, upon encountering blank image alternate text (Alt text section in above image), uses caption to fill the &#8220;alt&#8221; attribute of the image. Hence the image on post page will look like:

[code] <img src="http://someexamplesite.com/path/to/image/image.jpg" alt="your caption in plaintext here" &#8230;. />[/code]

### The problem

When you are using some plugins to show image gallery, they generally rely on caption and alternate text to show image description. In such cases, it becomes necessity to have alternate text in HTML entity rather than plaintext. But wordpress converts down the caption to plaintext and then fills up alt attribute of image. Hence, even if your caption reads
  
[code]<b>Hey, this is <i>caption</i></b>[/code]
  
, the image alt will come up like:

[code] <img src="http://image-url/image.jpg" alt="Hey, this is caption" />[/code]

This is when you might want to fill up image alt attribute in HTML encoded string, or in simple words, may want the above image tag to look something like this:

[code] <img src="http://image-url/image.jpg" alt="<b>Hey, this is <i>caption</i></b>" />[/code]

### The solution

Luckily, WordPress provides image hooks which you can use to modify image attributes on the fly. Hence, Here is one function which I think would work best
  
[code]<a href="https://developer.wordpress.org/reference/hooks/wp\_get\_attachment\_image\_attributes/">wp\_get\_attachment\_image\_attributes</a>[/code]
   
. Here is my solution. You can just copy paste the below code and drop into your theme&#8217;s functions.php.

[code lang=&#8221;php&#8221;]

function my\_filter\_gallery\_img\_atts( $atts, $attachment ) {
  
$atts[&#8216;alt&#8217;] = get\_post($attachment->ID)->post\_excerpt; //This get HTML encoded caption for an image
  
return $atts;
  
}
  
add\_filter( &#8216;wp\_get\_attachment\_image\_attributes&#8217;, &#8216;my\_filter\_gallery\_img_atts&#8217;, 10, 2 );

[/code]

&nbsp;

Hope it works for you. See you next time.