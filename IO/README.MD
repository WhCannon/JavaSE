# IO流
- ## [字符流](#字符流)
   - [FileWriter](#filewriter)
   - [FileReader](#filereader)
   - [IO异常的处理](#io异常的处理)
- ## [字节流](#字节流)






## 字符流
字节流读取文字字节数据后，不直接操作而是先查指定的编码表，获取对应的文字，再对这个文字进行操作。  
顶层抽象父类：Reader,Writer  
### FileWriter
将一些文字存储到硬盘的一个文件中
```java
public class FileWriterDemo {
private static final String LINE_SEPARATOR = System.getProperty("line.separator");//换行
	public static void main(String[] args) throws IOException {
		//创建一个可以往文件中写入字符数据的字符输出流对象。
		/*在创建对象时，就必须明确该文件名(用于存储数据的目的地)。
		  * 如果文件不存在，会自动创建。如果文件存在，则会被覆盖。
		 * 
		  * 如果构造函数中加入true，可以实现对文件进行续写！ */
		FileWriter fw = new FileWriter("demo.txt",true);//会发生IO异常
		
		// 调用Writer对象中的write(string)方法，写入数据。 
		fw.write("abcde"+LINE_SEPARATOR+"hahaha");//数据写入到临时存储缓冲区中
		fw.write("xixi");//写入时也会发生IO异常
		
		// 进行刷新，将数据写到文件中。
		fw.flush();
		// 关闭流，关闭资源。在关闭前会先调用flush刷新缓冲中的数据到目的地。
		fw.close();
//	fw.write("haha");// 关闭流之后，不能继续写入。java.io.IOException: Stream closed
	}
}
```
### FileReader
int read();读取单个字符
```java
//需求：读取一个文本文件。将读取到的字符打印到控制台。
public static void main(String[] args) throws IOException {
	//创建读取字符数据的流对象。
	//在创建读取流对象时，必须要明确被读取的文件。一定要确定该文件是存在的。  
	FileReader fr = new FileReader("demo.txt");
	//用Reader中的read方法读取单个字符。
	int ch = fr.read();//read方法返回的是整数（ASCII码表），如果已到达流的末尾，则返回-1
	System.out.println((char)ch);//转换为字符类型
	//用循环方式来读取
	int ch = 0;
	while((ch=fr.read())!=-1){
		System.out.println((char)ch);
		}
	fr.close();
	}
```
int read(char[] c);将字符读入数组，返回读到的字符数
```java
public static void main(String[] args) throws IOException {
FileReader fr = new FileReader("demo.txt");//假设存储的“abcde”
char[] buf = new char[3];//先创建字符数组,把数组长度变大,可以一次读完,一般为1024的整数倍
		
int num = fr.read(buf);//将读取到的字符存储到数组中。
System.out.println(num+":"+new String(buf));//3=abc
int num1 = fr.read(buf);//将读取到的字符存储到数组中。
System.out.println(num1+":"+new String(buf));//2=dec，读了2个，de覆盖了ab
int num2 = fr.read(buf);//将读取到的字符存储到数组中。
System.out.println(num2+":"+new String(buf));//-1=dec，读到结尾，返回-1，数组中还是dec
	//	改为循环方法
	int len = 0;
	while((len=fr.read(buf))!=-1){
		System.out.println(new String(buf,0,len));//从0读到len处
	}
	fr.close();
}
```
### IO异常的处理
```java
public static void main(String[] args) {
		FileWriter fw = null;//先创建引用型变量
		try {
			fw = new FileWriter("k:\\demo.txt");//再在try内进行初始化
			fw.write("abcde" + LINE_SEPARATOR + "hahaha");
		} catch (IOException e) {
			System.out.println(e.toString());
		} finally {
			if (fw != null)//如果try中创建对象的路径不存在，会创建失败，fw=null
				try {
					fw.close();
				} catch (IOException e) {
					// code....
					throw new RuntimeException("关闭失败");
				}
		}
	}
```

## 字节流
