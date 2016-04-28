---
title: Publish/Subscribe Pattern
date: 2016-04-18 14:46:20
tags:
- design pattern
categories:
- coding
---
观察者模式
===
> **观察者模式** 定义了对象之间的一对多依赖，这样一来，当一个对象改变状态时，它所有依赖者都会收到通知并自动更新。
<!--more-->
![subject observer](http://7xqgk3.com1.z0.glb.clouddn.com/image/design-pattern/subject-observer.jpg)
> subject observer UMP map

In php

### 主题
```
<?php

//被订阅的主题
interface subject {

    //注册的观察者
    private $observers = [];

    /**
     * Observer $observer
     *
     * return void
     */
    public function rigisterObserver(Observer $observer){
        $this->observers[] = $observer;
    }

    /**
     * Observer $observer
     *
     * return void
     */
    public function removeObserver(Observer $observer){
        unset($this->observers[array_search($observer)]);
    }

    /**
     * Notify all observer
     *
     * return void
     */
    public function notifyObservers(){
        foreach($this->observers as $observer){
            $observer->update();
        }
    }
}
```

### 观察者
```
<?php
//观察者
interface Observer{

    /**
     *  Do things when notify
     */
    abstract public function update();
}
```
