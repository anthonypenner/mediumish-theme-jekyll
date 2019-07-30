---
id: 60
title: Linux schedule a one off reboot or task
date: 2015-05-06T05:54:11+08:00
author: anthony
layout: post
guid: http://anthonypenner.com/?p=60
permalink: /linux-schedule-a-one-off-reboot-or-task/
dsq_thread_id:
  - "3739457232"
categories:
  - DevOps
  - Linux
---
So the server requires a reboot because you updated the kernel but clients are using it and you don&#8217;t work at 1:00am? Not a problem&#8230;

<pre class="wp-code-highlight prettyprint prettyprinted"><span class="pln">echo </span><span class="str">"/sbin/shutdown -r now"</span> <span class="pun">|</span><span class="pln">at </span><span class="lit">01</span><span class="pun">:</span><span class="lit">00</span><span class="pln"> tomorrow</span></pre>

You can also the check the queued jobs with:

<pre class="wp-code-highlight prettyprint prettyprinted"><span class="pln">atq

job 15 at Wed May  6 04:00:00 2015
</span></pre>

And you can cancel the job like so:

<pre class="alt2">at -r jobid</pre>

Or you can delete all jobs with:

<pre class="alt2">atrm $(atq | cut -f1)</pre>

And as always [RTFM](http://unixhelp.ed.ac.uk/CGI/man-cgi?at)