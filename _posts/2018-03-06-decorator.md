---
layout: post
title:  PHP设计模式-装饰器模式（Decorator）
key: 2018030604
tags:
  - php | 设计模式
lang: zh-Hans
date: 2018-03-06 10:04
---

## 大概什么意思

一个类中有一个方法，我需要经常改它，而且会反反复复，改完了又改回去。
一般要么我们直接改原来的类中的方法，要么继承一下类覆盖这个方法。
有没有一个办法可以不用继承，只需要增加一个类进去，就可以改掉那个方法。
有，装饰器模式。

## 场景

```$xslt
class plainCoffee {
    public function makeCoffee(){
        $this->addCoffee();
    }
    public function addCoffee(){}
}
```

这是一个煮咖啡的程序，现在我还想加点糖，一般做法：

```$xslt
class sweetCoffee extends plainCoffee {
    public function makeCoffee(){
        $this->addCoffee();
        $this->addSugar();
    }
    public function addSugar(){}
}
```

好了，下面如果我还想加点奶，加点奶油，加点巧克力，加点海盐？会extends到崩溃。

## 装饰器

要想使用装饰器，需要对最早那个类进行改造：

```$xslt
class plainCoffee {
    public function makeCoffee(){
        $this->addCoffee();
    }
    public function addCoffee(){}
}
```

我们想改造makeCoffee()这个方法，无非是在它前面或后面加点逻辑，于是：

```$xslt
class plainCoffee {
    private function before(){}
    private function after(){}
    public function makeCoffee(){
        $this->before();
        $this->addCoffee();
        $this->after();
    }
    public function addCoffee(){}
}
```

那么我们怎么在before和after中加入逻辑呢：

```$xslt
class plainCoffee {
    private $decorators;
    public function addDecorator($decorator){
        $this->decorators[] = $decorator;
    }
    private function before(){
        foreach($this->decorators as $decorator){
            $decorator->before()
        }
    }
    private function after(){
        foreach($this->decorators as $decorator){
            $decorator->after()
        }
    }
    public function makeCoffee(){
        $this->before();
        $this->addCoffee();
        $this->after();
    }
    public function addCoffee(){}
}
```

改造好了，我们来看看怎么写装饰器：

```$xslt
class sweetCoffeeDecorator{
    public function before(){
    }
    public function after(){
        $this->addSugar();
    }
    public function addSugar(){}
}
```

由于我们这里这里的逻辑只会写在后面，所以before就留空了。

## 使用

```$xslt
$coffee = new plainCoffee();
$coffee->addDecorator(new sweetCoffeeDecorator());
$coffee->makeCoffee();
```

这样就得到了加糖的coffee，如果要加奶的，就再新建一个类似的修饰器：

```$xslt
$coffee = new plainCoffee();
$coffee->addDecorator(new sweetCoffeeDecorator());
$coffee->addDecorator(new milkCoffeeDecorator());
$coffee->makeCoffee();
```

## 总结

不难发现，在这里可以自由的新增或注释掉 不同的装饰器。
是不是很灵活？

当你extends用过后又遇到需要再次extends的情况时，不妨考虑一下装饰器模式。

***

原文链接[陈帅同学](http://imshuai.cn/php/128.html)

