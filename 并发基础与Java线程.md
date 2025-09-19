# 第一部分：并发基础与Java线程

## 1. 概念辨析：进程 vs. 线程

这是一个宏观概念，我们用一个生动的比喻来快速理解。

- **进程 (Process)**：可以想象成一个正在运行的工厂。操作系统给这个工厂分配了独立的资源，比如土地（内存空间）、电力（CPU时间）、原材料仓库等。工厂之间是相互隔离的。

- **线程 (Thread)**：是工厂里的工人。多个工人可以在同一个工厂里协同工作，他们共享工厂的资源（内存、电力等）。每个工人有自己的小工具箱（程序计数器、虚拟机栈），并且负责执行一条具体的生产线任务。

### 核心区别总结：

| 特性 | 进程 (Process) | 线程 (Thread) |
|------|----------------|---------------|
| 基本单位 | 操作系统进行资源分配的基本单位 | CPU进行调度和执行的基本单位 |
| 资源拥有 | 拥有独立的内存空间、文件句柄等资源 | 共享所在进程的内存和资源，但有自己独立的栈和程序计数器 |
| 开销 | 创建和销毁的开销大 | 创建和销毁的开销小，是"轻量级"的 |
| 通信 | 进程间通信（IPC）复杂且开销大 | 线程间通信简单，因为共享内存，但需要处理同步问题 |
| 隔离性 | 一个进程崩溃不会影响其他进程 | 一个线程崩溃（如未捕获的异常）会导致整个进程崩溃 |

**一句话总结**：线程是运行在进程内部的、更轻量级的执行单元。

## 2. Java线程创建 (重点)

Java 提供了多种创建线程的方式，核心思想都是定义任务（做什么事）和创建线程（谁来做事）。

### 方式一：继承 Thread 类 (不推荐)

```java
class MyThread extends Thread {
    @Override
    public void run() { // run()方法定义了线程要执行的任务
        System.out.println("通过继承 Thread 类创建线程: " + Thread.currentThread().getName());
    }
}
// 启动： new MyThread().start();
```

**缺点**：Java 是单继承的，如果你的类继承了 Thread，就不能再继承其他类了，扩展性差。

### 方式二：实现 Runnable 接口 (推荐)

这是最常用、最推荐的方式，它将"任务"和"线程"解耦。

```java
class MyRunnable implements Runnable {
    @Override
    public void run() { // 同样是定义任务
        System.out.println("通过实现 Runnable 接口创建线程: " + Thread.currentThread().getName());
    }
}
// 启动：
// 1. 创建任务实例
Runnable myRunnable = new MyRunnable();
// 2. 创建线程实例，并将任务传递进去
Thread thread = new Thread(myRunnable);
// 3. 启动线程
thread.start();
```

**优点**：任务（Runnable）与执行者（Thread）分离，类可以继续继承其他父类，更灵活。多个线程可以共享同一个 Runnable 实例。

### 方式三：实现 Callable 接口 (需要获取返回结果时使用)

当你的任务需要一个返回值，或者可能会抛出异常时，使用 Callable。

```java
import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

class MyCallable implements Callable<String> { // <String> 是返回值的类型
    @Override
    public String call() throws Exception { // 方法名是 call()，可以有返回值，可以抛异常
        System.out.println("通过实现 Callable 接口创建线程: " + Thread.currentThread().getName());
        Thread.sleep(1000); // 模拟耗时操作
        return "任务执行完毕";
    }
}
// 启动并获取结果：
// 1. 创建 Callable 任务实例
Callable<String> myCallable = new MyCallable();
// 2. 使用 FutureTask 包装 Callable，FutureTask 同时实现了 Runnable 和 Future 接口
FutureTask<String> futureTask = new FutureTask<>(myCallable);
// 3. 创建线程实例，传入 FutureTask
Thread thread = new Thread(futureTask);
// 4. 启动线程
thread.start();
// 5. 获取结果 (futureTask.get() 会阻塞，直到任务执行完毕)
String result = futureTask.get();
System.out.println("获取到任务结果: " + result);
```

**优点**：功能强大，可以获得任务的执行结果或捕获其抛出的异常。是后续学习线程池和 Future 模式的基础。

## 3. 线程生命周期 (状态转换)

一个Java线程在其生命周期中，会处于以下几种状态之一。你可以通过 `thread.getState()` 方法获取。

- **NEW (新建)**: `new Thread()` 之后，`start()` 之前，线程对象已创建但尚未启动。

- **RUNNABLE (可运行)**: 调用 `start()` 方法后。这个状态包含了操作系统中的 Running (运行中) 和 Ready (就绪) 两种状态。线程可能正在CPU上执行，也可能在等待CPU分配时间片。

- **BLOCKED (阻塞)**: 线程等待获取一个监视器锁 (monitor lock) 时进入此状态。通常是进入 `synchronized` 修饰的代码块或方法时，发现锁被其他线程占用。

