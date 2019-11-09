+++
title = "Kotlin-Lambda"
lastmod = 2019-02-12T21:12:38+08:00
tags = ["Kotlin", "Lambdas", "函数", "高阶函数"]
draft = false
weight = 2001
author = "louiszgm"
+++

## 前言 {#前言}

文中的案例可以在 [这个网站](https://play.kotlinlang.org) 进行练习

说起Lambda，我们就会联想起函数式编程，高阶函数，闭包这几个名词。那么今天，我们就来弄清楚Lambda是什么东西，和这几个东西到底是什么关系？


### 高阶函数 {#高阶函数}

具备以下两点的其中一点的函数，我们把它叫做 `高阶函数`

-   将函数作为输入传给函数
-   让函数返回函数

`文章中，会围绕着下面这个类进行演示。`

新建一个叫Calculator的类。这个类的作用是，根据外部传入的 `函数类型` 的值。来动态的计算两个Int类型的值。并且返回结果

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

我们是用什么符号去表示一个String类型的值的，没错，就是用的双引号  `" "` 。

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

1.  用圆括号 `()` 将函数的输入参数包起来, 圆括号后面用右箭头 `->` ，右箭头后面接的是函数的返回值的类型。连起来就是像下面那样

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

我们声明一个函数类型的变量，分别用不同的方式去赋值。

-    使用代码块的方式：

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

-    使用callable reference

-    通过一个实现了函数类型的自定义类

    ```Kotlin
    class SumOperator: (Int, Int)->Int{
        override fun invoke(p1: Int, p2: Int): Int {
            return p1 + p2
        }
    }
    ```

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

这个在上面已经说过了,如下

```Kotlin
//声明一个函数类型的变量
var sumOperator: (num1: Int, num2: Int)->Int

//通过Lambda expression初始化这个变量值
sumOperator = {num1, num2 -> num1 + num2 }
```


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


### 高阶函数中的return {#高阶函数中的return}

> 这一小节的例子来自Kotlin官方文档

在高阶函数中，直接使用return是退出整个函数

```Kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return // non-local return directly to the caller of foo()
        print(it)
    }
    println("this point is unreachable")
}

//输出结果是： 12
//我们可以看到最后面那一句 "this point is unreachable"并没有打印出来.
//可见这里return的是foo()这个函数
//要直接使用return的话，对应的高阶函数必须是用inline修饰的内联函数。这里的forEach就是
```

如果我们想要后面的那句话也打印出来的话，可以使用如下的做法

```Kotlin

fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return@forEach // local return to the caller of the lambda, i.e. the forEach loop
        print(it)
    }
    print(" done with implicit label")
}

//输出结果是： 1245 done with implicit label
//我们使用了return@forEach的方式来返回lambda
```

我们也可以显示的指定label，如下，我们为forEach 贴个标签

```Kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach lit@{
        if (it == 3) return@lit // local return to the caller of the lambda, i.e. the forEach loop
        print(it)
    }
    print(" done with explicit label")
}
//输出结果为： 1245 done with implicit label
//我们这里return的label 是我们自己显示指定的lit
```

虽然我们在forEach函数中return了，但剩下的4和5两个数字也打印出来了。如果我们希望forEach函数里面return
了之后就不继续往下执行了（类似于for循环里面的break），我们应该怎么做呢？如下：

```Kotlin
fun foo() {
    run loop@{
        listOf(1, 2, 3, 4, 5).forEach {
            if (it == 3) return@loop // non-local return from the lambda passed to run
            print(it)
        }
    }
    print(" done with nested loop")
}

//输出结果是：12 done with nested loop
//这里我们用run{}这个函数把我们的嵌套起来，然后我们return的是run{}这个高阶函数
```


## 它在开发过程中使用的案例是什么样的 {#它在开发过程中使用的案例是什么样的}


## 它内部是怎么去做的 {#它内部是怎么去做的}


## 总结，附上源码地址 {#总结-附上源码地址}
