---
id: 35
title: Capistrano hangs on assets precompile and never finishes the deploy
date: 2015-04-19T04:04:29+08:00
author: anthony
layout: post
guid: http://anthonypenner.com/?p=35
permalink: /capistrano-hangs-on-assets-precompile-and-never-finishes-the-deploy/
dsq_thread_id:
  - "3774034606"
categories:
  - Uncategorized
---
This only started happening after I killed a deploy that was half completed. It could have been during the asset precompile process, I don&#8217;t remember. It&#8217;s weird though because the symptoms seem more like an ssh session issue and less like residual files from my cancelled deploy that is causing the issue.

I added keep alive options to ssh:

<pre>:ssh_options =&gt; {
  :keepalive =&gt; true,
  :keepalive_interval =&gt; 30
}</pre>

I also confirmed that tmp/cache was in my linked dirs to save some precompile time. It was suggested in the <a href="https://github.com/capistrano/rails/issues/55" target="_blank">repo issues thread</a>.

    set :linked_dirs, %w{bin log tmp/pids tmp/cache tmp/sockets public/system}