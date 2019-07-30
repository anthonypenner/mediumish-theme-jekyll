---
id: 27
title: Linux Backlight for CM Storm Devastator Keyboard
date: 2015-03-07T00:23:47+08:00
author: anthony
layout: post
guid: http://anthonypenner.com/?p=27
permalink: /linux-backlight-for-cm-storm-devastator-keyboard/
dsq_thread_id:
  - "3770961456"
categories:
  - Linux
  - Pro Bono
---
The CM Storm keyboard was dirt cheap and looks really cool. The only problem is that on Fedora 21 and other Linux distributions the Scroll Lock key is not enabled by default. And it just happens to be the key that toggles the keyboard backlight. I don&#8217;t know why they didn&#8217;t just put a switch on the keyboard.

For Fedora 21:

Install A keyboard binding tool

<pre>yum install xdotool</pre>

Add an easy alias to toggle the backlight from the command line with the &#8216;k&#8217; key

<pre>vim ~/.bashrc

alias k="xmodmap -e 'add mod3 = Scroll_Lock' && xdotool key --delay 10 'Scroll_Lock'"
</pre>

Even better than that, lets trigger the toggle command on login. This part is Gnome specific and wont work on Ubuntu (Unless you removed Unity)!

<pre>vim ~/.bash_profile

dbus-monitor --session "type='signal',interface='org.gnome.ScreenSaver'" | ( while true; do read X; if echo $X | grep "boolean true" &> /dev/null;then :; elif echo $X | grep "boolean false" &> /dev/null; then sh /home/penner/scripts/keyboard.sh; fi done ) &

vim /home/penner/scripts

#!/bin/bash/keyboard.sh

xmodmap -e "add mod3 = Scroll_Lock"
xdotool key --delay 10000 "Scroll_Lock"
</pre>

You need to adjust the script paths for yourself. Might want to tweak the time outs as well. Good luck.