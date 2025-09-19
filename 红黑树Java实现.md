# 红黑树Java实现详解

## 1. 完整的红黑树实现

```java
public class RedBlackTree<K extends Comparable<K>, V> {
    
    // 节点颜色常量
    private static final boolean RED = true;
    private static final boolean BLACK = false;
    
    // 根节点
    private Node root;
    // 树的大小
    private int size;
    
    // 节点内部类
    private class Node {
        K key;
        V value;
        Node left, right, parent;
        boolean color; // true表示红色，false表示黑色
        
        Node(K key, V value) {
            this.key = key;
            this.value = value;
            this.color = RED; // 新节点默认为红色
        }
    }
    
    // 获取树的大小
    public int size() {
        return size;
    }
    
    // 判断树是否为空
    public boolean isEmpty() {
        return size == 0;
    }
    
    // 判断节点是否为红色
    private boolean isRed(Node node) {
        return node != null && node.color == RED;
    }
    
    // 判断节点是否为黑色
    private boolean isBlack(Node node) {
        return node == null || node.color == BLACK;
    }
    
    // 设置节点为红色
    private void setRed(Node node) {
        if (node != null) {
            node.color = RED;
        }
    }
    
    // 设置节点为黑色
    private void setBlack(Node node) {
        if (node != null) {
            node.color = BLACK;
        }
    }
    
    // 获取父节点
    private Node getParent(Node node) {
        return node == null ? null : node.parent;
    }
    
    // 获取祖父节点
    private Node getGrandparent(Node node) {
        return getParent(getParent(node));
    }
    
    // 获取叔节点
    private Node getUncle(Node node) {
        Node parent = getParent(node);
        Node grandparent = getGrandparent(node);
        
        if (grandparent == null) {
            return null;
        }
        
        return parent == grandparent.left ? grandparent.right : grandparent.left;
    }
    
    // 左旋操作
    private void leftRotate(Node x) {
        Node y = x.right;
        x.right = y.left;
        
        if (y.left != null) {
            y.left.parent = x;
        }
        
        y.parent = x.parent;
        
        if (x.parent == null) {
            root = y;
        } else if (x == x.parent.left) {
            x.parent.left = y;
        } else {
            x.parent.right = y;
        }
        
        y.left = x;
        x.parent = y;
    }
    
    // 右旋操作
    private void rightRotate(Node x) {
        Node y = x.left;
        x.left = y.right;
        
        if (y.right != null) {
            y.right.parent = x;
        }
        
        y.parent = x.parent;
        
        if (x.parent == null) {
            root = y;
        } else if (x == x.parent.right) {
            x.parent.right = y;
        } else {
            x.parent.left = y;
        }
        
        y.right = x;
        x.parent = y;
    }
    
    // 插入操作
    public void put(K key, V value) {
        if (key == null) {
            throw new IllegalArgumentException("Key cannot be null");
        }
        
        Node newNode = new Node(key, value);
        
        if (root == null) {
            root = newNode;
            setBlack(root); // 根节点必须是黑色
        } else {
            insertNode(root, newNode);
        }
        
        size++;
    }
    
    // 插入节点的辅助方法
    private void insertNode(Node current, Node newNode) {
        int cmp = newNode.key.compareTo(current.key);
        
        if (cmp < 0) {
            if (current.left == null) {
                current.left = newNode;
                newNode.parent = current;
                fixInsertion(newNode);
            } else {
                insertNode(current.left, newNode);
            }
        } else if (cmp > 0) {
            if (current.right == null) {
                current.right = newNode;
                newNode.parent = current;
                fixInsertion(newNode);
            } else {
                insertNode(current.right, newNode);
            }
        } else {
            // 键已存在，更新值
            current.value = newNode.value;
            size--; // 抵消size++，因为只是更新
        }
    }
    
    // 插入修复
    private void fixInsertion(Node node) {
        Node parent = getParent(node);
        
        // 情况1：父节点是黑色，无需修复
        if (isBlack(parent)) {
            return;
        }
        
        Node grandparent = getGrandparent(node);
        Node uncle = getUncle(node);
        
        // 情况2：叔节点是红色
        if (isRed(uncle)) {
            setBlack(parent);
            setBlack(uncle);
            setRed(grandparent);
            fixInsertion(grandparent);
        } else {
            // 情况3：叔节点是黑色或不存在
            if (parent == grandparent.left) {
                if (node == parent.right) {
                    // 情况3a：新节点是右子节点
                    leftRotate(parent);
                    node = parent;
                    parent = getParent(node);
                }
                // 情况3b：新节点是左子节点
                setBlack(parent);
                setRed(grandparent);
                rightRotate(grandparent);
            } else {
                if (node == parent.left) {
                    // 情况3a：新节点是左子节点
                    rightRotate(parent);
                    node = parent;
                    parent = getParent(node);
                }
                // 情况3b：新节点是右子节点
                setBlack(parent);
                setRed(grandparent);
                leftRotate(grandparent);
            }
        }
    }
    
    // 查找操作
    public V get(K key) {
        if (key == null) {
            return null;
        }
        
        Node node = findNode(key);
        return node == null ? null : node.value;
    }
    
    // 查找节点的辅助方法
    private Node findNode(K key) {
        Node current = root;
        
        while (current != null) {
            int cmp = key.compareTo(current.key);
            
            if (cmp < 0) {
                current = current.left;
            } else if (cmp > 0) {
                current = current.right;
            } else {
                return current;
            }
        }
        
        return null;
    }
    
    // 删除操作
    public void remove(K key) {
        if (key == null) {
            return;
        }
        
        Node node = findNode(key);
        if (node != null) {
            deleteNode(node);
            size--;
        }
    }
    
    // 删除节点的辅助方法
    private void deleteNode(Node node) {
        // 找到替代节点
        Node replacement;
        
        if (node.left != null && node.right != null) {
            // 有两个子节点，找到后继节点
            replacement = findSuccessor(node);
            node.key = replacement.key;
            node.value = replacement.value;
            node = replacement;
        }
        
        // 现在node最多有一个子节点
        replacement = (node.left != null) ? node.left : node.right;
        
        if (replacement != null) {
            // 有一个子节点
            replacement.parent = node.parent;
            
            if (node.parent == null) {
                root = replacement;
            } else if (node == node.parent.left) {
                node.parent.left = replacement;
            } else {
                node.parent.right = replacement;
            }
            
            // 如果删除的是黑色节点，需要修复
            if (isBlack(node)) {
                fixDeletion(replacement);
            }
        } else if (node.parent == null) {
            // 根节点
            root = null;
        } else {
            // 叶子节点
            if (isBlack(node)) {
                fixDeletion(node);
            }
            
            if (node.parent != null) {
                if (node == node.parent.left) {
                    node.parent.left = null;
                } else {
                    node.parent.right = null;
                }
            }
        }
    }
    
    // 查找后继节点
    private Node findSuccessor(Node node) {
        if (node.right != null) {
            Node current = node.right;
            while (current.left != null) {
                current = current.left;
            }
            return current;
        }
        
        Node parent = node.parent;
        while (parent != null && node == parent.right) {
            node = parent;
            parent = parent.parent;
        }
        
        return parent;
    }
    
    // 删除修复
    private void fixDeletion(Node node) {
        while (node != root && isBlack(node)) {
            Node parent = getParent(node);
            
            if (node == parent.left) {
                Node sibling = parent.right;
                
                if (isRed(sibling)) {
                    // 情况1：兄弟节点是红色
                    setBlack(sibling);
                    setRed(parent);
                    leftRotate(parent);
                    sibling = parent.right;
                }
                
                if (isBlack(sibling.left) && isBlack(sibling.right)) {
                    // 情况2：兄弟节点的两个子节点都是黑色
                    setRed(sibling);
                    node = parent;
                } else {
                    if (isBlack(sibling.right)) {
                        // 情况3：兄弟节点的右子节点是黑色
                        setBlack(sibling.left);
                        setRed(sibling);
                        rightRotate(sibling);
                        sibling = parent.right;
                    }
                    
                    // 情况4：兄弟节点的右子节点是红色
                    sibling.color = parent.color;
                    setBlack(parent);
                    setBlack(sibling.right);
                    leftRotate(parent);
                    node = root;
                }
            } else {
                // 对称情况
                Node sibling = parent.left;
                
                if (isRed(sibling)) {
                    setBlack(sibling);
                    setRed(parent);
                    rightRotate(parent);
                    sibling = parent.left;
                }
                
                if (isBlack(sibling.left) && isBlack(sibling.right)) {
                    setRed(sibling);
                    node = parent;
                } else {
                    if (isBlack(sibling.left)) {
                        setBlack(sibling.right);
                        setRed(sibling);
                        leftRotate(sibling);
                        sibling = parent.left;
                    }
                    
                    sibling.color = parent.color;
                    setBlack(parent);
                    setBlack(sibling.left);
                    rightRotate(parent);
                    node = root;
                }
            }
        }
        
        setBlack(node);
    }
    
    // 中序遍历
    public void inOrderTraversal() {
        inOrderTraversal(root);
        System.out.println();
    }
    
    private void inOrderTraversal(Node node) {
        if (node != null) {
            inOrderTraversal(node.left);
            System.out.print(node.key + "(" + (node.color ? "红" : "黑") + ") ");
            inOrderTraversal(node.right);
        }
    }
    
    // 验证红黑树性质
    public boolean isValidRedBlackTree() {
        if (root == null) {
            return true;
        }
        
        // 检查根节点是否为黑色
        if (isRed(root)) {
            System.out.println("违反性质1：根节点不是黑色");
            return false;
        }
        
        // 检查所有路径的黑节点数量是否相同
        int blackCount = -1;
        return checkBlackHeight(root, 0, blackCount);
    }
    
    private boolean checkBlackHeight(Node node, int currentBlackCount, int expectedBlackCount) {
        if (node == null) {
            if (expectedBlackCount == -1) {
                expectedBlackCount = currentBlackCount;
            } else if (currentBlackCount != expectedBlackCount) {
                System.out.println("违反性质5：黑节点数量不一致");
                return false;
            }
            return true;
        }
        
        if (isBlack(node)) {
            currentBlackCount++;
        }
        
        // 检查红色节点的子节点是否为黑色
        if (isRed(node)) {
            if (isRed(node.left) || isRed(node.right)) {
                System.out.println("违反性质4：红色节点有红色子节点");
                return false;
            }
        }
        
        return checkBlackHeight(node.left, currentBlackCount, expectedBlackCount) &&
               checkBlackHeight(node.right, currentBlackCount, expectedBlackCount);
    }
}
```

