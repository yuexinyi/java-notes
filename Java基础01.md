1.整数默认int,小数默认double

2.数据默认值在main()中无效。（各个数据类型的默认值的使用，需要结合类才能观察到

3.在其他语言中，没有提供布尔类型，会用0表示false，非0表示true，但是在Java中没有这样的概念

4.char描述单一字符，String描述多个字符

5.使用“+”出现了字符串，则所有的数据类型都变成了String,如果想要得到正确的运算结果，必须使用()

```
String result = "计算结果:" +numA + numB ; // 此时“+”不是数学运算而是字符连接。
String result = "计算结果:" + (numA + numB ) ;
```
6.&和&&的区别

相同：当所有表达式的结果为true时，结果才为真，返回true，否则，返回false

不同：

1）&&是短路与，前面表达式为假，程序会终止执行后面的表达式。

2)&若前面的表达式为假，还是会执行后面的表达式，再得出结果false

拓展：|与||的区别
相同：在程序中，当有其中一个表达式为true,结果都为true

不同：||是短路或，当前面的表达式为true,直接返回true;而|程序还会继续执行后面的表达式，最后还是返回true

7.存在i+1<i的数吗

解析：存在，i是byte类型，i=127,i+1=-128<127;i是int类型，i=2147483647,i+1=-2147483648<2147483647;其他类型同理

8.0.6332的数据类型是double(默认double)

9.System.out.println("5"+2)的输出结果：52

10.float f = 3.4是否正确？

解析：不正确，正确答案是float f = (float)3.4或float = 3.4f(有小数点默认是double)

11.java中一个char变量能否表示一个汉字?

可以，因为Java字符采用unicode编码，unicode是两个字节可以表示一个汉字，char是两个字节

12.程序的三种结构：顺序结构、分支结构、循环结构

13.

区别|方法重载 |方法重写
---|---|---
参数（个数、顺序、类型）|不同 | 相同
发生|同一个类 | 有继承关系的类
返回值类型|可相同可不同|相同
权限|不小于父类方法|无要求
相同|    方法名称相同


方法重载：在同一个类中有多个方法名称相同的方法，但各个方法的参数列表不同（个数、顺序、类型）。不能有两个名字相同、参数类型也相同却返回不同类型值的方法

方法重写：在有继承关系的类中。意义在于子类继承父类方法需要扩展新的功能，受方法修饰符的限制（public,protected）

14.数组拷贝：两种方式

```
System.arraycopy(源数组，源数组开始，目标数组，目标数组开始，拷贝长度)
java.util.Arrays.copyof(源数组，数组长度)
```
15.引用传递的本质：一块堆内存可以被多个栈内存所指向

16.构造方法必须和类名相同，没有返回值类型声明，每一个类中至少存在一个构造方法（没有明确定义，系统会自动生成一个无参构造）

17.编译器是根据程序结构来区分普通方法和构造方法的，所以构造方法没有返回值类型声明

18.若在类中定义了构造方法，则默认的无参构造将不再生成

19.匿名对象不会有任何的栈空间指向，所以使用一次之后就成为垃圾空间

20.this：调用本类属性、本类方法、表示当前对象

21.传统属性特征：保存在堆内存中，且每个对象独享属性

static:描述共享属性，又称类属性，保存在全局数据区的内存之中，所有对象都可以进行该数据区的访问，不受对象实例化控制，使用类.属性名访问即可

22.所有static方法不允许调用非static定义的属性和方法；
所有的非static方法允许访问static方法和属性

23.代码块分为普通代码块、构造块、静态块、同步块

24.普通代码块：定义在方法中的代码块，一般如果方法中代码过长，为避免变量重名，使用普通代码块

25.构造块：定义在类中（不加修饰符）

26.主类静态块>主方法>非主类静态块>构造块>构造方法>其他方法
父>子

例子：

```
class A{
    public A() {
        System.out.println("A 构造");
    }

    {
        System.out.println("A 非静");
    }

    static {
        System.out.println("A 静");
    }
}

public class test extends A{
    public test() {
        System.out.println("B 构造");
    }

    //构造块
    {
        System.out.println("B 非静");
    }

    static {
        System.out.println("B 静");
    }

    public static void main(String[] args) {
        System.out.println("start");
        new test();
        new test();
        System.out.println("end");
    }
}
```
输出：
A静 B静 start A非静 A构造 B非静 B构造 A非静 A构造 B非静 B构造 end

