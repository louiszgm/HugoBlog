+++
title = "Kotlin-Lambda"
lastmod = 2019-02-04T12:25:46+08:00
draft = true
weight = 2001
author = "louiszgm"
+++

参考链接：<https://juejin.im/post/5abd8ed351882510fd3fb8b1#heading-17>

-   基本表达式，和引用
-   lambda和java的互相使用（可以说一下Java中可以用this拿到自己，lambda却不可以：比如在Listener里面Java可以用this取消自己，Kotlin不行）
-   带接收者的lambda（with apply等等，说说这些函数的实现原理）
-   高阶函数 （高阶函数的定义，Java中怎么调用）
-   内联函数
-   高阶函数中的return

目的是服务于Kotlin的高阶函数,他主要有三个作用

-   初始化函数类型的值
-   作为字面量直接传递给高阶函数的参数
-   作为高阶函数的返回值

至于大多数人说使用Lambdas可以使代码更加的简洁。简洁这个本身就是比较主观的东西，有的人觉得简洁了，有的人觉得可读性更差了。
这个我个人认为并不是Lambdas主要解决的问题。


## 前言 {#前言}

说起Lambda，我们就会联想起函数式编程，高阶函数，闭包这几个名词。那么今天，我们就来弄清楚Lambda是什么东西，和这几个东西到底是什么关系？

具备以下两点的其中一点的函数，我们把它叫做 `高阶函数`

-   将函数作为输入传给函数
-   让函数返回函数

文章中，会围绕着下面这个类进行演示。

新建一个叫Calculator的类。这个类的作用是，根据外部传入的函数类型的值。来动态的计算两个Int类型的值。并且返回结果

里面有一个函数叫做calculate，返回一个Int类型的值，输入参数分别是：

-   参数x，类型为Int
-   参数y，类型为Int
-   参数operator，类型为函数类型

```Kotlin
class Calculator{
    //calculate是一个高阶函数，因为他满足上面我们所说的高阶函数的条件之一: 函数作为输入参数传给了函数
    fun calculate(x: Int, y:Int, operator: (Int, Int)-> Int): Int{
        return operator(x,y)
    }

    //这里直接抛出异常，而没有用到 !! ?. 的相关操作法，是为了让对这方面不熟悉的同学看的清晰
    fun calculateWithNullableOperator(x: Int, y: Int,operator:((Int,Int)->Int)?):Int{
        if (operator == null){
            throw NullPointerException()
        }
        return operator(x,y)
    }
}
```


## 它是什么 {#它是什么}

首先，Lambda是一个表达式。使用这个表达式，我们可以用来表示一个函数。那换句话说，Lambda是服务于函数的。

什么是表达式？它有什么作用？我们平时在写代码的过程中肯定是用过很多的表达式，这里以String为例子:

我们是用什么符号去表示一个String类型的值的，没错，就是用的双引号  `" "` ，在双引号里面就是我们的值。

-   Lambda表达式是用来表示 `函数类型` 的值
-   双引号 `" "` 是用来表示 `String类型` 的值

我们这么一对比，就能够对Lambda是一个什么东西有了一个大概的了解

What? 函数类型又是什么? 我们先来说一下这个东西

<!--more-->


### 函数类型 {#函数类型}

我们先来做个类比。

字符串的叫做字符串类型，用 `String` 表示

函数的类型叫做函数类型，用什么表示呢？

有了类型，我们可以声明这个类型的变量，然后可以把这个变量到处传来传去。

而函数类型也是一样的。我们可以想象一下字符串类型可以做什么？相应的，函数类型也可以做到。


#### 函数类型是怎么表示的 {#函数类型是怎么表示的}

我们声明一个字符串类型的变量的时候，是怎么声明的

```Kotlin
//其中的String就是字符串类型的表示形式
var text:String
```

而函数类型的表示形式，有如下的几种：

-   用圆括号 `()` 将函数的输入参数包起来, 圆括号后面用右箭头 `->` ，右箭头后面接的是函数的返回值的类型。连起来就是像下面那样

```text
(A,B)->C, A和B是函数输入参数的类型，C是函数返回值的类型

//当参数为空时，不能省略圆括号
() -> C
//当返回值C的类型是Unit的时候（对应Java的Void），不能省略
(A,B)->Unit
```

既然是类型，那当然可以使用 `?`,来声明为nullable

输入参数中，也就是被圆括号括起来的，参数名字可以省略不写。但类型是必须要的

多个函数类型之间是可以互相嵌套的


#### 如何表示一个函数类型的值 {#如何表示一个函数类型的值}

表示一个String类型的值，如下：

```nil
var text:String = "Instantiating a String type"
```

而一个函数类型的值，有哪几种方式呢？

我们声明一个函数类型的变量，分别用不同的方式去初始化。

使用代码块的方式：

```Kotlin
private fun initFunctionTypeByCodeBlock(calculator: Calculator) {
    //声明一个函数类型的变量
    var sumOperator: (num1: Int, num2: Int)->Int

    //通过Lambda expression初始化这个变量值
    sumOperator = {num1, num2 -> num1 + num2 }
    calculator.calculate(1,1,sumOperator)

    //通过匿名函数的方式初始化这个变量值
    sumOperator = fun(num1: Int, num2: Int):Int{return num1 + num2}
    calculator.calculate(2,2,sumOperator)
}
```

通过两个冒号的方式来初始化，Kotlin中是叫做callable reference
这里提亮点在实践过程中会遇到的疑问：
冒号前面，我到底是用类名，实例变量，还是不加东西

```Kotlin

```

通过一个实现了函数类型的自定义类

```Kotlin
private fun initFunctionTypeByCustomClass(calculator: Calculator){
        var sumOperator: (num1: Int, num2: Int)->Int

        //通过实例化一个实现了对应函数类型的自定义类来赋值给函数类型的变量
        sumOperator = SumOperator()
        calculator.calculate(1,1,sumOperator)
    }
```

到这里，真正和Lambda相关的只有一个地方。那就是作为一个函数类型的值。


## 它解决了什么问题 {#它解决了什么问题}

Lambda表达式是用来表示 `函数类型` 的值


## 它怎么用 {#它怎么用}

说到这里，我们来看下Lambda作为函数类型值的时候的用法

写法就是用一个大括号包裹着，如下就是一个Lambda expression：

```Kotlin
{x,y -> x+y}
```


### 作为函数类型变量的值 {#作为函数类型变量的值}

这个在上面已经说过了


### 直接传递给高阶函数参数中的函数类型 {#直接传递给高阶函数参数中的函数类型}

说一下lambda表达式作为函数调用的最后一个参数时的三种写法，放到括号后面，省略括号和常规写法

```Kotlin
//常规传值，用 {} 包裹
calculator.calculate(1,1,{ x, y -> x+y })

//当函数类型的参数是最后一个的时候，可以将Lambda移到最外面
calculator.calculate(1,1) { x, y -> x+y }

//当函数的参数中只有一个函数类型的参数时，可以省略圆括号()
calculator.calculate { x, y -> x+y }
```


## 它在开发过程中使用的案例是什么样的 {#它在开发过程中使用的案例是什么样的}


## 它内部是怎么去做的 {#它内部是怎么去做的}


## 总结，附上源码地址 {#总结-附上源码地址}
