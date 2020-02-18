# Map

## 1.出现版本

JDK1.2

## 2.解决问题

保存单个对象的扩展

## 3.定义

一次性保存两个对象（结构：key = value），最大特点：可以通过key找到对应的value值

Map接口是Java中**保存二元偶对象（键值对）的最顶层接口**

key值唯一，通过一个key值一定能唯一找到一个value值

## 4.常用方法

- public V put(K key,V value):向Map中添加数据 
- public V get(K key):根据指定的key值取得相应的value值，若没有此key值，返回null 
- public Set<Map.Entry<K,V>> entrySet():将Map集合变为Set集合  
- public Set<K> keySet():返回所有Key值集合，**key不能重复** （key值重复，再次put变为相应value的更新操作）
- public Collection<V> values():返回所有value值，**value可以重复**

## 5.继承关系

- HashMap(常用)
- Hashtable
- TreeMap
- ConcurrentMap

![1581943138172](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1581943138172.png)

## 1.HashMap

### 1.1出现版本

JDK1.2

### 1.2特点

- key,value均可为null
- value可重复
- 线程不安全

### 1.3底层实现

哈希表+红黑树（JDK8后+红黑树）

### 1.4基本操作

#### 栗子1

代码：

```java
package SaveDouble.RMap;

import java.util.Map;
import java.util.HashMap;


public class test {
    public static void main(String[] args){
        Map<Integer,String> m = new HashMap<>();
        //添加元素
        m.put(1,"a");
        //key重复
        m.put(1,"b");
        //value重复
        m.put(2,"b");
        //key,value均为null
        m.put(null,null);
        System.out.println(m);
    }
}
```

输出：

![1581994992543](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1581994992543.png)

结论:

- key值相同，会被覆盖
- key，value均可为null
- value可重复



#### 栗子2

代码：

```java
package SaveDouble.RMap;

import java.util.Iterator;
import java.util.Map;
import java.util.HashMap;
import java.util.Set;


public class test {
    public static void main(String[] args){
        Map<Integer,String> m = new HashMap<>();
        //添加元素
        m.put(1,"a");
        //key重复
        m.put(1,"b");
        //value重复
        m.put(2,"b");
        //key,value均为null
        m.put(null,null);
        //取出map中的所有key值信息
        //System.out.println(m.keySet());
        Set<Integer> set = m.keySet();
        Iterator<Integer> iterator = set.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }
    }
}
```

输出：

![1581995305742](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1581995305742.png)

结论：

取得key的所有值

### 1.5HashMap内部实现基本分析

HashMap的内部结构可以看作：**数组（Node[] table）+链表**

- 哈希值决定键值对在这个数组的寻址，哈希值相同的键值对，则链式存储；
- 如果链表大小超过阈值(TREEIFY_THRESHOLD,8)，链表会被改造为树形结构；

![类图06](D:\MYLEARN\Java\REVIEW01\复习图片\类集\类图06.PNG)

1.先分析HashMap的构造函数，通过源码，发现只是给loadFactor赋了一个默认值，因为之前的ArrayList就是这样，所以怀疑HashMap也采用了“懒加载策略”，接下来观察添加函数；

```java
public HashMap() {
        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
    }
```

2.HashMap的添加元素是通过put()函数实现的；通过源码分析，并没有什么特殊，本着探索精神，深入研究putVal()

```java
public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
```

3.putVal的前几行代码是这样的

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
        ...
```

(1)第一个if语句表示如果tab为null，resize()方法会初始化tab

(2)resize()方法：初始化存储表，容量不满足需求扩容

```java
final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
            Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        ...
```

部分参数：

- 最大容量(2^30)

  ```java
  static final int MAXIMUM_CAPACITY = 1 << 30;
  ```

- 默认初始容量(16)

  ```java
  static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
  ```

- 负载因子(0.75)

  ```java
  static final float DEFAULT_LOAD_FACTOR = 0.75f;
  ```

结论：

- 门限值通常以倍数调整（newThr = oldThr << 1），即当元素个数超过门限大小，则调整Map大小；
- 门限 = 负载因子 * 容量（newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY)），如果没有指定，采用默认值；
- 扩容后，将原来的数组元素放置到新的数组中（table = newTab）；

根据代码分析：

容量和负载系数决定了可用桶的数量

负载因子 * 容量 > 元素数量；负载因子在源码中设置值为0.75，不建议更改；

(3)**内部哈希实现**：

为什么不直接使用hashCode方法？
返回值普遍较大，需要开辟大量空间

```java
static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

