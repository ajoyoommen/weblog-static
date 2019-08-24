---
layout: post
title: Debugging with adb logcat
date: 2015-11-18 06:11:48 +0530
category: Android
tags: Logging Debugging Cordova Android
author: Ajoy Oommen
published: true
---
**Usage**: `adb logcat [options] [filterspecs]`

Some examples explained:

  1. `adb logcat ActivityManager:I MyApp:D *:S`<br>Log level *info* for tag ActivityManager, *debug* for tag MyApp and *silent* (all logs suppressed) for all other tags.

Ref: [Reading and Writing Logs](http://developer.android.com/tools/debugging/debugging-log.html)