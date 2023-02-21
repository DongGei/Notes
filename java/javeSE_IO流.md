---
title: java—IO流
date: 2021-12-7 16:45:00
author: dong
img: /images/03.jpg
top: false 
hide: false
cover: false 
coverImg: /images/07.jpg
toc: true  
mathjax: false
summary: java—IO流
categories: Java
tags: 
- Java
---

# IO流
## 一. File
file:文件和目录路径名的抽象表示  
文件和目录是可以通过file封装成对象的  
### 1. 构造方法
```jave
 File(String pathname)//最简单的
 File(String parent, String child)//父路径和子路径 合起来就是一个完整的路径
 File(File parent, String child)
```
 它只是保存一个路径 没有验证路径是否正确。  
 相对路径：myFile\\java.txt  （ 默认是项目下的与模块同级 ）
 绝对路径:E:\\abc\\java.txt  
    

### 2. 创建功能
```java
 public boolean createNewFile() //当且仅当具有此名称的文件尚不存在时，以原子方式创建由此抽象路径名命名的新空文件。  
 public boolean mkdir() //创建此抽象路径名指定的目录。//是目录！！文件夹  
 public boolean mkdirs()//创建此抽象路径名指定的目录，包括任何必需但不存在的父目录。 创建多级目录
 
 // 3个都是如果存在返回flase
 File f1 =new File("E:\\abc\\java.txt"); 
 f1.createNewFile();
```

### 3. 获取和判断功能
 判断路径是文件还是目录
 获取父目录下的子目录（和文件）路径地址或名称 //返回的是一个数组
### 4. 删除功能
	f1.delete（）；
	删除文件或目录  
	删除目录时注意：目录下有文件无法直接删 ，先删文件再删目录。
### 5.递归
注意：递归出口，使用规则：与原问题相似的规模较小的问题
```java
/*
一对兔子 第3个月开始 每个月 生1对龙凤胎 兔子不死
找规律 每个月等于前两个月的和

 */
public class BuSiShengTu {
    public static void main(String[] args) {
        int[] arr=new int[20];
        arr[0]=1;
        arr[1]=1;
        for(int i=2;i<arr.length;i++){
            arr[i]=arr[i-2]+arr[i-1];
        }
        for(int i=0;i<arr.length;i++){
            System.out.println(arr[i]);
        }
        System.out.println(arr[19]);
        System.out.println(f(20));
    }
    public static int f(int n){
        if (n==1 || n==2){
            return 1;
        }else return f(n-1)+f(n-2);
    }
}
```

###  6.案例：遍历目录
```java
import java.io.File;
import java.nio.channels.FileLockInterruptionException;

public class BIanLiMuLu {
    public static void main(String[] args) {
        File srcfile = new File("D:\\杂\\钉钉\\DingDing");
        f(srcfile);
    }
    public static void f(File file){
        File[] files = file.listFiles();
        if (files!= null){
            for (File filei :files){
                if (filei.isDirectory()){
                    f(filei);
                }else {
                    System.out.println(filei.getAbsoluteFile());
                }

            }
        }
    }
}

```
##  二.字节流
### 1.IO流概述
+ IO: Input/Output
+ 流：是一张抽象概念，是数据传输的总称，数据在设备之间的传输为流。
+ 分类：输出流/输入流，
       字节流/字符流（在记事本读的懂的是字符流，读不懂的是字节流）（不确定用字节）

### 2.字节流写数据

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamDemo {
    public static void main(String[] args) throws IOException {
        FileOutputStream fileOutputStream = new FileOutputStream("D:\\学习\\java\\test\\fos.txt");
        /*
        完成了3步
        1.调用系统创建了文件
        2.创建了输出流对象
        3.输出流对象指向创建的文件
         */
        fileOutputStream.write(97);
        fileOutputStream.write(57);
        fileOutputStream.write(55);
        //IO相关的最后要释放资源
        //void close() 关闭此文件输出流并释放与此流关联的所有系统资源。
        fileOutputStream.close();
    }
}
```
### 3.字节流写数据的3种方式
void write(int b) 将指定的字节写入此文件输出流。    //一次写一个字节数据
void write(byte[] b, int off, int len) 将从偏移量 off开始的指定字节数组中的 len字节写入此文件输出流。  //一次写一个字节数组数据的部分数据
void write(byte[] b) 将指定字节数组中的 b.length字节写入此文件输出流。  //一次写一个字节数组数据
```java
		fileOutputStream.write(97);
        fileOutputStream.write(57);
        fileOutputStream.write(55);

		 byte[] bytes={97,98,99,100,101};
       	 fileOutputStream.write(bytes);
       	 
       	 byte[] bytes="abcdef".getBytes(); //byte[] getBytes() 使用平台的											默认字符集将此 String编码为字节											序列，将结果存储到新的字节数组中。
       
        fileOutputStream.write(bytes,1,3);//输入bcd
        
