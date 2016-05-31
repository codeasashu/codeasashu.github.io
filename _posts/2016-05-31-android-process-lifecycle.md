---
title: 'Android: Process lifecycle'
date: 2016-05-29T07:35:59+00:00
author: Ashutosh
layout: post
published: false
permalink: /process-lifecycle/
categories:
  - Android
---

Each android app is given a specific userid amd a process.Each process in android runs in its separate VM(Virtual Machines). Processes in android can be killed if extra memory is needed anytime. It depends on user's interaction that which process will be removed. A

- **onCreate**: This method gets called whenever an activity is created. This is where we call `setContentView(View)` and use `findViewById` method to retrieve UI widgets.
- **onPause**: This method gets called when a user leaves an activity. Any changes made by user should be commited here.

### Activity Lifecycle

Activities are managed in system as acitivity stacks. The stack is implemented in LIFO order, i.e. whenever new activity is started, it is put up on top of stack (thus becoming running activity) and the other activites are pushed at backstack. Once the new activity exits, the previous activity comes at top of stack, thus becoming current activity.

An activity has 3 states:

1. **active/running**: This is when activity is *FULLY VISIBLE*, in foreground of screen where user can interact with it.
2. **paused**: When activity is *PARTIALLY* visible, because of some other view obscuring the activity (for ex- a dialog screen), or some other transparent activity has focus on top of current activity, it is called paused state. In this state, user can not interact but may see the activity UI.
3. **stopped**: If the parent activity is *NOT AT ALL* visible, it is in stopped state. Although it still holds current data and states, it can not be seen by user (hidden), thus is more likely to get killed incase system needs memory.

The flow diagram for activity lifecycle is:
![activity lifecycle](/assets/activity_lifecycle.png)

There are three key loops you may be interested in monitoring within your activity:

- The entire lifetime of an activity happens between the first call to onCreate(Bundle) through to a single final call to onDestroy(). An activity will do all setup of "global" state in onCreate(), and release all remaining resources in onDestroy(). For example, if it has a thread running in the background to download data from the network, it may create that thread in onCreate() and then stop the thread in onDestroy().
- The visible lifetime of an activity happens between a call to onStart() until a corresponding call to onStop(). During this time the user can see the activity on-screen, though it may not be in the foreground and interacting with the user. Between these two methods you can maintain resources that are needed to show the activity to the user. For example, you can register a BroadcastReceiver in onStart() to monitor for changes that impact your UI, and unregister it in onStop() when the user no longer sees what you are displaying. The onStart() and onStop() methods can be called multiple times, as the activity becomes visible and hidden to the user.
- The foreground lifetime of an activity happens between a call to onResume() until a corresponding call to onPause(). During this time the activity is in front of all other activities and interacting with the user. An activity can frequently go between the resumed and paused states -- for example when the device goes to sleep, when an activity result is delivered, when a new intent is delivered -- so the code in these methods should be fairly lightweight.