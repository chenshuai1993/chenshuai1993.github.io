---
layout: post
title:  PHP设计模式-空对象模式（Null Object）
key: 2018030803
tags:
  - php | 设计模式
lang: zh-Hans
date: 2018-03-08 10:03
---

## 这简直不能算一种设计模式

这个模式不是经典的Gof（四人帮）搞的设计模式，但是鉴于它的高频率使用，有价值把它归纳成为一种行为性设计模式。

## 看完这个例子秒懂

```$xslt
interface Animal {
    public function makeSound();
}
class Dog implements Animal {
    public function makeSound() { 
        echo "Woof.."; 
    }
}
class Cat implements Animal {
    public function makeSound() { 
        echo "Meowww.."; 
    }
}
//这个就是空对象，里面的方法啥也不做，它的存在就是为了避免报错
class NullAnimal implements Animal {
    public function makeSound() { 
        // silence...
    }
}
$animalType = 'elephant';
switch($animalType) {
    case 'dog':
        $animal = new Dog();
        break;
    case 'cat':
        $animal = new Cat();
        break;
    default:
        $animal = new NullAnimal();
        break;
}
$animal->makeSound(); // ..the null animal makes no sound
```

## 总结

我们看，如果没有一个空对象作为『Place Holder』放在default这里，那么这个程序就要报错了，为了避免报错，我们可能需要写if 不等于猫狗等等，但是这样很麻烦，万一有100个动物呢，搞一个『空对象』放在这里就好了。

***
原文链接[陈帅同学](http://imshuai.cn/php/139.html)

