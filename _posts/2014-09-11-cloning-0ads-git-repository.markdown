---
layout: post
title: Cloning 0AD’s Git repository
date: 2014-09-11 17:10:39 +0530
category: 0AD
tags: Git unshallow clone Wget shallow 0AD fetch
author: Ajoy Oommen
published: true
---
Due to unforseen circumstances I had to clone the 0AD repository again. This time I decided to go with Git as I am now more familiar with it.

But cloning a git repo is no easy task if you have a huge project and limited bandwidth.

There are many ways to mitigate these issues. One such is the shallow-clone. In the case of 0ad, with about 16k commits, it is highly advisable to shallow-clone with depth not more than 1. The command for that would be

    git clone –depth 1 URL

But this would still be about 1.2 GB. You can deepen the history later with

    git fetch –depth N , or
    git fetch –unshallow

That would download the rest of the history(another 1.1 GB)

But if you exist in those part’s of the world where bandwidths are very narrow and networks unreliable, the above steps won’t probably work for you. This is because git does not have support, yet, to resume failed clones. My shallow clone, that I had been running all through the day, died sometime after midnight, as ‘the remote hung up unexpectedly’.

The next possible solution is to download the Git repo using wget or anything similar. This would download all the files in the directories and then you could just tell Git that the directory you have downloaded is a Git repo. (I did not try this one)

Or you could contact someone at 0ad and tell them to make available a compressed file of the latest repo, that one could download with wget

I have an AWS account in the free tier, with NGINX serving static files from an EBS volume. I cloned 0AD on AWS, compressed the repo and put it under the static folder. I downloaded the compressed source repo at around 60Kbps in 20 hours, with a lot of help from wget and it’s resume functions.

The way I solved it is by no means feasible for all. But if you have an external server, you sure can put it to good use.