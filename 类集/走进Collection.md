# Collection

### 1.出现版本

JDK1.2

### 2.解决问题

数组定长问题

### 3.定义

Collection接口是Java中**保存单个对象**的最顶层接口

```
public interface Collection<E> extends Iterable<E> 
```

继承了Interable接口

### 4.继承关系

![类集01](D:\MYLEARN\Java\REVIEW01\复习图片\类集\类集01.PNG)

### 5.常用操作方法

- 添加元素:boolean add(E e);

- 删除元素：boolean remove(Object o);

- 查找元素：boolean contains(Object o);

- 取得集合大小：int size();

- 转为数组：Object[] toArray();

- 取得类集的迭代器：Iterator<E> iterator();
- Collection接口中两个重要方法add()、inerator()在他的子接口中都有这两个方法

## 1.List

### 1.1List接口

​        List子接口与Collection接口相比最大的特点在于其有一个get()方法，可以根据索引取得内容。由于List本身还是接 口，要想取得接口的实例化对象，就必须有子类，在List接口下有三个常用子类：ArrayList、Vector、 LinkedList。

#### 1.1.1独有方法

index默认从0开始编号

- 根据索引下标取得元素：E get(int index);

- 根据索引下标修改元素，返回修改前的数据：E set(int index,E element);

#### 1.1.2常用子类

![类集02](D:\MYLEARN\Java\REVIEW01\复习图片\类集\类集02.PNG)

### 1.2ArrayList

出现版本：JDK1.2

#### 1.2.1基本操作

代码：

```java
package SaveSingle.RList;

import java.util.List;
import java.util.ArrayList;


public class test {
    public static void main(String[] args){
        // 此时集合里面只保存String类型
        List<String> list = new ArrayList<>();
        list.add("Hello") ;
        // 重复数据 
        list.add("Hello") ;
        list.add("World") ;
        System.out.println(list) ;
    }
}
```

结果：

![1581911782679](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1581911782679.png)

结论：List允许保存重复数据

#### 1.2.2List的get()操作

代码：

```java
package SaveSingle.RList;

import java.util.List;
import java.util.ArrayList;


public class test {
    public static void main(String[] args){
        // 此时集合里面只保存String类型
        List<String> list = new ArrayList<>();
        list.add("Hello") ;
        // 重复数据 list.add("Hello") ;
        list.add("World") ;
        for(int i = 0;i < list.size();i++){
            System.out.print(list.get(i)+"、");
        }
    }
}
```

结果：

![1581911736821](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1581911736821.png)

结论：list可以通过get(索引)获得数据元素

#### 1.2.3Collection进行输出处理

get()方法是List子接口提供的。如果现在操作的是Collection接口，那么对于此时的数据取出只能够将集合变为对象数组操作

代码：

```java
package SaveSingle.RList;

import java.util.List;
import java.util.ArrayList;
import java.util.Arrays;


public class test {
    public static void main(String[] args){
        // 此时集合里面只保存String类型
        List<String> list = new ArrayList<>();
        list.add("Hello") ;
        // 重复数据 
        list.add("Hello");
        list.add("World") ;
        Object[] result = list.toArray();
        System.out.println(Arrays.toString(result));
    }
}
```

结果：

![1581911674472](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1581911674472.png)

#### 1.2.4添加自定义类

List集合中添加自定义类

**使用contains()、remove()需要equals()的支持**

```java
public boolean equals(Object obj){
            if(this == obj){
                return true;
            }
            if(!(obj instanceof Person)){
                return false;
            }
            Person per = (Person) obj;
            return this.age.equals(per.age)&&this.name.equals(per.name);
        }
```

#### 1.2.5底层数组扩容详解

源代码：

```java
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }

    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
```

minCapacity:所需的最小数组容量

oldCapacity:原先的数组容量（扩容前）

newCapacity:新的数组容量（扩容后）