27.内部类可以访问该类定义所在作用域中的数据、可以对同一个包中的其他类隐藏起来、可以实现java单继承的缺陷、想要回调一个函数但是不想写大量代码可以选择匿名内部类

例子：解决单继承局限

```
class A{
    private String name = "A私有";

    public String getName() {
        return name;
    }
}

class B{
    private int age = 20;

    public int getAge() {
        return age;
    }
}

class Outter{
    private class InnerA extends A{
        public String name(){
            return super.getName();
        }
    }

    private class InnerB extends B{
        public int age(){
            return super.getAge();
        }
    }

    public String name(){
        return new InnerA().name();
    }

    public int age(){
        return new InnerB().age();
    }
}

public class test1 {
    public static void main(String[] args) {
        Outter outter = new Outter();
        System.out.println(outter.name());
        System.out.println(outter.age());
    }
}
```
28.内部类和外部类的关系

- 内部类的创建依赖外部类的实例对象
- 内部类是一个相对独立的实体
- 内部类可以直接访问外部类的元素（包含私有），但是外部类不可以直接访问内部类的元素
- 外部类可以通过内部类引用简洁访问内部类元素

例子：外部类可以通过内部类引用间接访问内部类元素

```

class Out{
    public void display(){
        //外部类访问内部元素，需要通过内部类引用来访问
        Inner in = new Inner();
        in.display();
    }
    class Inner{
        public void display(){
            System.out.println("inner");
        }
    }
}
public class test3 {
    public static void main(String[] args) {
        Out out = new Out();
        out.display();
    }
}
```
29.内部类的分类：成员内部类、静态内部类、方法内部类、
匿名内部类

30.
外部类外部创建内部类

```
Outter.Inner in = new Outter().new Inner();
```
外部类内部创建内部类

```
Inner in = new Inner();
```

31.
成员内部类注意：

- 成员内部类中不能存在任何static的变量和方法
- 成员内部类是依附于外部类的，只有创建了外部类才能创建内部类

静态内部类注意：

- 静态内部类的创建是不需要依赖于外围类的，可以直接创建
- 静态内部类不可以任何外围类的非static成员变量和方法，而内部类则都可以

创建语法：

```
Outter.Inner out = new Outter.Inner();
```
例子：

```
package com;

class Outter{
    private static String msg = "hello";
    static class Inner{
        public void print(){//只能使用外部类的static操作
            System.out.println(msg);
        }
    }
    public void fun(){
        Inner in = new Inner();
        in.print();
    }
}

public class test {
    public static void main(String[] args) {
        Outter.Inner in = new Outter.Inner();
        in.print();
    }
}
```


方法内部类注意：

- 只能在该方法中被使用
- 不允许使用访问权限修饰符
- 对外完全隐藏，除了创建这个类的方法
- 要想使用方法形参，必须使用final声明（JDK8以后隐式声明）
例子：

```
package com;

class Outter{
    private int num;
    public void display(int test){
        class Inner{
            private void fun(){
                num++;
                System.out.println(num);
                System.out.println(test);
            }
        }
        new Inner().fun();
    }
}

public class test {
    public static void main(String[] args) {
        Outter outter = new Outter();
        outter.display(20);
    }
}
```


匿名内部类注意：

- 没有访问修饰符
- 必须继承一个抽象类或实现一个接口
- 不存在任何静态成员或方法
- 没有构造方法，因为没有类名
- 也可引用方法形参，此时形参必须声明final


```
package com;

interface My{
    void test();
}
class Outter{
    private int num;
    public void display(int test){
        new My(){

            @Override
            public void test() {
                System.out.println("匿名"+test);
            }
        }.test();
    }
}

public class test {
    public static void main(String[] args) {
        Outter outter = new Outter();
        outter.display(20);
    }
}
```

32.继承注意

- 目的：扩展已有类的功能，使代码重用
- 不管如何操作，一定先实例化父类对象
- 不允许多重继承，只允许多层继承

```
class A{}
class B extends A{}
class C extends B{}
```
33.方法覆写注意：被覆写的不能够拥有比父类更为严格的访问控制权限（父类使用default,子类必须使用default或者public）

34.this和super

区别|this | super
---|---|---
概念|访问本类中的属性和方法 | 子类访问父类的属性、方法
查找范围|先找本类，本类没有调用父类 | 直接调用父类
特殊|表示当前对象|无

例子：this先找本类，没有调用父类

