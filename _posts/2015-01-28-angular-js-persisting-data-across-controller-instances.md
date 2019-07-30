---
id: 10
title: Angular JS 1.0 persisting data across controller instances
date: 2015-01-28T08:33:04+08:00
author: anthony
layout: post
guid: http://anthonypenner.com/?p=10
permalink: /angular-js-persisting-data-across-controller-instances/
dsq_thread_id:
  - "3776620478"
categories:
  - Angular JS
  - Ionic Framework
  - Mobile App Development
  - Web Development
---
Where do we want to store our data in Angular JS and how does it flow through your web/mobile app. This is especially important if you are using Angular JS in the mobile context. Recently I&#8217;ve been working on some Ionic Framework apps. Here is my example.

The user leaves the page then navigates back. We are duplicating API calls and losing the users context within the data. Behind the scenes we did this:

  1. Initialize controller A
  2. Load data set A into controller A via API call
  3. Load page B
  4. Initialize controller A
  5. Load data set A into controller A via API call

Alternatively we could have done this:

  1. Initialize controller A
  2. Load data set A into service A via API call
  3. Pass data from service A to Controller A via a promise
  4. Load page B
  5. Initialize controller A
  6. Load cached data set A from service A

An example of a controller and service pairing might look like this (NOTE: This is specific to my Ionic Cordova mobile app, but you can get the idea. Also note that the more function returns a promise):

<pre>.controller('HotCtrl', function($scope, UtilService, HotService) {
 UtilService.trackGAView('Hot');
 $scope.util = UtilService;
 $scope.movies = HotService.all();

 $scope.doRefresh = function() {
   HotService.clear();
   $scope.movies = [];
   setTimeout(function() {
     $scope.$broadcast('scroll.refreshComplete');
     $scope.moreMovies();
   }, 700);
 };

 $scope.moreMovies = function() {
   $scope.util.loading(true);
   HotService.more().$promise.then(function(){
     $scope.movies = HotService.all();
     setTimeout(function() {
       $scope.$broadcast('scroll.infiniteScrollComplete');
     }, 200);
     $scope.util.loading(false);
     $scope.hasMore = HotService.hasMore();
   });
 };
})

.factory('HotService', ['$resource', 'MovieService', 'UtilService', function($resource, MovieService, UtilService) {
 var page = 1;
 var movies = [];
 var regions = UtilService.getRegions();
 var hasMore = true;
 
 return hotObj = {
   all: function() {
     if(regions !== UtilService.getRegions()) {
       regions = UtilService.getRegions();
       hotObj.clear();
     }
     return movies;
   },
   more: function() {
     options = {page: page, n: 'hot', per_page: 500};
     return MovieService.query(UtilService.getParams(options), null, function(response, headers){
       if(response.length &lt; 1) {
         hasMore = false; 
       } else {
       hasMore = true;
     }
     page++;
     angular.forEach(response, function(value, key) {
       movies.push(value);
     });
     },
       function(response) {
     });
   },
   get: function(index) {
     return movies[index];
   },
   hasMore: function(){
     return hasMore;
   },
   clear: function() {
     page = 1;
     movies = [];
     hasMore = true;
   }
 }
}])</pre>