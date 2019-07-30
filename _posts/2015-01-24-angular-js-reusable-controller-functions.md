---
id: 6
title: Angular JS 1.0 reusable controller functions
date: 2015-01-24T15:09:36+08:00
author: anthony
layout: post
guid: http://anthonypenner.com/?p=6
permalink: /angular-js-reusable-controller-functions/
dsq_thread_id:
  - "3769811889"
categories:
  - Angular JS
  - Cordova
  - Ionic Framework
  - Mobile App Development
  - Web Development
---
So this problem initially started when I found myself repeating similar functions across controllers. I also wanted to be able to access these functions from the view, which is why they are attached to the $scope. Notice both controllers contain the same function. I suppose we could attach this function to the $rootScope but that sounds messier and harder to test. Also referencing that from the view would not be as clean.

<pre>&lt;a href="" ng-click="linkTo('https://www.digitalocean.com/?refcode=0fb20044d22d')" /&gt;

app.controller('MoviesCtrl',['$scope', function($scope){
  $scope.linkTo = function(link) {
    window.open(link, '_system');
  };
});

app.controller('TvShowsCtrl',['$scope', function($scope){
  $scope.linkTo = function(link) {
    window.open(link, '_system');
  };
});</pre>

My preferred solution is to this is

<pre>&lt;a href="" ng-click="util.linkTo('https://www.digitalocean.com/?refcode=0fb20044d22d')" /&gt;

app.factory('UtilService', function() {
  return {
    linkTo: function(link) {
      window.open(link, '_system');
    }
  }
});

app.controller('MoviesCtrl',['$scope', 'UtilService', function($scope, UtilService){
  $scope.util = UtilService;
});</pre>

I like to use a utility service like this dry out my controllers. This was especially useful in my most recent Ionic Cordova app.