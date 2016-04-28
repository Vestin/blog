---
title: bridge pattern 桥接模式
toc: true
categories:
  - coding
tags:
  - design pattern
date: 2016-04-28 11:03:38
description:
---

### 桥接模式(bridge pattern)
将**抽象**和**实现**解耦，使得二者可以**独立地变化**。
一般用在两个或多个维度（抽象）的变化。
![bridge](http://7xqgk3.com1.z0.glb.clouddn.com/image/design-pattern/bridge-pattern.jpg)

例如：
抽象1：Road 具体（高速公路，乡村公路)
抽象2：Car  具体（Jeep,BMW)
Jeep 可以在告诉公路上跑，也可以在乡村公路上跑，同样BMW也可以。

实现例子：
```
<?php

Abstract Road{
    /**
    * 在路上跑
    /*
    Abstract function onRoad();
}

Class SpeedRoad extends Road(){
    public function onRoad(){
        echo '在高速公路上';
    }
}

Class CountryRoad extends Road(){
    public function onRoad(){
        echo '在乡村公路上';
    }
}

Abstract Car{

    public $road;

    //车可以跑
    Abstract function run();

}

Class Jeep extends Car{
    public function run(){
        echo 'Jepp 跑';
        $this->road->onRoad();
    }
}

Class BMW extends Car{
    public function run(){
        echo 'BMW 跑';
        $this->road->onRoad();
    }
}

//test
$speedRoad = new SpeedRoad;
$countryRoad = new CountryRoad;
$Jeep = new Jeep();
$Jeep->road = $speedRoad;
$Jeep->run(); //Jepp 跑在高速公路上

$jeep->road = $countryRoad;
$jeep->run(); //Jepp 跑在乡村公路上

$Bmw = new BMW;
$Bmw->road = $speedRoad;
$Bmw->run(); //BMW 跑在高速公路上

$Bmw->road = $countryRoad;
$Bmw->run(); //BMW 跑在乡村公路上

```
