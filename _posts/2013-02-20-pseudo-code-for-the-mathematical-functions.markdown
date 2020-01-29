---
layout: post
title: Pseudo code for the mathematical functions
date: 2013-02-20 18:20:44 +0530
category: Asynchronous Key Generation for IBE
tags:
    - PBC
    - DKG
author: Ajoy Oommen
published: true
---
    function gen_privatekey(array D_i)
        for each (key, nodeID) in D-i
            l = lambda(nodeID)
            element_pow_zn(f, key, l)
            element_mul_zn(prkey, prkey, f)
        return prkey

    function lambda(node as integer)
        j = 1
        l = 1
        while(j < n)
            if j = node
                continue
            else
                r = j /(j – i)
                l = l * r
            j++