- **WAITING (无限期等待)**: 线程等待其他线程执行特定操作。以下方法会使线程进入此状态：
  - `Object.wait()` (没有超时)
  - `Thread.join()` (没有超时)
  - `LockSupport.park()`

- **TIMED_WAITING (限期等待)**: 和 WAITING 类似，但有等待时间。时间一到会自动唤醒。
  - `Thread.sleep(long millis)`
  - `Object.wait(long timeout)`
  - `Thread.join(long millis)`

- **TERMINATED (终止)**: 线程的 `run()` 方法执行完毕，或者因未捕获的异常而退出。

## 4. 线程控制方法

### `start()` vs `run()` (面试高频)

- **`thread.start()`**: 启动一个新线程，使其进入 RUNNABLE 状态，由JVM进行调度执行。这才是真正的多线程。

- **`thread.run()`**: 只是一个普通的方法调用，在当前线程中执行 `run()` 方法体，不会创建新线程。

### `sleep(long millis)`:

- 静态方法 `Thread.sleep()`。
- 使当前线程进入 TIMED_WAITING 状态，暂停执行指定时间。
- 不会释放它已经持有的锁。

### `yield()`:

- 静态方法 `Thread.yield()`。
- 一个"建议"，告诉CPU调度器："我愿意让出CPU，给其他同优先级的线程一个机会"。
- 调度器可以忽略这个建议。调用后线程从 Running 变为 Ready，但可能马上又被调度回来。

### `join()`:

- 实例方法 `threadB.join()`。
- 在线程A中调用 `threadB.join()`，意味着线程A会进入 WAITING 状态，直到线程B执行完毕 (TERMINATED)。
- 常用于等待子线程执行完成后，主线程再继续执行的场景。

### `interrupt()` (理解中断机制)

- 它不是强制停止一个线程，而是一种协作式的"通知"机制。
- `thread.interrupt()`: 设置目标线程的中断标志位为 true。
- 如果线程正处于 `sleep()`, `wait()`, `join()` 等阻塞状态，调用 `interrupt()` 会使它抛出 `InterruptedException`，并清除中断标志位。
- 如果线程正在正常运行，它需要自己通过 `Thread.currentThread().isInterrupted()` 来检查中断标志，并决定如何响应（通常是优雅地退出 `run()` 方法）。

## 5. 动手实践

下面是一个完整的示例，演示了多种线程创建方式和 `join()` 的使用。请你将它复制到你的IDE中运行，并观察控制台的输出顺序。

```java
import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

public class ThreadCreationDemo {

    // 方式一：继承 Thread
    static class MyThread extends Thread {
        @Override
        public void run() {
            System.out.println("1. 我是 MyThread，继承了 Thread 类。线程名: " + Thread.currentThread().getName());
        }
    }

    // 方式二：实现 Runnable
    static class MyRunnable implements Runnable {
        @Override
        public void run() {
            System.out.println("2. 我是 MyRunnable，实现了 Runnable 接口。线程名: " + Thread.currentThread().getName());
        }
    }

    // 方式三：实现 Callable
    static class MyCallable implements Callable<String> {
        @Override
        public String call() throws Exception {
            System.out.println("3. 我是 MyCallable，实现了 Callable 接口。线程名: " + Thread.currentThread().getName());
            Thread.sleep(1000); // 模拟耗时
            return "Callable 执行完成!";
        }
    }

    public static void main(String[] args) throws Exception {
        System.out.println("Main线程开始执行...");

        // 启动方式一
        MyThread myThread = new MyThread();
        myThread.start();

        // 启动方式二
        MyRunnable myRunnable = new MyRunnable();
        Thread threadFromRunnable = new Thread(myRunnable, "Runnable线程"); // 可以指定线程名
        threadFromRunnable.start();
        
        // 启动方式三
        MyCallable myCallable = new MyCallable();
        FutureTask<String> futureTask = new FutureTask<>(myCallable);
        Thread threadFromCallable = new Thread(futureTask, "Callable线程");
        threadFromCallable.start();

        // 使用 join() 等待 myThread 执行完毕
        // 如果不加 join，"MyThread 执行完毕" 这句话可能在 "Main线程即将结束" 之后打印
        myThread.join();
        System.out.println("Main线程确认：MyThread 已执行完毕。");

        // 获取 Callable 的结果，get() 方法会阻塞
        String result = futureTask.get();
        System.out.println("Main线程获取到 Callable 结果: " + result);

        System.out.println("Main线程即将结束...");
    }
}
```

## 实验与思考：

1. **多次运行上面的代码，观察前三个线程的输出顺序是否总是一样的？**（提示：大概率不一样，这体现了线程调度的不确定性）

2. **尝试把 `myThread.join();` 这行代码注释掉，再运行几次，观察 "Main线程确认" 这句话和 "我是 MyThread" 这句话的打印顺序有什么变化？**

3. **如果将 `myThread.start()` 改为 `myThread.run()`，会发生什么？**（提示：所有代码都将在 main 线程中顺序执行）。
