---
title: ES5核心技巧   
date: 2018-08-14 23:05:41
tags: ES5
categories: note
---
** {{ title }}：** <Excerpt in index | 首页摘要>
<!-- more -->
<The rest of contents | 余下全文>
# 变量提升   
```angular2html
(function(){
alert(a);  //function a(){}
var a =  1;//a的赋值要保存当前的词法作用域
function a(){
}
})()
```
等价于
```angular2html
(function(){
function a(){}
var a;//发现没有值就直接被忽略了
alert(a);//function a(){}
a =  1;
})()
```
# ES6里边的let作用域如何实现   
* try catch 实现
```angular2html
try{
    throw 1;
}catch(a){
    alert(a);
}
```
* with 实现（但是有问题）   
   *会延长作用域链
```angular2html
var s =  {
    a:1
}
with(s){
    b =  2;//lbs    没有b就会在全局创建一个b，只对已存在的属性赋值
}
alert(s.b)//undefined
```
# 回收问题   
```angular2html
functon test(){
    var a =  1;
    return function (){
    eval("");//加了这个a不会被回收，因为不确定会不会用到a
    //with,try catch也不会回收
    //不加这三个，a就会被回收
}
} 
test();
```
# this问题   
```angular2html
this.a =  20;
var s =  {
    a:30;
    init:function(){
    alert(a);
    }
} 
s.init();//30  s调用的
var s =  s.init;   
s();//20  
---------------------------------------------------------
this.a =  20;
var s =  {
    a:30,
    init:()=>{
    alert(this.a);//箭头函数改变了this的指向,总是指向词法作用域，也就是最外层的obj
    }
} 
s.init();//20
------------------------------------------------------------
this.a =  20;
var s =  {
    a:30,
    b:{
    a:30,
    fn:()=>{
    alert("fn"+this.a);
    }
},
init:()=>{
    alert(this.a);//箭头函数改变了this,找到父级最外边的作用域
    }
} 
s.init();//20
s.b.fn();//20
--------------------------------------------------------------
function test(){
    this.a =  20;
}
test.prototype.a =  30;
console.log((new test()).a)//20;原型链的优先级低于构造函数的优先级
---------------------------------------------------------
class test{
    a(){
    console.log(1)
    }
}
test.prototype.a =  function(){
console.log(2)
}
console.log((new test).a());//2   之所以会输出2是因为整个类的实现是基于js的原型链实现的
----------class是基于原型链实现的过程，上面的代码就等价与下面的代码---------
function test(){
}
test.prototype.a =  function(){
    console.log(1)
}
test.prototype.a =  function(){
    console.log(2)
}
console.log((new test).a());//2
 -----------------------------------------------------------------------
class基于原型链实现原理
class test{
    constructor(){
    this.a =  function(){
    console.log(0);
    };
}
a(){
    console.log(1)
}
}
test.prototype.a =  function(){
console.log(2)
}
console.log((new test).a());//0   整个类是基于es的原型链实现的

------------上面的代码等价于下面的代码------------

function test(){
    //test.constructor =  test;
    this.a =  function (){
    console.log(0)
    };
}

test.prototype.a =  function(){
    console.log(1)
}
test.prototype.a =  function(){
    console.log(2)
}
console.log((new test).a());//0
```
# 暂时性死区   
```angular2html
//还没定义就使用
var  i;
if(true){
    i  =  5;
    let i;
}
alert(i);/  报错，i is not defined
```
# 同步队列和异步队列，微任务和宏任务    
     js执行会生成一个同步队列和一个异步队列 异步队列包含（绑定的事件，ajax，延时），异步队列又分宏任务和微任务，浏览器加载的时候当同步队列的执行完毕之后会去异步队列取值 
# 按值传递和按引用传递
* 函数的参数如果是原始类型的值，传递方式是按值传递，这意味着在函数体内修改参数的值，不会影响到函数外部   
* 函数的参数如果是符合类型的值，传递方式是按引用传递，也就是传入函数的是原始值的地址，因此在函数内部修改参数，将会影响到原始值。
```angular2html
function   s(m){
    m.v =  5;//这个m和外边的m是不是一个m，只是指向同一个地址,可以这样写function s(a){a.v =  5},不会影响结果
    //如果被重写了
    m =  {k:1};    这样就会弹出undefined，因为指向了新的地址
}
var m =  {a:1}
s(m);
alert(m.v)//5
```
# 巧妙使用js写法
## 一句话写出0-100学生的等级
```angular2html
    var s=70;
    (function a(c,n){
        s>=c?console.log(n):a(c-10,++n);
    })(90,1);
```
## 用一句话遍历变量a,a='abc'
* 1.对ES6的理解，Array.from(a);   
* 2.对ES5的灵活运用，Array.prototype.slice(a);
* 3.a.split("");

# 原型链   
![image](https://raw.githubusercontent.com/WangGenzhen/learn/master/img/image.png)