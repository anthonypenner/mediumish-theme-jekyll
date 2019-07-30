---
id: 64
title: Elastic search order by date and indexing with Ruby-on-Rails
date: 2015-05-07T09:40:43+08:00
author: anthony
layout: post
guid: http://anthonypenner.com/?p=64
permalink: /elastic-search-order-by-date-and-indexing-with-ruby-on-rails/
dsq_thread_id:
  - "3769446448"
categories:
  - Elastic Search
  - Ruby on Rails
---
The elastic search date field type seems like a lot of work. You have to manually map each date field parameter. All I really wanted to do was order by date, and this doesn&#8217;t necessarily rule out more complicated date range filtering. You just have to convert everything to Epoch millisecond time. Sometimes it just seems like the most standard format.

Map the field type in your elastic search index as a float. Changing a field type requires recreating the index.

<pre>mappings dynamic: 'false' do
 indexes :created_at, type: 'float'
end</pre>

In your method that converts the model to json for indexing, convert the date to milliseconds.

<pre>def as_indexed_json(options={})
  model_attrs = {
    :created_at =&gt; self.created_at.to_f
  }
end</pre>

Now you can easily order by asc and desc.