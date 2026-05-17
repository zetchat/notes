# TreeMap自测问题

## 📝 基础概念题

### 1. 数据结构相关
**Q1: TreeMap的底层数据结构是什么？有什么特点？**
- [ ] A. 数组 + 链表
- [ ] B. 红黑树
- [ ] C. 哈希表
- [ ] D. 双向链表

**Q2: TreeMap的时间复杂度是多少？**
- [ ] A. 查找O(1)，插入删除O(n)
- [ ] B. 查找O(log n)，插入删除O(log n)
- [ ] C. 查找O(n)，插入删除O(1)
- [ ] D. 查找O(1)，插入删除O(1)

**Q3: TreeMap的空间复杂度是多少？**
- [ ] A. O(1)
- [ ] B. O(log n)
- [ ] C. O(n)
- [ ] D. O(n²)

### 2. 有序性相关
**Q4: TreeMap默认按照什么顺序排列？**
- [ ] A. 插入顺序
- [ ] B. 键的自然顺序
- [ ] C. 值的自然顺序
- [ ] D. 随机顺序

**Q5: 如何让TreeMap按照自定义顺序排列？**
- [ ] A. 使用Collections.sort()
- [ ] B. 实现Comparable接口
- [ ] C. 传入Comparator比较器
- [ ] D. 使用Arrays.sort()

## 🔧 构造方法题

### 3. 构造方法
**Q6: 以下哪个构造方法可以创建按字符串长度排序的TreeMap？**
```java
// A.
TreeMap<String, Integer> map1 = new TreeMap<>();

// B.
TreeMap<String, Integer> map2 = new TreeMap<>((a, b) -> a.length() - b.length());

// C.
TreeMap<String, Integer> map3 = new TreeMap<>(String::compareTo);

// D.
TreeMap<String, Integer> map4 = new TreeMap<>(new HashMap<>());
```

**Q7: 从现有Map创建TreeMap的正确方式是？**
- [ ] A. `new TreeMap<>(existingMap)`
- [ ] B. `TreeMap.copyOf(existingMap)`
- [ ] C. `Collections.copy(existingMap, new TreeMap<>())`
- [ ] D. `new TreeMap<>(existingMap.entrySet())`

## 🎯 核心方法题

### 4. 基本操作
**Q8: 以下代码的输出是什么？**
```java
TreeMap<String, Integer> map = new TreeMap<>();
map.put("banana", 2);
map.put("apple", 1);
map.put("cherry", 3);

for (String key : map.keySet()) {
    System.out.print(key + " ");
}
```
- [ ] A. "banana apple cherry "
- [ ] B. "apple banana cherry "
- [ ] C. "cherry apple banana "
- [ ] D. "apple cherry banana "

**Q9: TreeMap的键可以为null吗？**
- [ ] A. 可以，键和值都可以为null
- [ ] B. 不可以，键不能为null，值可以为null
- [ ] C. 可以，键可以为null，值不能为null
- [ ] D. 不可以，键和值都不能为null

### 5. 有序操作
**Q10: 以下代码的输出是什么？**
```java
TreeMap<Integer, String> map = new TreeMap<>();
map.put(10, "ten");
map.put(20, "twenty");
map.put(30, "thirty");
map.put(40, "forty");

System.out.println(map.firstKey() + " " + map.lastKey());
```
- [ ] A. "10 40"
- [ ] B. "40 10"
- [ ] C. "ten forty"
- [ ] D. 编译错误

**Q11: 以下哪个方法可以获取小于指定键的最大键？**
- [ ] A. `lowerKey(K key)`
- [ ] B. `higherKey(K key)`
- [ ] C. `floorKey(K key)`
- [ ] D. `ceilingKey(K key)`

**Q12: 以下哪个方法可以获取大于等于指定键的最小键？**
- [ ] A. `lowerKey(K key)`
- [ ] B. `higherKey(K key)`
- [ ] C. `floorKey(K key)`
- [ ] D. `ceilingKey(K key)`

### 6. 子Map操作
**Q13: 以下代码的输出是什么？**
```java
TreeMap<Integer, String> map = new TreeMap<>();
map.put(10, "A");
map.put(20, "B");
map.put(30, "C");
map.put(40, "D");

SortedMap<Integer, String> subMap = map.subMap(15, 35);
System.out.println(subMap.size());
```
- [ ] A. 0
- [ ] B. 1
- [ ] C. 2
- [ ] D. 3

