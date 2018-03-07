---
layout: post
title:  PHP设计模式-工厂方法模式 ( Factory Method)
key: 2018030503
tags:
  - php | 设计模式
lang: zh-Hans
date: 2018-03-05 10:03
---

## 概念

工厂方法模式 和 简单工厂模式非常接近，唯一不同的是，允许有多个工厂存在，相当于给工厂分组。

## 规则
1. 每个工厂必须继承一个抽象类或接口类, 使之成为多态。
2. 每个产品也必须继承一个抽象类或接口类,也使之成为多态。
3. 每个工厂必须有一个工厂方法返回产品的实例。

## 实现

>FactoryMethod.php

```$xslt
/**
 * Interface CarFactory
 * @汽车工厂接口
 */
interface CarFactory
{
    public function makeCar();

}


/**
 * Interface Car
 * @小轿车接口
 */
interface Car
{
    public function getType();

}


/* 工厂和产品实现 */

/**
 * Class SedanFactory
 * 轿车工厂
 */
class SedanFactory implements CarFactory
{
    public function makeCar()
    {
        return new Sedan();
    }
}


/**
 * Class Sedan
 * 轿车类
 */
class Sedan implements Car
{
    public function getType()
    {
        return 'sedan';
    }
}
```


## 使用

```$xslt
/**
 * 实现用法
 */
$factory = new SedanFactory();

$sedan = $factory->makeCar();

print_r($sedan->getType());
```

>照着这个思路，你还可以 搞一个 SUVFactory(); 然后生产出SUV汽车。
 
 你可能会问，这样做的意义？如何运用？
 我建议你暂时还是记住定义，搞清楚不同模式的区别（比如说得出简单工厂 和 工厂方法的区别就可以了），到遇到具体场景你想起来再查找就明白了。

***

原文链接[陈帅同学](http://imshuai.cn/php/120.html)

