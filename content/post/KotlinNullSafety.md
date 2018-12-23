+++
title = "Kotlin空安全以及对应的各种‘符号’的解释"
lastmod = 2018-12-23T10:20:28+08:00
draft = false
weight = 2001
author = "louiszgm"
+++

> Kotlin's type system is aimed at eliminating the danger of null references from code, also known as the [The Billion Dollar Mistake.](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare)

{{< figure src="/ox-hugo/billionDollarMistake.jpg" >}}

<!--more-->

-   Kotlin的空安全是什么？

可以大幅度的减少我们应用的 `NullPointerException` （文章后面会使用 `NPE` 来表示这个异常)

这个东西理解理解起来很简单，但是到了真正使用的时候。就会被他的各种符号给搞得糊涂了。例如下面的符号：

`?` `?.` `?:` `!!.`

我们从变量的声明开始去理解Kotlin帮我们做的这些空安全的操作

在Kotlin中，声明一个变量默认是空安全的。

如果希望一个变量可以是 `null` 的，可以通过在变量声明的类型后面加入 `?` 来声明。

{{< figure src="/ox-hugo/KotlinNullSafety.png" >}}

接下来是对已声明的对象的使用。

当声明的一个变量是 `nullable` 的时候，为了让程序不抛出 `NPE` 。我们想想，我们会怎么做？

常用的做法就是对一个变量进行判空,下面这行代码。写Java的人都在熟悉不过了。

```java
if(b == null) return
int length = b.length()
```

Kotlin帮我们做的一件事就是把这个判空操作提前到编译期了，不过现在的IDE也会有实时的提示。

{{< figure src="/ox-hugo/KotlinAccessNullableType.png" >}}

IDE的提示我们可以这么去理解，他让我们自行决定。 `你的这句话到底需不需要抛出NPE`

-   如果不允许抛出，则应该做判空处理

```Kotlin
if (b == null) return
val x = b.length
```

在Kotlin中，我们可以使用一个叫 `Safe Call 符号是 ?.` 的来达到上面的判空目的，下面的代码和上面基本等价的。

```Kotlin
val x = b?.length
```

上面为什么说是基本等价的呢？ 因为 `?.` 只在b不为 `null` 的情况下才会执行 `b.length` 并且返回。
如果b为 `null` 的并且只使用了 `?.` 的话，Kotlin默认会返回一个 `null` 。
但是，在实际的场景中，我们希望执行我们期望的行为，比如在上面，我们希望b为 `null` 的时候就直接 `return` 了。

在Kotlin中，提供了一个叫 `Elvis Operator` 的操作符可以让我们自行控制 `nullable变量类型为null的时候的行为` 。写法是 `?:`
也就是说，要和上面的判空处理真正等价的是下面的代码。

```Kotlin
val x = b ?.length ?:return
```

-   如果允许抛出，则需要自己显式的 `throw 一个NPE`

```Kotlin
if (b == null) throw NullPointerException()
val x = b.length
```

在Kotlin中，我们可以使用 `!!` 来达到上面显式抛出 `NPE` 的目的，下面的代码和上面的是等价的。

```Kotlin
val x = b!!.length
```

我们也可以把 `!!` 拆分为如下：

```Kotlin
val x = b ?.length ?:throw NullPointerException()
```

-   总结一下对空安全的理解：

```text
Kotlin帮我们做得是将NPE尽早的暴露在编译阶段，从而可以减少在运行时NPE的数量。
```

对于各种符号的困惑，可以参照如下的流程图来选择应该要使用哪种操作符:
![](/ox-hugo/NullSafetyFlowChart.png)
