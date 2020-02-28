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