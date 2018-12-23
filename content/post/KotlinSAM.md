+++
title = "谈谈Kotlin中的SAM"
lastmod = 2018-12-23T13:17:13+08:00
draft = false
weight = 2001
author = "louiszgm"
+++

首先，先说明一下什么是 `SAM` 和 `SAM Conversion`

-   SAM,全称是Single Abstract Method。是一个 `只有一个方法的接口` 。

例如Android中的点击事件的监听器 `OnClickListener` 就是一个SAM

```Java
public interface OnClickListener {
    void onClick(View v);
}
```

-   SAM Conversion, 指的是通过 `某种形式` ，转换为某个接口的具体实现。

<!--more-->


## [TL;DR](#总结) {#tl-dr--总结}

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


## 真正的问题 {#真正的问题}

在前面铺垫了这么多，我们来说一下这样子的话会有什么问题。

当一个Java的库需要转成Kotlin的时候，在Kotlin里可能是使用了以上3种SAM Conversion中的一种，

然而，除非在转换之后，一个参数类型为SAM的方法提供了另外一个参数类型为functional type的方法签名（如上所示）。
否则，通过Lambda 和 Method Reference的语法去使用都是不可以的。


## 总结 {#总结}

以上所说的可以总结为：

```text
当Java的类库转换为Kotlin时，一些参数类型为SAM的接口。

需要再增加一个相同名称且参数为函数类型的接口。
```
