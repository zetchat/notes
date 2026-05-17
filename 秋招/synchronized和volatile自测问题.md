# synchronized 和 volatile 自测问题

## 基础概念题 (每题10分，共40分)

### 1. synchronized 的三种用法
**问题**: 请写出synchronized的三种用法，并说明每种用法锁的对象是什么？

**参考答案**:
- 修饰实例方法：锁的是当前对象实例(this)
- 修饰静态方法：锁的是当前类的Class对象(YourClass.class)  
- 修饰代码块：锁的是指定对象

### 2. volatile 的作用
**问题**: volatile关键字有哪两大作用？它能否保证原子性？

**参考答案**:
- 保证可见性：修改后立即对其他线程可见
- 禁止指令重排：保证代码按书写顺序执行
- 不能保证原子性，对于复合操作(如i++)无效

### 3. 线程状态转换
**问题**: 当线程调用wait()后进入什么状态？被notify()唤醒后又进入什么状态？

**参考答案**:
- 调用wait()后进入WAITING状态
- 被notify()唤醒后先进入BLOCKED状态等待获取锁
- 获取锁成功后进入RUNNABLE状态

### 4. 锁的独立性
**问题**: 一个线程获取了实例锁，另一个线程能否同时获取类锁？为什么？

**参考答案**:
- 可以同时获取
- 因为实例锁和类锁是两把完全不同的锁
- 它们锁的对象不同，互不干扰

## 进阶理解题 (每题15分，共45分)

### 5. 可重入锁机制
**问题**: 什么是可重入锁？请用代码说明synchronized的可重入性。

**参考答案**:
```java
public class ReentrantDemo {
    public synchronized void method1() {
        System.out.println("method1");
        method2(); // 同一个线程可以再次获取同一个锁
    }
    
    public synchronized void method2() {
        System.out.println("method2");
    }
}
```

### 6. 原子性vs可见性
**问题**: 以下代码中，volatile能否保证最终结果是10000？为什么？

```java
public class Counter {
    private volatile int count = 0;
    
    public void increment() {
        count++; // 这个操作是原子的吗？
    }
}
```

**参考答案**:
- 不能保证结果是10000
- count++包含三个步骤：读-改-写，不是原子操作
- volatile只保证可见性，不保证原子性
- 需要synchronized或AtomicInteger来保证原子性

### 7. wait/notify的正确使用
**问题**: 以下代码有什么问题？如何修复？

```java
public class ProducerConsumer {
    private Queue<String> queue = new LinkedList<>();
    
    public void produce(String item) {
        queue.offer(item);
        notify(); // 问题：没有synchronized
    }
    
    public String consume() {
        if (queue.isEmpty()) {
            wait(); // 问题：没有synchronized
        }
        return queue.poll();
    }
}
```

**参考答案**:
- 问题：wait()和notify()必须在synchronized代码块内使用
- 修复：用synchronized包裹相关代码
- 还需要while循环检查条件，防止虚假唤醒

## 实战应用题 (每题15分，共45分)

### 8. 多线程卖票问题
**问题**: 请用三种不同的synchronized方式实现线程安全的卖票程序。

**要求**:
- 总共100张票
- 3个窗口同时卖票
- 不能出现重复卖票或负数票

**参考答案**:
```java
// 方式1：同步代码块
class Ticket implements Runnable {
    private int ticketCount = 100;
    private final Object lock = new Object();
    
    @Override
    public void run() {
        while (true) {
            synchronized (lock) {
                if (ticketCount > 0) {
                    System.out.println(Thread.currentThread().getName() + " 卖第 " + ticketCount-- + " 张票");
                } else {
                    break;
                }
            }
        }
    }
}

// 方式2：同步实例方法
class Ticket implements Runnable {
    private int ticketCount = 100;
    
    @Override
    public void run() {
        while (ticketCount > 0) {
            sellOneTicket();
        }
    }
    
    private synchronized void sellOneTicket() {
        if (ticketCount > 0) {
            System.out.println(Thread.currentThread().getName() + " 卖第 " + ticketCount-- + " 张票");
        }
    }
}

// 方式3：同步静态方法
class Ticket implements Runnable {
    private static int ticketCount = 100;
    
    @Override
    public void run() {
        while (ticketCount > 0) {
            sellOneTicket();
        }
    }
    
    private static synchronized void sellOneTicket() {
        if (ticketCount > 0) {
            System.out.println(Thread.currentThread().getName() + " 卖第 " + ticketCount-- + " 张票");
        }
    }
}
```

