---
layout: post
title:  PHP设计模式-原型模式（Prototype Pattern）
key: 2018030505
tags:
  - php | 设计模式
lang: zh-Hans
date: 2018-03-05 10:06
---

## 实质就是 对象的复制

对一些大型对象，每次去new，初始化开销很大，这个时候我们 先new 一个模版对象，然后其他实例都去clone这个模版， 这样可以节约不少性能。
这个所谓的模版，就是原型（Prototype）；
当然，原型模式比单纯的Clone要稍微升级一下。

## 普通clone

new 和 clone都是用来创建对象的方法。
在PHP中， 对象间的赋值操作实际上是引用操作 （事实上，绝大部分的编程语言都是如此! 主要原因是内存及性能的问题) ， 比如 :

```$xslt
class myclass {
public $data;
}
$obj1 = new myclass();
$obj1->data = "aaa"；
$obj2 = $obj1;
$obj2->data ="bbb";     //$obj1->data的值也会变成"bbb"
```

但是如果你不是直接引用，而是clone，那么相当于做了一个独立的副本：

```$xslt
$obj2 = clone $obj1;
$obj2->data ="bbb";     //$obj1->data的值还是"aaa"，不会关联
```

这样就得到一个和被复制对象完全没有纠葛的新对象，但两个对象长得是一模一样的。

## 浅复制和深复制

如果你以为你已经把它俩彻底分开了，你错了，没那么容易, 我们再看一个复杂点例子。
继续接着上面的例子看；

```$xslt
$item = new itemObject(
    public $count = 0;
    public function add(){
        $this->count = ++$this->count;
    }
);
$obj1->item = $item;
$obj2 = clone $obj1;
```
到目前为止，2边还是一样的。
```
$obj2->data ="bbb" //$obj1->data的值还是"aaa"，不会关联
```
但是运行这个代码
```$xslt
$obj2->item->add()
```
然后你打印一下这3个对象：
```$xslt
$item //$item->count = 1;
$obj1  //'aaa', $obj1->item->count = 1;
$obj2  //'bbb', $obj2->item->count = 1;
```

$obj2 改变了一个引用对象（$item）的属性，结果所有引用这个对象的对象，包括这个对象自己的属性都被改变了。

这就是所谓的 浅复制。

我们既然要Clone，目的就是要把 两个对象 完全分离开。

所以我们来聊一下 *深复制* 的方法：
非常简单，在被复制对象中加一个魔术方法就可以了。

```$xslt
class myclass {
    public $data;
    public $item;
     public function __clone() {
           $this->item = clone $this->item;
    }
}
```
这技巧就是，被复制对象一旦被复制的时候，就放弃自己的属性，把属性给要求复制的对象，然后自己存一个属性的副本；

这样我们复制这个对象的时候，后续对象就不会因为引用的关系而改变源对象了。


## 原型模式

现在来正式讲 原型模式
```$xslt
<?php
interface Prototype { public function copy(); }
class ConcretePrototype implements Prototype{
    private  $_name;
    public function __construct($name) { $this->_name = $name; } 
    public function copy() { return clone $this;}
}
class Demo {}
// client
$demo = new Demo();
$object1 = new ConcretePrototype($demo);
$object2 = $object1->copy();
?>
```

从上面的例子不难看出，所谓原型模式就是不直接用 clone这种关键字写法，而是创建一个原型类。

把需要被复制的对象丢进 原型类里面，然后这个类就具有了 复制自己的能力（方法），并且可以继承原型的一些公共的属性和方法。

如果你用过http://carbon.nesbot.com/ 这个处理时间的包，它里面的copy()方法就采用了这个模式。

但是上述案例不能解决浅复制的问题。

原型模式的来龙去脉就是这个回事，怎么合理使用可以遇到问题的时候回来查看例子。

***

原文链接[陈帅同学](http://imshuai.cn/php/124.html)