**Q14: `headMap(K toKey)`方法返回什么？**

- [ ] A. 小于等于toKey的所有键值对
- [ ] B. 小于toKey的所有键值对
- [ ] C. 大于toKey的所有键值对
- [ ] D. 大于等于toKey的所有键值对

## 💡 应用场景题

### 7. 实际应用
**Q15: 以下哪个场景最适合使用TreeMap？**
- [ ] A. 需要频繁随机访问的缓存系统
- [ ] B. 需要按分数排序的排行榜系统
- [ ] C. 需要保持插入顺序的日志系统
- [ ] D. 需要快速查找的用户管理系统

**Q16: 实现一个按分数降序排列的排行榜，应该使用什么比较器？**
- [ ] A. `Comparator.naturalOrder()`
- [ ] B. `Collections.reverseOrder()`
- [ ] C. `(a, b) -> a.compareTo(b)`
- [ ] D. `(a, b) -> b.compareTo(a)`

## ⚠️ 注意事项题

### 8. 陷阱和注意事项
**Q17: 以下代码有什么问题？**
```java
class Student {
    private String name;
    private int score;
    
    public Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
    
    public void setScore(int score) {
        this.score = score;
    }
    
    // getters...
}

TreeMap<Student, String> map = new TreeMap<>((a, b) -> a.getScore() - b.getScore());
Student s1 = new Student("Alice", 85);
map.put(s1, "Good");
s1.setScore(95); // 修改分数
```
- [ ] A. 没有编译错误，运行正常
- [ ] B. 会抛出ConcurrentModificationException
- [ ] C. TreeMap内部结构可能不一致
- [ ] D. 会抛出IllegalArgumentException

**Q18: 比较器必须满足什么条件？**

- [ ] A. 必须实现Serializable接口
- [ ] B. 必须满足一致性：compare(k1, k2) == 0 当且仅当 equals(k1, k2) == true
- [ ] C. 必须返回正数、负数或零
- [ ] D. 必须实现Comparable接口

## 🔄 性能优化题

### 9. 性能相关
**Q19: 以下哪种操作在TreeMap中性能最好？**
- [ ] A. 频繁的插入和删除操作
- [ ] B. 批量插入操作
- [ ] C. 随机访问操作
- [ ] D. 范围查询操作

**Q20: 如何优化TreeMap的比较器性能？**
- [ ] A. 在比较器中执行复杂计算
- [ ] B. 避免在比较器中执行复杂计算
- [ ] C. 使用反射获取字段值
- [ ] D. 每次都创建新的比较器对象

## 🚀 高级特性题

### 10. 线程安全和并发
**Q21: TreeMap是线程安全的吗？**

- [ ] A. 是，完全线程安全
- [ ] B. 不是，需要外部同步
- [ ] C. 是，但只在读操作时线程安全
- [ ] D. 不是，但内部有锁机制

**Q22: 如何让TreeMap变成线程安全的？**
- [ ] A. 使用synchronized关键字
- [ ] B. 使用Collections.synchronizedSortedMap()
- [ ] C. 使用ConcurrentHashMap
- [ ] D. 使用ReentrantReadWriteLock

**Q23: 如果需要线程安全的有序Map，应该使用什么？**
- [ ] A. TreeMap + synchronized
- [ ] B. HashMap + Collections.synchronizedMap()
- [ ] C. ConcurrentSkipListMap
- [ ] D. LinkedHashMap + ReentrantLock

## 📊 比较分析题

### 11. 与其他Map比较
**Q24: TreeMap和HashMap的主要区别是什么？**
- [ ] A. TreeMap有序，HashMap无序
- [ ] B. TreeMap线程安全，HashMap非线程安全
- [ ] C. TreeMap性能更好，HashMap性能较差
- [ ] D. TreeMap内存占用更少，HashMap内存占用更多

**Q25: 以下场景应该选择哪个Map实现？**
**场景：需要按键的自然顺序遍历，且需要频繁的查找操作**
- [ ] A. HashMap
- [ ] B. TreeMap
- [ ] C. LinkedHashMap
- [ ] D. ConcurrentHashMap

## 🔍 代码分析题

