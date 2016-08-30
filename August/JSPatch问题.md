# JSPatch问题

[TOC]

[PlaygroundTool](http://awhisper.github.io/2016/08/07/JPPlayground/)

## JSPatch总结

关于JSPatch的学习，是先知道有这样一个东西，然后花几天的时间开始简单的应用，之后才是去了解这是什么，怎么做到的，最后再完善之前写的demo，总之还是不够深入。

## 注意getter方法为isXXX的属性

如UISwtich中有一个属性为on，getter方法为isOn，在JSPatch中调用其get、set方法时应使用isOn()与setOn()

## JS与OC中字符串问题

在js里通过方法参数传入的字符串是js字符串而非OC，所以不能调用OC的任何方法，如果需要使用OC的方法，可以var一个变量来接收这个字符串，再当参数传入，这时就是OC的字符串了。

> var url = NSURL.urlWithString('http://www.baidu.com'); JS字符串
> var str = 'http://www.baidu.com';
> var = url = NSURL.urlWithString(str); OC字符串

## JS中function传值

js中可以通过function传值来实现OC中block的功能。

## NaN

用isNaN(xxx)来判断数值是否非法


## BridgeWebView performSelectorInOC

## EventTrack

## 真机

## 秒装

## RACObserve




