# Java I/O

I/O核心组成：五个类（File、OutputStream、InputStream、Reader、Writer）一个接口（Serializable）

# 1.File文件操作类

在java.io包中，File类是唯一一个与文件本身操作有关的类

## 1.1File类使用

实例化对象的两种方法：

```
public File(String pathname);
public File(String parent,String child);设置父路径和子路径
```

### 1.1.1创建文件

语法：

```
public boolean creatNewFile() throw IOException{}
```

代码实现：

```java
package com.iotest.RFile;

import java.io.File;
import java.io.IOException;

public class test {
    public static void main(String[] args) {
        File file = new File("/text.txt");//在该项目的根目录下/->D:/
        try {
            file.createNewFile();
            System.out.println("新文件创建成功");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 1.1.2判断文件是否存在

语法：

```
public boolean exists();
```

代码实现：

```java
package com.iotest.RFile;

import java.io.File;

public class test {
    public static void main(String[] args) {
        File file = new File("/text.txt");//在该项目的根目录下/->D:/
        System.out.println("text.txt是否存在："+file.exists());
    }
}
```



### 1.1.3删除文件

语法：

```
public boolean delete();
```

代码实现：

```java
package com.iotest.RFile;

import java.io.File;

public class test {
    public static void main(String[] args) {
        File file = new File("/text.txt");//在该项目的根目录下/->D:/
        System.out.println("删除成功？"+file.delete());//删除text.txt
    }
}
```

注意：实际项目部署可能与开发环境不同，在开发时尽量使用File.separator；

`File file = new File(File.separator+"users"+File.separator+"text.txt");`

## 1.2目录操作

### 1.2.1取得父路径或父File对象

语法：

```
public String getParent();
public File getParentFile();
```

### 1.2.2创建目录

语法：

```
public boolean mkdirs();
```

代码实现：

```java
package com.iotest.RFile;

import java.io.File;
import java.io.IOException;

public class test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:"+File.separator+"testio"+File.separator+"text.txt");//在该项目的根目录下/->D:/
        if (!file.getParentFile().exists()){
            //该File对象的父File对象不存在，则创建父目录
            file.getParentFile().mkdirs();
        }
        //如果该文件对象存在，删除，不存在则创建
        if (file.exists()){
            file.delete();
        }else{
            file.createNewFile();
        }
    }
}
```

结果：

![1582772109766](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1582772109766.png)

## 1.3文件信息

在File类里面提供有一系列取得文件信息的方法

1.判断是否为文件：public boolean isFile();

2.判断路径是否为目录：public boolean isDirectory();

3.取得文件大小（字节）：public long length();

4.取得文件最后一次修改日期：public long lastModified();

栗子：

```java
package com.iotest.RFile;

import java.io.File;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:"+File.separator+"testio"+File.separator+"text.txt");//在该项目的根目录下/->D:/
        System.out.println("text.txt是否为文件："+file.isFile());
        File file1 = new File("D:"+File.separator+"testio");
        System.out.println("D:/testio是否为目录"+file1.isDirectory());
        System.out.println("text.txt的文件大小："+file.length());
        //long-->date
        System.out.println("text.txt的最后一次修改时间："+new SimpleDateFormat("yyyy-MM-dd hh:mm:ss").format(new Date(file.lastModified())));
    }
}
```



### 栗子：递归打印当前目录下所有层级的文件信息

```java
package com.iotest.RFile;

import java.io.File;
import java.io.IOException;

