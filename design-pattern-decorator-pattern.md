---
title: decorator pattern
toc: true
categories:
  - coding
tags:
  - design pattern
date: 2016-04-18 17:43:59
description: detial of decorator pattern
---
装饰模式(Decorator Pattern)
===

> **装饰模式** 动态的将责任附加到对象上。

装饰模式是在不必改变原类文件和使用继承的情况下，动态的扩展一个对象的功能。它是通过创建一个包装对象，页就是装饰来包裹真实的对象。
<!--more-->

![Decorator Pattern](http://7xqgk3.com1.z0.glb.clouddn.com/image/design-pattern/decorator-pattern.png)
> Decorator Pattern UMP map **[More Detail](http://www.cnblogs.com/java-my-life/archive/2012/04/20/2455726.html)**

* 装饰模式优点
    * 装饰模式与继承关系的目的都是要扩展对象的功能，但是装饰模式可以提供比继承更多的灵活。装饰模式允许系统动态决定“贴上”一个需要的“装饰”，或者除掉一个不需要的“装饰”。继承关系则不同，继承关系是静态的，它在系统运行前就决定了。
    * 避免在层次结构高层的类有太多的特征
    * 通过使用不同的具体装饰类以及这些装饰类的排列组合，设计师可以创造出很多不同行为的组合。

* 装饰模式的缺点
    * 使用装饰模式会产生比使用继承关系更多的对象
    * 对象很像，排查错误比较困难

### 主题
```
<?php
abstract class Beverage{
    public $_name;
    abstract public function Cost();
}
// 被装饰者类
class Coffee extends Beverage{
    public function __construct(){
        $this->_name = 'Coffee';
    }
    public function Cost(){
        return 1.00;
    }
}
// 以下三个类是装饰者相关类
class CondimentDecorator extends Beverage{
    public function __construct(){
        $this->_name = 'Condiment';
    }
    public function Cost(){
        return 0.1;
    }
}

class Milk extends CondimentDecorator{
    public $_beverage;
    public function __construct($beverage){
        $this->_name = 'Milk';
        if($beverage instanceof Beverage){
            $this->_beverage = $beverage;
        }else
            exit('Failure');
    }
    public function Cost(){
        return $this->_beverage->Cost() + 0.2;
    }
}

class Sugar extends CondimentDecorator{
    public $_beverage;
    public function __construct($beverage){
        $this->_name = 'Sugar';
        if($beverage instanceof Beverage){
            $this->_beverage = $beverage;
        }else{
            exit('Failure');
        }
    }
    public function Cost(){
        return $this->_beverage->Cost() + 0.2;
    }
}

// Test Case
//1.拿杯咖啡
$coffee = new Coffee();

//2.加点牛奶
$coffee = new Milk($coffee);

//3.加点糖
$coffee = new Sugar($coffee);

printf("Coffee Total:%0.2f元\n",$coffee->Cost());
```
