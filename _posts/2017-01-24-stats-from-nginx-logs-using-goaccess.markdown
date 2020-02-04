---
layout: post
title: Stats from NGINX logs using Goaccess
date: 2017-01-24 06:15:28 +0530
category: NGINX
tags:
    - NGINX
    - Goaccess
author: Ajoy Oommen
published: true
---
[GoAccess](https://goaccess.io/) is a simple tool to draw stats from log files. It works well on NGINX access logs.

You can install it using `apt-get install goaccess`. Set some default format configurations in `/etc/goaccess.conf`, like:


    time-format %H:%M:%S

    date-format %d/%b/%Y

    log-format %h %^[%d:%t %^] "%r" %s %b

Quick usage instructions

* To view a report in the terminal:

        goaccess -f access.log

* To generate an HTML report:

        goaccess -f access.log -a -o report.html

* To stat multiple logs:

        zcat access.log-*.gz | goaccess -a -o report.html