### 12. 代码理解
**Q26: 分析以下代码的输出**
```java
TreeMap<String, Integer> map = new TreeMap<>((a, b) -> {
    if (a.length() != b.length()) {
        return a.length() - b.length();
    }
    return a.compareTo(b);
});

map.put("a", 1);
map.put("bb", 2);
map.put("ccc", 3);
map.put("d", 4);

for (String key : map.keySet()) {
    System.out.print(key + " ");
}
```

**Q27: 以下代码会抛出什么异常？**
```java
TreeMap<String, Integer> map = new TreeMap<>();
map.put(null, 1);  // 这行会怎样？
```

## 📝 答案和解析

### 答案：
1. B - 红黑树
2. B - 查找O(log n)，插入删除O(log n)
3. C - O(n)
4. B - 键的自然顺序
5. C - 传入Comparator比较器
6. B - 使用lambda表达式比较器
7. A - `new TreeMap<>(existingMap)`
8. B - "apple banana cherry "
9. B - 键不能为null，值可以为null
10. A - "10 40"
11. A - `lowerKey(K key)`
12. D - `ceilingKey(K key)`
13. C - 2 (包含20和30)
14. B - 小于toKey的所有键值对
15. B - 需要按分数排序的排行榜系统
16. B - `Collections.reverseOrder()`
17. C - TreeMap内部结构可能不一致
18. B - 必须满足一致性条件
19. D - 范围查询操作
20. B - 避免在比较器中执行复杂计算
21. B - 不是，需要外部同步
22. B - 使用Collections.synchronizedSortedMap()
23. C - ConcurrentSkipListMap
24. A - TreeMap有序，HashMap无序
25. B - TreeMap
26. 输出：a d bb ccc (按长度排序，长度相同时按字典序)
27. NullPointerException

