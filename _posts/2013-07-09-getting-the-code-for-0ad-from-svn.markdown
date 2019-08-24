---
layout: post
title: Getting the code for 0AD from SVN
date: 2013-07-09 19:02:12 +0530
category: 0AD
tags: Git GitHub clone Clone Subversion 0AD
author: Ajoy Oommen
published: true
---
I am documenting this here, as a reference for me as well as for others who might have or would experience this problem.

I tried to get the code from GitHub. But sadly, on my slow connection, the remote fails regularly. And since git cannot resume an update, I had to abandon cloning from git.

I tried git svn, as I was not familiar with SVN. But in git svn the download never starts. I haven’t figured out why.

Finally I had to resort to downloading from the SVN. SVN update always keeps your repo up-to-date with the master. Hence, when your download is interrupted you can directly start the update after a cleanup.

With SVN resuming your interrupted download is easy, if you know how SVN updates the repo.

SVN downloads files randomly into random folders(I don’t know yet if it has a fixed pattern). If there exists a folder ‘images’ with 8 images, SVN will download the 8 images into ‘images/.svn/tmp/text-base’. Only when the 8 images have been completely downloaded, will the images be added to the correct directory.

So if you need to interrupt the connection prematurely, ensure that the connection is active and give CTRL-C in the terminal. If the signal is received, the downloaded files are added to their base folders. Perform a ‘svn cleanup’ and then the update can be resumed again at leisure.

If the signal is not received, which happens when the connection has already been interrupted, you will have to kill the process. Now this will create a ‘.fuse_hidden####’ file in the download directory, which cannot be deleted as svn is still holding the file for the interrupted download. To counter this, you will have to kill svn, which I wasn’t successful at(If anyone does find a way, please tell me). I do this by restarting the computer, and then deleting the file manually on restart. After this the ‘svn cleanup’ will work.

When the ‘.fuse_hidden*’ file exists, svn cleanup fails saying that is not empty. If some other error pops up, you might consider deleting the log file in /.svn directory.

Well, this is the case, if you have a huge SVN project to clone.