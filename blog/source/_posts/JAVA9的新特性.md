---
title: 预计发布的Java 9中，最令人兴奋的特性是什么
date: 2017-12-29 17:03:40
categories：
    - Java
tags:
    - Java
---

　　预计发布的Java 9中，最令人兴奋的特性是什么？
有关Java9的消息最近显得有些沉寂，不要被它迷惑了。JDK开发者正在努力朝着下一个版本迈进，计划2015年12月前完成所有功能开发。之后，它会经历严格测试和bug修复以准备它的全面上市，按计划会在2016年9月发布。
<!--more-->
今天我们已经对Java 9中所期待的特性有了一个很清晰的图景。如果Java 8可以被描述为主要是lambdas表达式、streams和API变化的话，那么Java 9就是关于Jigsaw、额外的实用工具和内部的变化。在这篇文章中，收集了一些我们认为是Java 9中最期待的特性——除了通常的猜测之外，Jigsaw项目，承担了打破JRE并对Java核心组件模块化的使命。
这里有一些特性是Java 9中绝对必要了解的，其中的一些已经在早期的发布版本中为你捣鼓做好了准备。
### 1.Java + REPL = jshell
是的。之前我们怀疑Kulla项目是否会在Java 9中准时发布，但现在已得到了官方确认。下一版发布的Java将会有称为jshell的新命令行工具，它会添加本地支持和以Java方式对REPL（交互式解释器）进行推广。意思是说，如果你想只运行几行Java代码，你不必把它包装进一个单独的工程或者方法。
噢，你可以忘掉那些分号了：
```Java
-> 2 + 2
| 表达式的值是4
| 将临时变量$1的类型设为int
```
　　还有一些像REPL加载项一样的替代品会增加到流行的IDE和解决方案中，就像Java REPL网页控制台。但目前为止，还没有官方的或者合适的方式来这么做。jshell在早期的版本中已经可以用了，等着你给它来个测试运行。

### 2、微基准测试要来了

　　由Alexey Shipilev开发的Java微基准测试套件（Java Microbenchmarking Harness）正在其进化的下一阶段，并加入Java作为官方基准解决方案。我们真的很喜欢在Takipi做基准，所以一套标准化的执行方式是我们期待的。

　　JHM是一组用来编译、运行和分析nano/micro/milli/macro基准的套件。当涉及到精确基准评估，对结果产生很大影响的能力将备受关注，比如预热时间和优化。当你以微秒或纳秒计时的情况下尤其如此。所以，如果你想要更加精确的结果来帮助跟踪基准以做出正确的决定，JMH是你的最佳选择——并且现在它已经成为Java 9的同义词了。

### 3、G1会成为新的默认垃圾收集器吗？

　　我们经常听说的一个误解是：Java只有一个垃圾收集器，而事实上它有4个。Java 9中，仍有一个运行提议，关于替换由Java 7引入的G1默认垃圾收集器（并行/吞吐量收集）的讨论。不同收集器之间差别精简概述，可以查看这篇里的文章。

　　通常来说，G1被设计来更好地支持大于4GB的堆，并且不会造成频繁的GC暂停，但当暂停发生时，往往会处理更长时间。最近我们和Outbrain的性能专家Haim Yadid讨论了关于GC的方方面面，来帮助你了解更多各收集器之间不同的权衡。同样，如果你想要深入了解相关讨论，那么hotspot-dev和jdk9-dev的邮件组是个开始学习不错的地方。

### 4、未来是HTTP 2.0

　　官方的HTTP 2.0标准是几个月之前被批准的，基于Google的SPDY算法构建。SPDY已经展示了相对HTTP 1.1巨大的速度提升，范围在11.81%到47.7%之间，并且它已经存在于大多数现代的浏览器中了。Java 9将全面支持HTTP 2.0，并且为Java配备一个全新的HTTP客户端来替代HttpURLConnection，并且同时还实现HTTP 2.0和websockets。

### 5、进程API得到了巨大的推动

　　到目前为止，通过Java来控制和管理操作系统进程能力有限。例如在早期版本的Java中，为了做一些简单的事情，像得到进程PID，要么访问本机代码，要么用某种神奇的临时解决方法。此外，还可能需要一个对于每个平台提供不同实现来保证你得到正确的结果。

　　在Java 9中，除了获取Linux PID的代码，现在都像这样来获取：

```Java
public static void main(String[] args) throws Exception {
    Process proc = Runtime.getRuntime().exec(new String[]{ "/bin/sh", "-c", "echo $PPID" });
    if (proc.waitFor() == 0) {
        InputStream in = proc.getInputStream();
        int available = in.available();
        byte[] outputBytes = new byte[available];
        in.read(outputBytes);
        String pid = new String(outputBytes);
        System.out.println("Your pid is " + pid);
    }
}
```

转向像这样的代码（同样也支持所有的操作系统）：
```Java
System.out.println("Your pid is" + Process.getCurrentPid());
```
　　这一更新将扩展Java与操作系统交互的能力：全新的直接操作PID、进程名和状态的方法，操作JVM线程和进程等等能力。
你不会在Java 9中见到什么？
　　我们以为两个有趣的特性会作为即将到来的Java发布版本中的一部分——但现在我们知道它们将不会出现在这次发布的版本。
　　
    1、一个标准的轻量级JSON API

　　在我们进行的一项对350名开发人员的调查中，JSON API就像Jigsaw一样被大肆宣传，但看起来它好像没在发布版本中，原因可能是资金问题。Mark Reinhold，Java平台的首席架构师，在JDK 9的邮件列表中写到：
“这个JEP对于平台来说是个有益的补充，但长远来看，考虑到资金的因素以及Oracle资助的其它特性，它并不如其它特性一样重要。我们考虑可能在JDK 10或者之后的版本再发布这个JEP。”
　　
    2、金钱和货币API

　　有一条新闻，似乎看起来金钱和货币API也缺少Oracle的支持。这是我们从Anatole Tresch那里得到的答案，这个API的产品推广师：
@tkfxin 目前不会。从Oracle那里没得到支持。取而代之的，我们将提高Java EE支持并且spring也将支持它 :)
– Anatole Tresch (@atsticks) 2015年6月16日
　　我们遗漏了什么吗？请在下面的评论区告诉我们吧。没有空闲时间？来看看何时以及为何在产品中代码会出现失败中断。
