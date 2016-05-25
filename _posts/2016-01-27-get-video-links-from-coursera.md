---
id: 78
title: Get video links from Coursera
date: 2016-01-27T05:50:23+00:00
author: Ashutosh
layout: post
guid: http://aboutashu.com/blog/?p=78
permalink: /get-video-links-from-coursera/
views:
  - 24
dsq_thread_id:
  - 4526765029
categories:
  - Javascript
tags:
  - coursera
  - download
  - javascript
  - jquery
---
I have gone threw coursera classes now and then. What I felt is need for a coursera video downloader. I searched but couldn&#8217;t found any good working one.

Hence here is a small script I developed which would give you all video links from coursera. Here is what you&#8217;ll have to do:

  1. Goto your class. For ex- Mine was  https://www.coursera.org/course/startup
  2. Enroll and go to &#8220;video lectures&#8221; from left tab. Here is the screenshot
  
    <a href="http://aboutashu.com/blog/wp-content/uploads/2016/01/v1.jpg" rel="attachment wp-att-79"><img class="alignnone size-medium wp-image-79" src="http://aboutashu.com/blog/wp-content/uploads/2016/01/v1-300x277.jpg" alt="v1" width="300" height="277" srcset="http://107.170.45.123/blog/wp-content/uploads/2016/01/v1-300x277.jpg 300w, http://107.170.45.123/blog/wp-content/uploads/2016/01/v1.jpg 702w" sizes="(max-width: 300px) 100vw, 300px" /></a>
  3. in chrome, press \`f12\` to bring up chrome inspector. You can also right click anywhere on webpage and click &#8220;inspect element&#8221;. Go to **console** tab.
  4. Paste the following script into the console. Screenshot:
  
    <a href="http://aboutashu.com/blog/wp-content/uploads/2016/01/v2.jpg" rel="attachment wp-att-80"><img class="alignnone size-medium wp-image-80" src="http://aboutashu.com/blog/wp-content/uploads/2016/01/v2-300x86.jpg" alt="v2" width="300" height="86" srcset="http://107.170.45.123/blog/wp-content/uploads/2016/01/v2-300x86.jpg 300w, http://107.170.45.123/blog/wp-content/uploads/2016/01/v2-768x221.jpg 768w, http://107.170.45.123/blog/wp-content/uploads/2016/01/v2-1024x295.jpg 1024w, http://107.170.45.123/blog/wp-content/uploads/2016/01/v2.jpg 1366w" sizes="(max-width: 300px) 100vw, 300px" /></a>Script:</p> <pre class="lang:js decode:true">(function($){
$("ul.course-item-list-section-list &gt; li").each(function(){
var link = $(this).find("a[data-link-type=\"lecture:download.mp4\"]").attr('href');
console.log(link)
});
})(jQuery);</pre>

  5. Press enter and hopefully, it will parse every video link in the page.

Hope you&#8217;ve liked the article and it helped you. Please do comment if I owe any explanations. Share, it might be helpful to others.