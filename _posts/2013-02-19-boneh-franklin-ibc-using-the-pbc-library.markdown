---
layout: post
title: Boneh Franklin IBC using the PBC library
date: 2013-02-19 17:37:31 +0530
category: Asynchronous Key Generation for IBE
tags: Pairing-based C Decryption IBC Identity-based Cryptography Franklin Boneh Encryption
author: Ajoy Oommen
published: true
---
I found this code in the PBC library developer's group : [bonehfranklin.c](https://groups.google.com/forum/?fromgroups=#!searchin/pbc-devel/bonehfranklin/pbc-devel/jmyPok2P8-E/aXiA_B1fBnUJ)

    /***
        Boneh & Franklin encryption scheme as presented in the article "Id-based encryption from the Weil pairing"
    ***/
    
    #include<stdio.h>
    #include<stdlib.h>
    #include<time.h>
    #include<assert.h>
    #include<fcntl.h>
    #include<math.h>
    #include<string.h>
    #include<openssl/sha.h>
    #include<openssl/err.h>
    #include<openssl/ssl.h>
    #include<pbc/pbc.h>
    #include<pbc/pbc_test.h>
    
    
    int main(int argc, char **argv)
    {
      pairing_t pairing;
      element_t P, Ppub, Did, Qid, U, s, r, xt, gid;
      int fd=0,i=0;
      char m[80]={0}, mv[80]={0}, c[80]={0}, id[20]="157.157.157.157", hash[20]={0}, err[80]={0}, *gs=NULL;
    
    
      /***
          errors strings initialization for SHA1 & clock initialization for times computation
      ***/
      ERR_load_crypto_strings();
      SSL_load_error_strings();
    
      /***initialize the message which is going to be signed***/
      fd = open("/dev/urandom",O_RDONLY);
      read(fd, m, 80);
      close(fd);
    
      /***
          pairing function initalization from the input file which contains the pairing parameters
      ***/
      pbc_demo_pairing_init(pairing, argc, argv);
      if (!pairing_is_symmetric(pairing)) pbc_die("pairing must be symmetric");
    
      /***
          initialization of G1 elements
      ***/
      element_init_G1(P, pairing);
      element_init_G1(Ppub, pairing);
      element_init_G1(Qid, pairing);
      element_init_G1(Did, pairing);
      element_init_G1(U, pairing);
    
      /***
          initialization of Zr elements
      ***/
      element_init_Zr(s, pairing);
      element_init_Zr(r, pairing);
    
      /***
          initialization of GT elements
      ***/
      element_init_GT(gid, pairing);
      element_init_GT(xt, pairing);
      /***
          PKG generation of P, s and Ppub
      ***/
      element_random(P);
      element_random(s);
      element_mul_zn(Ppub, P, s);
      element_printf("++s: %B\n",s);
      element_printf("++P:  %B\n", P);
      element_printf("++Ppub: %B\n", Ppub);
    
    
      /***
          key generation
      ***/
      if(SHA1(id, 20, (unsigned char *)hash)==NULL)
        {
          ERR_error_string(ERR_get_error(),err);
          printf("%s\n",err);
        }
      element_from_hash(Qid, hash, strlen(hash));
      element_mul_zn(Did, Qid, s);
      element_printf("++Qid: %B\n", Qid);
      element_printf("++Did: %B\n", Did);
    
      /***
          encryption
      ***/
      element_random(r);
      element_mul_zn(U, P, r);
      element_pairing(gid, Qid, Ppub);
      element_pow_zn(gid, gid, r);
      gs=malloc(element_length_in_bytes(gid));
      element_to_bytes(gs,gid);
      if(SHA1(gs , 20, (unsigned char *)hash)==NULL)
        {
          ERR_error_string(ERR_get_error(),err);
          printf("%s\n",err);
        }
      for(i=0;i<20;i++)
        {
          c[i]=m[i]^hash[i];
          c[i+20]=m[i+20]^hash[i];
          c[i+40]=m[i+40]^hash[i];
          c[i+60]=m[i+60]^hash[i];
        }
      free(gs);
    
      element_printf("++r: %B\n",r);
      element_printf("++U: %B\n",U);
      element_printf("++gid: %B\n",gid);
    
      /***
          decryption
      ***/
      element_pairing(xt, Did, U);
      gs=malloc(element_length_in_bytes(xt));
      element_to_bytes(gs,xt);
      if(SHA1(gs , 20, (unsigned char *)hash)==NULL)
        {
          ERR_error_string(ERR_get_error(),err);
          printf("%s\n",err);
        }
      for(i=0;i<20;i++)
        {
          mv[i]=c[i]^hash[i];
          mv[i+20]=c[i+20]^hash[i];
          mv[i+40]=c[i+40]^hash[i];
          mv[i+60]=c[i+60]^hash[i];
        }
    
      element_printf("e(Did,U):%B\n",xt);
      for(i=0;i<80;i++)
        {
          printf(":%d %d %d\n",i,m[i],mv[i]);
        }
    
      printf("\n%d\n",strncmp(m,mv,80));
    
      /***free mem***/
      element_clear(P);
      element_clear(Ppub);
      element_clear(Qid);
      element_clear(Did);
      element_clear(U);
      element_clear(gid);
      element_clear(r);
      element_clear(xt);
      element_clear(s);
      pairing_clear(pairing);
      free(gs);
      ERR_free_strings();
    
      return 0;
    }