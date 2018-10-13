---
title: markdown语法学习
date: 2018-08-23 10:05:41
tags: markdown
categories: note
---
** {{ title }}：** <Excerpt in index | 首页摘要>
<!-- more -->
<The rest of contents | 余下全文>
# 1.标题
```
# h1
## h2
### h3
#### h4
##### h5
###### h6  
```
> 注：#后面保持空格
# 2.目录生成asdfsdkjlkjffdkfjddsdsdfsf

```
[TOC]
```
>注：根据标题生成目录，兼容性一般
# 3.引用,,..,,,,
```
    >hello
```
# 4.引用嵌套
```
    >a
    >>b
    >>>c
```
> 演示：

> a
>>b
>>>c
# 5.行内标记
```
    `hello`
```
>演示：

标记之外`被标记内容`标记之外
# 6.代码块
    ```
    <div>
        <p></p>
    </div>
    ```
>也可用tab缩进或敲4个空格
# 7.代码块自定义颜色
```javascript
    var color = 'red';
```
# 8.插入链接
>内联式   
```
    [龙心且行](longxin.site)
```
[龙心且行](http://longxin.site)
>引用式
```
    [龙心且行][blog]
    [blog]:longxin.site   
```
[龙心且行][blog]   

[blog]:http://longxin.site 
# 9.插入图片
```
    ![image](图片地址)
```
![](http://item.dsconsulting.com/meituan_university/images/title.png)

# 10.插入图片带有链接
```
    [![image](图片地址)](链接地址)
```
# 11.序表
## 有序
```
    1. one
    2. two
    3. three
```
1. one
2. two
3. three
>注：点后保持空格
## 无序
```
    * one
    * two
```
* one
* two
## 序表嵌套
```
    1. one
        1.one
        2.two
    2.two
        * one
        *two
```
1. one
    1.one1
    2.one2
2. two
    * two1
    * two2
## 序表嵌套代码块
```
    * one

        var a = 1;//与上行保持空行，并加两个tab
```
* one   

        var a =1
# 12.表格
```
    |a|b|c|
    |:-:|:-|-:|
    |内容|内容二|内容三|
```
|a|b|c|
|:-:|:-|-:|
|居中|左对齐|右对齐|
|内容|内容二|内容三|

>注：：代表对齐方式，：与|之间不要有空格，否则对齐会有些不兼容
# 13.语义标记
```
    斜体 *斜体* 或 _斜体_
    加粗 **加粗**
    斜体加粗 ***斜体加粗***   或  **_加粗斜体_**
    删除线  ~~删除线~~
```
斜体 *斜体* 或 _斜体_   
加粗 **加粗**   
斜体加粗 ***斜体加粗***   或  **_加粗斜体_**   
删除线  ~~删除线~~
# 14.语义标签
```
    斜体 <i>斜体<i>
    加粗 <b>加粗</b>
    强调 <em>强调</em>
    上标 z<sup>a</sup>
    下标 z<sub>a</sub>
    键盘文本 <kbd>ctrl</kbd>
```
斜体 <i>斜体<i>   
加粗 <b>加粗</b>   
强调 <em>强调</em>   
上标 z<sup>a</sup>   
下标 z<sub>a</sub>   
键盘文本 <kbd>ctrl</kbd>   
# 15.格式化文本
```
    写法一
    <pre>
        hello
            word
        hello
    </pre>    
      
    写法二
    ```
        hello
            word
    ```
```
# 16.公式
```
    $ x \href{why-equal.html}{=} y^2 + 1 $
```
# 17.分隔符
```
    ***
    ---
```
***
---
>注：最少三个***或三个---
# 18.脚注
```
    Markdown[^1]      
    [^1]: markdown是一种纯文本标记语言
```
# 19.自动邮箱链接
```
    <1280210282@qq.com>
```
# 20.流程图
```flow                     // 流程
st=>start: 开始|past:> http://www.baidu.com // 开始
e=>end: 结束              // 结束
c1=>condition: 条件1:>http://www.baidu.com[_parent]   // 判断条件
c2=>condition: 条件2      // 判断条件
c3=>condition: 条件3      // 判断条件
io=>inputoutput: 输出     // 输出
//----------------以上为定义参数-------------------------

//----------------以下为连接参数-------------------------
// 开始->判断条件1为no->判断条件2为no->判断条件3为no->输出->结束
st->c1(yes,right)->c2(yes,right)->c3(yes,right)->io->e
c1(no)->e                   // 条件1不满足->结束
c2(no)->e                   // 条件2不满足->结束
c3(no)->e                   // 条件3不满足->结束
```
# 21.参考地址
[Markdown语法整理大集合](https://www.jianshu.com/p/b03a8d7b1719)

