---
layout: post
title:  PHP设计模式-依赖注入（Dependency Injection）
key: 2018030605
tags:
  - php | 设计模式
lang: zh-Hans
date: 2018-03-06 10:05
---

## 简单理解

终于要讲到这个著名的设计原则，其实它比其他设计模式都简单。
依赖注入的实质就是把一个类不可能更换的部分 和 可更换的部分 分离开来，通过注入的方式来使用，从而达到解耦的目的。

## 场景

一个数据库连接类

```$xslt
class Mysql{
    private $host;
    private $port;
    private $username;
    private $password;
    private $db_name;
    public function __construct(){
        $this->host = '127.0.0.1';
        $this->port = 22;
        $this->username = 'root';
        $this->password = '';
        $this->db_name = 'my_db';
    }
    public function connect(){
        return mysqli_connect($this->host,$this->username ,$this->password,$this->db_name,$this->port); 
    }
}
```

使用

```$xslt
$db = new Mysql();
$con = $db->connect();
```

通常应该设计为单例，这里就先不搞复杂了。

## 依赖注入

显然，数据库的配置是可以更换的部分，因此我们需要把它拎出来

```$xslt
class MysqlConfiguration
{
    private $host;
    private $port;
    private $username;
    private $password;
    private $db_name;
    public function __construct(string $host, int $port, string $username, string $password,string $db_name)
    {
        $this->host = $host;
        $this->port = $port;
        $this->username = $username;
        $this->password = $password;
        $this->db_name = $db_name;
    }
    public function getHost(): string
    {
        return $this->host;
    }
    public function getPort(): int
    {
        return $this->port;
    }
    public function getUsername(): string
    {
        return $this->username;
    }
    public function getPassword(): string
    {
        return $this->password;
    }
    public function getDbName(): string
    {
        return $this->db_name;
    }
}
```

然后不可替换的部分这样：

```$xslt
class Mysql
{
    private $configuration;
    public function __construct(MysqlConfiguration $config)
    {
        $this->configuration = $config;
    }
    public function connect(){
        return mysqli_connect($this->configuration->getHost(),$this->configuration->getUsername() ,$this->configuration->getPassword,$this->configuration->getDbName(),$this->configuration->getPort()); 
    }
}
```

这样就完成了配置文件和连接逻辑的分离。


## 使用

```$xslt
$config = new MysqlConfiguration('127.0.0.1','root','','my_db',22);
$db = new Mysql($config);
$con = $db->connect();
```

## 总结
$config是注入Mysql的，这就是所谓的依赖注入。


***

原文链接[陈帅同学](http://imshuai.cn/php/129.html)

