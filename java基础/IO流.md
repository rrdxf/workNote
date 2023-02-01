### IO流

![image-20211004203100889](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211004203100889.png)

#### 概念：

I：input

o：output

通过IO可以完成硬盘文件的读和写

#### 分类：

有多种分类方式：

- 一种方式是按照流的方向进行分类：
  - 以内存作为参照物：
    - 往内存中去，叫做Input，或者叫read
    - 从内存中出，叫做输出Output，或者write
- 另一种方式是按照读取数据方式的不同进行分类
  - 优点流按照字节的方式读取数据，一次读取一个字节byte，等同于读取8个二进制位。这种流是万能的，什么类型的文件都可以读取。包括文本文件，图片，声音，视频等
  - 有的按照字符的方式读取数据，一次读取一个字符，这种流是为了方便读取普通文本文件而存在的，这种流不能读取：图片、声音、视频等。只能读取纯文本文件，连word文件都无法读取
    - 假设问价file1.txt，采用字符流的话
    - a中国bc张三fe
    - 第一次读：‘a’字符（a在windows中占用1字节）
    - 第二次：‘中’（占用2个字节）
- 输入流、输出流、字节流、字符流

java中所有的流都在java.io.*;下

#### 四大家族：

1. 字节流：
   - java.io.InputStream
   - java.io.OutputStream
2. 字符流
   - java.io.Reader
   - java.io.Writer

都是抽象类

​	*所有的流都是可关闭的，都有close()方法，流毕竟是一个管道，这个内存和硬盘之间的通道，用完之后一定要关闭，不然会耗费很多资源。*

​	所有的输出流都实现了java.io.Flushable接口，都是可刷新都，都有flush（）方法。输出流在最终输出之后，一定要flush（）刷新一下。这个刷新表示将通道管道中剩余未输出的数据强行输出完。刷新的管道就是清空中剩余的数据全部刷新，没有flush可能会丢失资源

**在java中，以Stream结尾的是字节流，以Reader/Writer是字符流**

***********************************

#### 需要掌握的16个流

文件专属：

java.io.FileInputStream

```java
 FileInputStream fis=null;
        try{
            fis=new FileInputStream("test/src/io/temp");
            byte[] bytes=new byte[4];
            int readcount=0;
            while((readcount=fis.read(bytes))!=-1){
                System.out.print(new String(bytes,0,readcount));
            }
        }catch(IOException e){
            e.printStackTrace();
        }finally{
            try{
                if(fis!=null){
                    fis.close();
                }
            }catch(IOException e){
                e.printStackTrace();
            }
        }
```

java.io.FileOutputSteam

```java
fos=new FileOutputStream("tem");
//新建文件
fos1=new FileOutputStream("tem",true);
//这样会在源文件的后面追加
```

##### 文件拷贝

![image-20211004215306501](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211004215306501.png)

```java
/*
使用FileInputStream+FileOutputStream实现文件的拷贝
拷贝的过程是一边读一边写
使用以上的字节流拷贝文件的时候，文件类型随意。
*/
public static void main(String [] args){
	FileInputStream fis=null;
	FileOutputStream fos=null;
	try{
		fis=new FileInputStream("st");
		fos=new FileOutputStream("ou");
		byte[] bytes=new byte[1024*1026];//1M
		//1字节八位
		/*
		int 四字节
		long 八位字节
		byte一字节
		char二字节
		*/
		int readcount=0;
		while((readcount=fis.read(bytes))!=-1){
			fos.write(bytes,0,readcount);
		}
	
	}catch(IOException e){
		e.printStackTrace();
	}finally{
		if(fis!=null){
			try{
				fis.close();
            }catch(IOException e){
                e.printStackTrace();
            }
		}
		
		if(fos!=null){
			try{
				fos.close();
            }catch(IOException e){
                e.printStackTrace();
            }
		}
	
	}

}
```



java.io.FileReader

java.io.FileWriter

****

转换专属：将字节流转换成字符流

java.io.InputStreamReader

java.io.OutPutStreamWriter

***

缓冲流专属

java.io.BufferedReader

java.io.BufferedWriter

java.io.BufferedInputStream

java.io.BufferedOutputStream

***

数据流

java.io.DataInputStream

java.io.DataOutputStream

***

标准输出流

java.io.ObjectInputStream

java.io.ObjetctInputStream

***

对象专属流

java.io.PrintWriter

java.io.PrintStream

#### idea默认的当前路径project的根