## 2. 测试代码

```java
public class RedBlackTreeTest {
    public static void main(String[] args) {
        RedBlackTree<Integer, String> tree = new RedBlackTree<>();
        
        // 测试插入
        System.out.println("=== 插入测试 ===");
        tree.put(10, "A");
        tree.put(20, "B");
        tree.put(30, "C");
        tree.put(40, "D");
        tree.put(50, "E");
        tree.put(25, "F");
        tree.put(35, "G");
        
        System.out.println("中序遍历结果：");
        tree.inOrderTraversal();
        
        System.out.println("树的大小：" + tree.size());
        System.out.println("是否为有效的红黑树：" + tree.isValidRedBlackTree());
        
        // 测试查找
        System.out.println("\n=== 查找测试 ===");
        System.out.println("查找键25：" + tree.get(25));
        System.out.println("查找键100：" + tree.get(100));
        
        // 测试删除
        System.out.println("\n=== 删除测试 ===");
        tree.remove(25);
        System.out.println("删除键25后的中序遍历：");
        tree.inOrderTraversal();
        System.out.println("删除后树的大小：" + tree.size());
        System.out.println("删除后是否为有效的红黑树：" + tree.isValidRedBlackTree());
    }
}
```

## 3. HashMap中的红黑树实现

```java
// HashMap中的TreeNode类（简化版）
static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
    TreeNode<K,V> parent;  // 父节点
    TreeNode<K,V> left;    // 左子节点
    TreeNode<K,V> right;   // 右子节点
    TreeNode<K,V> prev;    // 前驱节点（用于链表）
    boolean red;           // 颜色标记
    
    TreeNode(int hash, K key, V val, Node<K,V> next) {
        super(hash, key, val, next);
    }
    
    // 获取根节点
    final TreeNode<K,V> root() {
        for (TreeNode<K,V> r = this, p;;) {
            if ((p = r.parent) == null)
                return r;
            r = p;
        }
    }
    
    // 树化过程
    final void treeify(Node<K,V>[] tab) {
        TreeNode<K,V> root = null;
        for (TreeNode<K,V> x = this, next; x != null; x = next) {
            next = (TreeNode<K,V>)x.next;
            x.left = x.right = null;
            if (root == null) {
                x.parent = null;
                x.red = false;
                root = x;
            } else {
                K k = x.key;
                int h = x.hash;
                Class<?> kc = null;
                for (TreeNode<K,V> p = root;;) {
                    int dir, ph;
                    K pk = p.key;
                    if ((ph = p.hash) > h)
                        dir = -1;
                    else if (ph < h)
                        dir = 1;
                    else if ((kc == null &&
                              (kc = comparableClassFor(k)) == null) ||
                             (dir = compareComparables(kc, k, pk)) == 0)
                        dir = tieBreakOrder(k, pk);
                    
                    TreeNode<K,V> xp = p;
                    if ((p = (dir <= 0) ? p.left : p.right) == null) {
                        x.parent = xp;
                        if (dir <= 0)
                            xp.left = x;
                        else
                            xp.right = x;
                        root = balanceInsertion(root, x);
                        break;
                    }
                }
            }
        }
        moveRootToFront(tab, root);
    }
    
    // 插入平衡
    static <K,V> TreeNode<K,V> balanceInsertion(TreeNode<K,V> root,
                                                TreeNode<K,V> x) {
        x.red = true;
        for (TreeNode<K,V> xp, xpp, xppl, xppr;;) {
            if ((xp = x.parent) == null) {
                x.red = false;
                return x;
            }
            else if (!xp.red || (xpp = xp.parent) == null)
                return root;
            if (xp == (xppl = xpp.left)) {
                if ((xppr = xpp.right) != null && xppr.red) {
                    xppr.red = false;
                    xp.red = false;
                    xpp.red = true;
                    x = xpp;
                }
                else {
                    if (x == xp.right) {
                        root = rotateLeft(root, x = xp);
                        xpp = (xp = x.parent) == null ? null : xp.parent;
                    }
                    if (xp != null) {
                        xp.red = false;
                        if (xpp != null) {
                            xpp.red = true;
                            root = rotateRight(root, xpp);
                        }
                    }
                }
            }
            else {
                if (xppl != null && xppl.red) {
                    xppl.red = false;
                    xp.red = false;
                    xpp.red = true;
                    x = xpp;
                }
                else {
                    if (x == xp.left) {
                        root = rotateRight(root, x = xp);
                        xpp = (xp = x.parent) == null ? null : xp.parent;
                    }
                    if (xp != null) {
                        xp.red = false;
                        if (xpp != null) {
                            xpp.red = true;
                            root = rotateLeft(root, xpp);
                        }
                    }
                }
            }
        }
    }
}
```

