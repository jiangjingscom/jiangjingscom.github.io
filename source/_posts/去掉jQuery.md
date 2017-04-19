---
title: 去掉jQuery
date: 2017-04-01 15:59:49
categories: Font-end
tags: JavaScript
---
一直在Angular中混用jQuery，用来操作DOM，因为它(wo)很(tai)方(nian)便(qing)，最近将jQuery从项目中移除了，将所有涉及到jQuery操作换为用angular提供的api或原生js来实现......虽然jQuery压缩过才100kb左右。
1.Angular内置jqLite，封装了部分操作DOM的api，使用angular.element(ele)相当于jQuery中的$,具体使用见官方[文档](https://code.angularjs.org/1.5.4/docs/api/ng/function/angular.element)。
因此我们所使用的jQuery选择器，例如：
``` javascript
$("#empl-name").val();
``` 
可以换成：
``` javascript
angular.element(document.querySelector("#empl-name"));
``` 
使用document.querySelector 的原因官方文档中有说明:

<img src="../../../../assets/img/4-1-1.png"   align=center />

<!--more-->

2.替换掉使用jQuery的插件。
例如，使用的jQuery的tooltip组件插件，现在换成ui bootstrap中的tooltip组件。
原来引入jQuery tooltip插件代码后，封装成指令：
``` javascript
"use strict";
var moduleName="kass.widget.tooltip";
angular.module(moduleName,[])
.directive("tooltip",[function(){
	return{
		restrict : "A",
		link : function(scope, element, attrs){//element:指令标签对象     attrs:指令属性
			var opts ={
				//将带有tooltip属性的"Target element"传给插件创建tooltip
				formatter: function(scope){
					return scope.$target.attr("tooltip");//$target是利用$(this)封装的jquery对象
				},
				//插件提供的tooltip属性值direction与指令属性tooltipDirection
				direction : attrs.tooltipDirection ? attrs.tooltipDirection : "bottom"
			};
			//插件提供的tipper方法设置tooltip属性值
			element.tipper(opts);
			element.one("$destroy",function(){
				element.tipper("destroy");
			});
		}
	}
}]);
module.exports=moduleName;
``` 
引入ui bootstrap 中的tooptip部分：

``` javascript
require("./position");//ui bootstrap的position部分
require("./stackedMap");//ui bootstrap的stackedMap部分
angular.module('ui.bootstrap.tooltip', ['ui.bootstrap.position', 'ui.bootstrap.stackedMap',"uib/template/tooltip/tooltip-html-popup.html","uib/template/tooltip/tooltip-popup.html","uib/template/tooltip/tooltip-template-popup.html"])     
.provider('$uibTooltip', function(){........})
......
.directive('uibTooltipTemplatePopup', function() {
  return {
    restrict: 'A',
    scope: { contentExp: '&', originScope: '&' },
    templateUrl: 'uib/template/tooltip/tooltip-template-popup.html'
  };
});
......
angular.module("uib/template/tooltip/tooltip-popup.html", []).run(......);
......
angular.module('ui.bootstrap.tooltip').run(......);
``` 
发现github上的一个封装angular tooltip的[项目](https://github.com/720kb/angular-tooltips),使用方便有demo,⊙o⊙......

3.jQuery的动画部分。
一些动画可以直接使用css的animation、transition属性进行设置，还可以使用angular提供的动画。
例如jQuery一个简单的改变位置动画，需要在动画完成后移除这个指令组件：

``` javascript
$(".side-modal").animate({right:'-100%'},300,function(){
	$scope.$apply(function(){
		$scope.onRemove();
	});
});
``` 
使用angular的动画，要引入angular-animate.js文件，注入ngAnimate：

``` javascript
"use strict";
require("./angular-animte-min");
var moduleName = "animateTest";
angular.module(moduleName,["ngAnimate"])
.directive(myTest,["$animate",function("$animate"){
    return{
        restrict : "AE",
        replace : false,
        scope : {
            onRemove : "&"
        },
        template : require("html!../template/to/mytest.html"),
        controller : ["$scope",function($scope){
            ......
            var slideBox = angular.element(document.querySelector(".side-modal"));
            $animate.addClass(slideBox,'set-right',function(){
                $scope.onRemove();//callback,动画完成后移除指令
            });
            ......
        }],
        link:function(scope,element,attrs){......}
    }
}]);
module.exports=moduleName;
``` 
4.其他部分可以参考github上的一个项目：[You Don't Need jQuery]( https://github.com/oneuijs/You-Dont-Need-jQuery)。


************

<font color=red>April Fool's Day </font>....⊙o⊙....