# ArrayDeque 自测问题集

## 📋 基础概念题

### 1. 基本定义
**Q1**: ArrayDeque实现了哪些接口？
- [ ] List<E>
- [ ] Queue<E>
- [ ] Deque<E>
- [ ] Set<E>

**Q2**: ArrayDeque的底层数据结构是什么？
- [ ] 链表
- [ ] 数组
- [ ] 红黑树
- [ ] 哈希表

**Q3**: ArrayDeque的初始容量是多少？
- [ ] 8
- [ ] 16
- [ ] 32
- [ ] 64

### 2. 特性判断
**Q4**: 以下哪些是ArrayDeque的特性？（多选）
- [ ] 支持双端操作
- [ ] 动态扩容
- [ ] 高性能
- [ ] 线程安全
- [ ] 不支持随机访问

**Q5**: ArrayDeque的扩容策略是什么？
- [ ] 每次增加1
- [ ] 按2的幂次方扩容
- [ ] 按1.5倍扩容
- [ ] 按固定大小扩容

## 🔧 方法使用题

### 3. 添加元素方法
**Q6**: 在队首添加元素，应该使用哪个方法？
- [ ] add(E e)
- [ ] addFirst(E e)
- [ ] addLast(E e)
- [ ] offer(E e)

**Q7**: 以下哪个方法在队列满时不会抛出异常？
- [ ] add(E e)
- [ ] offer(E e)
- [ ] addFirst(E e)
- [ ] addLast(E e)

**Q8**: 在队尾添加元素，以下哪些方法可以使用？（多选）
- [ ] add(E e)
- [ ] offer(E e)
- [ ] addLast(E e)
- [ ] offerLast(E e)

### 4. 获取元素方法
**Q9**: 获取队首元素但不移除，应该使用哪个方法？
- [ ] getFirst()
- [ ] peekFirst()
- [ ] pollFirst()
- [ ] removeFirst()

**Q10**: 以下哪个方法在空队列时会抛出异常？
- [ ] peekFirst()
- [ ] getFirst()
- [ ] pollFirst()
- [ ] peek()

**Q11**: 获取并移除队尾元素，应该使用哪个方法？
- [ ] getLast()
- [ ] peekLast()
- [ ] removeLast()
- [ ] pollLast()

### 5. 其他方法
**Q12**: 获取队列大小的方法是什么？
- [ ] length()
- [ ] size()
- [ ] count()
- [ ] getSize()

**Q13**: 清空队列的方法是什么？
- [ ] removeAll()
- [ ] clear()
- [ ] empty()
- [ ] reset()

## 🎯 使用场景题

### 6. 栈的使用
**Q14**: 使用ArrayDeque作为栈，压栈应该使用哪个方法？
- [ ] add()
- [ ] push()
- [ ] offer()
- [ ] addFirst()

**Q15**: 使用ArrayDeque作为栈，弹栈应该使用哪个方法？
- [ ] poll()
- [ ] pop()
- [ ] remove()
- [ ] get()

### 7. 队列的使用
**Q16**: 使用ArrayDeque作为队列，入队应该使用哪个方法？
- [ ] add()
- [ ] offer()
- [ ] push()
- [ ] addFirst()

**Q17**: 使用ArrayDeque作为队列，出队应该使用哪个方法？
- [ ] pop()
- [ ] poll()
- [ ] remove()
- [ ] get()

## ⚡ 性能分析题

### 8. 时间复杂度
**Q18**: ArrayDeque添加元素的时间复杂度是多少？
- [ ] O(n)
- [ ] O(1)
- [ ] O(log n)
- [ ] O(n²)

**Q19**: ArrayDeque查找元素的时间复杂度是多少？
- [ ] O(n)
- [ ] O(1)
- [ ] O(log n)
- [ ] O(n²)

**Q20**: ArrayDeque获取大小的时间复杂度是多少？
- [ ] O(n)
- [ ] O(1)
- [ ] O(log n)
- [ ] O(n²)

### 9. 空间复杂度
**Q21**: ArrayDeque的内存效率比LinkedList如何？
- [ ] 更高
- [ ] 更低
- [ ] 相同
- [ ] 无法比较

## 🔄 对比分析题

### 10. 与LinkedList对比
**Q22**: 以下哪些特性ArrayDeque比LinkedList更好？（多选）
- [ ] 内存效率
- [ ] 遍历性能
- [ ] 随机访问
- [ ] 扩容策略