public class test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:"+File.separator+"testio");//在该项目的根目录下/->D:/
        listAllFiles(file);
    }

    public static void listAllFiles(File file){
        if (file.isDirectory()){
            File[] result = file.listFiles();
            if (result != null){
                for(File f : result){
                    listAllFiles(f);
                }
            }
        }else {
            System.out.println(file);
        }
    }
}
```

输出:

![1582777378008](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1582777378008.png)



# 2.字节流与字符流

File类不支持文件内容操作，如果要处理文件内容，必须通过流的操作模式来完成。流分为输入流和输出流。

在java.io中，流分为字节流和字符流

1.字节流:InputStream、OutputStream

2.字符流:Reader、Writer

## 2.1流操作

字节操作流

![1582777884854](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1582777884854.png)

字符操作流

![1582777896773](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1582777896773.png)

注意：

1.字节流与字符流操作的本质区别：字节流是原生操作，字符流是经过处理后的操作

2.在进行网络数据传输、磁盘数据保存所支持的数据类型：字节

3.而所有磁盘中的数据必须先读取到内存后才能进行操作，而内存中会将字节->字符。字符更适合处理中文

### 字节流/字符流的操作流程

（文件操作为例）

1. 根据文件路径创建File类对象；
2. 根据字节流/字符流的子类实例化父类对象
3. 进行数据的读取或写入操作
4. 关闭流

## 2.2字节输入流InputStream

1.定义

在程序中读取文件内容

```
public abstract class InputStream implements Closeable 
```

2.常用方法

- 读取数据到字节数组中，返回数据的读取个数：public int read(byte b[]);
- 读取部分数据到字节数组中：public int read(byte b[],int off,int len);
-  读取单个字节：public abstract int read();

3.实现

文件信息的读取

```java
package com.iotest.RStream;

import java.io.*;

public class test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:"+File.separator+"testio"+File.separator+"hello.txt");
        if (file.exists()){
            InputStream in = new FileInputStream(file);//创建字节流对象
            byte[] data = new byte[1024];//创建一个字节数组
            int len = in.read(data);//将文件内容读取到data字节数组中，返回读取长度
            String result = new String(data,0,len);//将字节数组内容转换为字符串
            System.out.println("hello.txt的文件内容为：["+result+"]");//输出文件内容
            in.close();
        }
    }
}
```

输出：

![1582786973134](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1582786973134.png)

## 2.3字节输出流OutputStream

1.定义

通过程序进行内容输出

```
public abstract class OutputStream implements Closeable,Flushable
```

2.常用方法

- 将给定的字节数组进行全部输出：public void write(byte b[]);
- 将部分字节数组内容输出：public void write(byte b[],int off,int len);
- 输出单个字节：public abstract void write(int b);

3.实现

```java
package com.iotest.RStream;

import java.io.*;

public class test {
    public static void main(String[] args) throws IOException {
        //out.txt文件不存在但会自动创建
        File file = new File("D:"+File.separator+"testio"+File.separator+"out.txt");
        OutputStream out = new FileOutputStream(file);
        out.write("hello,outputStream".getBytes());
        out.close();
    }
}
```

注意：

1.在进行文件输出时，所有的文件都会自动帮助用户创建（目录不会），不需要自己创建文件

2.如果此时程序重复执行，不会出现内容追加的情况而是会一直覆盖，如果想要追加的话，需要适用另一种构造方法

文件内容追加实现：

```java
package com.iotest.RStream;

import java.io.*;

public class test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:"+File.separator+"testio"+File.separator+"out.txt");
        OutputStream out = new FileOutputStream(file,true);
        out.write("\nhello,world".getBytes());
        out.close();
    }
}
```

部分内容输出实现：

```java
package com.iotest.RStream;

import java.io.*;

public class test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:"+File.separator+"testio"+File.separator+"out.txt");
        OutputStream out = new FileOutputStream(file,true);
        out.write("\nhello,java".getBytes(),0,5);//存储\nhell
        out.close();
    }
}
```



## 2.4字符输入流Reader

1.定义

在程序中读取内容操作

2.常用方法

Reader类中没有方法可以直接读取字符串类型，只能通过字符数组进行读取操作

- 读取字符数组的全部内容：public int read(char cbuf[]);
- 读取字符数组的部分内容：public int read(char cbuf[],int off,int len);
- 读取单个字符：public int read();

3.实现

```java
package com.iotest.RStream;

import java.io.*;

public class test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:"+File.separator+"testio"+File.separator+"out.txt");
        if (file.exists()) {
            Reader in = new FileReader(file) ;
            char[] data = new char[1024] ;
            int len = in.read(data) ; // 将数据读取到字符数组中
            String result = new String(data, 0, len) ;
            System.out.println("读取内容【"+result+"】") ;
            in.close();
        }
    }
}
```



## 2.5字符输出流Writer

字符适合处理中文数据

1.定义

与OutputStream非常相似，只是Write对中文的支持很好，字符不需要转换为字节数组

比OutputStream相比多了一个Appendable接口

```
public abstract class Writer implements Appendable,Closeable,Flushable
```

2.常用方法

给定字符输出：public void write(String str);

3.实现

```java
package com.iotest.RStream;

