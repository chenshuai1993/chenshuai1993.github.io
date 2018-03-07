---
layout: post
title:  PHP设计模式-简单工厂模式
key: 2018030503
tags:
  - php | 设计模式
lang: zh-Hans
---

## 介绍

这也是一个很基本，很简单，很常用的设计模式。

本来我们要获取一个类的实例，需要用到new关键字。但是如果new 直接写到业务代码里，一个类在很多地方都实例化过，以后要是这个类出了什么问题，比如要改个名字(实际中，你更多的可能是修改构造函数方法)，那么就尴尬了，需要改很多地方。

工厂模式，顾名思义，就是不用new来获得实例，而是把业务类放进一个工场类里，由工厂（类）『生产』出对应的实例。


## 实现

>SimpleFactory.php
```$xslt
/**
 * Class SimpleFactory
 * @简单工厂类
 */
class SimpleFactory
{
    /**
     * @return Bicycle
     * 工厂模式创建自行车类
     */
    public function createBicycle() : Bicycle
    {
        return new Bicycle();
    }

    /**
     * @return Plane
     * 工厂模式创建飞机类
     */
    public function createPlane() : Plane
    {
        return new Plane();
    }
}
```


>Bicycle.php
```$xslt
/**
 * Class Bicycle
 * @自行车类
 */
class Bicycle
{
    public function driveTo(string $destination)
    {
        echo $destination;
    }
}
```


>Plane.php
```$xslt
/**
 * Class Plane
 * @飞机类
 */
class Plane
{
    public function flyTo(string $destination)
    {
        echo $destination;
    }
}
```

## 使用
```$xslt
$factory = new SimpleFactory();

$bicycle = $factory->createBicycle(new Bicycle());

print_r($bicycle->driveTo('我要骑车'));


$plane = $factory->createPlane(new Plane());

print_r($plane->flyTo('我要开飞机'));
```

***

原文链接[陈帅同学](http://imshuai.cn/php/119.html)