## 4. 性能测试

```java
public class PerformanceTest {
    public static void main(String[] args) {
        int size = 100000;
        
        // 测试红黑树
        RedBlackTree<Integer, String> rbTree = new RedBlackTree<>();
        long startTime = System.currentTimeMillis();
        
        for (int i = 0; i < size; i++) {
            rbTree.put(i, "Value" + i);
        }
        
        long endTime = System.currentTimeMillis();
        System.out.println("红黑树插入" + size + "个元素耗时：" + (endTime - startTime) + "ms");
        
        // 测试查找
        startTime = System.currentTimeMillis();
        for (int i = 0; i < size; i++) {
            rbTree.get(i);
        }
        endTime = System.currentTimeMillis();
        System.out.println("红黑树查找" + size + "个元素耗时：" + (endTime - startTime) + "ms");
        
        // 测试删除
        startTime = System.currentTimeMillis();
        for (int i = 0; i < size / 2; i++) {
            rbTree.remove(i);
        }
        endTime = System.currentTimeMillis();
        System.out.println("红黑树删除" + (size / 2) + "个元素耗时：" + (endTime - startTime) + "ms");
    }
}
```

## 5. 总结

这个完整的Java实现展示了：

1. **完整的红黑树操作**：插入、删除、查找
2. **正确的平衡修复**：所有插入和删除的修复情况
3. **性质验证**：确保红黑树的五个性质得到满足
4. **性能测试**：验证时间复杂度
5. **HashMap应用**：展示在HashMap中的实际使用

掌握这个实现，你就能在面试中自信地讨论红黑树的实现细节，展示你的编程能力！