import java.io.*;

public class test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:"+File.separator+"testio"+File.separator+"out.txt");
        Writer writer = new FileWriter(file);
        writer.write("\ni like eat");//字符直接写入
        writer.close();
    }
}
```



## 2.6字节流VS字符流

1.字符流适合处理中文，字节流适合处理一切数据类型（对中文支持不好）

2.实际开发中，字节流一定是优先考虑的（图片，音乐，文字），只有在处理中文时才会考虑字符流

3.所有的字符流操作，无论是写入还是输出，数据都是先保存在缓存中的；如果字符流不关闭，数据就有可能保存在缓存中并没有输出的目标源，这种情况必须强制刷新



# 3.转换流

字符流<=>字节流

1.定义

转换流：将字节流和字符流互转

- OutputStreamWriter:字节输出流->字符输出流
- InputStreamReader:字节输入流->字符输入流

2.实现

```java
package com.iotest.RStream;

import java.io.*;

public class test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:"+File.separator+"testio"+File.separator+"out.txt");
        if (file.exists()) {
            OutputStream out = new FileOutputStream(file);
            OutputStreamWriter ow = new OutputStreamWriter(out);//字节流->字符流
            ow.write("hello");
            ow.close();
        }
    }
}
```

# 4.字符编码

## 4.1常用字符编码

1.GBK、GB2312:描述中文的编码；GBK包含简体中文和繁体中文，GB2312只包含简体中文

2.UNICODE：java提供的16进制编码；存在问题：如果现在所有字母都使用，编码太庞大，造成网络负担

3.ISO 8859-1:国际通用编码，但是所有编码都需要进行转换

4.UTF:结合了UNICODE和ISO 8859-1;需要使用16进制文字使用UNICODE，字母使用ISO 8859-1

## 4.2乱码产生原因

本地系统所用编码和程序所用编码不同，强制转换就会出现乱码

乱码产生本质：编码和解码不统一

以后就使用UTF-8

# 5.内存流

1.定义

IO操作也可以发生在内存中，这种流称为内存操作流。

需求：需要进行IO处理，但是又不希望产生文件。这种情况下可以使用内存作为操作终端

内存流分类：

- 字节内存流：ByteArrayInputStream、ByteArrayOutputStream
- 字符内存流：CharArrayReader、CharArrayWriter

2.继承关系

内存输出流

![1582810049942](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1582810049942.png)

内存输入流

![1582810062419](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1582810062419.png)

3.实现

内存流实现大小写转换

```java
package com.iotest.RStream;

import java.io.*;

public class test {
    public static void main(String[] args) throws IOException {
        InputStream in = new ByteArrayInputStream("hello".getBytes());
        OutputStream out = new ByteArrayOutputStream();
        int temp = 0;
        while ((temp = in.read()) != -1){
            // 每个字节进行处理,处理之后所有数据都在outputStream类中
            out.write(Character.toUpperCase(temp));
        }
        System.out.println(out);
        in.close();
        out.close();
    }
}
```

合并文件

```java
package com.iotest.RStream;

import com.sun.xml.internal.messaging.saaj.util.ByteInputStream;

import java.io.*;

public class test {
    public static void main(String[] args) throws IOException {
        //data_a.txt和data_b.txt文件为合并的两个文件
        File[] files = {new File("D:"+File.separator+"testio"+File.separator+"data_a.txt"),
                new File("D:"+File.separator+"testio"+File.separator+"data_b.txt")};
        String[] data = new String[2];//存储两个文件内容的字符数组
        for (int i = 0;i < files.length;i++){
            data[i] = readFile(files[i]);
        }
        StringBuffer buffer = new StringBuffer();
        String[] contentA = data[0].split(" ");
        String[] contentB = data[1].split(" ");
        for (int i = 0;i < contentA.length;i++){
            //文件合并
            buffer.append(contentA[i]).append("(").append(contentB[i]).append(")").append(" ");
        }
        //打印合并字符串
        System.out.println(buffer);
        //将合并内容写入新建文件data_merge.txt
        File file = new File("D:"+File.separator+"testio"+File.separator+"data_merge.txt");
        FileWriter writer = new FileWriter(file);
        writer.write(String.valueOf(buffer));//将StringBuffer->String
        writer.flush();//刷新
        writer.close();//关闭输出流
    }