h >>> 16--无符号右移16位，保留高16位；
**保留高16位（保留有效位数），让哈希值得高16位和低16位参与异或运算，降低哈希冲突的概率**

为何哈希表长度必须为2^n?
保证哈希表中所有索引位置都有可能被访问到

HashMap中元素真正的索引下标计算取决于(n - 1) & hash;控制hash值在n-1的范围内；

(4)树化分析

树化逻辑：**当前桶中链表的长度 >= 8 并且 哈希表的长度 >= 64**,否则只是进行简单的扩容处理；

putVal()部分代码

```java
static final int TREEIFY_THRESHOLD = 8;
```

```java
if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
	treeifyBin(tab, hash);
break;
```

treeifyBin()部分代码

```java
static final int MIN_TREEIFY_CAPACITY = 64;
```

```java
int n, index; Node<K,V> e;
if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
	resize();
else if ((e = tab[index = (n - 1) & hash]) != null) {}
```

当binCount(桶中链表的长度) >= 8时，

MIN_TREEIFY_CAPACITY（哈希表的长度） = 64

容量 < 64 =>扩容

容量 >= 64 =>树化

**为什么引入红黑树**？
优化链表过长导致查找性能急剧降低；O(n)（链表查找性能）>O(logn)（树的查找性能）

树化的原因：**安全问题**

在元素放置过程中，一个对象哈希冲突，放置在同一个桶中，会形成一个链表，链表查询是线性的，会严重影响存取性能；

**解树化原因**：当红黑树节点个数在扩容或删除时个数 <= 6,在下一次resize()过程中会将红黑树退化为链表，节省空间；

## 2.Hashtable

### 2.1出现版本

JDK1.0

### 2.2特点

- key、value均不能为null
- value可重复
- 线程安全（⽅法上加了synchronized关键字） 

### 2.3底层实现

哈希表

### 2.4基本操作

#### 栗子1

代码：

```java
package SaveDouble.RMap;

import java.util.Map;
import java.util.Hashtable;


public class test {
    public static void main(String[] args){
        Map<Integer,String> m = new Hashtable<>();
        //添加元素
        m.put(1,"a");
        //key重复
        m.put(1,"b");
        //value重复
        m.put(2,"b");
        System.out.println(m);
    }
}
```

输出：

![1582001617367](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1582001617367.png)

结论：

- key值不可重复（重复覆盖），value可重复；

#### 栗子2

代码：

```java
package SaveDouble.RMap;

import java.util.Map;
import java.util.Hashtable;


public class test {
    public static void main(String[] args){
        Map<Integer,String> m = new Hashtable<>();
        //添加元素
        m.put(1,"a");
        //key重复
        m.put(1,"b");
        //value重复
        m.put(2,"b");
        //key,value均为null
        m.put(null,null);
        System.out.println(m);
    }
}
```

输出：

![1582001512433](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1582001512433.png)

结论：

- key、value均不能为空

## 3.TreeMap

### 3.1出现版本

JDK1.2

### 3.2特点

- **key不能为空，value可以为空**
- 可排序（按照key值排序）---实现Comparable接口完成

结论：有Comparable出现的地方，判断数据就依靠compareTo（）方法完成，不再需要equals()与hashCode()

- 线程不安全

### 3.3底层实现

红黑树

## 4.ConcurrentHashMap

### 4.1出现版本

JDK1.5

### 4.2特点

- 线程安全

### 4.3底层实现

哈希表+红黑树

### 4.4解决问题

提高并发操作的效率（Hashtable低效，所有并发操作都要竞争同一把锁，一个线程在进行同步操作时，其他线程只能等待，大大降低了并发操作的效率）



## 5.HashMap **VS** Hashtable

| 区别     | HashMap            | Hashtable                                                    |
| -------- | ------------------ | ------------------------------------------------------------ |
| 出现版本 | JDK1.2             | JDK1.0                                                       |
| 性能     | 异步处理，性能较高 | 同步处理，性能很低（读读互斥）                               |
| 安全性   | 线程不安全         | 线程安全                                                     |
| null操作 | 允许存放null       | 不允许存放null                                               |
| 其他     | 懒加载策略         | 产生对象时初始化内部哈希表（默认16），采用synchronized同步方法 |



