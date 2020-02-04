---
layout: post
title: Importing and exporting an element in PBC
date: 2013-03-05 18:42:05 +0530
category: Asynchronous Key Generation for IBE
tags:
    - PBC
    - C++
    - DKG
author: Ajoy Oommen
published: true
---
I added this function to the PBC wrapper classes:

    void Zr::dumpfile(FILE *f)const{
      unsigned char sharedump[20];
      int bt;
      bt = element_length_in_bytes(*(element_t*)&amp;r);
      element_to_bytes(sharedump, *(element_t*)&amp;r);
      fwrite(sharedump, bt, 1, f);
    }

So when the DKG completes, I call ‘result.share.dumpfile’. Not going into the depths. Just documenting these functions.

To extract the element saved in binary, I use this function :

    int read_share(){
      FILE *fp;
      unsigned char str[20];
      fp = fopen("../secrets","rb");
      if(!fp)
        return -1;
      fread(str, 20, 1, fp);
      fclose(fp);
      element_init_Zr(share, pairing);
      element_from_bytes(share, str);
    }