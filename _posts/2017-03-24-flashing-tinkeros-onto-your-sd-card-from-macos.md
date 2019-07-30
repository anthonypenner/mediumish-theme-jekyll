---
id: 138
title: Flashing TinkerOS onto your sd card from MacOS
date: 2017-03-24T01:17:48+08:00
author: anthony
layout: post
guid: http://anthonypenner.com/?p=138
permalink: /flashing-tinkeros-onto-your-sd-card-from-macos/
categories:
  - Linux
  - MacOs
---
The main landing page for the Tinker Board only provides instructions for Window and Linux. And at this point the Linux zip they provide for you to download is corrupt or just refuses to unzip on MacOS. Anyways, heres the steps I took to get my card flashed and ready to go.

  1. Download TinkerOS 1.4 (or latest) from [here](https://www.asus.com/uk/Single-board-Computer/TINKER-BOARD/HelpDesk_Download/)
  2. Unzip: 
    <pre>unzip 20170223-tinker-board-linaro-jessie-alip-v14.zip</pre>

  3. Insert micro sd card into macbook via adapter
  4. Open disk utility and ensure sd card is formatted as FAT 32
  5. Take note of the device, in my case disk2s1, but we just need to know that it&#8217;s disk2
  6. Unmount the card via disk utility
  7. Image card: 
    <pre>sudo dd if=/Users/anthony/Desktop/20170223-tinker-board-linaro-jessie-alip-v14.img of=/dev/rdisk2 bs=1m</pre>

  8. Took about 70 seconds to compete the dd command
  9. Plug into Tinker Board and tinker away