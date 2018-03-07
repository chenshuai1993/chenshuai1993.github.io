---
layout: post
title:  PHP设计模式-模板方法（Template Method）
key: 2018030703
tags:
  - php | 设计模式
lang: zh-Hans
date: 2018-03-07 10:03
---

## 简单理解

这是最常见的设计模式之一，其实质就是父类提供一系列模板方法，有的实现了逻辑，有的只是一个接口。而子类继承大部分共有方法，同时对接口方法进行不同的实现，从而完成对父类模板的个性化改造，起到一对多的解耦目的。

可以说PHP的抽象类就是为了实现这个设计模式而推出的功能，在PHP中，抽象类本身就是模板方法模式。

## 实现

重温一下抽象类
>Journey.php 模板类

```$xslt
abstract class Journey
{
    private $thingsToDo = [];

    //final关键字的作用是不让这个方法被子类覆盖
    final public function takeATrip()
    {
        $this->thingsToDo[] = $this->buyAFlight();
        $this->thingsToDo[] = $this->takePlane();
        $this->thingsToDo[] = $this->enjoyVacation();
        $buyGift = $this->buyGift();
        if ($buyGift !== null) {
            $this->thingsToDo[] = $buyGift;
        }
        $this->thingsToDo[] = $this->takePlane();
    }

    //子类必须实现的抽象方法
    abstract protected function enjoyVacation(): string;

    protected function buyGift()
    {
        return null;
    }

    private function buyAFlight(): string
    {
        return 'Buy a flight ticket';
    }

    private function takePlane(): string
    {
        return 'Taking the plane';
    }

    //把所有旅行中干过的事情列出来
    public function getThingsToDo(): array
    {
        return $this->thingsToDo;
    }

}
```

BeachJourney.php子类一

```$xslt
/**
 * Class BeachJourney
 * @海滩旅行类
 */
class BeachJourney extends Journey
{
    protected function enjoyVacation(): string
    {
        return "Swimming and sun-bathing";
    }
}
```

BeachJourney.php子类二

```$xslt
/**
 * Class CityJourney
 * @城市旅行类
 */
class CityJourney extends Journey
{
    protected function enjoyVacation(): string
    {
        return "Eat, drink, take photos and sleep";
    }
    //覆盖父类已有方法
    protected function buyGift(): string
    {
        return "Buy a gift";
    }
}
```

## 使用

```$xslt
$city = new CityJourney();
$city->takeATrip();
print_r($city->getThingsToDo());
```

## 总结
这是一个关于旅行流程的模板，先做了一个一般性的旅行流程模板，然后做了2个个性化旅行的子类。

这个方案就是所谓的模板方法，非常常见。

如果你还不能深刻理解抽象类，无法区分接口和抽象类的区别，这个例子再仔细看看，对加深理解有帮助。

***
原文链接[陈帅同学](http://imshuai.cn/php/135.html)

