---
layout: post
title: Using python to complete IBC
date: 2013-03-02 18:39:15 +0530
category: Asynchronous Key Generation for IBE
tags: Pairing-based C C++ Python Cryptography Key Ctypes Generation Distributed
author: Ajoy Oommen
published: true
---
Well, after a few days of trying to modify the DKG protocol, it seems obvious that the code is well structured(I admire the authors) and editing a few sections without complete knowledge would be a disaster(the code has no documentation that I know of). Above all, due to my lack of any knowledge in complex C++, trying to decipher the code seems a waste.

Hence, for simplicity, I have decided to take another approach. After the DKG protocol completes, the python code will switch control to another module that will complete the remaining phases as discussed in previous posts.

I have started building the code. The ingredients that will be needed for the final section are :

* Sockets, to listen for messages and send messages
* Threads, to run these sockets on
* C functions for the PBC encryptions
* Ctypes to import these C libraries