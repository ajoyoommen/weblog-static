---
layout: post
title: Understanding DKG
date: 2013-02-23 18:22:21 +0530
category: Asynchronous Key Generation for IBE
tags:
    - C++
    - PBC
    - DKG
    - VSS
author: Ajoy Oommen
published: true
---
The DKG source is programmed in C++. It requires the PBC library, which in turn depends on the GMP library.

The main code of the DKG lies in node.cc. This file starts a thread and runs a DKG node at a specified port.

The node:run() runs a code that executes functions based on the network messages and user messages.

Based on various messages received (network), the run() calls the following functions:

1. hybridVSSInit
2. startAgreement
3. completeDKG
4. sendLeaderChangeMessage
5. changePhase

The code for these functions must be studied to understand how messages are declared and sent. For adding the required functions and features, I would have to declare network messages (in networkmessage.h and networkmessage.cc), add functions in class Node() and add event code to respond to the new messages and call the corresponding new functions from Node().