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
- value不可重复
- 线程不安全

### 1.3底层实现

哈希表+红黑树（JDK8后+红黑树）

## 2.HashTable

## 3.TreeMap

## 4.ConcurrentMap









