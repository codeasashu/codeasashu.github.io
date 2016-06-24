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

## Part 1 - XMPP Basics

In the [last post](http://aboutashu.com/blog/xmpp-chat-server/) of this series, I talked about setting up XMPP based chat server, using ejabberd. In this post, I will be talking about how to implement XMPP chat client in android and let two users talk to each other. After reading this post series, you'll be able to

  1. Implement chat system such as whatsapp in your own android app
  2. Allow yourself to chat with online users (roasters)
  3. Making the chat as service so that chat messages still come through even while app is in background

Since it would be too long to explain everything within a single post, I am going to explain part by part code structure and basics of XMPP here.

## XMPP Client

XMPP Clients is a software or application which allows you to connect to XMPP servers for instant messaging with other people. Simple enough!! If you are a frequent internet user, you must've used one to many XMPP clients such as pidgin, gtalk, Yahoo! IM etc. which allows you to send Instant Message to other users you are connected to. What we will do this in post is to make one such client in android.

## What You will need

  1. An android IDE for app development. While I prefer [Android Studio](http://developer.android.com/sdk/index.html), [Eclipse](https://eclipse.org/) also does the job.
  2. Decent Working internet.

## Starting the Project

Lets begin!!

#### Creating the project:

Create a blank project in android studio. Once you get to editor screen, add following dependencies to your app specific `build.gradle`:
```
compile 'org.igniterealtime.smack:smack-android-extensions:4.1.0'
compile 'org.igniterealtime.smack:smack-tcp:4.1.0'
compile 'com.google.code.gson:gson:2.6.2'
```

The first two lines are mandatory for getting smack to work on android. The third line (about gson) allows you to convert raw messages in GSON format. Since we will be sending messages in GSON format, I added it to my needs. Check out [build.gradle](https://github.com/ashutosh2k12/XmppDemo/blob/master/app/build.gradle) of my app to get an idea.

## Basics

Before diving straight into the code, lets understand few things about XMPP and in what manner are we going to implement them. 

#### What is XMPP?

XMPP is basically a set of protocols (or rules) to allow communication based on XML. Thus, whenever you send a message, or your presence (available, unavailable etc.) to some other user (or server), it goes out in XML format, like this:

```
<presence>
  <show>dnd</show>
</presence>
```

But we don't have to go threw this right now. You just need to know that whenever you are seeing someone online, or sending a message, the client communicates with each other in this mannar. Read more about these [here](https://xmpp.org/rfcs/rfc3921.html)

#### Jabber ID

Jabber Id or simply JID is a unique ID (just like your username on facebook) which helps you recognize in XMPP environments. Jabber ID is usually in the format `username@domain`. Sometimes, you may see your JID in form of `username@domain/resource` .Resource is nothing but a string representing your chat environment. 

`username@domain` sometimes is reffered to as *Bare JID* while `username@domain/resource` as *Full JID*.

#### Roasters

Just like you have facebook friends, that only they can check out your shared items, XMPP too does have this security thing of its own kind. Whenever you gets connected to a XMPP server, few things about you travels along to server. So, for example, whenever you login to your GTalk, or pidgin, you instantly gets this "green"  button to show you are online. You also see your last status that you've put up. you see your friendlist and online friends from them. 

The information associated with you, as soon as you login to any XMPP client, includes mainly:

1. Your presence information (wether you are available or not)
2. Your status
3. Whom you are subscribed to (and who subscribed to you)

These informations are kept on server in database. XMPP have certain security features that decides who is going to see those informations. For ex, if you are subscribed to User A, and user A is also subscribed to you, then you both can see each other's presences. However, if only user A has subscribed to you, only you can see his presence. 

**Roasters** are people (or group of people) who are subscribed to you (or you are subscribed to them). They are like your friends on facebook, with limited access depending upon the subscription status between you both.

To put it simply, the online users that you see in your gtalk are roasters. Any user whom you have added as contact on  XMPP clients are roasters.

#### Subscriptions

Subscriptions is one way of saying *sending friend requests*. Just like, when you send someone a friend request on facebook, you subscribe to someone on XMPP. You subscribe to someone by sending them a subscription presence packet to their JID. While I will begin with code, I will show you how to send a subscription packets and receive one. Thus, subscription state exists between two users. 

For sake of simplicity, you just need to know that in order to make someone friends on XMPP, or allowing someone to make you a friend, you need subscription.

Subscriptions have 4 modes: `subscribe_none`, `subscribe_to`, `subscribe_from`, `subscribe_both`.

1. **subscribe_none** - When you are not subscribed in any way to a given user. 
1. **subscribe_to** - When only you are subscribed to a given user (one way). 
1. **subscribe_from** - When you are not subscribed in any way to a given user. 
1. **subscribe_both** - When you are not subscribed in any way to a given user. 

