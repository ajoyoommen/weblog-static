---
layout: post
title: Chaining SSL Certificates
date: 2014-08-16 16:52:36 +0530
category: SSL
tags: Comodo Untrusted Authority Chaining SSL Certificate Certificates
author: Ajoy Oommen
published: true
---
I recently came across this issue after we had installed SSL certificates for a website. The SSL certificates were well recognised and accepted across all browsers on desktop machines. On Android, Chrome refused to accept the certificate saying that the certificate was issued by someone untrusted.

After some Google and SO searches, I figured out that this issue can be caused by two factors, each solved by the same simple solution.

When you apply for an SSL certificate (From COMODO for example), the CA gives you two files – the CRT and the CA-BUNDLE.

The CRT contains the certificate for your domain, signed by the CA. This would look something like :

    Identity : http://www.example.com
    Verifier : Intermediate CA

The CA-BUNDLE contains certificates of the intermediate CA signed by some Global CA. For the above example :

    Identity : Intermediate CA
    Verifier : Another intermediate CA

    Identity : Another intermediate CA
    Verifier : Trusted CA

The two factors that might cause this are:
1. Chrome does not truly recognise Intermediate CA.
2. Your certificates might be chained, but not in the proper order.

In my case, I believe that the cause was 1.

In my sever configuration, I had only specified the CRT that contained identity for my domain, but did not provide the certificates that verify the Intermediate CA.

The solution to both the problems is simple. Copy the certificates in the CA-BUNDLE and place them after your certificate in the CRT. In case your certificates are not ordered properly, rearrange them as below.

This is how your CRT should look like when you at done with it:

    Identity : http://www.example.com
    Verifier : Intermediate CA

    Identity : Intermediate CA
    Verifier : Another intermediate CA

    Identity : Another intermediate CA
    Verifier : Trusted CA

CRT files are simple text files that have the certificates wedged between a BEGIN CERTIFICATE and an END CERTIFICATE. But to actually read the contents of the certificate you will need to use OpenSSL or gcr-viewer under Linux.

When you have chained your certificates, you’re done. Chrome on Android will now accept the certification.