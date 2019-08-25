---
layout: post
title: Modifying the DKG protocol
date: 2013-02-19 18:03:34 +0530
category: Asynchronous Key Generation for IBE
tags: Verifiable Sharing IBC Identity-based Cryptography Franklin Secret Key Boneh Generation Distributed
author: Ajoy Oommen
published: true
---
The DKG protocol executes the following functions when it encounters the corresponding triggers :

* hybridVSSInit
* startAgreement
* completeDKG

To enable the functions proposed in the paper [Asynchronous Distributed Private-Key Generators for Identity-Based Cryptography](http://eprint.iacr.org/2009/355.pdf), we will need to add an additional function after the DKG completes.

This function generateIBCKey will

1. Send the ID of the node to all nodes
2. Receive the ID of some other node x and return (h(ID))^share
3. Receive the return value from other nodes and save it into some D_i
4. Calculate the private key using D_i
5. Verify the private_key. If faulty, verify the values from each node.