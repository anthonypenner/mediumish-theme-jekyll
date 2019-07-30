---
id: 18
title: USDA Nutrient Data SR23 POSTGRES SQL dump
date: 2015-02-24T17:14:52+08:00
author: anthony
layout: post
guid: http://anthonypenner.com/?p=18
permalink: /usda-nutrient-data-sr23-postgres-sql-dump/
dsq_thread_id:
  - "3863374211"
categories:
  - DevOps
  - Pro Bono
  - Ruby on Rails
---
I found the mysql dump online but these days I prefer Postgres. For others who are in the same boat as me, I thought I would save you the troubles! Enjoy.

<a href="http://anthonypenner.com/?attachment_id=20" rel="attachment wp-att-20">USDA Nutrient Data (SR23) Postgres Dump</a>

I used [py-mysql2pgsql](https://github.com/philipsoutham/py-mysql2pgsql) and renamed all the tables to lower case.

I plan to hook this data into elastic search so that I can search on it with a rails api and return JSON. Maybe I&#8217;ll open source the elastic search rails API I&#8217;m going to build.

UPDATE:

I noticed that this data set is missing the food categories, you can seed these with this:

<pre># db/seeds.rb

groups = [
  ["0100", "Dairy and Egg Products"],
  ["0200", "Spices and Herbs"],
  ["0300", "Baby Foods"],
  ["0400", "Fats and Oils"],
  ["0500", "Poultry Products"],
  ["0600", "Soups, Sauces, and Gravies"],
  ["0700", "Sausages and Luncheon Meats"],
  ["0800", "Breakfast Cereals"],
  ["0900", "Fruits and Fruit Juices"],
  ["1000", "Pork Products"],
  ["1100", "Vegetables and Vegetable Products"],
  ["1200", "Nut and Seed Products"],
  ["1300", "Beef Products"],
  ["1400", "Beverages"],
  ["1500", "Finfish and Shellfish Products"],
  ["1600", "Legumes and Legume Products"],
  ["1700", "Lamb, Veal, and Game Products"],
  ["1800", "Baked Products"],
  ["1900", "Sweets"],
  ["2000", "Cereal Grains and Pasta"],
  ["2100", "Fast Foods"],
  ["2200", "Meals, Entrees, and Side Dishes"],
  ["2500", "Snacks"],
  ["3500", "American Indian/Alaska Native Foods"],
  ["3600", "Restaurant Foods"]
]

groups.each do |g|
  FoodGroup.first_or_create(FdGrp_Cd: g[0], FdGrp_Desc: g[1])
end
</pre>

Additionally, if you&#8217;d like some rails models for your api, this might be a good start:

<pre># app/models/food.rb
class Food &lt; ActiveRecord::Base
  self.table_name = "food_des"
  self.primary_key = "NDB_No"

  has_many :measures, :foreign_key => "NDB_No"
  has_one :food_group, primary_key: "FdGrp_Cd", :foreign_key =>  "FdGrp_Cd"
end

# app/models/food_group.rb
class FoodGroup &lt; ActiveRecord::Base
  self.table_name = "fd_group"
end

# app/models/measure.rb
class Measure &lt; ActiveRecord::Base
  self.table_name = "weight"

  belongs_to :food, primary_key: "NDB_No", :foreign_key =>  "NDB_No"
end
</pre>