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
I have gone threw coursera classes now and then. What I felt is need for a coursera video downloader. I searched but couldn't found any good working one.

Hence here is a small script I developed which would give you all video links from coursera. Here is what you'll have to do:

1. Goto your class. For ex- Mine was  [https://www.coursera.org/course/startup](https://www.coursera.org/course/startup)
2. Enroll and go to video lectures from left tab. Here is the screenshot
3. in chrome, press <kbd>f12</kbd> to bring up chrome inspector. You can also right click anywhere on webpage and click **inspect element**. Go to **console** tab.
4. Paste the following script into the console:
    ```js
    (function($){
      $("ul.course-item-list-section-list > li").each(function(){
        var link = $(this).find("a[data-link-type=\"lecture:download.mp4\"]").attr('href');
        console.log(link)
        });
    })(jQuery);
    ```
5. Press enter and hopefully, it will parse every video link in the page.


Hope you've liked the article and it helped you. Please do comment if I owe any explanations. Share, it might be helpful to others.