---
layout: post
category: notes
title:  "初学scala常见问题汇总"
tags: [scala]
---
## scala object和class的区别  
scala中没有static关键字，object 中的东西都是java中的static，object是包含static方法的singleton
object伴生对象本身就是一个Singleton，不同的是，它有一个与之同名的类（这里的class Companion），二者可以相互访问彼此的私有成员。两者必须定义在同一文件中
<!-- more -->
## implicit关键字的作用
Scala提供的隐式转换特性可以在效果上给一个类增加一些方法，或者用于接收不同类型的对象.  
Scala 可以用 implicit class 来声明类，并且它的主构造器 (Primary Constructor) 只有一个参数时，就可以用来把参数隐式转换成该类型。  
[Scala 2.10.0 新特性之使用隐式类进行类型隐式转换](http://unmi.cc/scala-2-10-0-new-features-implicit-class/)  

implicit关键字定义一个类型在需要时，如何自动转换成另外一种类型。   
只有哪些使用implicit关键字的定义才是可以使用的隐式定义。关键字implicit用来标记一个隐式定义。编译器才可以选择它作为隐式变化的候选项。你可以使用implicit来标记任意变量，函数或是对象。   
编译器在选择备选implicit定义时，只会选取当前作用域的定义  
[Scala 专题教程-隐式变换和隐式参数(2):使用implicits的一些规则](http://www.tuicool.com/articles/fuy6Jz)  
以下下代码定义了如何将元组转化为NameValuePair  
implicit def tupleToNameValuePair(pair : (String, String)) : NameValuePair = new BasicNameValuePair(pair._1, pair._2)
## scala val和var
val不可改变变量内容，而var是可以为变量重新赋值的,scala不建议过多使用var  
[理解Scala的函数式风格：从var到val的转变](http://developer.51cto.com/art/200907/134956.htm)
