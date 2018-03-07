---
layout: post
title:  PHP设计模式-组合模式（Composite）
key: 2018030603
tags:
  - php | 设计模式
lang: zh-Hans
date: 2018-03-06 10:03
---

## 大概什么意思

一个接口对于多个实现，并且这些实现中都拥有相同的方法（名）。
有时候你需要只运行一个方法，就让不同实现类的某个方法或某个逻辑全部执行一遍。在批量处理多个实现类时，感觉就像在使用一个类一样。

## 先来看如何使用

```$xslt
//先建立一个表单
$form = new Form();
//在表单中增加一个Email元素
$form->addElement(new TextElement('Email:'));
$form->addElement(new InputElement());
//在表单中增加一个密码元素
$form->addElement(new TextElement('Password:'));
$form->addElement(new InputElement());
//把表单渲染出来
$form->render();
```

这个例子形象的介绍了组合模式，表单的元素可以动态增加，但是只要渲染一次，就可以把整个表单渲染出来。

## 实现

顶层渲染接口 RenderableInterface.php

```$xslt
interface RenderableInterface
{
    public function render(): string;
}
```

表单构造器 Form.php

```$xslt
//必须继承顶层渲染接口
class Form implements RenderableInterface
{
    private $elements;
    //这里很关键，相当于是批量处理接口实现类
    public function render(): string
    {
        $formCode = '<form>';
        foreach ($this->elements as $element) {
            $formCode .= $element->render();
        }
        $formCode .= '</form>';
        return $formCode;
    }
    //这个方法用来注册 接口实现类
    public function addElement(RenderableInterface $element)
    {
        $this->elements[] = $element;
    }
}
```

具体实现类一 TextElement.php

```$xslt
class TextElement implements RenderableInterface
{
    private $text;
    public function __construct(string $text)
    {
        $this->text = $text;
    }
    public function render(): string
    {
        return $this->text;
    }
}
```

具体实现类二 InputElement.php

```$xslt
class InputElement implements RenderableInterface
{
    public function render(): string
    {
        return '<input type="text" />';
    }
}
```

你还可以定义更多的元素，来构建表单。

## 输出结果

当你执行$form->render()后，输出如下:
```$xslt
<form>Email:<input type="text" />Password:<input type="text" /></form>
```

## 总结

不知道你有没有写过表单构造器（Form Builder），没写过应该也见过或使用过，如果你想实现一个类似的功能，可以参考PHP设计模式之组合模式（Composite）。


***

原文链接[陈帅同学](http://imshuai.cn/php/127.html)

