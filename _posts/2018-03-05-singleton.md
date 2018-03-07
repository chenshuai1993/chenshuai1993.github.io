---
layout: post
title:  PHP设计模式-单例模式
key: 2018030511
tags:
  - php | 设计模式
lang: zh-Hans
---

## 最简单的设计模式

很容易理解，也很简单。


最常见的场景就是一个数据库的链接，我们每次请求只需要连接一次，也就是说如果我们用类来写的话，只需要用一个实例就够了（多了浪费）。


##实现

```$xslt
class Singleton {

    private static $conn;

    public function __construct()
    {
        self::$conn = mysqli_connect('localhost','root','xxx');
    }


    public static function getInstance()
    {
        if(!(self::$conn instanceof self)){
            self::$conn = new self();
        }

        return self::$conn;
    }


    //防止对象被复制
    public function __clone(){
        trigger_error('Clone is not allowed !');
    }

    //防止反序列化后创建对象
    private function __wakeup(){
        trigger_error('Unserialized is not allowed !');
    }
}
```
单例一般就是像这样用一个静态方法取得。


原文链接[陈帅同学](http://imshuai.cn/php.html)