```bash
Q21: TreeMap是线程安全的吗？

A. 是，完全线程安全

B. 不是，需要外部同步

C. 是，但只在读操作时线程安全

    D. 不是，但内部有锁机制

正确答案：B
选项分析：

    A. 是，完全线程安全 (错误)

        这是最直接的错误。Java 集合框架中，java.util 包下的绝大部分集合类，如 ArrayList, LinkedList, HashMap, HashSet, TreeMap 等，都是非线程安全的。

        原因：设计它们的主要目标是单线程环境下的性能。增加线程安全机制（如锁）会带来性能开销，而在绝大多数不需要并发的场景下，这种开销是不必要的。

    B. 不是，需要外部同步 (正确)

        这准确地描述了 TreeMap 的特性。因为它内部没有任何用于并发控制的机制，所以如果在多线程环境下共享一个 TreeMap 实例，并且至少有一个线程会修改它（添加、删除元素），那么必须由**调用者（外部代码）**来保证同步。

        如果不进行同步，可能会导致数据不一致、无限循环，或者在迭代时抛出 ConcurrentModificationException。

    C. 是，但只在读操作时线程安全 (错误)

        这个说法具有迷惑性，但依然是错的。即使所有线程都只进行读操作，也可能存在问题。一个线程正在读取时，另一个线程的修改操作（即使是无锁的）可能会导致第一个线程读到处于中间状态、不一致的数据。这被称为“内存可见性”问题。一个线程的修改可能没有及时刷新到主内存，导致其他线程看不到最新的数据。

    D. 不是，但内部有锁机制 (错误)

        TreeMap 内部没有任何锁机制。有内部锁机制的是 java.util.Hashtable 和 java.util.Vector 这两个早期遗留的线程安全类，它们的方法都用了 synchronized 关键字，但性能较差。

一句话总结 Q21：java.util.TreeMap 为追求单线程性能而设计，完全不考虑线程安全，需要用户自己从外部加锁。
Q22: 如何让TreeMap变成线程安全的？

A. 使用synchronized关键字

B. 使用Collections.synchronizedSortedMap()

C. 使用ConcurrentHashMap

    D. 使用ReentrantReadWriteLock

正确答案：B
选项分析：

    A. 使用synchronized关键字 (不准确/不完整)

        虽然 synchronized 是实现线程安全的核心工具，但这个选项太模糊了。它只是一个关键字，而不是一个完整的解决方案。你需要用它来修饰方法或者创建一个同步代码块，手动包裹住所有对 TreeMap 的访问。这当然可行，但容易出错（比如忘记锁某个操作），而且 Collections 工具类已经提供了更优雅、标准的封装。

    B. 使用Collections.synchronizedSortedMap() (最标准的答案)

        这是 Java 官方提供的标准“包装器”方案。Collections 工具类提供了一系列 synchronizedXxx() 方法，用于将非线程安全的集合包装成线程安全的版本。

        Collections.synchronizedSortedMap(new TreeMap<>()) 会返回一个 SortedMap 的线程安全实现。它的内部实现原理很简单：创建一个新的 SynchronizedSortedMap 类，它持有一个原始 TreeMap 的引用，并把所有的方法（如 put, get, remove 等）都用 synchronized (mutex) 代码块包裹起来，从而保证了每次只有一个线程能操作底层的 TreeMap。

        优点：简单、直接、官方标准。

        缺点：性能一般。因为它使用一个全局锁，无论读写都会锁住整个对象，并发性能不高。

    C. 使用ConcurrentHashMap (错误)

        ConcurrentHashMap 是一个线程安全的 Map，但它不保证顺序。TreeMap 的核心特性之一是有序性（按键排序）。因此，用 ConcurrentHashMap 替换 TreeMap 会丢失有序性，不满足需求。它解决了线程安全问题，但改变了数据结构的基本性质。

    D. 使用ReentrantReadWriteLock (是一种方法，但不是最佳选项)

        和 A 类似，ReentrantReadWriteLock (读写锁) 是一种比 synchronized 更精细的加锁工具。你可以用它来手动实现对 TreeMap 的线程安全访问，并且可以优化性能（允许多个线程同时读，但写操作独占）。

        但是，这需要你手动编写大量的模板代码，并且容易出错。相比之下，B 选项是现成的、封装好的解决方案。所以 D 是一种可行的“底层实现方式”，但不是“如何让它变安全”这个问题的最佳答案。

一句话总结 Q22：Collections.synchronizedSortedMap() 是将现有 TreeMap 变为线程安全的标准、最直接的方法。
Q23: 如果需要线程安全的有序Map，应该使用什么？

A. TreeMap + synchronized

C. ConcurrentSkipListMap

B. HashMap + Collections.synchronizedMap()

    D. LinkedHashMap + ReentrantLock

正确答案：C

这道题问的是最佳实践。如果你从头开始就需要一个“线程安全的有序Map”，那么应该选择哪个类？
选项分析：

    A. TreeMap + synchronized (可用，但非最优)

        这指的是通过 Collections.synchronizedSortedMap() 包装 TreeMap，或者手动加 synchronized 锁。正如 Q22 所述，这是可行的，但它的问题是性能瓶颈。所有操作都竞争同一把锁，当并发量高时，线程会大量阻塞，性能急剧下降。

    B. HashMap + Collections.synchronizedMap() (错误)

        这个组合是线程安全的，但 HashMap 本身是无序的，所以它不满足“有序Map”这个核心要求。

    C. ConcurrentSkipListMap (最佳选择)

        这是 Java java.util.concurrent 包中专门为此场景设计的类。

        线程安全：它使用了更先进的、非阻塞的**CAS（Compare-And-Swap）**操作和精细的锁机制，而不是一个全局的粗粒度锁。

        有序：它的底层数据结构是跳表（Skip List），一种可以实现对数时间复杂度查找、插入、删除的有序数据结构。

        高性能：在高并发场景下，它的性能远超于 synchronizedSortedMap(new TreeMap<>())。不同的线程可以同时在跳表的不同部分进行操作，冲突大大减少。

        因此，ConcurrentSkipListMap 是需要“线程安全”和“有序”这两个特性时的首选和最佳实践。

    D. LinkedHashMap + ReentrantLock (错误)

        LinkedHashMap 保证的是插入顺序或访问顺序，而不是像 TreeMap 那样的自然排序或自定义比较器排序。所以它不满足“有序”（按键排序）的要求。同时，手动使用 ReentrantLock 也是一种性能不如 ConcurrentSkipListMap 的自定义实现方式。
```



### 复习建议：
1. 重点关注TreeMap的底层数据结构（红黑树）
2. 理解有序操作方法的区别（lowerKey vs floorKey等）
3. 掌握比较器的一致性原则
4. 了解TreeMap的适用场景和性能特点
5. 注意线程安全问题和解决方案


