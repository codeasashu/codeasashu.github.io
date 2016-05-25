---
id: 72
title: How to download multiple videos from youtube-dl
date: 2016-01-25T18:38:03+00:00
author: Ashutosh
layout: post
guid: http://aboutashu.com/blog/?p=72
permalink: /how-to-download-multiple-videos-from-youtube-dl/
views:
  - 23
dsq_thread_id:
  - 4522490583
sociallikes:
  - 1
categories:
  - python
tags:
  - download
  - python
  - youtube-dl
---
Like me, you must&#8217;ve also faced a similar situation when you needed to download multiple videos from a site. In case you&#8217;re also a frequent webnair/seminar viewer or a youtube fan, you must&#8217;ve came across a beautiful ultility called <a title="Youtube-dl " href="https://rg3.github.io/youtube-dl" target="_blank">Youtube-dl</a> to download videos from your favourite sites such as youtube, vimeo etc.

Here is how to download multiple videos from your favourite sites (I am using videmo as an example).

1. Open up a command prompt (windows) or terminal(Linux), and go to location where you want to download the videos.

2.  Create a blank text file in that location. Paste your list of URLs in that file. Remember to press Enter after each link. Lets say, you named the file &#8220;download.txt&#8221;

3. Copy and paste the following command in your terminal/command prompt

<pre class="lang:batch decode:true "> youtube-dl -ci --batch-file=download.txt</pre>

This will start downloading every url mentioned in the file.

&nbsp;

For me, the download speed was slow as compared to some download accelerator programs. I wanted just the download links, so I can download it via some custom made downloaders. Here is how you can output the links to another file.

<pre class="lang:batch decode:true">youtube-dl -cgi --batch-file=download.txt &gt; output.txt</pre>

If you also want the titles along with each link to be printed, just add &#8220;-e&#8221; switch to above command. This will become

<pre class="lang:batch decode:true">youtube-dl -cgei --batch-file=download.txt &gt; output.txt
</pre>