```
package com;

class testF{
    public void print(){
        System.out.println("father");
    }
}

class testSon extends testF{

    public void display() {
        this.print();
    }
}

public class test{
    public static void main(String[] args) {
        testSon ts = new testSon();
        ts.display();
    }
}
```


35.final

- 修饰类、方法、属性
- final成员变量必须在声明时初始化或者在构造器中初始化，否则编译错误
- 使用final类不能有子类（String类就是使用了final定义）
- final类，该类的所有方法默认加上final修饰（不包含成员变量）

36.多态性

- 方法的多态性
- - 方法重载
- - 方法覆写
- 对象的多态性
- - 对象的向上转型
- - 对象的向下转型

注意:

- 多态性的本质：方法的覆写
- 两个没有关系的类对象不能进行转型，一定会产生ClassCastException

37.抽象类注意：

- private不能和abstract不能同时使用
- 抽象类中允许不定义任何抽象方法，但是此时依然无法创建实例化对象
- 内部抽象类语序使用static

38.对象实例化的核心步骤：

- 进行类加载
- 进行类对象的空间开辟
- 进行类对象的属性初始化（构造方法）


39.开闭原则：对扩展开放，对修改关闭

40.模版方法模式：在一个方法中定义一个算法的骨架，而将一些步骤延迟到子类中。模版方法使得子类可以在不改变算法结构的情况下，重新定义算法中的某些步骤

例子：冲泡咖啡和饮料



41.
抽象类（模版类）：

```
package moban;

abstract class CaffeineBeverage {

    final void prepareRecipe(){//模版方法，避免子类改变算法顺序
        boilWater();
        brew();
        pourInCup();
        //顾客选择性调用加料方法
        if (customerWantsCondiments()){
            addCondiments();
        }
    }

    abstract void brew();

    abstract void addCondiments();

    void boilWater(){
        System.out.println("Boiling water");
    }

    void pourInCup(){
        System.out.println("Pouring into cup");
    }

    //钩子方法，子类可以选择性覆写
    boolean customerWantsCondiments(){
        return true;
    }
}
```
子类：

```
package moban;

public class Tea extends CaffeineBeverage{
    @Override
    void brew() {
        System.out.println("Steeping the tea");
    }

    @Override
    void addCondiments() {
        System.out.println("Adding lemon");
    }
}


package moban;

import java.util.Scanner;

public class Coffee extends CaffeineBeverage {
    @Override
    void brew() {
        System.out.println("Dripping Coffee through filter");
    }

    @Override
    void addCondiments() {
        System.out.println("Adding Sugar and Milk");
    }

    @Override
    boolean customerWantsCondiments() {
        String answer = getUserInput();
        if (answer.equals("y")){
            return true;
        }else {
            return false;
        }
    }

    private String getUserInput(){
        String answer = null;
        System.out.println("你想要在咖啡中加牛奶或糖吗？（y/n）");
        Scanner scanner = new Scanner(System.in);
        answer = scanner.next();
        return answer;
    }
}

```

42.接口：抽象方法和全局常量的集合

- 接口只允许public权限（属性and方法）
- 子类先使用extends继承抽象类，后使用implements实现多个接口
- 一个抽象类可以实现多个接口，但是接口不能继承抽象类
- 一个接口可以使用extends继承多个父接口

```
interface C extends A,B
```

43.简单工厂模式

定义：专门定义一个类用来创建其他类的实例，被创建的类拥有共同父类

组成：一个抽象产品类、具体产品类、一个工厂

优点：简单易实现、类的实例交给了工厂

缺点：添加具体产品违反了开闭原则


```
package SimpleFactory;

import java.util.Scanner;

interface Fruit {
    void printFruit();
}

class Apple implements Fruit{

    @Override
    public void printFruit() {
        System.out.println("give you an apple");
    }
}

class Banana implements Fruit{

    @Override
    public void printFruit() {
        System.out.println("give you a banana");
    }
}

class FruitFactor{
    public Fruit MakeFruit(String type){
        Fruit fruit = null;
        if ("apple".equals(type)){
            fruit = new Apple();
        }else if("banana".equals(type)){
            fruit = new Banana();
        }
        return fruit;
    }
}

public class test{
    public void getFruit(Fruit fruit){
        fruit.printFruit();
    }
    public static void main(String[] args) {
        System.out.println("请输入你想要的水果：");
        Scanner in = new Scanner(System.in);
        new test().getFruit(new FruitFactor().MakeFruit(in.next()));
    }
}
```
44.工厂方法模式
定义：定义一个用来创建对象的接口，子类决定实例化哪一个类

