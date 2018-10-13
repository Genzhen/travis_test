---
title: jQuery技术内幕
date: 2018-08-19 23:05:41
tags: jQuery
categories: note
---
** {{ title }}：** <Excerpt in index | 首页摘要>
<!-- more -->
<The rest of contents | 余下全文>
# jQuery的无new操作
```angular2html
  (function (window, undefined) {
        /*
         undefined = 42;
         alert(undefined);//42 在函数内部就是变量，可以被赋值，在外部就是关键字，不可以不赋值
         */

        /*
         //new的话，s就能访问jq原型链的所有方法
         //不new的话，q也就能访问jq原型链的所有方法->value
        var s = new $('.test');
        var q = $('.test');
        //为什么呢？
        //new jQuery ->1构造函数 2prototype的方法
        //new 第一步 返回一个init函数 原型链上挂载了一个init函数 没有主动的执行
        //init没调用 被搁置了
        //构造函数内部的 return new生效了
        //jQuery.prototype
        */

        var jQuery = function(selector,context){
            //默默的做了一个new, 实际上就是new的原型jQuery.prototype
            return new jQuery.fn.init(selector,context);
        };
        jQuery.fn = jQuery.prototype = {
            init:function(selector,context){

            }
        };
        jQuery.fn.init.prototype = jQuery.fn;
    })(window);
      //为什么绕一圈来new自己，而不是直接new自己？
       为了得到jQuery原型链上的方法，暴露出去一个fn，fn上拥有jQuery原型链上所有的方法


    //$.fn
    //jQuery.fn.extend，给jQuery对象添加方法。直接挂载到了jQuery原型链上
    //jQuery.extend，为扩展jQuery类本身.为类添加新的方法。 直接挂载到了jQuery对象身上
    /*
    //一般写插件
    jQuery.fn.extend({
        a:function(){
            console.log(123);
        }
    });
    $('').a();
    */

    /*
    //为jQuery类添加类方法，可以理解为添加静态方法。
    jQuery.extend({
        a:function(){
            console.log(13);
        }
    });
    $.a;//13
    */

    /*
     undefined = 42;
     alert(undefined);//undefined 在外边是关键字
     */
```
# jQuery的链式调用如何实现的
```angular2html
 /*
    //链式调用如何实现的
    var s = {
        a:function(){
            console.log('first')
        },
        b:function(){
            console.log('second')
        },
        c:function(){
            console.log('three')
        }
    };
    s.a().b().c();//first 报错
    var s = {
        a:function(){
            console.log('first');
            return this//把指针return回去
        },
        b:function(){
            console.log('second');
            return this
        },
        c:function(){
            console.log('three');
            return this
        }
    };
    s.a().b().c();//first second three
    */
```
# 实现函数的重载
```angular2html
//jQuery的一个用法，$()，括号中可以接受不同的值，实际上就引出了一个概念，函数的重载
$('.test').val();//取值 赋值
    //$('.test',"td")
    //$(['.test',"td"])
    //$(function(){})
    //$()->函数 函数的重载
函数的重载：同一个函数接受不同的参数处理不同的事情    
如何实现：
  思路一：通过arguments.length进行判断，因为参数不知道会有多少个，每个都需要判断，穷举法不行
  思路二：利用闭包可以保存变量的特性,

         // old undefined obj.find->find0
         // old find0    obj.find->find1
         // old find1    obj.find->find2

    function addMethod(obj,name,f){
  console.log(f);//find0() find1() 
			console.log(f.length);//0//1//函数的参数长度
			console.log(arguments.length);//3//3
        var old = obj[name];//第一次undefined
        obj[name] = function(){
//如果f的形参等于实参，则说明找到了合适的处理
           if(f.length === arguments.length){
               //this等于obj
               return f.apply(this,arguments)
           }else{
//如果f的形参不等于实参，则说明没有找到合适的处理，则会继续找
               return old.apply(this,arguments)
           }
        }
    }
    var people = {
        name:['张三','李四','王','高']
    };
    var find0 = function(){
        return this.name;
    };
    var find1 = function(name){
        var arr = this.name;
      for(var i = 0;i<=arr.length;i++ ){
          if(arr[i] == name){
              return arr[i]+"在"+i+"位";
          }
      }
    };
var find2 =  function(name,age){\
console.log("naemage")
}
    addMethod(people,'find',find0);
    addMethod(people,'find',find1);
    addMethod(people,'find',find2);
    // console.log(people.find());//(3) ["张三", "李四", "王五"]
     console.log(people.find("李四"));//李四在1位
     console.log(people.find("",""));//
```
# 短路表达式
```angular2html
 /*  var a ;
 if(a){
    foo = a;
  }else {
    foo = b;
   }
 var foo = a||b;//第一个为真就输出第一个，第一个为假第二个为真就输出第二个，第一个为假第二个为假就输出第二个
 var foo = a&&b;//第一个为真，第二个为真输出第二个，第一个为假输出第一个*/                             

执行顺序是从右往左
示例：
var a=1,b=2,c=0;
a&&b&&c  //0
实际执行顺序是：a&&(b&&c)
```
# 钩子机制 
```
  var data = {
    index1:1,
    index2:2
   }
  var s = "index1";
  data[s]&&data[s]();
  //如果有就执行函数
  //之前的话需要用if,else进行判断
```
# jQuery ready的实现
```
jQuery的ready方法实现了当页面加载完成后才执行的效果，但它并不是window.onload或者document.onload的封装，而是使用标准w3c浏览器dom隐藏api和ie浏览器缺陷来完成的。代码如下：
DOMContentLoaded =  function(){
    if(document.addEventListener){
        //取消事件监听，执行ready方法
        document.removeEventListener("DOMContentLoaded",DOMContentLoaded,false);
        jQuery.ready();
    }else  if(document.readyState ===  "complete"){
        //取消事件监听，执行ready方法
        document.detachEvent("onreadystatechange",DOMContentLoaded);
        jQuery.ready();
    }
}
jQuery.ready.promise =  function(obj){
    readyList =  jQuery.deferred();
}
```

