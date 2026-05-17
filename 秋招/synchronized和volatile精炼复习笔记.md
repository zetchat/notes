# synchronized 和 volatile 精炼复习笔记

## 一、synchronized - Java内置锁

### 1. 三种用法

#### 修饰实例方法
```java
public synchronized void sellTicket() {
    // 锁的是当前对象实例 (this)
    // 同一个对象的多个实例同步方法互斥
    // 不同对象的实例同步方法不互斥
}
```

#### 修饰静态方法
```java
public static synchronized void sellTicket() {
    // 锁的是当前类的Class对象 (YourClass.class)
    // 同一个类的所有静态同步方法都互斥
    // 无论通过哪个对象实例调用都互斥
}
```

#### 修饰代码块
```java
public void sellTicket() {
    synchronized (lock) { // 锁的是指定对象
        // 最灵活的方式，可以实现细粒度锁
        // 支持"线程流水线"模式
    }
}
```

### 2. 核心特性

- **原子性**: 保证被包裹的代码块原子执行
- **可见性**: 释放锁时强制刷新到主内存，获取锁时强制从主内存读取
- **可重入锁**: 同一个线程可以多次获取同一个锁
- **非公平锁**: 默认非公平，追求更高吞吐量

### 3. 锁的独立性

- **实例锁**和**类锁**是两把完全不同的锁
- 一个线程获取实例锁，不会阻止另一个线程获取类锁
- 它们井水不犯河水，可以同时执行

### 4. wait() 和 notify()

#### 使用场景
- 用于线程间通信，协调线程执行
- 必须在 synchronized 代码块内使用

#### 线程状态转换
```
[RUNNABLE] --[获取锁失败]--> [BLOCKED] (阻塞，等锁)
    |
    | [调用 wait()]
    v
[WAITING/TIMED_WAITING] (等待，等条件)
    ^
    | [被 notify() 唤醒]
    |
    +--> [重新进入 BLOCKED 状态，等待获取锁]
```

#### 重要区别
- **阻塞状态(BLOCKED)**: 等锁，锁释放后自动唤醒，无需notify
- **等待状态(WAITING)**: 等条件，必须由notify()显式唤醒

#### 超时等待
```java
// 推荐使用超时等待，避免无限等待
while (condition_not_met) {
    try {
        lock.wait(5000); // 最多等5秒
        if (condition_not_met) {
            // 超时处理逻辑
            return;
        }
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
        return;
    }
}
```

### 5. 实践建议

- **优先使用超时等待**: wait(long timeout) 比 wait() 更健壮
- **while循环检查条件**: 防止虚假唤醒
- **手动实现回滚**: synchronized不提供自动回滚，需要try-catch处理

## 二、volatile - 轻量级同步机制

### 1. 两大作用

- **保证可见性**: 修改后立即对其他线程可见
- **禁止指令重排**: 保证代码按书写顺序执行

### 2. 重要限制

- **不保证原子性**: 对于复合操作(如i++)无效
- **适用场景**: "一写多读"或变量更新不依赖当前值

### 3. 可见性演示

```java
class Worker implements Runnable {
    private volatile boolean running = true; // 关键：volatile
    
    public void stop() { this.running = false; }
    
    @Override
    public void run() {
        while (running) {
            // 空转
        }
        System.out.println("安全退出");
    }
}
```

### 4. 原子性演示

```java
public class AtomicityDemo {
    private volatile int count = 0; // 即使volatile，结果仍不正确
    
    public void increment() {
        count++; // 非原子操作：读-改-写
    }
    
    // 多次运行，结果总是小于10000
}
```

## 三、synchronized vs volatile

| 特性     | synchronized       | volatile         |
| -------- | ------------------ | ---------------- |
| 原子性   | ✅ 保证             | ❌ 不保证         |
| 可见性   | ✅ 保证             | ✅ 保证           |
| 有序性   | ✅ 保证             | ✅ 保证(禁止重排) |
| 性能开销 | 较大               | 较小             |
| 适用场景 | 复合操作、复杂同步 | 简单可见性需求   |

## 四、高频面试点

### 1. 锁的对象是什么？
- 实例方法：锁this对象
- 静态方法：锁Class对象
- 代码块：锁指定对象

### 2. 实例锁和类锁的关系？
- 完全独立，互不干扰
- 可以同时获取，不会产生竞争

### 3. synchronized的可重入性？
- 同一个线程可以多次获取同一个锁
- 避免自己锁死自己

### 4. volatile的局限性？
- 只保证可见性，不保证原子性
- 适用于简单场景，复杂场景用synchronized

### 5. wait/notify的使用要求？
- 必须在synchronized代码块内
- 推荐使用超时等待wait(timeout)
- 用while循环检查条件

## 五、最佳实践

1. **优先考虑JUC工具**: BlockingQueue、CountDownLatch等
2. **次选手写wait/notify**: 默认使用超时等待
3. **谨慎使用wait()**: 只在100%确定notify会发生的场景
4. **细粒度锁**: 只锁真正需要同步的代码
5. **防御性编程**: 假设最坏情况会发生，使用超时机制

---

*这份笔记涵盖了synchronized和volatile的核心概念和关键用法，适合快速回顾和面试准备。*
