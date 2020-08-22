---
title: Kotlin
date: 2020-05-27 10:31:12
tags: Android


---

<!--more-->

##  1. Kotlin基础

### Lambda编程

#### Lambda语法结构

{参数名1：参数类型，参数名2：参数类型 -> 函数体} 

函数体最后一行代码作为Lambda表达式的返回值

#### 集合基础

不可变集合初始化（不可增删改）：

```kotlin
val list = listOf("apple", "banana", "orange")
val set = setOf("apple", "banana", "orange")
```

可变集合初始化：

```kotlin
val list = mutableListOf("apple", "banana", "orange")
list.add("Pear")
```

集合遍历：

```kotlin
for (fruit in list) {
    println(fruit)
}
```

Map集合初始化：

```kotlin
// 写法1
val map = HashMap<String, Int>()
map["apple"] = 1
map["banana"] = 2
map["orange"] = 3

// 写法2
val map = mapOf("apple" to 1, "banana" to 2, "orange" to 3)
```

Map集合遍历：

```kotlin
for ((fruit, number) in map) {
    println("fruit is " + fruit + ", number is " + number)
}
```

#### 集合的函数式API

## 2. Kotlin技巧

### 字符串内嵌表达式

### 函数的参数默认值

### 函数键值对传参