---
layout: post
title:  "From WordPress and Django to Jekyll"
date:   2020-02-04 09:20:00 +0530
category: blogging
tags:
    - AWS
    - Jekyll
    - AWS EC2
    - AWS Lambda
    - AWS Amplify
    - WordPress
    - Bootstrap
author: "Ajoy Oommen"
published: true
comments: true
---
In this post, I want to discuss about the various platforms and tools I have used to host this blog over the years. I haven't blogged as frequently as I would have liked to. In fact, I have been more concerned with developing and hosting this blog. I have experimented with both dynamic and static site generators and these are some experiences that I'd like to share here.

### Managed by WordPress

I started this blog on Wordpress in November 2012. Then, it was called [The Turing Blog](https://theturingblog.worpress.com).
It was easy working with WordPress, as it took care of the design, layouts and rendering. However, I wanted a blog where I had more control over the layout and design of posts.

### Django on AWS EC2

When I started working with Django in 2014, I wanted to build a blog using Django to learn more about development and hosting. Although an overkill for a small blog with no audience, I decided to host the blog with a database. At the end of 2015, I started working on the project, called [weblog](https://github.com/ajoyoommen/weblog). I used Markdown for formatting and added models for Post, Category and Tag. I also had views to render posts by category, year and month.

I hosted the blog at [blog.ajoyoommen.com](https://blog.ajoyoommen.com/) in July 2016 using a EC2 t2.micro instance. I used Bootstrap to design the layout for the home page, posts and all the custom views. Overall, the performance and design seemed great to me, in spite of the costs of running an EC2 instance perpetually.

### Django on AWS Lambda
Around July 2018, I used [Zappa](https://github.com/Miserlou/Zappa) to deploy the blog on AWS Lambda. I moved the database from the t2.micro instance to a t2.nano. This reduced costs by almost 35%.  I was quite decided by then, that I need to move to a static site generator, especially because I would not need to host a database.

### Jekyll on AWS Amplify
Last August, I started setting up my blog ([weblog-static](https://github.com/ajoyoommen/weblog-static) on my GitHub) using Jekyll. The major rework was converting the template structure in Django to Jekyll's format. For migrating the database into files, I used an IPython shell to iterate over each post from the database and create the markdown file with the correct filename and parameters.

In September, I published the blog using [AWS Amplify](https://aws.amazon.com/amplify/console/). According to its website, what AWS Amplify provides is

> Hosting for fullstack serverless web apps with continuous deployment

AWS Amplify requires access to the Github repository to trigger builds when changes are pushed to a branch. Although builds, deploys and hosting are charged, the charges for a blog this size are almost NIL. I am quite happy with the results now. Especially because Jekyll is easy to manage and AWS Amplify is quite cost-friendly.

### Disqus comments
Last week I enabled Disqus comments on this blog, and this will be the first blog where comments are enabled. That is another milestone in the evolution of my blog. It has been quite an experience and I plan to focus more on writing than maintaining the blog.
