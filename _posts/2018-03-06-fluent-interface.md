---
layout: post
title:  PHP设计模式-链式操作（fluent interface）
key: 2018030607
tags:
  - php | 设计模式
lang: zh-Hans
date: 2018-03-06 10:07
---

## 简单理解

fluent interface（流利接口）有一个更广为人知的名字『链式操作』，可能大多数人大概都是从Jquery最先熟悉的，在laravel中，ORM的一系列sql操作，也是链式操作，特点是每次都返回一个Query Builder对象。

## 实现

```$xslt
<?php
class Employee
{
    public $name;
    public $surName; 
    public $salary;
    public function setName($name)
    {
        $this->name = $name;
        return $this;
    }
    public function setSurname($surname)
    {
        $this->surName = $surname;
        return $this;
    }
    public function setSalary($salary)
    {
        $this->salary = $salary;
        return $this;
    }
    public function __toString()
    {
        $employeeInfo = 'Name: ' . $this->name . PHP_EOL;
        $employeeInfo .= 'Surname: ' . $this->surName . PHP_EOL;
        $employeeInfo .= 'Salary: ' . $this->salary . PHP_EOL;
        return $employeeInfo;
    }
}
//链式操作的效果
$employee = (new Employee())
                ->setName('Tom')
                ->setSurname('Smith')
                ->setSalary('100');
echo $employee;
# 输出结果
# Name: Tom
# Surname: Smith
# Salary: 100
```

## 总结

这里面能够连续链式操作的关键就在于 每个方法 都返回 return $this;

***

原文链接[陈帅同学](http://imshuai.cn/php/131.html)

