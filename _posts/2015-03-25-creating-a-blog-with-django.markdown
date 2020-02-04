---
layout: post
title: Creating a blog with Django
date: 2015-03-25 18:29:05 +0530
category: blogging
tags:
    - Markdown
    - Django
    - Wordpress
author: Ajoy Oommen
published: true
---
I have been blogging with Wordpress.com for a couple of years now. Recently I thought about creating and maintaining my own blog software. Python was my language of choice, so I considered CherryPy, Django and Flask  but settled for Django because of its wonderful ORM and admin interface.

In this post I will discuss some features of this blog and possible enhancements. This code is available at my GitHub repository [django_site](http://github.com/ajoyatgithub/django_site).

Posts are organized under categories. Categories can be nested up to any level, but for now, I limit them to single level. Root categories are used to sort posts into logical topics. Posts are created under leaf categories.

A post can have multiple tags. The section on Related Posts uses these tags to determine possible relation between posts

Posts are written in Markdown. I have used [Python-Markdown](https://pypi.python.org/pypi/Markdown) to render the markdown. I am also planning to add [Pygments](http://pygments.org/) for syntax coloring.