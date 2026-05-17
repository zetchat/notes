# ArrayDeque 面试准备笔记

## 📚 核心概念

### 1. 基本定义
- **ArrayDeque**：Java集合框架中的双端队列实现
- **实现接口**：`Deque<E>`、`Cloneable`、`Serializable`
- **继承关系**：`AbstractCollection<E>` → `ArrayDeque<E>`
- **底层结构**：基于数组的循环队列

### 2. 关键特性
- ✅ **双端操作**：支持队首和队尾的添加、删除、查看操作
- ✅ **动态扩容**：初始容量16，按2的幂次方扩容
- ✅ **高性能**：添加删除操作平均O(1)时间复杂度
- ❌ **非线程安全**：多线程环境需要外部同步
- ❌ **不支持随机访问**：没有get(index)方法

## 🏗️ 数据结构详解

### 底层实现原理
```java
// 核心成员变量
transient Object[] elements;    // 存储元素的数组
transient int head;             // 队首指针
transient int tail;             // 队尾指针
```

### 扩容机制
```java
// 扩容策略：2的幂次方
private void doubleCapacity() {
    int p = head;
    int n = elements.length;
    int r = n - p; // 右半部分元素数量
    int newCapacity = n << 1;   // 左移1位，相当于*2
    // ... 复制元素逻辑
}
```

## 🔧 核心方法分类

### 添加元素方法
| 方法 | 队首添加 | 队尾添加 | 返回值 | 异常处理 |
|------|----------|----------|--------|----------|
| `add(E e)` | ❌ | ✅ | boolean | 可能抛出异常 |
| `offer(E e)` | ❌ | ✅ | boolean | 不抛异常 |
| `addFirst(E e)` | ✅ | ❌ | void | 可能抛出异常 |
| `offerFirst(E e)` | ✅ | ❌ | boolean | 不抛异常 |
| `addLast(E e)` | ❌ | ✅ | void | 可能抛出异常 |
| `offerLast(E e)` | ❌ | ✅ | boolean | 不抛异常 |

### 获取元素方法
| 方法 | 操作位置 | 是否移除 | 返回值 | 异常处理 |
|------|----------|----------|--------|----------|
| `getFirst()` | 队首 | ❌ | E | 空队列抛异常 |
| `peekFirst()` | 队首 | ❌ | E | 空队列返回null |
| `getLast()` | 队尾 | ❌ | E | 空队列抛异常 |
| `peekLast()` | 队尾 | ❌ | E | 空队列返回null |

### 移除元素方法
| 方法 | 操作位置 | 返回值 | 异常处理 |
|------|----------|--------|----------|
| `removeFirst()` | 队首 | E | 空队列抛异常 |
| `pollFirst()` | 队首 | E | 空队列返回null |
| `removeLast()` | 队尾 | E | 空队列抛异常 |
| `pollLast()` | 队尾 | E | 空队列返回null |

## 🎯 使用场景

### 1. 作为栈使用
```java
ArrayDeque<String> stack = new ArrayDeque<>();
stack.push("元素1");        // 压栈
stack.push("元素2");
String top = stack.pop();   // 弹栈
String peek = stack.peek(); // 查看栈顶
```

### 2. 作为队列使用
```java
ArrayDeque<String> queue = new ArrayDeque<>();
queue.offer("任务1");       // 入队
queue.offer("任务2");
String task = queue.poll(); // 出队
String peek = queue.peek(); // 查看队首
```

### 3. 作为双端队列使用
```java
ArrayDeque<String> deque = new ArrayDeque<>();
deque.addFirst("头部");     // 队首添加
deque.addLast("尾部");      // 队尾添加
String head = deque.getFirst(); // 获取队首
String tail = deque.getLast();  // 获取队尾
```

## ⚡ 性能分析

### 时间复杂度
- **添加/删除**：O(1) 平均情况
- **查找元素**：O(n) 需要遍历
- **获取大小**：O(1) 直接返回size字段

### 空间复杂度
- **内存效率**：比LinkedList高（数组vs节点）
- **扩容开销**：2的幂次方扩容，减少哈希冲突

## 🔄 与LinkedList对比

| 特性 | ArrayDeque | LinkedList |
|------|------------|------------|
| **底层结构** | 数组 | 双向链表 |
| **内存效率** | 更高 | 较低 |
| **随机访问** | 不支持 | 支持 |
| **插入删除** | O(1) | O(1) |
| **遍历性能** | 更好 | 较差 |
| **扩容策略** | 2的幂次方 | 按需增长 |

## 🚨 注意事项

### 1. 线程安全问题
```java
// ❌ 错误用法
ArrayDeque<String> deque = new ArrayDeque<>();
// 多线程并发访问...

// ✅ 正确用法
synchronized(deque) {
    deque.add("元素");
}

// 或者使用线程安全版本
Deque<String> safeDeque = Collections.synchronizedDeque(new ArrayDeque<>());
```

### 2. 空值处理
```java
ArrayDeque<String> deque = new ArrayDeque<>();
deque.add(null);  // 允许null值

// 操作前检查
if (deque.peek() != null) {
    String element = deque.poll();
}
```

### 3. 容量选择
```java
// 如果知道大概元素数量，指定初始容量
ArrayDeque<String> deque = new ArrayDeque<>(1000); // 避免频繁扩容
```

## 💡 面试高频考点

### 1. 为什么选择数组而不是链表？
- **内存效率**：数组连续内存，缓存友好
- **扩容策略**：2的幂次方扩容，减少哈希冲突
- **遍历性能**：数组遍历比链表快

### 2. 如何实现循环队列？
- 使用head和tail指针
- 当tail到达数组末尾时，从0开始
- 通过(head + size) % capacity计算实际位置

### 3. 扩容时机和策略？
- 当size == capacity时触发扩容
- 新容量 = 旧容量 << 1（左移1位，相当于*2）
- 重新排列元素，保持顺序

### 4. 与Stack类的区别？
- **Stack继承Vector**：线程安全但性能较差
- **ArrayDeque实现Deque**：性能更好，推荐使用
- **Java官方建议**：优先使用ArrayDeque而不是Stack

## 🔍 常见面试题

### 1. 实现滑动窗口最大值
### 2. 括号匹配问题
### 3. 用队列实现栈
### 4. 用栈实现队列
### 5. 单调队列的应用

## 📝 总结要点

1. **ArrayDeque是双端队列的最佳实现**
2. **既可以作为栈也可以作为队列使用**
3. **性能优秀，内存效率高**
4. **非线程安全，多线程需要同步**
5. **适合需要两端操作的场景**
6. **在算法题和实际开发中广泛应用**

---

*记住：ArrayDeque是Java中实现双端队列的标准选择，掌握它的使用对于面试和实际开发都非常重要！*