```
### 4.写入换行
加一个 fos.write("\n\r".getBytes());    \n\r 是为了适应多种系统
### 追加写入
FileOutputStream(String name, boolean append) 创建文件输出流以写入由指定的File对象表示的文件。 如果第二个参数是true ，则字节将写入文件的末尾而不是开头。
### 5.字节流写数据加异常处理
原因
```java
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamDemo01 {
    public static void main(String[] args) {
        FileOutputStream fileOutputStream = new FileOutputStream("D:\\学习\\java\\test\\fos.txt");
        try {
            fileOutputStream.write("abcef".getBytes());
            fileOutputStream.close();//如果上面一行发生错误，程序直接运行  e.printStackTrace(); 而没有释放系统资源
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
finally:在异常处理时提供finally块来执行所以清除操作。
被finally执行的语句一定会执行，除非JVM退出
```java
    FileOutputStream fileOutputStream = null;
        try {
            fileOutputStream = new FileOutputStream("D:\\学习\\java\\test\\fos.txt");
            fileOutputStream.write("abcef".getBytes());
            //如果上面一行发生错误，程序直接运行  e.printStackTrace(); 而没有释放系统资源
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if (fileOutputStream !=null){ //防止路径错误，导致指向null
                try {
                    fileOutputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
### 6.字节流读数据
1. 一个读一个
```java
import java.io.FileInputStream;
import java.io.IOException;

public class FileInPutDemo {
    public static void main(String[] args) throws IOException {
        FileInputStream fis =new FileInputStream("D:\\学习\\java\\test\\fos.txt");
//int read() 从此输入流中读取一个字节的数据。
//        int read = fis.read();
//        System.out.println(read);
//        read = fis.read();
//        System.out.println(read);


        //先判断再执行 再判断再执行  判断和语句也会循环执行
        int by =0;
        while((by=fis.read())!= -1){//读到文本尾部值是-1
            System.out.print((char)by);
        }

        fis.close();
    }
}
```
2. 一次读一个字节数组
```java
int read(byte[] b)// 从此输入流 b.length最多 b.length字节的数据读 b.length字节数组。 
```
+ 返回的是实际读到的长度 如果byte【5】，返回4，说明第5个位置没改动
+ 换行会读两个字节长度   \r\n 再次输出时 输出一个空行
```java
import java.io.FileInputStream;
import java.io.IOException;

public class FileInPutDemo2 {
    public static void main(String[] args) throws IOException {
        FileInputStream fis =new FileInputStream("D:\\学习\\java\\test\\fos.txt");
        byte[] bys =new byte[1024]; //一般给1024及其整数倍
        int len=0;
        while (-1!= (len=fis.read(bys))){ //没有读到数据len=-1
             String s = new String(bys, 0, len);//取bys里的0到len
             System.out.println(s);
             //fos.write(bys,o,len)
        }
        fis.close();
    }

}

```

### 7.字节流复制文本文件
```java
while((by=fis.read())!= -1){
         fos.write(by);
        }
```
### 8.字节流复制图片
```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyJPGDemo {
    public static void main(String[] args) throws IOException {
        FileInputStream inputStream = new FileInputStream("D:\\zha\\photo\\001.png");

        FileOutputStream fileOutputStream = new FileOutputStream("D:\\学习\\java\\test\\001.png");
        int len =0;
        byte[] bys = new byte[1024];
        while ((len=inputStream.read(bys))!=-1){
            fileOutputStream.write(bys,0,len);
        }
        fileOutputStream.close();
        inputStream.close();
    }
}
```
### 9.字节缓冲流
避免不断调用底层系统
仅仅提供缓冲区，真正的读写数据还得依靠基本的字节流对象进行操作
```java
BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("D:\\学习\\java\\test\\bos.txt"));
        bos.write("hello\r\n".getBytes());
        bos.write("world".getBytes());
        bos.close();

        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("D:\\学习\\java\\test\\bos.txt"));
        int by =0;
        while((by=bis.read())!= -1){
            System.out.print((char)by);
        }
        bis.close();
```
### 复制文件运行速度比较
字节缓冲流一次读写一个数组>  
基本字节流一次读写一个数组>  
字节缓冲流一次读写一个字节>  
基本字节流一次读写一个字节  
## 二.字符流
### 1.需求
一个汉字  GKB编码占两个字节
			UTF-8编码占3个字节
两种编码 汉字储存时第一个字节都是负的，便以识别

字符流=字节流+编码表
### 2.编码表
### 3.字符串编码解码问题
String (byte[] bytes, String charsetName) 构造一个新的String由指定用指定的字节的数组解码 
byte[] getBytes(String charsetName) 使用命名的字符集将此 String编码为字节序列，将结果存储到新的字节数组中  //eg：new string（”abcd“）.getBytes（“GKB”）

### 4.字符流的编码解码问题
待续