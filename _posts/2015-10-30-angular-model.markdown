---
layout:     post
title:      "AngularJS 问题解决"
subtitle:   "AngularJS ng-repeat下使用ng-model"
date:       2015-10-30 09:05:00
author:     "Franklee"
header-img: "img/post-bg-04.jpg"
---
举例：
<div style="display:table;background:#eee;border:1px solid #333;">
  blue:&lt;input type="radio" value="1" ng-model="selectValue"/&gt;
  red:&lt;input type="radio" value="2" ng-model="selectValue"/&gt;
  yellow: &lt;input type="radio" value="3" ng-model="selectValue"/&gt;
</div>
以上代码实现一个单选框功能，当你选中其中的一个单选框，可以从$scope.selectValue中得到你选中的的选项的value。
同时改变$scope.selectValue的值，也可以让界面上选中相应的单选框。

假设单选框的个数是不固定的，用ng-repeat来展现。
<div style="display:table;background:#eee;border:1px solid #333;">
  &lt;table&gt;
  &lt;tr ng-repeat="row in collections"&gt;
  &lt;td&gt;
  {{row.name}}: &lt;input type="radio" value="{{row.value}}" ng-model="selectValue"/&gt;
  &lt;/td&gt;
  &lt;/tr&gt;
  &lt;/table&gt;
</div>
当你书写了上述代码后。你会发现点击其中的对话框，$scope.selectValue中并没有保存你选中的对应单选框的值。

这是因为处在ng-repeat之间的代码，对全局的$scope里变量的内容是不可见的，像{{row.name}}里的row，并不是全局$scope里的成员。
而是为ng-repeat创建的子scope里面的。所以要引用全局$scope里的成员，你可以使用$parent 来引用全局的$scope

<div style="display:table;background:#eee;border:1px solid #333;">
&lt;table&gt;
&lt;tr ng-repeat="row in collections"&gt;
&lt;td&gt;
{{row.name}}: &lt;input type="radio" value="{{row.value}}" ng-model="$parent.selectValue"/&gt;
&lt;/td&gt;
&lt;/tr&gt;
&lt;/table&gt;
</div>