    public static String readFile(File file) throws IOException {
        InputStream in = new FileInputStream(file);
        ByteArrayOutputStream out = new ByteArrayOutputStream();
        int temp = 0;
        byte[] data = new byte[10];
        while ((temp = in.read(data)) != -1){//将file文件内容读入data字节数组，返回读取字节个数
            // 每个字节进行处理,处理之后所有数据都在内存中，不会产生文件
            out.write(data,0,temp);
        }
        in.close();
        out.close();
        return new String(out.toByteArray());
    }
}
```

如果只是使用InputStream类，在进行数据完整读取的时候会很不方便，结合内存流的使用会好很多

# 6.打印流
打印流解决的是OutputStream的设计缺陷，属于OutputStream功能的加强版。如果操作的不是二进制数据，只是想通过程序向终端目标输出信息的话，OutputStream不是很方便，有以下两个缺点：

- 所有数据必须转换为字节数组
- 如果输出的是int、double等类型就不方便

## 6.1打印流概念

设计的主要目的是解决OutputStream的设计问题，其本质不会脱离OutputStream


```
package com.iotest.RStream;

import java.io.*;

/**
 * @Author:Star
 * @Date:Created in 11:42 2020/2/28
 * @Description:
 */

class printUtil{
    private OutputStream out;

    public printUtil(OutputStream out) {
        this.out = out;
    }