**Q23**: 如果需要随机访问元素，应该选择？
- [ ] ArrayDeque
- [ ] LinkedList
- [ ] 两者都可以
- [ ] 都不合适

### 11. 与Stack对比
**Q24**: Java官方建议使用哪个类来实现栈？
- [ ] ArrayDeque
- [ ] Stack
- [ ] LinkedList
- [ ] Vector

**Q25**: Stack类继承自哪个类？
- [ ] AbstractCollection
- [ ] Vector
- [ ] AbstractList
- [ ] Object

## 🚨 注意事项题

### 12. 线程安全
**Q26**: ArrayDeque是线程安全的吗？
- [ ] 是
- [ ] 否
- [ ] 部分线程安全
- [ ] 取决于使用方式

**Q27**: 多线程环境下使用ArrayDeque，应该如何处理？
- [ ] 直接使用
- [ ] 使用synchronized同步
- [ ] 使用volatile关键字
- [ ] 使用final关键字

### 13. 空值处理
**Q28**: ArrayDeque允许存储null值吗？
- [ ] 允许
- [ ] 不允许
- [ ] 取决于泛型类型
- [ ] 运行时决定

## 💡 高级概念题

### 14. 实现原理
**Q29**: ArrayDeque如何实现循环队列？
- [ ] 使用链表结构
- [ ] 使用head和tail指针
- [ ] 使用环形数组
- [ ] 使用哈希表

**Q30**: 当tail到达数组末尾时，ArrayDeque如何处理？
- [ ] 抛出异常
- [ ] 从0开始
- [ ] 停止添加
- [ ] 自动扩容

## 🔍 算法应用题

### 15. 实际应用
**Q31**: 以下哪些算法可以使用ArrayDeque实现？（多选）
- [ ] 滑动窗口最大值
- [ ] 括号匹配
- [ ] 用队列实现栈
- [ ] 单调队列

**Q32**: 实现滑动窗口最大值时，ArrayDeque的作用是什么？
- [ ] 存储所有元素
- [ ] 维护单调递减队列
- [ ] 计算平均值
- [ ] 排序元素

## 📝 代码分析题

### 16. 代码理解
**Q33**: 以下代码的输出是什么？
```java
ArrayDeque<Integer> deque = new ArrayDeque<>();
deque.add(1);
deque.add(2);
deque.addFirst(3);
System.out.println(deque.getFirst());
```
- [ ] 1
- [ ] 3
- [ ] 2
- [ ] 抛出异常

**Q34**: 以下代码的输出是什么？
```java
ArrayDeque<String> deque = new ArrayDeque<>();
deque.push("A");
deque.push("B");
deque.push("C");
System.out.println(deque.pop());
System.out.println(deque.peek());
```
- [ ] A, B
- [ ] C, B
- [ ] B, A
- [ ] C, A

## 🎯 综合应用题

### 17. 实际场景
**Q35**: 在电商系统中，购物车的商品列表适合使用ArrayDeque吗？
- [ ] 适合，因为需要双端操作
- [ ] 不适合，因为需要随机访问
- [ ] 适合，因为性能好
- [ ] 不适合，因为容量固定

**Q36**: 在任务调度系统中，任务队列适合使用ArrayDeque吗？
- [ ] 适合，因为需要FIFO操作
- [ ] 不适合，因为需要随机访问
- [ ] 适合，因为需要双端操作
- [ ] 不适合，因为性能差

---

## 📊 答案统计

**正确答案数量**: ___ / 36

**正确率**: ___ %

## 🔍 错题分析

请记录做错的题目编号，重点复习相关知识点：

**基础概念错题**: ___
**方法使用错题**: ___
**性能分析错题**: ___
**对比分析错题**: ___
**注意事项错题**: ___
**高级概念错题**: ___
**算法应用错题**: ___
**代码分析错题**: ___
**综合应用错题**: ___

## 📚 复习建议

- **正确率 ≥ 90%**: 知识点掌握很好，可以进入下一阶段学习
- **正确率 70-89%**: 知识点基本掌握，重点复习错题部分
- **正确率 < 70%**: 需要重新学习相关知识点，建议重新阅读笔记

---

*建议：每天复习一遍，一周后再次自测，巩固记忆！*


