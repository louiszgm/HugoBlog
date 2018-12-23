+++
title = "Kotlin上手指南指导方向"
lastmod = 2018-12-23T10:28:29+08:00
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

所以，这里回答这一小节的问题。Java 和 Kotlin的交互式没有问题的。

-   Kotlin调用Java
-   Java调用Kotlin


#### Kotlin和Rxjava，Retrofit的交互 {#kotlin和rxjava-retrofit的交互}

Rxjava 和 Retrofit ，这两个库基本上是Android应用开发者的必备库了。所以，在用kotlin的时候，估计这个点是被考虑的最多的。


#### Kotlin中的SAM {#kotlin中的sam}

当Kotlin中使用SAM作为方法参数的时候，就不能使用SAM Conversion了。
必须得使用一个对应类型的变量，或者使用 `object`

接下来，先说明一下什么是 `SAM` 和 `SAM Conversion`

-   SAM,全称是Single Abstract Method。是一个 `只有一个方法的接口` 。

例如Android中的点击事件的监听器 `OnClickListener` 就是一个SAM

```Java
public interface OnClickListener {
    void onClick(View v);
}
```

SAM Conversion, 指的是通过 `某种形式` ，转换为某个接口的具体实现。

在Java8以上， SAM Conversion有如下几种：

-   Lambda (Java8加入)
-   Method Reference （Java8加入）
-   Anonymous Implementation

```Java
public class JavaSamPractice extends Activity{

    private Button button;
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        //通过匿名实现的方式
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                System.out.println("Hello From Anonymous Implementation");
            }
        });
        //通过Lambda的语法
        button.setOnClickListener(view -> System.out.println("Hello From Lambda"));
        //通过Method Reference的语法
        button.setOnClickListener(this::clickSayHello);
    }

    private void clickSayHello(View view){
        System.out.println("Hello From Method Reference");
    }
}
```

在Kotlin中，方法的调用者为 `Java对象` 和 `Kotlin对象` 时，SAM Conversion分别如下所示:

当方法的调用者是 `Java` 对象时，SAM Conversion有如下几种：

-   Lambda (Java8加入)
-   Method Reference （Java8加入）
-   Anonymous Implementation

这里的 `button` 就是Java对象

```Kotlin
class KotlinSamPractice : Activity() {
    lateinit var button: Button
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        //通过匿名实现的方式，在Kotlin中，用object 来声明一个匿名的接口实现
        button.setOnClickListener(object : View.OnClickListener {
            override fun onClick(v: View?) {
                println("Hello From Anonymous Implementation")
            }
        })
        //通过Lambda的语法
        button.setOnClickListener({ view -> println("Hello From Lambda") })

        //通过Method Reference的语法
        button.setOnClickListener(this::clickSayHello)
    }
    fun clickSayHello(view: View) {
        println("Hello From Method Reference")
    }
}
```

我们可以看到，这里的三种方式是和上面Java 的SAM Conversion是一样的。

当方法的调用者是 `Kotlin` 对象时，只有以下的一种SAM Conversion：

-   Anonymous Implementation

这里的KotlinConsumer就是一个SAM

```Kotlin
interface KotlinConsumer<T> {
    fun accept(value: T)
}

class KotlinObservable<T> {
    fun subscribe(onNext: KotlinConsumer<T>) {}
}

class KotlinSamPractice {
    fun main(){
        //通过匿名实现的方式
        KotlinObservable<String>().subscribe(object :KotlinConsumer<String>{
            override fun accept(value: String) {
                println("Hello From Anonymous Implementation")
            }
        })
    }
}
```

下面是对KotlinObservable新增了一个类型是函数字面量的方法，这样子就可以使用Kotlin 的Lambda表达式了

```Kotlin
interface KotlinConsumer<T> {
    fun accept(value: T)
}

class KotlinObservable<T> {
    fun subscribe(onNext: KotlinConsumer<T>) {}
    fun subscribe(consumer: (value: T) -> Unit){}
}

class KotlinSamPractice {
    fun main(){
        //通过匿名实现的方式
        KotlinObservable<String>().subscribe(object :KotlinConsumer<String>{
            override fun accept(value: String) {
                println("Hello From Anonymous Implementation")
            }
        })

        //Lambda表达式
        KotlinObservable<String>().subscribe { value -> println("Hellow From Lambda") }
    }
}
```


#### 有哪些Java的东西在Kotlin中是 ‘没有’的 {#有哪些java的东西在kotlin中是-没有-的}

在Java中，有很多情况下我们需要传入一个


## 现在有哪些公司在用Kotlin了？ {#现在有哪些公司在用kotlin了}
