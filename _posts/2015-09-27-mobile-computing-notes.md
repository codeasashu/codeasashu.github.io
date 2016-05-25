---
id: 61
title: Mobile Computing notes
date: 2015-09-27T16:13:55+00:00
author: Ashutosh Chaudhary
layout: post
guid: http://codeasashu.tk/blog/?p=61
permalink: /mobile-computing-notes/
dsq_thread_id:
  - 4170953945
views:
  - 55
sociallikes:
  - 1
categories:
  - M.Tech
  - Mobile Computing
tags:
  - mobile computing
  - mtech
---
This is only a gentle introduction. I will try to cover as much as I can in further posts. Will make posts topic wise so as to cover everything related to a topic in a single post.

**Process Migration- **

  * Act of transferring processes between two computers, through a wired or wireless medium.
  * A process has its process table, code segments, some data and pointers etc. This all needs to be transferred by some logic.
  * Used to balance load on systems. One overloaded machine can transfer process to an under-loaded one.
  * Originated with distributed systems. Still used in multicore(multiple processors) systems as part of process scheduling of OS.

**Mobile Computing-**

  * Defined as _ability to use technology_(_this part is called as computing_) to access to some centrally located resources/information by remote and mobile devices such as  smartphones.
  * Mobility is divided into 3 categories: 
      1. <span style="text-decoration: underline;">Macro Mobility</span>: When moving into and out of a global network. Maintaining connectivity is main challenge here. _Mobile IP_ take cares for this one.
      2. <span style="text-decoration: underline;">Micro Mobility</span>: When moving inside one region(cell) only. _Cellular IP_ take cares for this
      3. <span style="text-decoration: underline;">Ad hoc Mobility</span>: Refers to mobility within an Mobile Ad hoc Networks(MANET). Happens due to continuous changes in the network infrastructure. Thus, the infrastructure-less networks (_MANET_) take cares for this one.
  * Major benefit is increased computation power regardless of location/infrastructure. Wether be it a office or restaurant, you&#8217;ll always be connected to remote services, thus increasing productivity.

**Mobile Agents**&#8211;

  * Programs that moves across the networks, here and there, on behalf of user.
  * Gathers information as well as execute automated tasks given by users.
  * Specifically, &#8220;_A mobile agent is a composition of computer program, data, and execution state, which is able to move from one computer to another autonomously and continue its execution on the destination computer_&#8220;
  * Widely used by e-commerce industries, stock holders etc. It is a specific form of <a title="Wikipedia" href="https://en.wikipedia.org/wiki/Mobile_code" target="_blank">mobile code</a>
  * Examples are- agent Tcl, aglet etc.
  * **Working (TL;DR)** : The program can suspend its execution at an arbitrary point, transport itself to another machine, and then resume execution from the point of suspension.

&nbsp;

&nbsp;