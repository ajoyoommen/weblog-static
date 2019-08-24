---
layout: post
title: Content Security Policy for Cordova
date: 2015-10-11 15:11:39 +0530
category: Cordova
tags: Policy Security tag Meta EmberJS HTML Cordova Content
author: Ajoy Oommen
published: true
---
If you have used the [cordova-plugin-whitelist](https://github.com/apache/cordova-plugin-whitelist) you will also have to provide CSP for your Android or iOS applications.

Here is an example meta tag containing all params for a Cordova app built with EmberJS

    <meta content="default-src 'self' data: gap: your.domain.com https://ssl.gstatic.com; style-src 'self' 'unsafe-inline'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; media-src *" http-equiv="Content-Security-Policy">

Points to remember:

* gap: is required only on iOS (when using UIWebView) and is needed for JS->native communication
* https://ssl.gstatic.com is required only on Android and is needed for TalkBack to function properly
* default-src allows everything from same origin (self) and your.domain.com in addition to gap:, https://ssl.gstatic.com and data:
* style-src allows all css from same origin and inline styles
* script-src allows all same origin, inlines and eval()
* media-src allows all media

---

The above tag works with an InAppBrowser plugin for Samsung S2, but doesn't work on Moto G (1st gen). To fix the error in Moto G, I have to add either a `connect-src` rule or edit the `default-src` to allow the application to connect to https://your.domain.com

So I changed the `default-src`. Turns out that adding 'your.domain.com' is not enough. To enable all connections to my domain I modified this

    default-src 'self' data: gap: your.domain.com https://ssl.gstatic.com;

to this

    default-src 'self' data: gap: https://your.domain.com https://ssl.gstatic.com;

To allow requests from a domain you will also have to add:

    <allow-navigation href="https://your.domain.com/*"/>