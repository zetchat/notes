# HashSet知识点自测题

## 📝 说明
本自测题包含多种题型，建议先独立完成，再对照答案检查。每题都有详细解析，帮助您查漏补缺。

---

## 🔤 选择题（每题2分，共20分）

### 1. HashSet的底层实现是什么？
A. ArrayList  
B. HashMap  
C. LinkedList  
D. TreeMap

### 2. HashSet中允许存储多少个null值？
A. 0个  
B. 1个  
C. 多个  
D. 无限个

### 3. HashSet的默认初始容量是多少？
A. 8  
B. 16  
C. 32  
D. 64

### 4. HashSet的默认负载因子是多少？
A. 0.5  
B. 0.75  
C. 1.0  
D. 1.25

### 5. 向HashSet中添加重复元素会怎样？
A. 抛出异常  
B. 覆盖原有元素  
C. 操作被忽略  
D. 元素被添加到末尾

### 6. HashSet是否保证元素的迭代顺序？
A. 保证插入顺序  
B. 保证自然排序  
C. 不保证任何顺序  
D. 保证升序排列

### 7. HashSet的contains方法平均时间复杂度是多少？
A. O(1)  
B. O(log n)  
C. O(n)  
D. O(n²)

### 8. 以下哪个方法可以获取HashSet的迭代器？
A. getIterator()  
B. iterator()  
C. getNext()  
D. hasNext()

### 9. HashSet是否线程安全？
A. 完全线程安全  
B. 部分线程安全  
C. 非线程安全  
D. 取决于JVM版本

### 10. 使用HashSet进行去重操作时，主要依赖对象的哪些方法？
A. toString()和equals()  
B. hashCode()和equals()  
C. clone()和toString()  
D. finalize()和hashCode()

---

## 🔍 填空题（每题2分，共20分）

### 11. HashSet内部维护一个______实例来存储元素。

### 12. 所有HashSet元素的value都指向同一个______对象，这样可以节省内存。

### 13. 当元素数量超过______时，HashSet会自动扩容。

### 14. HashSet的add方法返回______类型，表示元素是否成功添加。

### 15. 使用______方法可以批量添加元素到HashSet中。

### 16. HashSet的______方法用于检查集合是否为空。

### 17. 通过______方法可以获取HashSet中元素的数量。

### 18. 使用______方法可以删除HashSet中的所有元素。

### 19. HashSet的______方法用于检查指定元素是否存在于集合中。

### 20. 如果两个对象的hashCode相同，HashSet会调用______方法进行进一步比较。

---

## 📝 简答题（每题5分，共25分）

### 21. 简述HashSet的特点和适用场景。

### 22. 解释HashSet的扩容机制是如何工作的。

### 23. 为什么说HashSet的性能取决于对象的hashCode和equals方法？

### 24. 在多线程环境下使用HashSet需要注意什么？有哪些解决方案？

### 25. HashSet、LinkedHashSet和TreeSet有什么区别？如何选择？

---

## 💻 编程题（每题10分，共30分）

### 26. 实现一个简单的去重功能
```java
// 要求：编写方法，使用HashSet对字符串数组进行去重
public String[] removeDuplicates(String[] array) {
    // 请在此处编写代码
}
```

### 27. 实现集合运算
```java
// 要求：编写方法，计算两个HashSet的交集
public <T> HashSet<T> getIntersection(HashSet<T> set1, HashSet<T> set2) {
    // 请在此处编写代码
}
```

### 28. 自定义对象去重
```java
// 要求：创建一个Student类，重写hashCode和equals方法
// 然后使用HashSet对Student对象进行去重
public class Student {
    private String name;
    private int age;
    
    // 请在此处编写构造函数、getter/setter方法
    // 以及重写的hashCode和equals方法
}
```

---

## 🔑 答案与解析

