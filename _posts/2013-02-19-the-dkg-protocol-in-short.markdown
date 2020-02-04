---
layout: post
title: The DKG protocol, in short
date: 2013-02-19 17:58:52 +0530
category: Asynchronous Key Generation for IBE
tags:
    - VSS
    - DKG
author: Ajoy Oommen
published: true
---
When the DKG protocol completes, it outputs a secret for each of the nodes participating in the protocol. In short, the above process proceeds as below:

1. HybridVSS – In this process, a node selects a secret and shares it among all the other nodes. At the end of the process, every other node has a share of the secret, such that any subset (of nodes), of size greater than t, will be able to reconstruct the original secret from the shares.
2. All nodes run the HybridVSS. When all the nodes complete the VSS, every node will have a share of the secret selected by every other node.
3. Finally, all the shares that a node holds, are combined to generate the DKG share.