---
layout:     post
title:      "Head First 设计模式"
subtitle:   "Head First 设计模式？"
date:       2017-11-01
author:     "StephenLau"
header-img: "img/post-bg-road-version.jpg"
tags:
- 心理学
- 哈佛幸福课
---
# Head First

## 元认知

让你的大脑认为你在学习的新东西确实很重要。
慢方法：大量地重复。
更快的方法是尽一切可能让大脑活动起来。图片，交谈式风格

理解的越多，需要记的就越少。
上床睡觉之前不要再看别的书了，大脑需要自己的时间处理知识。
多喝水。
大声说出来。

## 策略模式(Strategy Pattern)
定义了算法组，分别封装起来，让它们之间可以相互替代。

Duck类 swim() fly() display()
Duck类 swim()  display()  Flyable接口 fly()

**设计原则：把代码中会变化的部分封装出来。**
**设计原则：针对接口(超类型supertype)编程，而不是针对实现编程。**

FlyBehavior接口有fly()方法。FlyWithWings类，FlyNoWay类实现同样的接口。
Duck类has，FlyBehavior接口。
**设计原则：多用组合，少用继承。**

## 观察者模式

一句话：被观察者接口用列表记录观察者接口，并在需要通知时调用列表中观察者接口的更新方法。

http://blog.csdn.net/qq1263292336/article/details/60332478

```Java
WeatherData {
    getTemperature();       // 返回最近的气象测量数据：温度
    getHumidity();          // 返回最近的气象测量数据：湿度
    getPressure();          // 返回最近的气象测量数据：气压
    measurementsChanged();  // 一旦气象测量更新，此方法会被调用
}
```

需要实现三个使用天气数据的布告板，目前情况、气象统计、天气预告。
```java
public class WeatherData {
    // 实例变量声明
    public void measurementsChanged() {
        float temp = getTemperature();
        float humidity = getHumidity();
        float pressure = getPressure();

        currentConditionDisplay.update(temp, humidity, pressure);
        statisticsDisplay.update(temp, humidity, pressure);
        forecastDisplay.update(temp, humidity, pressure);
    }
    // 其他的WeatherData方法
}
```
这样做的坏处有：

1. 针对具体实现编程，而非针对接口编程
2. 对于每个新的布告板，我们都得修改代码
3. 我们无法再运行时动态地增加（或删除）布告板
4. 我们尚未封装改变的部分

![img](https://github.com/linpeiyou/design-patterns-java/raw/master/observer/image/3.png)

### 观察者模式：观察者模式定义了对象之间的一对多依赖，这样一来，当一个对象改变状态时，它的所有依赖者都会受到通知并自动更新。
在报纸行业中，出版者+订阅者=观察者模式
在软件行业中，出版者称为“主题”（Subject），订阅者称为“观察者”（Observer）。
松耦合的威力

设计原则：为了交互对象之间的松耦合设计而努力。
当两个对象之间松耦合，它们依然可以交互，但是不太清楚彼此的细节。
观察者模式提供了一种对象设计，让主题和观察者之间松耦合。

### Java内置的观察者模式

java.util包内包含了最基本的Observer接口与Observable类，Observer接口与Observable类使用上更方便，因为许多功能都已经事先准备好了。你甚至可以使用推（push）或拉（pull）的方式传送数据。

#### Java内置的观察者模式如何运作

1. 实现观察者接口（java.util.Observer），然后调用Observable的addObserver()方法，即可成为观察者
2. 主题（扩展java.util.Obserable）先调用setChanged()方法，标记状态已改变的事实。然后调用notifyObservers()或者notifyObservers(Object arg)
3. 观察者通过update(Observable o, Object arg)接收通知

## 装饰

Beverage类 description cost

每个饮料都继承Beverage，类爆炸。-> 用实例变量和继承。

**设计原则：类应该对扩展开发，对修改关闭。**

