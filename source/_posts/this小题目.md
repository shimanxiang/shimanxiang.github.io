---
title: JavaScript中有关this的几个小题目
layout: page
tags: this
date: 2015-06-22 14:03:07
---

请看以下代码，alert出来的值分别是什么(答案按alert先后顺序写的)

``` bash
var a = 10;  
function test() {  
    a = 100;  
    alert(a);  
    alert(this.a);  
    var a;  
    alert(a);  
}  
test();  
```
正确答案：100,10,100；
说明：this.a中的this指的是window对象。

``` bash
function showName() { 
    alert(this.name); 
} 
var obj = { 
    id:1,
    name:"jacky",
    showName:showName 
  }; 
obj.showName(); 
```
正确答案：jacky
说明：this关键字指向函数(方法)的所有者,即obj对象。

<!-- more -->

``` bash
var fullname ='John Doe';
    var obj ={
        fullname:'Colin Ihrig',
        prop:{
        fullname:'Aurelio De Rosa',
        getFullname:function(){
             return this.fullname;
         }
        }
    };
    alert(obj.prop.getFullname());
    var test = obj.prop.getFullname;
    alert(test());
```
正确答案： Aurelio De Rosa ，John Doe。
说明：第一个alert出来的this指的是obj对象，而test函数等价于

``` bash
var test=function(){
return this.fullname;
}
```

此时this指的是全局对象

``` bash
var myObject= {
    num: 2,
    add: function(){
            this.num=3;
            (function(){
            alert(this.num);//undefined
            this.num=4;
            })();
            alert(this.num)//3
        }
}
myObject.add();
```
正确答案：undefined , 3

``` bash
var a = 'global';
var obj = {
    a : 'local',
    test : function(){
            function test1(){
                alert(this.a);//global
            }
            alert(this.a);//local
            test1();
    }
};
obj.test();
```
正确答案：local ，global

**说明：此题与上题属于一个类型。《JS权威指南》里明确指明了当一个函数作为函数而不是方法调用时这个this关键字引用全局对象。特别指明了，当一个嵌套的函数在一个包含函数中调用，而这个包含的函数是作为方法调用时，这也是成立的：this在包含的函数中有一个值，但是它却不太直观地引用嵌套的函数体的内部的全局对象。**