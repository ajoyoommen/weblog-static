---
layout: post
title: Redirecting with NGINX
date: 2015-09-01 09:33:50 +0530
category: NGINX
tags:
    - NGINX
author: Ajoy Oommen
published: true
---
How do you redirect a URL, say `/shop` to another URL, say `/product` within your website? You add a redirect in `location`.

        location /shop/ {
          rewrite ^/shop/(.*)$ /product/$1 permanent;
	}

This location redirect does the following:

1. It catches URLs beginning with /shop/
2. It redirects `/shop/[whatever]` it to `/product/[whatever]`
3. Sends a 301 permanent redirect

So it will redirect `/shop/ABCDEF` and `/shop/ABCDEF/` to `/product/ABCDEF` and `/product/ABCDEF/` respectively.