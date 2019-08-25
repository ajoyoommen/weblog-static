---
layout: post
title: Installing Cordova and android platform tools
date: 2016-02-23 15:56:56 +0530
category: Cordova
tags: Node Cordova Android
author: Ajoy Oommen
published: false
---
These are the precise steps that I used to install Cordova on Ubuntu 14.04 (64-bit).

* Install npm

      sudo apt-get install npm

* Install cordova

      sudo npm install -g cordova

* Download android sdk (Eg: [SDK 24.4.1](http://dl.google.com/android/android-sdk_r24.4.1-linux.tgz))
* Extract the SDK to a folder, say 'android-sdk-linux'
* Install java

      sudo apt-get install openjdk-7-jdk

 Find installation directory, usually `/usr/lib/jvm/java-7-openjdk-amd64/`.

* Install apache ant

      sudo apt-get install ant

 ANT_HOME is at `/usr/share/ant`

* Install build and platform tools - Run `android` from `android-sdk-linux/tools/` and select and install all required packages

* Add the following environment variables: `ANDROID_HOME`, `ANT_HOME`, `ANDROID_PLATFORM_TOOLS`and `JAVA_HOME`

 ```
 export HOME=/home/username

 export ANDROID_HOME=$HOME/android-sdk-linux

 export ANT_HOME=/usr/share/ant

 export ANDROID_PLATFORM_TOOLS=$HOME/android-sdk-linux/platform-tools

 export PATH=$PATH:$ANDROID_HOME:$ANT_HOME/bin:$ANDROID_PLATFORM_TOOLS

 export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/
 ```

* If `cordova platform` gives an error - /usr/bin/env: node: No such file or directory, add this link

      sudo ln -s /usr/bin/nodejs /usr/bin/node

* If `cordova platform` gives error: Cannot find module 'bplist-parser', run

      sudo npm update -g

* I had to install 32-bit libraries of libc and libstdc to get `adb` working

      sudo apt-get install libc6:i386 libstdc++6:i386

* `cordova run android` was failing with "Process 'command '/home/username/android-sdk-linux/build-tools/23.0.1/aapt'' finished with non-zero exit value 127"

      sudo apt-get install zlib1g:i386