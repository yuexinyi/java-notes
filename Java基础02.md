1.String类与equals的区别

- '=='数值比较，比较的是两个字符串对象的内存地址数值
- equals进行字符串内容比较

2.在JVM底层实际上会自动维护一个对象池（字符串对象池），如果现在采用了直接赋值的模式进行String类的对象
实例化操作，那么该实例化对象（字符串内容）将自动保存到这个对象池之中。如果下次继续使用直接赋值的模式
声明String类对象，此时对象池之中如若有指定内容，将直接进行引用；如若没有，则开辟新的字符串对象而后将
其保存在对象池之中以供下次使用

3.String类中两种对象实例化的区别

- 直接赋值：只会开辟一块堆内存空间，并且该对象可以自动保存在对象池中以供下次使用
- 构造方法：会开辟两块堆内存空间，其中一个成为垃圾空间，不会自动保存在对象池，可以手工入池intern()

4.任何字符串常量都是String对象，String的常量一旦声明不可改变，如果改变对象内容，改变的只是其引用的指向而已

5.字符串查找最好用contains()

6.StringBuffer的reverse()可以实现字符串反转

7.String、StringBuffer、StringBuilder的区别：

- String内容不可修改，StringBuffer和StringBuilder内容可以修改
- StringBuffer采用同步处理（方法上加synchronized），线程安全；StringBuilder采用异步处理，线程不安全

8.
String->int:Integer.parseInt()

String->double:Double.parseDouble()

String->boolean:Boolean.parseBoolean()

基本数据类型->String:String.valueOf()

9.class与public class的区别

- public class:文件名称必须与类名称一致，希望一个类被其他包访问。
- class:在一个.java文件中可有多个class,但是这类不允许被其他包访问

10.
private->default>protected->public

- private:同包同类
- default:同包同类，同包不同类
- protected:同包同类，同包不同类,不同包的子类
- public:同包同类，同包不同类,不同包的子类,不同包非子类
- 
11.单例模式

- 饿汉式

```
class Singleton {
    private final static Singleton Instance = new Singleton();

    private Singleton() {
    }

    public static Singleton getInstance(){
        return Instance;
    }
}
```

- 懒汉式

```
class Singleton {
    private static Singleton Instance;

    private Singleton() {
    }

    public static Singleton getInstance(){
        if (Instance == null){
            Instance = new Singleton();
        }
        return Instance;
    }
}
```
12.主方法本身也属于一个方法，所以主方法上也可以使用throws进行异常抛出，这个时候如果产生了异常就会交给JVM处理。

13.throw和throws的区别
 
 - throw:用在方法内部，手工异常输出
 - throws:方法声明上，告诉用户可能产生的异常，该方法可能不处理该异常
 

14.Exception和RuntimeException的区别：

- Exception是RuntimeException的父类，使用Exception定义的异常必须使用异常处理，而使用RuntimeException定义的由用户选择性处理
- 常见的RuntimeException:ClassCastException、NullPointerException

15.enum与Enum区别

enum是一个关键字，使用enum定义的枚举本质上就相对于一个类继承了Enum这个抽象类而已

16.
准确覆写：@Override
声明过期：@Deprecated(表示该方法不建议使用，但是使用了也不会出错)
压制警告：@SuppressWarnings

17.从JDK1.8开始，接口中可以使用default定义普通方法，需要通过对象调用，可以使用static来定义静态方法，通过接口名可以调用

18.方法引用

类型：

- 引用静态方法：类名::static 方法名称

```
package testdemo;

@FunctionalInterface//函数式编程接口，只允许有一个方法
interface IUtil<P,R>{
    public R switchPara(P p);
}

public class test {
    public static void main(String[] args) {
        IUtil<Integer,String> iu = String::valueOf;
        String str = iu.switchPara(1000);
        System.out.println(str.length());
    }
}
```


- 引用某个对象的方法：实例化对象::普通方法


```
package testdemo;

@FunctionalInterface//函数式编程接口，只允许有一个方法
interface IUtil<R>{
    public R switchPara();
}

public class test {
    public static void main(String[] args) {
        IUtil<String> iu = "hello"::toUpperCase;
        System.out.println(iu.switchPara());
    }
}
```


- 引用某个特定类的方法：类名::普通方法

```
package testdemo;

@FunctionalInterface//函数式编程接口，只允许有一个方法
interface IUtil<R,P>{
    public R compare(P p1,P p2);
}

public class test {
    public static void main(String[] args) {
        IUtil<Integer,String> iu = String::compareTo;
        System.out.println(iu.compare("中","国"));
    }
}
```


- 引用构造方法：类名::new


```
package testdemo;

class Person{
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

@FunctionalInterface//函数式编程接口，只允许有一个方法
interface IUtil<R,PN,PA>{
    public R createObject(PN p1,PA p2);
}

public class test {
    public static void main(String[] args) {
        IUtil<Person,String,Integer> iu = Person::new;
        System.out.println(iu.createObject("zhangsan",18));
    }
}
```