    // 核心功能就一个
    public void print(String str){
        try {
            this.out.write(str.getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public void println(String str){
        this.print(str+"\n");
    }

    //打印int数据
    public void print(int data){
        this.print(String.valueOf(data));
    }

    public void println(int data){
        this.println(String.valueOf(data));
    }

    //打印double数据
    public void print(double data){
        this.println(String.valueOf(data));
    }

    public void println(double data){
        this.println(String.valueOf(data));
    }

}


public class test1 {
    public static void main(String[] args) throws FileNotFoundException {
        printUtil printUtil = new printUtil(new FileOutputStream(new File("D:"+File.separator+"testio"+File.separator+"text.txt")));
        printUtil.print("姓名：");
        printUtil.println("star");
        printUtil.print("年龄");
        printUtil.println(18);
    }
}
```
经过简单的处理之后，本质就是对OutputStream的功能做了一个封装而已。java提供专门的打印流处理类：PrintStream、PrintWriter

## 6.2系统打印流

字节打印流：PrintStream

字符打印流：PrintWriter

打印流的设计属于装饰设计模式：核心依然是某个类的功能，但是为了得到更好的操作效果，让其支持的功能多一些。

栗子：打印流的使用以及格式化输出

```
package com.iotest.RStream;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.PrintWriter;

public class test2 {
    public static void main(String[] args) throws FileNotFoundException {
        String name = "star";
        int age = 20;
        //使用系统的打印流
        PrintWriter printWriter = new PrintWriter(new FileOutputStream(new File("D:"+File.separator+"testio"+File.separator+"text.txt")));
        printWriter.printf("姓名:%s,年龄:%d",name,age);//格式化输出
        printWriter.close();
    }
}
```

# 7.System类对IO的支持
实际上我们一直在使用的系统输出就是利用了IO流的模式完成，在System类中定义了三个操作的常量。

1.标准输出：public final static PrintStream out

2.错误输出：public final static PrintStream err

3.标准输入：public final static InputStream in

## 7.1系统输出

系统输出一共有两个常量：out、err，并且这两个常量表示的都是PrintStream类的对象

1.out输出的是希望用户能看到的内容
2.err输出的是不希望用户看到的内容

注意：由于System.out是PrintStream的实例化对象，而PrintStream又是OutputStream的子类，所以可以直接使用System.out直接为OutputStream实例化，这个时候的OutputStream输出的位置将变为屏幕。

栗子：使用System.out为OutputStream实例化

```
package com.iotest.RStream;

import java.io.*;

public class test2 {
    public static void main(String[] args) throws IOException {
        OutputStream out = System.out;
        out.write("hello".getBytes());
    }
}
```
## 7.2系统输入

System.in对应的类型是InputStream，而这种输入流指的是用户通过键盘输入。Java本身没有直接的用户输入处理，要实现这种操作，必须使用java.io模式来完成

栗子：使用InputStream实现数据输入

```
package com.iotest.RStream;

import java.io.*;

public class test2 {
    public static void main(String[] args) throws IOException {
        InputStream in = System.in;//实例化输入流对象
        byte[] bytes = new byte[1024];//创建新的字节数组
        System.out.println("请输入内容：");
        int temp = in.read(bytes);//读取输入到字节数组中
        System.out.println("输入内容为："+new String(bytes,0,temp));//将字节数组中的内容转换为字符输出
    }
}
```
上面的程序存在一个问题：创建的新的字节数组长度固定，如果输入长度大于字节数组的长度，那么只能接受部分数据，此时可以借助内存流来改善这一现象


```
package com.iotest.RStream;

import java.io.*;

public class test2 {
    public static void main(String[] args) throws IOException {
        InputStream in = System.in;//实例化输入流对象
        ByteArrayOutputStream out = new ByteArrayOutputStream();
        byte[] bytes = new byte[10];//创建新的字节数组
        System.out.println("请输入内容：");
        int temp = 0;
        while ((temp = in.read(bytes)) != -1){//读取输入到字节数组中
            out.write(bytes,0,temp);
            //需要用户判断，因为此时的输入流不是内存流，需要数组来暂存输入内容
            if (temp < bytes.length){
                break;
            }
        }
        System.out.println("输入内容为："+new String(out.toByteArray()));
        in.close();
        out.close();
    }
}
```

# 8.两种输入流

## 8.1BufferedReader类
BufferedReader类是一个缓冲的输入流，而且是一个字符流操作对象，java中有两类缓冲流：字节缓冲流BufferedInputStream、字符缓冲流BufferedReader

解决问题：IuputStream的缺陷，需要字节数组来接受读取的内容。

BufferedReader的常用方法：
读取一行数据：
```
String readLine();
```
栗子：使用BufferedReader实现键盘输入

```
package com.iotest.RStream;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class test3 {
    public static void main(String[] args) throws IOException {
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        System.out.println("请输入内容：");
        String string = reader.readLine();//读取数据
        System.out.println("输入内容为："+string);
    }
}
```
注意：BufferedReader还有一个最大特点就是接受的数据类型为String,可以使用String类的各类操进行数据处理变成各种常见数据类型

## 8.2Scanner类

定义：专门进行输入流处理的程序类，利用这个类可以方便处理各种数据类型

解决问题：BufferedReader的缺陷，更加的方便，有各种常见数据的读取方法，减少转型处理

常见方法：
1.判断是否有指定数据类型：public boolean hasNextXXX()

2.取得指定类型的数据：public 数据类型 nextXXX()

3.定义分隔符：public Scanner useDelimiter(Pattern pattern)

4.构造方法：public Scanner(InputStream source)

栗子：使用Scanner实现数据输入

```
package com.iotest.RStream;

import java.io.IOException;
import java.util.Scanner;

public class test3 {
    public static void main(String[] args) throws IOException {
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入年龄：");
        if (scanner.hasNextInt()) {
            // 有输入内容,不判断空字符串
            int age = scanner.nextInt();
            System.out.println("输入内容为: " + age);
        } else {
            System.out.println("输入的不是数字!");
        }
        scanner.close();
    }
}
```
总结：除了对二进制文件的拷贝处理外，针对程序的信息输出使用打印流（PrintStream、PrintWriter），信息输入使用Scanner

# 9.序列化

1.定义

对象序列化：对象->二进制数据流

2.实现

```
package com.iotest.RStream;

import java.io.*;

class Person implements Serializable {
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

public class test4 {
    public static final File file = new File("D:"+File.separator+"testio"+File.separator+"text.txt");
    public static void main(String[] args) throws IOException {
        ser(new Person("张三",18));
    }

    //序列化
    public static void ser(Object person) throws IOException {
        ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream(file));
        out.writeObject(person);
        out.close();
    }

    //反序列化
    public static void dser() throws IOException, ClassNotFoundException {
        ObjectInputStream in = new ObjectInputStream(new FileInputStream(file));
        in.readObject();
        in.close();
    }
}
```
注意：Serializable默认会将对象中所有属性进行序列化保存。如果希望某些属性不被保存，可以使用transient关键字