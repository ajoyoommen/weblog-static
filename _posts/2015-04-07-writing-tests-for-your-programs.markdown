---
layout: post
title: Writing tests for your programs
date: 2015-04-07 03:42:54 +0530
category: Testing
tags: 
author: Ajoy Oommen
published: true
---
Writing tests for your programs is a very novel way to assert it's integrity and functionality. If your program is small enough, you might not need tests as you can have all possible edge cases and required functions in your mental stack. It would be easy to visualize all possible changes and predict what lines can break your code. But as your program increases in complexity and spans multiple files and modules and spaces, it will become *very hard and time consuming, if not impossible*. And the purpose behind programming itself is to automate and reduce repetition.

Testing is an automation to ensure that you do not have to continually check the same cases. Let me try to sum up all the advantages of testing:

 1. Write tests once, test the code forever.
 2. If you forget how a module or function is supposed to work, start with its tests.
 3. No fear of breaking the program. If you do break the program, you know where exactly to look.