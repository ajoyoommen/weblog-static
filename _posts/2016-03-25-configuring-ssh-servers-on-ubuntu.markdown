---
layout: post
title: Configuring SSH servers on Ubuntu
date: 2016-03-25 05:16:47 +0530
category: SSH
tags:
    - SSH
    - Linux
    - Ubuntu
author: Ajoy Oommen
published: true
---
If you need to SSH to multiple servers regularly, this is the simplest way to do this:

* Create  a file (if it doesn't already exist)  `~/.ssh/config`

* Add the following entry for each of your servers

        Host blog
          HostName ec2-192-168-1-1.compute-1.amazonaws.com
          User ec2-user
          IdentityFile ~/.ssh/acces-key.pem

* To SSH to your server, run `ssh blog`

* To browse the files on your server, open any file, go to File > Connect to server.. In server address, enter `sftp://blog` and you will be able to browse files in your server.