---
title: design-pattern-template-pattern
toc: true
categories:
  - coding
tags:
  - design pattern
date: 2016-07-4
description:
---
Template Pattern
===

定义一个操作中算法的骨架，而将一些步骤延迟到子类中，在不改变算法结果的情况下重新定义它的步骤。
<!--more-->

与strategy模式的区别:
Template Method 模式 适用于存在几个概念上相似，但不相同的过程。每个过程都是互相耦合的，因为他们与某个过程相关。

```
<?php
Abstract Class Calculate{
	
	public $a;
	public $b;
	
	public function __construct($a,$b){
		$this->a = $a;
		$this->b = $b;
	} 
	public function run(){
		$resA = $this->calculateA($this->a);
		$resB = $this->calculateB($this->b);
		return $resA+$resB;
	}
	
	abstract public function calculateA($a);
	abstract public function calculateB($b);
}

Class AddCalculate extends Calculate{
	public function calculateA($a){
		return $a+$a;
	}
	public function calculateB($b){
		return $b+$b;
	}
}

Class PlusCaculate extends Calculate{
	public function calculateA($a){
		return $a*$a;
	}
	public function calculateB($b){
		return $b*$b;
	}
}

//test
echo (new AddCalculate(1,2))->run();	//6
echo PHP_EOL;
echo (new PlusCaculate(1,2))->run();	//5
echo PHP_EOL;
```
