---
id: 68
title: Automating WordPress updates with cron and the wp-cli
date: 2015-05-08T11:33:34+08:00
author: anthony
layout: post
guid: http://anthonypenner.com/?p=68
permalink: /automating-wordpress-updates-with-cron-and-the-wp-cli/
dsq_thread_id:
  - "3769345975"
categories:
  - DevOps
  - Linux
  - Web Development
---
Ain&#8217;t nobody got time to worry about updates. I&#8217;d rather have it break while updating than have it hacked by a script kiddie because I&#8217;ve neglected to login for a while. We will see if this ever breaks anything.

<a href="http://wp-cli.org/" target="_blank">Install WordPress-CLI</a> if you have not already.

Add all your sites to the update_wp.sh script:

<pre>#!/bin/bash
declare -a arr=(
  "anthonypenner.com" 
  "trekcamp.org"
  "etc"
)

for i in "${arr[@]}"
do
  echo $i
  cd /usr/share/nginx/html/$i
  sudo -u nobody wp core update
done
</pre>

NOTE: Substitute /usr/share/nginx/html for the directory that holds your WordPress sites. Change user nobody to the user that owns the aforementioned directories. Check with ls -al, it might be the user that apache/nginx runs as, check that with top, htop, or ps -aux

Make sure it&#8217;s executable:

<pre>chmod +x <span class="s1">update_wp.sh</span></pre>

Run the script daily and live dangerously!

<pre class="p1"><span class="s1">30 2 * * * bash /home/anthony/update_wp.sh &gt;/dev/null 2&gt;&1</span></pre>