# JSPatch问题

[TOC]

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


## bridgewebview performselectorinoc

## eventtrack

## 真机

## 秒装

