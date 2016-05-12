---
title: factory pattern
toc: true
categories:
  - coding
tags:
  - design pattern
date: 2016-04-18 17:46:58
description: detail of factory pattern
---
工厂模式(Factory Pattern)
===
[TOC]

工厂模式是最重要的模式，因为大多数模式都需要用到工厂模式。如果不能正确的运用工厂模式，那么可以说无法成为合格的架构师。
多数设计模式的内容讲解的都是如何设计接口。接口如何产生呢？如果在客户代码（类库的使用者称之为客户）中直接使用具体类，那么就失去了接口的意义。因为接口的使用目的，就是要降低客户对具体类的依赖程度。如果在客户代码中直接使用接口，那么就造成了客户对具体类名称的依赖。（客户最终需要以某种方式指明所需要的具体类，如配置文件或代码，但是只需要指出一次，所以说降低对具体类的依赖程度）。要使客户代码不依赖具体类，唯一的方法，就是让客户代码不依赖具体类的部分不知道具体类的名称。知道具体类名称的部分，仅仅是配置部分。（配置文件或者配置代码）。
依赖不可能完全消除，除非二者毫无联系。但是可以将这种依赖的程度降到最低。
既然不能直接创建具体类，那么就需要通过一个创建者类来创建接口的实现类。这样就产生了工厂类。

<!--more-->

## 简单工厂模式
> **工厂模式** 简单工厂模式是属于创建型模式，又叫做静态工厂方法（Static Factory Method）模式

简单工厂模式的工厂类一般是使用静态方法，通过接收的参数的不同来返回不同的对象实例。

简单工厂模式是由一个工厂对象决定创建出哪一种产品类的实例。简单工厂模式是工厂模式家族中最简单实用的模式，可以理解为是不同工厂模式的一个特殊实现。

