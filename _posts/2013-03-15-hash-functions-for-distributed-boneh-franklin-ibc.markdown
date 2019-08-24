---
layout: post
title: Hash functions for distributed Boneh Franklin IBC
date: 2013-03-15 18:45:21 +0530
category: Asynchronous Key Generation for IBE
tags: Pairing-based C Mathematics Hash IBC Identity-based Cryptography Franklin Key Boneh function Generation Distributed
author: Ajoy Oommen
published: true
---
These are the hash functions that are needed to implement the Boneh-Franklin IBC after the distributed share generation.

> H1 : {0,1}* -> G2*

> H2 : GT -> {0,1}^l, l = 20

> H3 : {0,1}^l X {0,1}^l -> Zp

> H4 : {0,1}^l -> {0,1}^l

This is the C code of the hash functions using the PBC library :

    void hash1(char * b){
      /* Mathematically
       * H1 : {0,1}* -> G2*
       * This function will take an input as char, hash it and
       * convert it into a element h1 in G2.
       */
      unsigned char h[20];
      SHA1(b, sizeof(b), h);
      element_from_hash(h1, h, 20);
    }
     
    void hash2(unsigned char *b){
      /* Mathematically,
       * H2 : GT -> {0,1}^l, l = 20
       * This function will take a GT value stored in an element h2gt
       * and store a 20 byte hash in the unsigned char b
       */
      unsigned char temp[128];
      element_to_bytes(temp, h2gt);
      SHA1(temp, sizeof(temp), b);
    }
     
    void hash3(unsigned char * b1, unsigned char * b2){
      /* Mathematically,
       * H3 : {0,1}^l X {0,1}^l -> Zp
       * This function will take two binary strings and generate an element
       * of Zr and store it in h3zr
       */
      int i;
      unsigned char rnd[20];
      for(i=0; i<20; i++){
        rnd[i] = b1[i] ^ b2[i];
      }
      element_from_bytes(h3zr, rnd);
    }
     
    void hash4(unsigned char * b, unsigned char * hb){
      /* Mathematically,
       * H4 : {0,1}^l -> {0,1}^l
       * This function will take a 20 byte string, and return its hash
       * back to hb.
       */
      SHA1(b, sizeof(b), hb);
    }
     
    void init_hashes(){
      element_init_G2(h1, pairing);
      element_init_GT(h2gt, pairing);
      element_init_Zr(h3zr, pairing);
    }