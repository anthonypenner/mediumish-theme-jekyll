---
id: 100
title: RuboCop and the 80 character line length limit is absurd
date: 2015-07-06T15:14:20+08:00
author: anthony
layout: post
guid: http://anthonypenner.com/?p=100
permalink: /rubocop-and-the-80-character-line-length-limit-is-absurd/
dsq_thread_id:
  - "3911385559"
categories:
  - Ruby on Rails
  - Vim
---
Having a line length limit is absurd. Just change your wrap setting in vim! IMO this gives you best of both worlds you can have everything on the screen when you need it or off when you don&#8217;t. Also, it&#8217;s easier to read indentation and syntax on one liner code snippets. When the code flows off the screen it can hide a lot of grit and make it easier to get a general lay of the land (ie. control logic) and then turn wrap on and hack.

Every time you adjust the select query you have to fiddle with the line length because OMG, it might overflow 80 and break your mac.

I myself think any line length limit is absurd, just use common sense. If it is clearer to break, then break it, otherwise don&#8217;t. But breaking it for the sake of an arbitrary length limit?

You&#8217;re inevitably going to use a line limit (You might even agree with it), so put this in your .vimrc, it highlights the 100th character. (Yeah I use a 100 character line length limit). I think Github displays 120 characters so that might actually make the most sense.

<pre>autocmd Filetype ruby highlight ColorColumn ctermbg=red
autocmd Filetype ruby call matchadd('ColorColumn', '\%101v', 120)
</pre>