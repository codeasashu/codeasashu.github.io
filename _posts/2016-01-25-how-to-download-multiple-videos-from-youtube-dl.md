---
id: 72
title: How to download multiple videos from youtube-dl
date: 2016-01-25T18:38:03+00:00
author: Ashutosh
layout: post
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
Like me, you must've also faced a similar situation when you needed to download multiple videos from a site. In case you're also a frequent webnair/seminar viewer or a youtube fan, you must've came across a beautiful ultility called [Youtube-dl](https://rg3.github.io/youtube-dl) ( to download videos from your favourite sites such as youtube, vimeo etc.

Here is how to download multiple videos from your favourite sites (I am using videmo as an example).

- Open up a command prompt (windows) or terminal(Linux), and go to location where you want to download the videos.

-  Create a blank text file in that location. Paste your list of URLs in that file. Remember to press Enter after each link. Lets say, you named the file &#8220;download.txt&#8221;

- Copy and paste the following command in your terminal/command prompt
- 
```bash
youtube-dl -ci --batch-file=download.txt
```

This will start downloading every url mentioned in the file.

For me, the download speed was slow as compared to some download accelerator programs. I wanted just the download links, so I can download it via some custom made downloaders. Here is how you can output the links to another file.

```bash
youtube-dl -cgi --batch-file=download.txt > output.txt
```

If you also want the titles along with each link to be printed, just add **-e** switch to above command. This will become

```bash 
youtube-dl -cgei --batch-file=download.txt > output.txt
```