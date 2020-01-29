---
layout: post
title: Algorithm to Generate IBC Keyshares
date: 2013-02-19 18:11:35 +0530
category: Asynchronous Key Generation for IBE
tags:
    - IBC
author: Ajoy Oommen
published: true
---
    function generateIBCKeys(ID as string)
        h = hash(ID)
        for all nodes in active_nodes
            send IBCReq(h, self.nodeID) to nodes

        for each node in received.IBCReq
            key = h ^ share
            send IBCkey(key, self.ID) to node(nodeID)

        for each received.IBCkey(key, nodeID)
            add (key, nodeID) to D_i

        if(len(D_i)>t)
            Gen_privatekey(D_i)

        verify(private_key)
        if(!verified)
            for each (h, nodeID) in D_i

            verify(h, nodeID)
            if(!verified)
                post(“Complaint on nodeiD”)