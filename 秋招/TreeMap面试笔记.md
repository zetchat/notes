# TreeMap面试笔记

## 📚 核心概念

### 1. 基本定义
- **TreeMap**：基于红黑树的有序Map实现
- **有序性**：所有键值对按照键的自然顺序或自定义比较器顺序排列
- **线程安全**：非线程安全，需要外部同步

### 2. 数据结构
- **底层实现**：红黑树（Red-Black Tree）
- **时间复杂度**：查找、插入、删除均为O(log n)
- **空间复杂度**：O(n)

## 🏗️ 构造方法

```java
// 1. 默认构造 - 自然顺序
TreeMap<K, V> map1 = new TreeMap<>();

// 2. 自定义比较器
TreeMap<K, V> map2 = new TreeMap<>(Comparator<? super K>);

// 3. 从其他Map构造
TreeMap<K, V> map3 = new TreeMap<>(Map<? extends K, ? extends V>);

// 4. 从SortedMap构造
TreeMap<K, V> map4 = new TreeMap<>(SortedMap<K, ? extends V>);
```

## 🔑 核心方法

### 基本操作
```java
// 增删改查
V put(K key, V value);           // 添加/更新
V get(Object key);                // 获取值
V remove(Object key);             // 删除
boolean containsKey(Object key);   // 是否包含键
boolean containsValue(Object v);   // 是否包含值
int size();                       // 大小
boolean isEmpty();                // 是否为空
void clear();                     // 清空
```

### 有序操作
```java
// 边界操作
K firstKey();                     // 第一个键
K lastKey();                      // 最后一个键
Map.Entry<K,V> firstEntry();      // 第一个键值对
Map.Entry<K,V> lastEntry();       // 最后一个键值对

// 相对位置操作
K lowerKey(K key);                // 小于key的最大键
K higherKey(K key);               // 大于key的最小键
K floorKey(K key);                // 小于等于key的最大键
K ceilingKey(K key);              // 大于等于key的最小键
```

### 子Map操作
```java
// 范围查询
SortedMap<K,V> subMap(K fromKey, K toKey);      // [fromKey, toKey)
SortedMap<K,V> headMap(K toKey);                // 小于toKey
SortedMap<K,V> tailMap(K fromKey);              // 大于等于fromKey
```

### 视图操作
```java
// 获取视图
Set<K> keySet();                              // 键的Set视图
Collection<V> values();                        // 值的Collection视图
Set<Map.Entry<K,V>> entrySet();               // 键值对的Set视图
```

## 💡 使用示例

### 基本使用
```java
TreeMap<String, Integer> scores = new TreeMap<>();
scores.put("Alice", 95);
scores.put("Bob", 87);
scores.put("Charlie", 92);

// 自然顺序遍历
for (Map.Entry<String, Integer> entry : scores.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
// 输出：Alice: 95, Bob: 87, Charlie: 92
```

### 自定义排序
```java
// 按字符串长度排序，长度相同时按字典序
TreeMap<String, Integer> customOrder = new TreeMap<>(
    Comparator.comparing(String::length).thenComparing(String::compareTo)
);

customOrder.put("a", 1);
customOrder.put("bb", 2);
customOrder.put("ccc", 3);
// 输出顺序：a, bb, ccc
```

### 范围查询
```java
TreeMap<Integer, String> events = new TreeMap<>();
events.put(10, "Event A");
events.put(20, "Event B");
events.put(30, "Event C");
events.put(40, "Event D");

// 获取15-35之间的所有事件
SortedMap<Integer, String> range = events.subMap(15, 35);
// 结果：{20=Event B, 30=Event C}
```

## 🎯 应用场景

### 1. 排行榜系统
```java
// 按分数降序排列
TreeMap<Integer, String> leaderboard = new TreeMap<>(Collections.reverseOrder());
leaderboard.put(100, "Player1");
leaderboard.put(95, "Player2");
leaderboard.put(88, "Player3");
```

### 2. 时间序列数据
```java
// 按时间排序的事件
TreeMap<LocalDateTime, String> timeline = new TreeMap<>();
timeline.put(LocalDateTime.now(), "Event");
```

### 3. 范围统计
```java
// 统计某个范围内的数据
TreeMap<Integer, Integer> data = new TreeMap<>();
// ... 填充数据
SortedMap<Integer, Integer> range = data.subMap(min, max);
```

## ⚠️ 注意事项

### 1. 键的可变性
- 键应该是不可变的，或在插入后不改变影响排序的属性
- 避免在插入后修改键的排序相关属性

### 2. 比较器一致性
```java
// 必须满足：compare(k1, k2) == 0 当且仅当 equals(k1, k2) == true
TreeMap<String, Integer> map = new TreeMap<>((a, b) -> {
    int result = a.compareToIgnoreCase(b);
    return result == 0 ? a.compareTo(b) : result;
});
```

### 3. 空值处理
- 键不能为null（抛出NullPointerException）
- 值可以为null

## 🔄 性能优化

### 1. 批量操作
```java
// 避免频繁的单次操作
List<Map.Entry<K, V>> batch = new ArrayList<>();
// 收集所有操作
for (Map.Entry<K, V> op : batch) {
    treeMap.put(op.getKey(), op.getValue());
}
```

### 2. 比较器优化
```java
// 避免在比较器中执行复杂计算
TreeMap<String, Integer> good = new TreeMap<>(String::compareTo);
```

## 📊 与其他Map比较

| 特性 | TreeMap | HashMap | LinkedHashMap |
|------|---------|---------|---------------|
| 有序性 | ✅ 按键排序 | ❌ 无序 | ✅ 插入顺序 |
| 性能 | O(log n) | O(1) | O(1) |
| 内存 | 中等 | 低 | 中等 |
| 适用场景 | 需要排序 | 一般用途 | 保持插入顺序 |

## 🚀 高级特性

### 1. 线程安全包装
```java
// 使用Collections.synchronizedSortedMap包装
SortedMap<String, Integer> syncMap = Collections.synchronizedSortedMap(new TreeMap<>());
```

### 2. 并发版本
```java
// Java 8+ 可以使用ConcurrentSkipListMap作为并发替代
ConcurrentSkipListMap<String, Integer> concurrentMap = new ConcurrentSkipListMap<>();
```

## 📝 面试要点总结

1. **数据结构**：红黑树，O(log n)时间复杂度
2. **有序性**：按键排序，支持自定义比较器
3. **应用场景**：排行榜、时间序列、范围查询
4. **注意事项**：键不可变、比较器一致性、空值处理
5. **性能特点**：查找快，但插入删除相对较慢
6. **线程安全**：非线程安全，需要外部同步

## 🔍 常见面试问题

- TreeMap的底层数据结构是什么？
- TreeMap和HashMap的区别？
- 什么情况下使用TreeMap？
- TreeMap的键为什么不能为null？
- 如何实现TreeMap的线程安全？
- TreeMap的性能特点是什么？