MAX_ARRAY_SIZE:MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8

分析：

1. 获取到原先的数组容量；

2. 新容量= 1.5旧容量；
3. 如果new < min(新容量 < 当前所需的最小容量)，则将**当前所需容量赋为新的数组容量**；
4. 如果new > max(新容量 > 数组最大容量)，调用hugeCapacity()函数
   1. min < 0(当前所需最小容量 < 0)，抛出内存错误异常；
   2. min > max(当前所需最小容量 > 数组最大容量)，此时将**Integer.MAX_VALUE赋为新的数组容量**；否则，将**MAX_ARRAY_SIZE赋为新的数组容量**；

### 1.3LinkedList

#### 1.3.1出现版本

JDK1.2

#### 1.3.2基本操作

和ArrayList操作差不多



### 1.4Vector

#### 1.4.1提出版本

JDK1.0

#### 1.4.2基本操作

代码：

```java
package SaveSingle.RList;

import java.util.List;
import java.util.Vector;

public class test {
    public static void main(String[] args){
        List<String> list = new Vector<>() ;
        list.add("hello");
        list.add("hello"); 
        list.add("bit");
        System.out.println(list);
        list.remove("hello");
        System.out.println(list);
    }
}
```

结果：

![1581913035138](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1581913035138.png)

#### 1.4.3底层数组扩容详解

源代码：

```java
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                         capacityIncrement : oldCapacity);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        elementData = Arrays.copyOf(elementData, newCapacity);
    }

    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
```

分析：

注意到，这里与ArrayList那个扩容操作基本一致，但是有一句不同哦

```java
int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                         capacityIncrement : oldCapacity);
```

简单来讲，一下这个语句吧，其他请看上面ArrayList那块的部分

1. capacityIncrement是什么?

   ```java
   protected int capacityIncrement;
   ```

   这是什么鬼，继续找会发现，这个参数可以在实例化对象时自定义

   ```java
   public Vector(int initialCapacity, int capacityIncrement) {
           super();
           if (initialCapacity < 0)
               throw new IllegalArgumentException("Illegal Capacity: "+
                                                  initialCapacity);
           this.elementData = new Object[initialCapacity];
           this.capacityIncrement = capacityIncrement;
       }
   ```

   摸索许久，这个参数作用大概是：控制数组填满了之后要怎么增加，算是一种增加策略吧。

2. 比如，在实例化对象时，给定capacityIncrement=10(>0)，那么后续newCapacity = oldCapacity+10;如果不给定值或者给一个<0的值，那么后续newCapacity = 2oldCapacity；

### 1.5List子类的"炫耀"（各自优势）

#### 1.5.1ArrayList **VS** LinkedList

相同点：

- 出现版本：均出现与JDK1.2时期；
- 线程安全性：异步处理，线程不安全，效率较高

不同点：

- ArrayList:基于数组实现，在实例化对象时自定义了数组大小，在内部就会开辟一个自定义大小的数组，存储元素多导致数组容量不够时，会进行动态扩容扩充数组的容量。但是，这种扩充并不是无限的（适用于经常查找的情况下）

- LinkedList:底层采用**双向链表**实现（**不存在扩容操作**）;

  特殊：**是JDK Queue的一个子类**（适用于增删频繁的情况下）

#### 1.5.2ArrayList VS Vector

|              | ArrayList                                                    | Vector                                                       |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 出现版本     | JDK1.2                                                       | JDK1.0                                                       |
| 调用无参构造 | **懒加载策略**，在构造方法阶段并不初始化对象数组，第一次添加元素时初始化对象数组大小为10 | 产生对象时，内部数组初始化为10的数组                         |
| 扩容策略     | 新数组大小变为原数组的1.5倍                                  | 新数组大小变为原数组的2倍                                    |
| 线程安全     | 异步处理，**线程不安全**，效率较高                           | 在方法上加锁(synchronized同步方法)，**线程安全**，效率较低（锁的是当前Vector对象，当前对象的所有方法均上锁，其他线程不可调用）（读读互斥） |
| 遍历         | 不支持较老的迭代器Enumeration                                | 支持较老迭代器Enumeration                                    |
| 特殊         |                                                              | JDK内置的Stack继承Vector                                     |