### 选择题答案
1. **B** - HashSet底层基于HashMap实现
2. **B** - HashSet允许存储一个null值
3. **B** - 默认初始容量是16
4. **B** - 默认负载因子是0.75
5. **C** - 添加重复元素时操作被忽略
6. **C** - HashSet不保证任何顺序
7. **A** - 平均时间复杂度是O(1)
8. **B** - 使用iterator()方法获取迭代器
9. **C** - HashSet是非线程安全的
10. **B** - 主要依赖hashCode()和equals()方法

### 填空题答案
11. **HashMap**
12. **PRESENT**
13. **容量 × 负载因子**
14. **boolean**
15. **addAll()**
16. **isEmpty()**
17. **size()**
18. **clear()**
19. **contains()**
20. **equals()**

### 简答题参考答案

#### 21. HashSet的特点和适用场景
**特点：**
- 不允许重复元素
- 允许null值（一个）
- 非线程安全
- 无序性
- 高性能（平均O(1)）

**适用场景：**
- 需要快速查找和去重的场景
- 缓存实现
- 集合运算（交集、并集、差集）
- 不需要保持元素顺序的场景

#### 22. HashSet的扩容机制
当元素数量超过 `容量 × 负载因子` 时：
1. 创建新的更大的哈希表
2. 重新计算所有元素的哈希值
3. 将元素重新分布到新的哈希表中
4. 默认扩容策略：容量翻倍

#### 23. 性能依赖hashCode和equals的原因
- **hashCode**：决定元素在哈希表中的存储位置，影响查找效率
- **equals**：当哈希值相同时，用于进一步比较确定元素是否相同
- 如果这两个方法实现不当，会导致：
  - 哈希冲突增多
  - 查找性能下降
  - 去重功能失效

#### 24. 多线程环境注意事项
**注意事项：**
- HashSet本身非线程安全
- 并发修改可能导致数据不一致
- 可能抛出ConcurrentModificationException

**解决方案：**
- 使用Collections.synchronizedSet()
- 使用ConcurrentHashMap.newKeySet()
- 外部同步（synchronized关键字）
- 使用CopyOnWriteArraySet

#### 25. 三种Set的区别和选择
**HashSet：**
- 无序，性能最好
- 适用：纯去重，不需要排序

**LinkedHashSet：**
- 保持插入顺序，性能稍慢
- 适用：需要保持插入顺序的去重

**TreeSet：**
- 有序（自然排序或自定义排序），性能最慢
- 适用：需要有序集合

### 编程题参考答案

#### 26. 去重功能实现
```java
public String[] removeDuplicates(String[] array) {
    if (array == null || array.length == 0) {
        return new String[0];
    }
    
    HashSet<String> set = new HashSet<>();
    for (String item : array) {
        set.add(item);
    }
    
    return set.toArray(new String[0]);
}
```

#### 27. 集合交集实现
```java
public <T> HashSet<T> getIntersection(HashSet<T> set1, HashSet<T> set2) {
    if (set1 == null || set2 == null) {
        return new HashSet<>();
    }
    
    HashSet<T> intersection = new HashSet<>(set1);
    intersection.retainAll(set2);
    return intersection;
}
```

#### 28. Student类实现
```java
public class Student {
    private String name;
    private int age;
    
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
    
    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Student student = (Student) obj;
        return age == student.age && Objects.equals(name, student.name);
    }
    
    @Override
    public String toString() {
        return "Student{name='" + name + "', age=" + age + "}";
    }
}
```

---

## 📊 评分标准

- **选择题**：20分
- **填空题**：20分  
- **简答题**：25分
- **编程题**：30分
- **总分**：95分

**优秀**：85分以上  
**良好**：70-84分  
**及格**：60-69分  
**需要加强**：60分以下

---

## 💡 学习建议

1. **基础概念**：确保理解HashSet的基本特性和实现原理
2. **实践操作**：多写代码，熟悉各种方法的使用
3. **性能理解**：理解时间复杂度，知道何时使用HashSet
4. **注意事项**：重点关注线程安全和对象方法重写
5. **对比学习**：理解HashSet与其他Set实现的区别

祝您学习进步！🚀







