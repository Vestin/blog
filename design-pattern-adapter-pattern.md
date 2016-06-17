---
title: design-pattern-adapter-pattern
toc: true
categories:
  - coding
tags:
  - design pattern
date: 2016-06-17 09:48:53
description:
---
Adapter Pattern
===

在软件系统中，由于应用环境的变化，常常需要将“一些现存的对象”放在新的环境中应用，但是新环境要求的接口是这些现存对象所不满足的。Adapter设计模式就是为了应对这种“迁移的变化”，以使客户系统既能利用现有对象的良好实现，同时又能满足新的应用环境所要求的接口。
<!--more-->

```
<?php

//原运输系统 和 车辆
Class Transport{

	private $car;	//运输的车辆

	public function __construct(Car $car){
		$this->car = $car;
	}

	public function transporting(){
		$this->car->run();
	}
}

//车
interface Car{
	//搞运输
	public function run();
}

//正常运输车
Class Jeep implements Car{
	public function run(){
		echo 'run...';
	}
}

//正常情况是这样的
$jeep = new Jeep;
$transport = new Transport($jeep);
$transport->transporting();	//'run...';

//但有一种车辆，他是这样的
Class BMW {
	public function move(){
		echo 'move...';
	}
}

//BMW 如果想加入运输系统，就需要adapter 模式
Class BMWCaradapter implements Car{

	private $bmw;

	public function __construct($bmw){
		$this->bmw = $bmw;
	}

	public function run(){
		$bmw->move();
	}
}

//运行
$bmw = new BMW;
$BMWCarAdapter = new BMWCarAdapter($bmw);
$transport = new Transport($BMWCarAdapter);
$transport->transporting();	//'move...';

//这样就完成了适配
```