组成：一个抽象产品类、多个具体产品类、一个抽象工厂、多个具体工厂

优点：降低代码耦合度、满足开闭原则（添加子产品不需要修改原有代码）

缺点：增加代码量、


```
package SimpleFactory;

import java.util.Scanner;

interface Fruit {
    void printFruit();
}

class Apple implements Fruit{

    @Override
    public void printFruit() {
        System.out.println("give you an apple");
    }
}

class Banana implements Fruit{

    @Override
    public void printFruit() {
        System.out.println("give you a banana");
    }
}

interface FruitFactor{
    public Fruit MakeFruit();
}

class AppleFactor implements FruitFactor{

    @Override
    public Fruit MakeFruit() {
        return new Apple();
    }
}

class BananaFactor implements FruitFactor{

    @Override
    public Fruit MakeFruit() {
        return new Banana();
    }
}

public class test{
    public void getFruit(Fruit fruit){
        fruit.printFruit();
    }
    public static void main(String[] args) {
        new test().getFruit(new AppleFactor().MakeFruit());
    }
}
```

45.抽象工厂模式

定义：提供一个创建一系列相关或相互依赖对象的接口，无需指定它们具体的类

组成：多个抽象产品类、具体产品类、抽象工厂、具体工厂

优点：实现多个产品族、满足开闭原则、灵活易扩展

缺点：扩展产品族麻烦（破坏开闭原则）、是工厂方法模式的扩展很笨重


```
package SimpleFactory;

import java.util.Scanner;

interface Fruit {
    void printFruit();
}

class Apple implements Fruit{

    @Override
    public void printFruit() {
        System.out.println("give you an apple");
    }
}

class Banana implements Fruit{

    @Override
    public void printFruit() {
        System.out.println("give you a banana");
    }
}

interface juice{
    void printJuice();
}

class AppleJuice implements juice{

    @Override
    public void printJuice() {
        System.out.println("hi,this is appleJuice");
    }
}

class BananaMilk implements juice{

    @Override
    public void printJuice() {
        System.out.println("hi,this is bananaMilk");
    }
}

interface FruitFactor{
    Fruit MakeFruit();
    juice MakeJuice();
}

class AppleFactor implements FruitFactor{

    @Override
    public Fruit MakeFruit() {
        return new Apple();
    }

    @Override
    public juice MakeJuice() {
        return new AppleJuice();
    }


}

class BananaFactor implements FruitFactor{

    @Override
    public Fruit MakeFruit() {
        return new Banana();
    }

    @Override
    public juice MakeJuice() {
        return new BananaMilk();
    }
}

public class test{
    public void getFruit(Fruit fruit){
        fruit.printFruit();
    }

    public void getJuice(juice juice){
        juice.printJuice();
    }

    public static void main(String[] args) {
        new test().getFruit(new AppleFactor().MakeFruit());
        System.out.println("----------------------");
        new test().getJuice(new BananaFactor().MakeJuice());
    }
}
```

46.代理设计模式

定义：两个类共同实现一个接口，其中一个服负责真实业务，另一个完成辅助操作


```
package daili;

interface IceCream{
    void buyIceCream();
}

class Real implements IceCream{

    @Override
    public void buyIceCream() {
        System.out.println("2.拿到冰淇淋");
    }
}

class Proxy implements IceCream{
    IceCream iceCream;

    public Proxy(IceCream iceCream) {
        this.iceCream = iceCream;
    }

    public void before(){
        System.out.println("1.付钱");
    }

    public void after(){
        System.out.println("3.吃掉");
    }

    @Override
    public void buyIceCream() {
        before();
        this.iceCream.buyIceCream();
        after();
    }
}

class Factory{
    public static IceCream getInstance(){
        return new Proxy(new Real());
    }
}

public class test {
    public static void main(String[] args) {
        IceCream iceCream = Factory.getInstance();
        iceCream.buyIceCream();
    }
}
```

47.抽象类和接口

区别|抽象类 | 接口
---|---|---
结构|普通方法、抽象方法 | 全局常量、抽象方法
权限|不限|public
子类使用|extends|implements
关系|一个抽象类可以实现若干接口|接口不能继承抽象类，但是可以使用extends继承多个父接口
子类限制|单继承|多实现



