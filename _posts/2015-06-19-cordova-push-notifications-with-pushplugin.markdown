---
layout: post
title: Cordova Push Notifications with PushPlugin
date: 2015-06-19 18:03:44 +0530
category: Cordova
tags: 
author: Ajoy Oommen
published: true
---
This post is about additional resources and links related to [PushPlugin](https://github.com/phonegap-build/PushPlugin), which is a:

> Cordova Push Notification plugin for Android, iOS, WP8, Windows8, BlackBerry 10 and Amazon Fire OS

#### Answers on StackOverlfow

1. Notification message not received if the notification is discarded by the user. Solution: [use sendExtras](http://stackoverflow.com/a/29437083/1761793)
2. [PushPlugin.register returns OK even if not connected to the Internet](http://stackoverflow.com/a/29519304/1761793)
3. Multiple notifications are created if user doesn't read the previous. Use [notification IDs](http://stackoverflow.com/a/31040766/1761793)