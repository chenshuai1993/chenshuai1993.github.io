---
layout: post
title:  PHP设计模式-对象池模式（Objects Pool）
key: 2018030505
tags:
  - php | 设计模式
lang: zh-Hans
date: 2018-03-05 10:05
---

## 场景

正常的情况下，一个对象随着请求产生，也会随着请求结束被销毁。
有一些对象，需要依赖外部资源，比如说mysql数据库的连接，socket的连接，memcached的连接，以及一些大对象，比如说图片，字体对象等，每次创建的时候耗时比较长，极大的影响系统性能，而且上述这些案例的影响是全局的。
那我们有没有一种可以自动管理实例创建，如果有实例就不在重复创建呢？

听上去很耳熟，这不就是单例模式吗？
是的，单例模式可以解决这个问题，这里要介绍单例模式的一种升级模式：对象池模式；

## 实现

>ObjectsPool.php

```$xslt
class ObjectPool
{
    private $instances = [];

    public function get($key)
    {
        if(isset($this->instances[$key])){
            return $this->instances[$key];
        }else{
            $item = $this->make($key);
            $this->instances[$key]=$item;
            return $item;
        }
    }

    public function add($object, $key)
    {
        $this->instances[$key] = $object;
    }

    public function make($key){
        if($key =='mysql'){
            return 'mysql';new Mysql();
        }elseif($key =='socket'){
            return new Socket();
        }
    }
}

class ReusableObject
{
    public function doSomething()
    {
        // ...
    }
}
```

## 使用
```$xslt
$obj = new ObjectPool();

$mysql = $obj->get('mysql');

print_r($mysql);
```

>如果对象池里有mysql的对象实例，就拿出来，如果没有就新建一个，这样无论怎样mysql的实例只会被创建一次，并且会保存在内存中，以便复用。

>与单例模式的区别就是，相当于一个对象池 管理多个单例。

***

原文链接[陈帅同学](http://imshuai.cn/php/121.html)

