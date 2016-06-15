---
id: 137
title: 'Implement XMPP chat client in Android &#8211; Part 1'
date: 2016-05-01T08:39:37+00:00
author: Ashutosh
layout: post
guid: http://aboutashu.com/blog/?p=137
permalink: /implement-xmpp-chat-client-android-part-1/
series: android-xmpp-client
views:
  - 14
dsq_thread_id:
  - 4790948561
categories:
  - Uncategorized
---
In the [last post](http://aboutashu.com/blog/xmpp-chat-server/) of this series, I talked about setting up XMPP based chat server, using ejabberd. In this post, I will be talking about how to implement XMPP chat client in android and let two users talk to each other. After reading this post, you&#8217;ll be able to

  1. Implement chat system such as whatsapp in your own android app
  2. Allow multiple users to chat with each other
  3. Making the chat as service so that chat messages still come through even while app is in background

## XMPP Client

XMPP Clients is a software or application which allows you to connect to XMPP servers for instant messaging with other people. Simple enough!! If you are a frequent internet user, you must&#8217;ve used one to many XMPP clients such as pidgin, gtalk, Yahoo! IM etc. which allows you to send Instant Message to other users you are connected to. What we will do this in post is to make one such client in android.

## What You will need

  1. An android IDE for app development. While I prefer [Android Studio](http://developer.android.com/sdk/index.html), while [Eclipse](https://eclipse.org/) also does the job.
  2. Decent Working internet.
  3. 1-2 hours patience

## Starting the Project

Lets begin!!

#### Creating the project:

  1. Open Android studio and hit the button &#8220;Start a new Android Studio project&#8221;.
  2. Enter anything in &#8220;Application name&#8221;. For ex- Chatterbox . Hit _next_ button.
  3. Choose API level here. Google for what you should choose. Mine have API level 15. Hit _next._
  4. Choose &#8220;Blank Activity&#8221; in next screen and hit the _next_ button.
  5. Click &#8220;finish&#8221; button to start your project.

After this, android studio will bring up XML layout editor and Activity with some default code. We will be using same activity to start a chat.

#### Importing necessary things (I mean, libraries):

  1. In the left pane of android studio, you will see lots of directory list and some files. Choose app&#8217;s build.gradle (not the one outside the app folder). See image below for reference.
  
    [<img class="aligncenter size-full wp-image-138" src="http://aboutashu.com/blog/wp-content/uploads/2016/04/gt1.png" alt="gt1" width="242" height="309" srcset="http://107.170.45.123/blog/wp-content/uploads/2016/04/gt1.png 242w, http://107.170.45.123/blog/wp-content/uploads/2016/04/gt1-235x300.png 235w" sizes="(max-width: 242px) 100vw, 242px" />](http://aboutashu.com/blog/wp-content/uploads/2016/04/gt1.png)
  2. Enter following code in dependencies list 
    [code]
  
    dependencies {
  
    compile fileTree(dir: &#8216;libs&#8217;, include: [&#8216;*.jar&#8217;])
  
    compile &#8216;com.android.support:appcompat-v7:22.2.0&#8217;
  
    compile "org.igniterealtime.smack:smack-android-extensions:4.1.0"
  
    compile "org.igniterealtime.smack:smack-tcp:4.1.0"
  
    [/code]