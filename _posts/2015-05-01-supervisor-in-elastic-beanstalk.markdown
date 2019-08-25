---
layout: post
title: Supervisor in Elastic Beanstalk
date: 2015-05-01 14:50:06 +0530
category: Supervisor
tags: EC2 SSH EB AWS
author: Ajoy Oommen
published: true
---
This was an issue I encountered while fixing a deployment in AWS EB. Whenever I ran:

    supervisorctl -c supervisord.conf restart celery

I got this error:

    celery: ERROR (no such process)

To solve this, I had to run:

    supervisorctl reread
    supervisorctl update

But another important thing to remember is that this issue also occurs if you started supervisor with a **relative url for  `-c`** and then try `supervisorctl` from a different directory.

**Recommended** - Always use an absolute path:

    supervisord -c /etc/supervisor/supervisord.conf