### 9. 生产者-消费者模式
**问题**: 实现一个生产者-消费者模式，要求：
- 使用wait/notify机制
- 队列大小为5
- 生产者生产10个产品后停止
- 消费者消费完所有产品后停止

**参考答案**:
```java
public class ProducerConsumerDemo {
    private static final int QUEUE_SIZE = 5;
    private static final int TOTAL_PRODUCTS = 10;
    
    private Queue<Integer> queue = new LinkedList<>();
    private int producedCount = 0;
    private int consumedCount = 0;
    
    public void produce() throws InterruptedException {
        synchronized (queue) {
            while (queue.size() >= QUEUE_SIZE) {
                queue.wait(); // 队列满了，等待
            }
            queue.offer(++producedCount);
            System.out.println("生产: " + producedCount);
            queue.notify(); // 通知消费者
        }
    }
    
    public void consume() throws InterruptedException {
        synchronized (queue) {
            while (queue.isEmpty() && consumedCount < TOTAL_PRODUCTS) {
                queue.wait(); // 队列空了，等待
            }
            if (!queue.isEmpty()) {
                int product = queue.poll();
                consumedCount++;
                System.out.println("消费: " + product);
                queue.notify(); // 通知生产者
            }
        }
    }
}
```

### 10. 线程安全计数器
**问题**: 实现一个线程安全的计数器，要求：
- 支持increment()和getCount()方法
- 分别用synchronized、volatile+AtomicInteger、ReentrantLock三种方式实现
- 测试多线程并发访问的正确性

**参考答案**:
```java
// 方式1：synchronized
public class SynchronizedCounter {
    private int count = 0;
    
    public synchronized void increment() {
        count++;
    }
    
    public synchronized int getCount() {
        return count;
    }
}

// 方式2：volatile + AtomicInteger
public class AtomicCounter {
    private volatile AtomicInteger count = new AtomicInteger(0);
    
    public void increment() {
        count.incrementAndGet();
    }
    
    public int getCount() {
        return count.get();
    }
}

// 方式3：ReentrantLock
public class LockCounter {
    private int count = 0;
    private final ReentrantLock lock = new ReentrantLock();
    
    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }
    
    public int getCount() {
        lock.lock();
        try {
            return count;
        } finally {
            lock.unlock();
        }
    }
}
```

## 综合思考题 (每题20分，共40分)

### 11. 锁的性能优化
**问题**: 在一个高并发系统中，如何优化synchronized的使用来提升性能？

**参考答案**:
- 减小锁的范围：只锁真正需要同步的代码
- 使用细粒度锁：不同资源用不同的锁对象
- 考虑读写锁：读多写少的场景用ReadWriteLock
- 使用原子类：简单操作用AtomicInteger等
- 避免锁的嵌套：防止死锁

### 12. 死锁预防
**问题**: 如何预防死锁？请用代码说明。

**参考答案**:
- 按序加锁：所有线程按相同顺序获取锁
- 使用超时机制：避免无限等待
- 一次性获取所有锁：减少锁的持有时间

```java
public class DeadlockPrevention {
    private final Object lock1 = new Object();
    private final Object lock2 = new Object();
    
    // 错误方式：可能死锁
    public void wrongWay() {
        synchronized (lock1) {
            synchronized (lock2) {
                // 业务逻辑
            }
        }
    }
    
    // 正确方式：按序加锁
    public void rightWay() {
        synchronized (lock1) {
            synchronized (lock2) {
                // 业务逻辑
            }
        }
    }
    
    // 另一种正确方式：一次性获取所有锁
    public void betterWay() {
        synchronized (lock1) {
            synchronized (lock2) {
                // 业务逻辑
            }
        }
    }
}
```

## 评分标准

- **90-100分**: 优秀，对并发编程有深入理解
- **80-89分**: 良好，掌握了核心概念和基本应用
- **70-79分**: 及格，理解了基本用法，但需要加强实践
- **60-69分**: 需要复习，对核心概念理解不够深入
- **60分以下**: 建议重新学习基础概念

## 自测建议

1. **独立完成**: 不要看答案，先自己思考
2. **代码实践**: 对于编程题，一定要动手写代码
3. **理解原理**: 不仅要知道怎么做，更要理解为什么
4. **查漏补缺**: 根据得分情况，重点复习薄弱环节

---

*祝你在并发编程的学习中取得好成绩！*
