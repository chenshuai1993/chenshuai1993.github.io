---
layout: post
title:  PHP设计模式-抽象工厂模式（Abstract Factory）
key: 2018030504
tags:
  - php | 设计模式
lang: zh-Hans
date: 2018-03-05 10:04
---

## 区别

抽象工厂模式与工厂方法模式在某种程度上是一样的，区别在于子工厂必须全部继承或实现自同一个抽象类或接口。

## 规则
1. 每个工厂必须继承同一个抽象类或实现同一个接口。
2. 每个工厂必须包含多个工厂方法。
3. 每个工厂的方法必须一致，每个方法发返回的实例，必须是继承或实现了同一个抽象类或接口的多态类。

## 实现

>AbstractFactory.php

```$xslt
abstract class Button{}

abstract class Border{}

class MacButton extends Button{}

class WinButton extends Button{}

class MacBorder extends Border{}

class WinBorder extends Border{}

/**
 * Interface AbstractFactory
 * @抽象类
 */
interface AbstractFactory {
    public function CreateButton();
    public function CreateBorder();
}

/**
 * Class MacFactory
 * 子类
 */
class MacFactory implements AbstractFactory{
    public function CreateButton(){ return new MacButton(); }
    public function CreateBorder(){ return new MacBorder(); }
}

/**
 * Class MacFactory
 * 子类
 */
class WinFactory implements AbstractFactory{
    public function CreateButton(){ return new WinButton(); }
    public function CreateBorder(){ return new WinBorder(); }
}
```

>抽象工厂一般使用接口，特点是层层约束的，缺点是增加产品比较困难，比如再加个CreateLine()，接口和实现都得改。优点是增加固定类型产品的不同品牌比较方便，比如我要加一个Linux的品牌，那么再建一LinuxFactory就可以了。
 
>你想做统一标准的时候，比如写了一个框架，数据库操作定义了一套接口，你自己写了一个Mysql的实现，那么其他人参与开发，比如另一个人写了一个Oracle的实现，那么这种标准的价值就体现出来了，它会让你的代码非常一致，不会被别人写乱。

## 说明

>此模式其实比较难理解，还是建议先记住区别，等遇到真实场景就知道怎么运用了，如果你到网上去求证，80%都是错误的解释。

***

原文链接[陈帅同学](http://imshuai.cn/php/121.html)