![Static Factory Pattern](http://7xqgk3.com1.z0.glb.clouddn.com/image/design-pattern/static-factory-pattern.jpg)
![simple factory pattern](http://7xqgk3.com1.z0.glb.clouddn.com/image/design-pattern/SimpleFactory.jpg)
> Static Factory Pattern UMP map

角色分工：
* **工厂（Creator）角色**
    * 简单工厂模式的核心，它负责实现创建所有实例的内部逻辑。工厂类的创建产品类的方法可以被外界直接调用，创建所需的产品对象。
* 抽象产品（Product）角色
    * 简单工厂模式所创建的所有对象的父类，它负责描述所有实例所共有的公共接口。
* 具体产品（Concrete Product）角色
    * 是简单工厂模式的创建目标，所有创建的对象都是充当这个角色的某个具体类的实例。
#### 简单工厂模式的优缺点分析：

**优点：**

*有利于系统优化*

工厂类是整个模式的关键所在。它包含必要的判断逻辑，能够根据外界给定的信息，决定究竟应该创建哪个具体类的对象。用户在使用时可以直接根据工厂类去创建所需的实例，而无需了解这些对象是如何创建以及如何组织的。有利于整个软件体系结构的优化。

**缺点：**

*违反了单一职责原则(SRP),开放-封闭原则(OCP)*

由于工厂类集中了所有实例的创建逻辑，这就直接导致一旦这个工厂出了问题，所有的客户端都会受到牵连；而且由于简单工厂模式的产品室基于一个共同的抽象类或者接口，这样一来，但产品的种类增加的时候，即有不同的产品接口或者抽象类的时候，工厂类就需要判断何时创建何种种类的产品，这就和创建何种种类产品的产品相互混淆在了一起，违背了单一职责，导致系统丧失灵活性和可维护性。而且更重要的是，简单工厂模式违背了“开放封闭原则”，就是违背了“系统对扩展开放，对修改关闭”的原则，因为当我新增加一个产品的时候必须修改工厂类，相应的工厂类就需要重新编译一遍。

```
//抽象产品
interface Car{
    abstract public run();
}

//具体产品
Class Jeep{
    public function run(){
        echo 'Jeep';
    }
}

Class BMW{
    public function run(){
        echo 'BMW';
    }
}

//简单工厂
Class SimpleCarFactory{
    static public createCar(type){
            switch(type){
                case 1:
                    return new Jeep;
                case 2:
                    return new BMW;
            }
    }
}

//test
$car = SimpleCarFactory::createCar(1);
$car->run(); //Jeep

$car = SimpleCarFactory::createCar(2);
$car->run(); //BMW
```

## 工厂模式
> 工厂方法是针对每一种产品提供一个工厂类。通过不同的工厂实例来创建不同的产品实例。
在同一等级(产品)结构中，支持增加任意产品。

![factory-pattern-image](http://7xqgk3.com1.z0.glb.clouddn.com/image/design-pattern/FactoryMethod.jpg)

```
//汽车产品
interface Car{
    public function run();
}

//具体产品jeep
Class Jeep implements Car{
    public function run(){
        echo 'jeep run...';
    }
}

//具体产品BMW
Class BMW implements Car{
    public function run(){
        echo 'BMW run...';
    }
}

//工厂抽象
abstract Class CarFactory{
    abstract public function createCar();
}

//Jeep 工厂
Class JeepCarFactory{
    public function createCar(){
        return new Jeep;
    }
}

//BMW 工厂
Class BMWCarFactory{
    public function createCar(){
        return new BMW;
    }
}

//Client
$factory = new JeepCarFactory;
$car = $factory->createCar();
$car->run();    //jeep run...

$factory = new BMWCarFactory;
$car = $factory->createCar();
$car->run();    //bmw run...
```

## 抽象工厂
> 抽象工厂是应对产品族概念的。比如说，每个汽车公司可能要同时生产轿车，货车，客车，那么每一个工厂都要有创建轿车，货车和客车的方法。
应对产品族概念而生，增加新的产品线很容易，但是无法增加新的产品。

![abstract factory pattern](http://7xqgk3.com1.z0.glb.clouddn.com/image/design-pattern/AbstractFactory.jpg)

```
//抽象车
interface Car{
    public function run();
}

//抽象自行车
interface Bike{
    public function run();
}

//Jeep车
Class Jeep implements Car{
    public function run(){
        echo 'Jeep run...'.PHP_EOL;
    }
}

//Jeep自行车
Class JeepBike implements Bike{
    public function run(){
        echo 'JeepBiked run...'.PHP_EOL;
    }
}

//BMW车
Class BMW implements Car{
    public function run(){
        echo 'BMW run..'.PHP_EOL;
    }
}
//BMW自行车
Class BMWBike implements Bike{
    public function run(){
        echo 'BMWBike run..'.PHP_EOL;
    }
}

//抽象交通工具生产工厂
interface Vehicle{
    public function createCar();
    public function createBike();
}

//Jeep 工厂
Class JeepFactory implements Vehicle{
    public function createCar(){
        return new Jeep;
    }

    public function createBike(){
        return new JeepBike;
    }
}

//BMW 工厂
Class BMWFactory implements Vehicle{
    public function createCar(){
        return new BMW;
    }
    public function createBike(){
        return new BMWBike;
    }
}

//Client
$factory = new JeepFactory;
$car = $factory->createCar();
$bike = $factory->createBike();
$car->run();
$bike->run();

$factory = new BMWFactory;
$car = $factory->createCar();
$bike = $factory->createBike();
$car->run();
$bike->run();
```

## 简单工厂模式、工厂模式、抽象工厂模式的差别
> 可以参考文章[三种工厂模式的差别](http://www.cnblogs.com/jy02414216/archive/2012/08/10/2633084.html)

简单工厂 ： 用来生产同一等级结构中的任意产品。（对于增加新的产品，无能为力）

工厂方法 ：用来生产同一等级结构中的固定产品。（支持增加任意产品）   
抽象工厂 ：用来生产不同产品族的全部产品。（对于增加新的产品，无能为力；支持增加产品族）  

以上三种工厂 方法在等级结构和产品族这两个方向上的支持程度不同。所以要根据情况考虑应该使用哪种方法。

简单工厂，一般是两级结构。工厂类创建接口。随着接口创建复杂性的增强，可能在接口创建的过程中，一个创建者类，无法承担创建所有的接口类的职责。可能会有这样的情况，我们定义了一个接口，有6个实现类分别是123456号。但是，这六个实现类不可能用一个工厂创建出来，因为123号是windows下的实现，而456号是linux上的实现。（假设我们使用的语言不是广大人民群众热爱的java语言），那么这个时候，我还需要客户方用相同的方式来创建这个借口，而不是在代码中到处写

```
Java代码 :
if (操作系统=="windows");{   
...   
}   
else{   
...   
}
```

那样就太麻烦了。设计模式就是为了减少麻烦，而不是什么别的废话，比如什么太极八卦、天人合一、面向xx之类的。因为怕麻烦，所以搞出设计模式这个咚咚减少麻烦。如果你发现用了设计模式更麻烦了，那么肯定是你用错了。
这个时候为了省事，我就把工厂也抽象成一个接口（因为我有两个相似的工厂，如果只有一个，我还废话什么呢），这样就成了工厂方法。
当然，既然工厂方法成了一个接口，那么当然也需要用一个工厂来创建它。这个时候，创建是三级结构，简单工厂（此时是工厂的工厂）创建工厂接口（本来是个类，现在因为进一步的抽象，成为接口了），工厂接口创建产品。
过了一段时间，随着我们的工厂业务不断发展，我们有了很多产品。比如，我们有锤子和钉子两种产品。这两种产品都有windows品牌和linux品牌的。我们给锤子和钉子各自定义了一个创建的接口。

```
Java代码 :
interface 锤子工厂{   
造锤子（）；   
}   
interface 钉子工厂{   
造钉子();;   
}
```

可是，我们发现某些用户，用windows的锤子去敲linux的钉子，从而把程序敲出了bug。这当然是我们的错误，因为我们违反了一条金科玉律：
要想使你的程序稳定运行，你假设用户是猪。所以，我们把锤子和钉子的工厂合并，让一个工厂只能造出配套的锤子和钉子，这样猪就没有犯错误的机会了。于是我们搞出一个抽象工厂：
```
interface 铁匠铺{
造锤子（）；
造钉子();
}
```
当然，这个铁匠铺是个接口，所以同样需要用一个工厂来创建它。所以，这个时候，工厂还是三级结构。
我们的工厂，业务很多，而且产品一般都是配套使用的（这样可以多骗点钱），所以，我们大多数情况下，都是使用抽象工厂和简单工厂。简单工厂用来创建工厂，抽象工厂创建产品。
工厂的作用，就是创建接口。
其实我们不知道什么是设计模式，我们只是怕麻烦。什么是麻烦呢？我们觉得把同样的代码写两遍就非常麻烦。所以，我们宁可多写几句，也要解决麻烦。猪不怕麻烦，可以日复一日的重复相同的事情，可是我们不是猪。

## 参考文章
* [http://www.cnblogs.com/jy02414216/archive/2012/08/10/2633084.html](http://www.cnblogs.com/jy02414216/archive/2012/08/10/2633084.html)
* [http://blog.csdn.net/superbeck/article/details/4446177](http://blog.csdn.net/superbeck/article/details/4446177)
* [http://blog.chinaunix.net/uid-25958655-id-4243289.html](http://blog.chinaunix.net/uid-25958655-id-4243289.html)
* [http://wxg6203.iteye.com/blog/740229](http://wxg6203.iteye.com/blog/740229)
