+++
title = "Kotlin上手指南指导方向"
lastmod = 2018-12-15T17:24:46+08:00
draft = false
weight = 2001
author = "louiszgm"
+++

国际惯例，先上Hello World。

```Kotlin
package hello

fun main() {
    println("Hello World")
}
```

<!--more-->


## 快速体验Kotlin {#快速体验kotlin}

要想体验Kotlin，你不需要本地先安装环境。你只需要：

-   能上网
-   一个浏览器

通过官方文档，是一个能够最快速体验Kotlin的方式。你心里肯定想"这不是废话吗"。

别慌，请听我说完。Kotlin官方文档提供了一个 `可交互` 式的体验。

所谓的可交互式其实就是可以在网页上面直接运行Kotlin文档上提到的例子。

{{< figure src="/ox-hugo/kotlin-helloworld.gif" >}}


### 官方提供了哪些快速上手的渠道？ {#官方提供了哪些快速上手的渠道}


## Kotlin的好处以及坏处 {#kotlin的好处以及坏处}

使用Kotlin可以提高代码的简洁性，

-   `创建POJO`  在Kotlin中，创建一个POJO只需要一行代码。在Java中，需要写很多的比如 `getters,setters,toString()` 等方法

```Kotlin
data class Customer(val name: String, val email: String, val company: String)
```

-   `单例的创建` 在Kotlin中，可以很简单的通过 `object`  关键字就创建一个单例。在Java写过单例的同学们都知道，创建单例的方式有N种。

```Kotlin
object ThisIsASingleton {
    val companyName: String = "JetBrains"
}
```

-   `lambda表达式` 在Kotlin中，天生就支持 `lambda表达式`

```Kotlin
val positiveNumbers = list.filter { it > 0 }
```


## 在现有的项目中应该怎么引进 {#在现有的项目中应该怎么引进}


## 引进Kotlin过程中的疑惑 {#引进kotlin过程中的疑惑}

在只看官方文档的情况下，我相信大部分人都能够无障碍的使用起来。

但是，现实的情况是大部分的项目都已经的使用Java来写的。


### Java 和 Kotlin交互有什么问题？ {#java-和-kotlin交互有什么问题}

引用Kotlin官方文档中的一句话

> Kotlin is designed with Java Interoperability in mind

-   Kotlin调用Java
-   Java调用Kotlin


#### Kotlin和Rxjava，Retrofit的交互 {#kotlin和rxjava-retrofit的交互}

Rxjava 和 Retrofit ，这两个库基本上是Android应用开发者的必备库了。所以，在用kotlin的时候，估计这个点是被考虑的最多的。


#### 有哪些Java的东西在Kotlin中是 ‘没有’的 {#有哪些java的东西在kotlin中是-没有-的}


#### 匿名内部类 {#匿名内部类}


### Kotlin空安全以及对应的各种‘符号’的解释 {#kotlin空安全以及对应的各种-符号-的解释}

-   Kotlin的空安全是什么？

可以大幅度的减少我们应用的 `NullPointerException`.

-   ?
-   !!
-   ?.
-   !!.


## 现在有哪些公司在用Kotlin了？ {#现在有哪些公司在用kotlin了}