- 调用无参构造

  - ArrayList:

    无参构造赋给底层数组一个空数组；

    ```java
    public ArrayList() {
            this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
        }
    ```

  - Vector:

    构造函数时指定底层数组初始化大小为10；

    ```java
    public Vector() {
            this(10);
        }
    ```

- 扩容策略

  - ArrayList:

    扩容1.5倍；

    ```java
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    ```

    

  - Vector:

    扩容2倍；

    ```java
    int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                             capacityIncrement : oldCapacity);
    ```

    

- 线程安全

  - ArrayList:

    ```java
    public boolean add(E e) {
            ensureCapacityInternal(size + 1);  // Increments modCount!!
            elementData[size++] = e;
            return true;
    }
    ```

    

  - Vector:

    方法上加锁;

    ```java
    public synchronized void addElement(E obj) {
            modCount++;
            ensureCapacityHelper(elementCount + 1);
            elementData[elementCount++] = obj;
        }
    ```

- 遍历

  Vector支持较老迭代器Enumeration，该方法返回一个Enumeration迭代器对象

  ```java
  public Enumeration<E> elements() {
          return new Enumeration<E>() {
              int count = 0;
  
              public boolean hasMoreElements() {
                  return count < elementCount;
              }
  
              public E nextElement() {
                  synchronized (Vector.this) {
                      if (count < elementCount) {
                          return elementData(count++);
                      }
                  }
                  throw new NoSuchElementException("Vector Enumeration");
              }
          };
      }
  ```

  

- 特殊

  Stack继承了Vector，Stack类也是JDK1.0出现的

  ```java
  class Stack<E> extends Vector<E> {
  ```



## 2.Set

### 2.1Set接口

#### 2.1.1出现版本

JDK1.2

#### 2.1.2特点

- 不允许数据重复
- 本质是Map接口，value值相同的Map接⼝ 

```java
public TreeSet() {
        this(new TreeMap<E,Object>());
    }
```

- 只实现了Collection接口，并没有对其扩充

#### 2.1.3常用子类

- HashSet：无序存储
- TreeSet：有序存储

![1581936138854](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1581936138854.png)

### 2.2TreeSet

#### 2.2.1出现版本

JDK1.2

#### 2.2.2特点

- 有序存储--按照升序存储；

- 不允许存放null；

- 不允许数据重复；

  利用实现Comparable接口/传入外部比较器接口的返回值判断是否重复

#### 2.2.3底层实现

红黑树

#### 2.2.4基本操作

代码：

```java
package SaveSingle.RSet;

import java.util.Set;
import java.util.TreeSet;

public class test {
    public static void main(String[] args) {
        Set<String> set = new TreeSet<>();
        set.add("C") ;
        set.add("C") ;
        set.add("D") ;
        set.add("B") ;
        set.add("A") ;
        System.out.println(set) ;
    }
}
```

输出：

![1581936504168](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1581936504168.png)

结论：TreeSet中不允许元素重复并且升序存储

#### 2.3.3TreeSet的升序解析

首先明白排序无非是所在类实现了Comparable接口（内部比较器）或者Comparator（外部比较器）

https://blog.csdn.net/weixin_44369212/article/details/103215685

### 2.3HashSet

#### 2.3.1出现版本

JDK1.2

#### 2.3.2特点

不允许重复的无序存储



在实际使用之中，使用TreeSet过于麻烦了。项目开发之中，简单java类是根据数据表设计得来的，如果一个类的属性很多，那么比较起来就很麻烦了。所以我们一般使用的是HashSet。 