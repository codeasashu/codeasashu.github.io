---
id: 66
title: Process Migration
date: 2015-09-28T04:53:38+00:00
author: Ashutosh Chaudhary
layout: post
guid: http://codeasashu.tk/blog/?p=66
permalink: /process-migration/
dsq_thread_id:
  - 4196426863
views:
  - 54
sociallikes:
  - 1
categories:
  - M.Tech
  - Mobile Computing
tags:
  - mobile computing
  - mtech
---
A general block diagram of process in an OS looks something like this figure:

<div style="width: 480px" class="wp-caption alignnone">
  <img class="" src="http://www.csie.ntnu.edu.tw/~swanky/os/chap4/PCB.png" alt="Process Control Block diagram" width="470" height="830" />
  
  <p class="wp-caption-text">
    Process Control Block diagram
  </p>
</div>

Main things to consider regarding a process are:

  * Process state
  * Data being used by a process
  * Pointers and stuff in the Program counter

To move a process to another machine, above things have to be created in the destination machine. This is what process migration is all about.

Remember, process is just a program in execution. so, basically, we have to create new process with set of thread that are executing in source machine.

**Steps in process migration**&#8211;

  1. Source machine request destination machine if migration can happen. Known as _migration request. _After some kind of negotiation, it is granted.
  2. The source machine then halt the process. communication is also temporarily queued so that it can later to sent to the new copy of current process in destination machine.
  3. System then analyze and collect process&#8217;s state, which includes memory state(memory content owned by process), processor state(content in CPU registers) as well as open files or any kernel context codes(if any). Some files and kernel code are OS dependent which is not allowed for transfer.
  4. A new process instance on destination machine is then created. The state saved in previous step is now transferred to this new instance. Sometimes, not all the states are required immediately, so they gets transferred later.
  5. Once the sufficient amount of state transfer is done, process gets activated on destination machine.
  6. The communication, which was being queues(in step 2), is now redirected to this new process. The queuing process runs parallel wrt STEP 3,4,5.
  7. Sometimes, OS makes reference of migrated process, in order to control or communicate the process in future.

&nbsp;