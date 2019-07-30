---
id: 14
title: 'Rails 4.2 and Elastic Search &#8211; Querying and Filtering Across Multiple Nested Models'
date: 2015-02-10T13:32:36+08:00
author: anthony
layout: post
guid: http://anthonypenner.com/?p=14
permalink: /rails-4-2-and-elastic-search-querying-and-filtering-across-multiple-nested-models/
dsq_thread_id:
  - "3769900800"
categories:
  - Elastic Search
  - Ruby on Rails
---
At work recently I have been working on a new filtering and search page for ourselves. The requirements were searching and filtering said search results. Things get complicated and performance tends to suffer when dealing with complicated and far reaching relationships. You end joining every table in the database and even then you are only filtering and not actually searching. Sometimes denormalized data just makes sense. It&#8217;s still a WIP but here it as anyway, hope this snippet helps you get rolling with your project.

<pre>def self.search(query="", options={})

   params = {
      query: {
        filtered: {
          query: {
            multi_match: {
              query: query,
              fields: ['title^10', 'overview']
            },
            match_all: {}
          },
          filter: {
            bool: {
              must: [
                {
                  nested: {
                    path: 'regions',
                    filter: {
                      bool: {
                        must: [
                          {
                            terms: { 'regions.id' =&gt; options[:regions] }
                          }
                        ]
                      }
                    }
                  }
                },
                {
                  nested: {
                    path: 'genres',
                    filter: {
                      bool: {
                        must: [
                          {
                            terms: { 'genres.id' =&gt; options[:genres] }
                          }
                        ]
                      }
                    }
                  }
                }
              ]
            }
          }
        }
      },
      sort: [
        {
          options[:col].try(:downcase) =&gt; {
            order: options[:direction].try(:downcase) #, ignore_unmapped: true
          }
        }
      ]
    }

    params[:query][:filtered][:query].delete(:match_all) if query.present?
    params[:query][:filtered][:query].delete(:multi_match) if query.blank?
    params.delete(:sort) if options[:col].blank? || options[:direction].blank?

    __elasticsearch__.search(params).page(options[:page]).per(options[:limit])
  end
</pre>

&nbsp;