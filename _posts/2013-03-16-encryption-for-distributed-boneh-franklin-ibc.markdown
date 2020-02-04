---
layout: post
title: Encryption for distributed Boneh-Franklin IBC
date: 2013-03-16 18:49:06 +0530
category: Asynchronous Key Generation for IBE
tags:
    - PBC
    - C
    - IBC
author: Ajoy Oommen
published: true
---
This is the function for encryption once the keys are generated. I have added the corresponding mathematical expressions, to make it easy to understand and modify. I have not yet tested it.

    void encrypt20(unsigned char * message, char * rid){
      unsigned char u[128], v[20], w[20], tv[20], tw[20];
      unsigned char sig[20], r[20];
      int i;
      element_t temp1;

      element_random(h2gt);
      hash2(sig);                           //    sig now has a random number
                                            //sig = {0,1}l
      hash3(sig, message);                  //    h3zr ~ r
                                            //r = H3(sig, M)
      hash1(rid);                           //hid = H1(ID) ~ ~ h1 = H1(rid)

      element_init_G1(g, pairing);
      element_random(g);

      element_t U;
      element_init_G1(U, pairing);
      element_pow_zn(U, g, h3zr);           //U = g ^ r, then u = U

      element_init_GT(temp1, pairing);
      pairing_apply(temp1, U, h1, pairing); //temp1 = e(U, h1) = e(g^r,hid)
      element_pow_zn(h2gt, temp1, h3zr);    //h2gt = temp1^h3zr ~~ h2gt=temp1^r
      hash2(tv);                            //tv = H2(h3zr) ~ H2(e(g^r,hid)^r)
      for(i=0;i<20;i++){
        v[i] = sig[i]^tv[i];
      }                                     //v = sig XOR tv
      hash4(sig, tw);                       //tw = H4(sig)
      for(i=0;i<20;i++){
        w[i] = message[i]^tw[i];
      }                                     //w = message XOR tw
      element_to_bytes(u, U);
    }