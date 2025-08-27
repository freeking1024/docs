# JMM内存模型(Java Memory Model)
> 每个OS有自己的内存模型，跨平台特性需要Java提供一套内存模型屏蔽系统差异

`Goals`：屏蔽硬件差异、提供编程模型、平衡性能和正确性。
<img src="https://oss.javaguide.cn/github/javaguide/java/concurrent/jmm.png" width = "500px">
## 三大问题：可见性、原子性、有序性

> 简化多线程编程

1. 可见性
    - `sychronized`
    - `volatile`
    - `final`
2. 原子性
    - `sychronized`
    - `java.util.concurrent.atomic`
3. 有序性
    - `volatile`
    - `sychronized`

## happens-before
> 准确的说，happens-before原则只是保证了前一个操作对后一个操作的可见性，即使两个操作不在一个线程里。

A发生于B之前，A的执行结果对B可见。<br/>
如果A、B进行重排序后执行结果一致，则JMM允许这样重排序。