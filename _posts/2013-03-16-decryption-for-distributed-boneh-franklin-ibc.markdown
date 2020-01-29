---
layout: post
title: Decryption for distributed Boneh-Franklin IBC
date: 2013-03-16 18:53:44 +0530
category: Asynchronous Key Generation for IBE
tags:
    - PBC
    - C
    - IBC
author: Ajoy Oommen
published: true
---
This is the corresponding decryption function for a message C = (u, v, w)

    void decrypt20(unsigned char * u, unsigned char * v, unsigned char * w){
      int i;
      unsigned char sig[20], tsig[20], hsig[20], m[20];
      element_t U, temp1, g, gpub;
      element_from_bytes(U, u);

      element_init_GT(temp1, pairing);
      pairing_apply(temp1, U, pk, pairing); //temp1 = e(U, pk) ~ e(u, did)
      hash2(tsig);                          //tsig = H2(e(u, did))
      for(i=0;i<20;i++){
        sig[i] = v[i] ^ tsig[i];            //sig = v XOR H2(e(u, did))
      }
      hash4(sig, hsig);                     //hsig = H4(sig)
      for(i=0;i<20;i++){
        m[i] = w[i] ^ hsig[i];              //m = w XOR H4(sig)
      }
      hash3(sig, m);                        //r = H3(sig, m), r = h3zr
      element_init_G1(g, pairing);
      element_init_G1(gpub, pairing);
      element_random(g);

      element_pow_zn(gpub, g, h3zr);        //gpub = g ^ h3zr <==> g^r

      if (!element_cmp(gpub, U))            //if g^r != u, reject
        printf("Message is %s\n", m);
      else
        printf("Invalid message.\n");
    }