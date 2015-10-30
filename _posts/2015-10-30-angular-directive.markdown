---
layout:     post
title:      "AngularJS 中定义 directive 解决问题"
subtitle:   "几种 directive 搜集，解决常见问题"
date:       2015-10-30 22:05:00
author:     "Franklee"
header-img: "img/post-bg-06.jpg"
---
为超链接加后退的功能。
<pre>
app.directive 'backButton', ->
  {
    restrict: 'A'
    link: (scope, element, attrs) ->
      element.bind 'click', ->
        history.back()
        scope.$apply()
        return
      return
  }
</pre>
使用方法:
  [a href back-button]后退[/a]


为UI-Router加动态Title的功能
<pre>
app.directive 'updateTitle', [
  '$rootScope'
  '$timeout'
  ($rootScope, $timeout) ->
    { link: (scope, element) ->
      $rootScope.$on '$stateChangeSuccess', (event, toState) ->
        title = '默认标题'
        if toState.title
          title = toState.title
        $timeout (->
          element.text title
        ), 0, false
 }
]
</pre>
使用方法:
<pre>
  [title update-title][/title]
  $stateProvider
    .state 'datum.category',
      url: '/category'
      title: '资料帮'
      views:
        'stage':
          templateUrl: 'templates/datum-category.html'
          controller: 'DatumCategoryCtl'
</pre>
