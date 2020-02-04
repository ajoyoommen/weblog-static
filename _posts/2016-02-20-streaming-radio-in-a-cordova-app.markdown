---
layout: post
title: Streaming radio in a Cordova app
date: 2016-02-20 07:42:39 +0530
category: Cordova
tags:
    - Javascript
    - Android
    - Audio
    - EmberJS
    - Cordova
author: Ajoy Oommen
published: true
---
Recently, I built a player to stream an audio URL in a Cordova app. For this player, I used [cordova-plugin-media](https://github.com/apache/cordova-plugin-media).

**cordova-plugin-media** is a plugin to record and play audio files. But you can use it to stream from an audio URL Therefore, it does not have callbacks to report an interrupted stream, fetching track information, etc. The plugin has methods like `getDuration` which only work for audio files. If you are streaming a radio, the only methods that would work are `play`, `pause`, `stop` and `release`.

The lack of proper error callbacks hinders a seamless user experience and it creates a lot of bugs. I intend to write a good plugin to do so, but in this post I use available plugins to wrap play and stop actions. Here's how:

* Create a new Media resource

        var media = new Media(src, mediaSuccess, [mediaError], [mediaStatus]);

    On Android, `mediaSuccess`, `mediaError` and `mediaStatus` are never triggered. Hence I had to make do without them.

* Once `media` is initialized, it is easy to play and stop streaming by calling `media.play()` and `media.stop()`. But if the network connection is interrupted, streaming stops. You will have to stop and then play the media to fix the interruption.

* To work around this problem, I used [cordova-plugin-network-information](https://github.com/apache/cordova-plugin-network-information). This plugin provides two helpful events [offline](https://github.com/apache/cordova-plugin-network-information#offline) and [online](https://github.com/apache/cordova-plugin-network-information#online). You can also check the connection at any time by checking `connection.type`.

* Using the callbacks provide by network-infomation plugin, you can stop your radio as soon as the internet is disconnected and resume connection when internet is restored. This way, you get to provide for missing plugin-media functionality by using network status.