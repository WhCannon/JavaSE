# IO流
- ## [字符流](#字符流)
   - [FileWriter](#filewriter)
   - [FileReader](#filereader)
   - [缓冲区](#缓冲区)
   - [转换流](#转换流)
   - [IO异常的处理](#io异常的处理)
- ## [字节流](#字节流)
   - [FileInputStream](#fileinputstream)
   - [FileOutputStream](#fileoutputstream)
   - [管道流](#管道流)
   - [DateStream](#datestream)
- ## [File类](#file-类)
- ## [打印流](#打-印-流)
- ## [序列流](#sequenceinputstream)
- ## [对象输入输出流](#object流)
- ## [RandomAccessFile](#randomaccessfile随机读取文件)


- ## [练习](#练-习)






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
### BufferedWriter



### BufferedReader




示例：
使用缓冲区复制文件
```java
public static void main(String[] args) throws IOException {

		FileReader fr = new FileReader("buf.txt");		
		BufferedReader bufr = new BufferedReader(fr);
		
		FileWriter fw = new FileWriter("buf_copy.txt");
		BufferedWriter bufw = new BufferedWriter(fw);
		
		String line = null;
		while((line=bufr.readLine())!=null){
			bufw.write(line);
			bufw.newLine();
			bufw.flush();
		}
		bufw.close();
		bufr.close();
	}
```

### 缓冲区
缓冲区对象操作的都是缓冲区内的数据。

#### BufferedWriter
newLine();写入一个行分隔符
```java
public class BufferedWriterDemo {

private static final String LINE_SEPARATOR = System.getProperty("line.separator");
	public static void main(String[] args) throws IOException {
		
		FileWriter fw = new FileWriter("buf.txt");
		
		//为了提高写入的效率。使用了字符流的缓冲区。
		//创建了一个字符写入流的缓冲区对象，并和指定要被缓冲的流对象相关联
		BufferedWriter bufw = new BufferedWriter(fw);
		
		//使用缓冲区的写入方法将数据先写入到缓冲区中。
		bufw.write("abcdefq"+LINE_SEPARATOR+"hahahha");
		bufw.newLine();//只有BufferedWriter具备newLine方法
		bufw.write("heheheheh");
		//使用缓冲区的刷新方法将数据刷目的地中。
		bufw.flush();
//		改为循环方式
		for(int x=1; x<=4; x++){
			bufw.write("abcdef"+x);
			bufw.newLine();
			bufw.flush();
		}
		//关闭缓冲区。其实关闭的就是被缓冲的流对象。
		bufw.close();
	}
}

```

#### BufferedReader
实现字符、数组和行(根据行分隔符判断有几行)的高效读取,readLine()
```java
public static void main(String[] args) throws IOException {
		FileReader fr = new FileReader("buf.txt");
		BufferedReader bufr = new BufferedReader(fr);
		String line = null;
		while((line=bufr.readLine())!=null){
			System.out.println(line);
		}
		bufr.close();
	}

```

#### LineNumberReader
setLineNumber(int);设置行号  
getLineNumber();获取行号，号随数据读取在每个行结束符处递增。
```java
public static void main(String[] args) throws IOException {
		FileReader fr = new FileReader("IO流_2.txt");
		LineNumberReader lnr = new LineNumberReader(fr);
		
		String line = null;
		lnr.setLineNumber(100);
		while((line=lnr.readLine())!=null){
			System.out.println(lnr.getLineNumber()+":"+line);
		}
		lnr.close();
	}

```





### 转换流
#### InputStreamReader

是字节流通向字符流的桥梁，使用指定的charset（字符集编码表）读取字节并将其解码为字符。  
为了提高效率，可以考虑在BufferedReader内包装InputStreamReader,如：
```java
BufferedReader in=new BufferedReader(new InputStreamReader(System.in))；
```

#### OutputStreamWriter
是字符流转向字节流的桥梁，使用指定的charset（字符集编码表）将要写入流中的字符编码成字节。

#### [示例](#练习4)

#### 转换流的编码解码
```java
public static void writeText_2() throws IOException {
OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("gbk_3.txt"),"GBK");
FileWriter fw = new FileWriter("gbk_1.txt");
/* 这两句代码的功能是等同的。 
* FileWriter：其实就是转换流指定了本机默认码表的体现。作为转换流的子类对象，可以方便操作文本文件。
*   简单说，FileWriter就是操作文件的字节流+本机默认的编码表。这是按照默认码表来操作文件的便捷类。
* 如果操作文本文件需要明确具体的编码。FileWriter就不行了，必须用转换流。 */
		osw.write("你好");
		osw.close();
	}
public static void writeText_3() throws IOException {
		//指定一个编码表进行编码
		OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("u8_1.txt"),"UTF-8");
		osw.write("你好");
		osw.close();
	}
同理，可以按照指定的码表进行解码，来读取。
public static void readText_2() throws IOException, FileNotFoundException {
		InputStreamReader isr = new InputStreamReader(new FileInputStream("gbk_1.txt"),"utf-8");
		char[] buf = new char[10];
		int len = isr.read(buf);
		String str = new String(buf,0,len);
		System.out.println(str);
		isr.close();
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
### FileInputStream


### FileOutputStream


### 管道流
输入输出可以直接进行连接，通过结合线程使用。  
PipedInputStream、PipedOutputStream  
通常，数据由某个线程从PipedInputStream对象读取，并由其他线程将其写入到相应的PipedOutputStream，  
不建议对这两个对象使用单线程，这样可能会导致死锁。  
```java
connect(PipedOutputStream p); 将管道流的输入与输出连接
public class PipedStream {
	public static void main(String[] args) throws IOException {

		PipedInputStream input = new PipedInputStream();
		PipedOutputStream output = new PipedOutputStream();
		
		input.connect(output);
		
		new Thread(new Input(input)).start();
		new Thread(new Output(output)).start();
	}
}

class Input implements Runnable{
	
	private PipedInputStream in;
	Input(PipedInputStream in){
		this.in = in;
	}
	public void run(){
		try {
			byte[] buf = new byte[1024];
			int len = in.read(buf);//阻塞方法，有数据才能读，否则就一直等待
			String s = new String(buf,0,len);
			System.out.println("s="+s);
			in.close();
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
}

class Output implements Runnable{
	private PipedOutputStream out;
	Output(PipedOutputStream out){
		this.out = out;
	}
	public void run(){
		try {
			Thread.sleep(5000);
			out.write("hi，管道来了！".getBytes());
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
}

```

### DateStream
操作基本数据类型的流对象。有writeInt、WriteLong等方法。
```java
public class DataSteamDemo {
	public static void main(String[] args) throws IOException {
//		writeData();
		readData();
	}

	public static void readData() throws IOException {
		DataInputStream dis = new DataInputStream(new FileInputStream("data.txt"));
		String str = dis.readUTF();
		System.out.println(str);
	}

	public static void writeData() throws IOException {
		DataOutputStream dos = new DataOutputStream(new FileOutputStream("data.txt"));
		dos.writeUTF("你好");//按UTF-8编码表写出
		dos.close();
	}
}
```


## File 类
将文件或者文件夹封装成对象，方便对文件和文件夹的属性信息进行操作。File对象可以作为参数传递给流的构造函数。  
public static final String separator; 与系统相关的默认名称分隔符。  
该字段被初始化为包含系统属性值file.separator的第一个字符。在UNIX系统上，该字段的值为'/'，在Microsoft Windows系统上是'\\'。  
public static final String pathSeparator;与系统相关的路径分隔符。  
该字段被初始化为包含系统属性值path.separator的第一个字符。此字符用于将路径列表中给出的文件序列中的文件名分隔。在UNIX系统上，这个字符是':';在Microsoft Windows系统上，它是';'。 

### 构造方法
可以将一个已存在的，或者不存在的文件或者目录封装成file对象。
```java
File f1 = new File("c:\\a.txt");
File f2 = new File("c:\\","a.txt");
		
File f = new File("c:\\");
File f3 = new File(f,"a.txt");
//分隔符的表示		
File f4 = new File("c:"+File.separator+"abc"+File.separator+"a.txt"); //c:\abc\a.txt
```


### 常用方法
1、获取  
* 1.1获取文件名称  
* 1.2获取文件路径  
* 1.3获取文件大小  
* 1.4获取文件修改时间  
```java
		File file = new File("a.txt");
//		File file = new File("E:\\java0331\\day22e\\a.txt");//绝对路径
		
		String name = file.getName();
		
		String absPath = file.getAbsolutePath();//绝对路径。
		String path = file.getPath();//相对路径
		
		long len = file.length();//文件大小
		
		long time = file.lastModified();//最后修改时间
		Date date = new Date(time);
		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG,DateFormat.LONG);
		String str_time = dateFormat.format(date);
```
2、创建与删除  
```java
		File file = new File("file.txt");
		//文件的创建
		//和输出流不一样，如果文件不存在则创建，返回true；如果文件存在就不创建，返回false。 
		boolean b = file.createNewFile();		
		//删除
		boolean b = file.delete();
		file.deleteOnExit();//在虚拟机退出时删除
		
		File dir = new File("abc");
		boolean b = dir.mkdir();//make directory创建目录
		
		File dir = new File("abc\\q\\e\\c\\z\\r\\w\\y\\f\\e\\g\\s");
		dir.mkdirs();//创建多级目录
		System.out.println(dir.delete());//删除了s目录
```
3、判断
```java
		File f = new File("a.txt");
		boolean b = f.exists();//是否存在，最好先判断是否存在。
		System.out.println(f.isFile());//是文件吗
		System.out.println(f.isDirectory());//是目录吗
```
4、重命名
```java
	//重命名
		File f1 = new File("c:\\9.mp3");
		File f2 = new File("d:\\aa.mp3");//路径不同时，会将文件移至指定的路径，并重命名
		boolean b = f1.renameTo(f2);
		System.out.println("b="+b);
```
5、其他
* 获取系统根目录
```java
		//获取系统根目录C、D、E...盘
		File[] files  = File.listRoots();		
		for(File file : files){
			System.out.println(file);
		}
		
		File file = new File("d:\\");
		System.out.println("getFreeSpace:"+file.getFreeSpace());//分区中未分配的字节数
		System.out.println("getTotalSpace:"+file.getTotalSpace());//指定分区的大小
```
* list方法
```java
		/* 获取当前目录下的文件以及文件夹的名称，包含隐藏文件。
		 * 调用list方法的File对象中封装的必须是目录。
		 * 否则会发生NullPointerException
		 * 如果访问的系统级目录也会发生空指针异常。
		 * 如果目录存在但是没有内容，会返回一个数组，但是长度为0.
		 */
		File file = new File("c:\\");
		String[] names = file.list();
		System.out.println(names.length);
		
		for(String name : names){
			System.out.println(name);
		}
```
list只能获取文件夹或文件的名称，只能对名称进行操作。  
listFiles获取的是File对象，可以进行其他的操作。
* list过滤器
```java
定义一个过滤器
public class SuffixFilter implements FilenameFilter {1
	private String suffix ;
	public SuffixFilter(String suffix) {
		super();
		this.suffix = suffix;
	}
	public boolean accept(File dir, String name) {
		return name.endsWith(suffix);
	}
}
public static void listDemo_2() {
		File dir = new File("c:\\");
		String[] names = dir.list(new SuffixFilter(".txt"));
		for(String name : names){
			System.out.println(name);
		}
```

## 打 印 流
* PrintStream 与 PrintWriter
```java
print与write的区别
PrintStream out = new PrintStream("print.txt");//可以直接打印到文件中
out.write(97);//写入一个字节（int有四个字节），是参数的八个低位，其他高位被忽略，写的二进制被解码为字母a
out.write(610);//只写最低8位，'b'
out.print(97);//写什么就打印什么，将97先变成字符串保持原样，再打印到目的地，打印"97"
out.close();
```
PtintWriter
```java
BufferedReader bufr = new BufferedReader(new InputStreamReader(System.in));
		
PrintWriter out = new PrintWriter(new FileWriter("out.txt"),true);//自动刷新
		
		String line =  null;
		while((line=bufr.readLine())!=null){
			if("over".equals(line))
				break;
			out.println(line.toUpperCase());
//			out.flush();定义了自动刷新，可以不用flush
		}
		out.close();
		bufr.close();
```
## SequenceInputStream
一个读取流只能关联一个源，要想关联多个源就要使用多个读取流，  
这时为了方便操作，可以将这些流合并为一个流，这个流就是序列流。  
使用示例：  
```java
需求：将1.txt 2.txt 3.txt文件中的数据合并到一个文件中。
public static void main(String[] args) throws IOException {
		/*
		 * 需求：将1.txt 2.txt 3.txt文件中的数据合并到一个文件中。
		 */
		//序列流需要接收一个枚举，所以要先获得一个枚举对象。
/*		Vector<FileInputStream> v = new Vector<FileInputStream>();		
		v.add(new FileInputStream("1.txt"));
		v.add(new FileInputStream("2.txt"));
		v.add(new FileInputStream("3.txt"));
		Enumeration<FileInputStream> en = v.elements();
		Vector集合最好不用。
		*/
		
		//改用效率更高的其他集合来获取枚举
		ArrayList<FileInputStream> al = new ArrayList<FileInputStream>();
		for(int x=1; x<=3; x++){
			al.add(new FileInputStream(x+".txt"));
		}
		Enumeration<FileInputStream> en = Collections.enumeration(al);
		/*Collections.enumeration的原理
		final Iterator<FileInputStream> it = al.iterator();
		Enumeration<FileInputStream> en = new Enumeration<FileInputStream>(){
			public boolean hasMoreElements() {
				return it.hasNext();
			}
			public FileInputStream nextElement() {
				return it.next();
			}
		};*/
		
		SequenceInputStream sis = new SequenceInputStream(en);//序列流需要接收一个枚举
		FileOutputStream fos = new FileOutputStream("1234.txt");
		
		byte[] buf = new byte[1024];
		int len = 0;
		while((len=sis.read(buf))!=-1){
			fos.write(buf,0,len);
		}
		fos.close();
		sis.close();
	}
```

## Object流
* ObjectOutputStream
可以将对象进行序列化
```java
public static void writeObj() throws IOException, IOException {
		
		ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("obj.object"));
		//对象序列化。  被序列化的对象必须实现Serializable接口。 
		oos.writeObject(new Person("小强",30));
		oos.close();
	}
	public class Person implements Serializable/*标记接口*/
```
静态数据不能被序列化。  
transient：非静态数据不想被序列化可以使用这个关键字修饰  

* ObjectInputStream 
对以前使用ObjectOutputStream写入的对象进行反序列化。
```java
public static void readObj() throws IOException, ClassNotFoundException {
		
		ObjectInputStream ois = new ObjectInputStream(new FileInputStream("obj.object"));
		//对象的反序列化。 
		Person p = (Person)ois.readObject();
		System.out.println(p.getName()+":"+p.getAge());
		ois.close();
	}
```
* 序列化接口Serializable
```java
用于给被序列化的类加入ID号，反序列化时用于判断类和对象是否和原来同一个版本。
如果使用默认计算的ID，则可能由于编译器的不同，导致反序列化ID时无法匹配。
因此，最好自己声明一个ID，格式如下：
private static final long serialVersionUID = 123L;
```

## RandomAccessFile随机读取文件
随机访问文件，不属于IO体系中的子类，自身具备读写的方法。多线程写入经常用。  
特点：
* 1、该对象即能读，又能写；
* 2、该对象内部维护了一个byte数组，并通过指针可以操作数组中的元素；
* 3、可以通过getFilePointer方法获取指针的位置，通过seek方法设置指针的位置，来达到随机读写的功能；
* 4、其实该对象就是将字节输入流和输出流进行了封装； 
* 5、该对象的源或者目的只能是文件，通过构造函数就可以看出。
示例：
```java
//使用RandomAccessFile对象写入一些人员信息，比如姓名和年龄。
	public static void writeFile() throws IOException{
		//	如果文件不存在，则创建，如果文件存在，不创建
		RandomAccessFile raf = new RandomAccessFile("ranacc.txt","rw");
		
		raf.write("张三".getBytes());
		raf.writeInt(97);
		raf.write("小强".getBytes());
		raf.writeInt(99);
		raf.close();
		
//往指定位置写入数据。
RandomAccessFile raf = new RandomAccessFile("ranacc.txt", "rw");
		raf.seek(3*8);
		raf.write("哈哈".getBytes());
		raf.writeInt(108);
		raf.close();
	}
public static void readFile() throws IOException {
		RandomAccessFile raf = new RandomAccessFile("ranacc.txt", "r");
		//通过seek设置指针的位置,可以从指定位置开始读（可叫做随机读取）
		raf.seek(1*8);
		byte[] buf = new byte[4];
		raf.read(buf);
		
		String name = new String(buf);
		int age = raf.readInt();
		
		System.out.println("name="+name);
		System.out.println("age="+age);
		System.out.println("pos:"+raf.getFilePointer());
		raf.close();
}
```


## 练 习
### [练习1](#答案1)
将C盘一个文本文件复制到D盘

### [练习2](#答案2)
自定义一个读取缓冲区。(模拟一个BufferedReader)

### [练习3](#答案3)
复制一个MP3文件

### [练习4](#答案4)
获取用户键盘录入的数据， 并将数据变成大写显示在控制台上，如果用户输入的是over，结束键盘录入

### [练习5](#答案5)
对指定的目录，列出其包含的所有内容（包含子目录中的内容），即深度遍历。

### [练习6](#答案6)
删除一个带内容的目录

### [练习7](#答案7)
对文件进行切割和合并。

### 答案1
```java
public class CopyTextTest_2 {
	private static final int BUFFER_SIZE = 1024;
	
	public static void main(String[] args) {
		FileReader fr = null;
		FileWriter fw = null;
		try {
			fr = new FileReader("IO流_2.txt");
			fw = new FileWriter("copytest_2.txt");
			//创建一个临时容器，用于缓存读取到的字符。
			char[] buf = new char[BUFFER_SIZE];//这就是缓冲区。 
			
			//定义一个变量记录读取到的字符数(其实就是往数组里装的字符个数 )
			int len = 0;
			while((len=fr.read(buf))!=-1){
				fw.write(buf, 0, len);
			}
		} catch (Exception e) {
			throw new RuntimeException("读写失败");
		}finally{
			if(fw!=null)
				try {
					fw.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			if(fr!=null)
				try {
					fr.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
		}
	}
}
```

### 答案2
分析：  
缓冲区中无非就是封装了一个数组，  
并对外提供了更多的方法对数组进行访问。  
其实这些方法最终操作的都是数组的角标。  
缓冲的原理：  
其实就是从源中获取一批数据装进缓冲区中。  
再从缓冲区中不断的取出一个一个的数据。  
在此次取完后，继续从源中继续取一批数据进缓冲区。  
当源中的数据取光时，用-1作为结束标记。  
```java
public class MyBufferedReader extends Reader {

	private Reader r;
	
	//定义一个数组作为缓冲区。
	private char[] buf = new char[1024];
	//定义一个指针用于操作这个数组中的元素。当操作到最后一个元素后，指针应该归零。	
	private int pos = 0;	
	//定义一个计数器用于记录缓冲区中的数据个数。 当该数据减到0，就从源中继续获取数据到缓冲区中。
	private int count = 0;

	MyBufferedReader(Reader r){
		this.r = r;
	}
	
	public int myRead() throws IOException{
		/*
	 	该方法从缓冲区中一次取一个字符。 
		//从源中获取一批数据到缓冲区中。需要先做判断，只有计数器为0时，才需要从源中获取数据。
		if(count==0){
			count = r.read(buf);//将数据读入缓冲区
			if(count<0)
				return -1;
			pos = 0;//每次从缓冲区获取数据时，角标应该归零.
			char ch = buf[pos];//从缓冲区获取数据
			pos++;
			count--;
			return ch;
		}else if(count>0){
			char ch = buf[pos];
			pos++;
			count--;
			return ch;
		}*/
		//	方法改良
		if(count==0){
			count = r.read(buf);
			pos = 0;
		}
		if(count<0)
			return -1;
		char ch = buf[pos++];
		count--;
		return ch;
	
	}
	
	public String myReadLine() throws IOException{
		StringBuilder sb = new StringBuilder();//存储一行数据的缓冲区
		int ch = 0;
		while((ch = myRead())!=-1){
			if(ch=='\r')
				continue;
			if(ch=='\n')
				return sb.toString();
			//将从缓冲区中读到的字符，存储到缓存行数据的缓冲区中。
			sb.append((char)ch);
		}		
		if(sb.length()!=0)
			return sb.toString();
		return null;
	}

	public void myClose() throws IOException {
		r.close();
	}
}

使用这个方法
public static void main(String[] args) throws IOException {
	FileReader fr = new FileReader("buf.txt");
	MyBufferedReader bufr = new MyBufferedReader(fr);
	String line = null;
		while((line=bufr.myReadLine())!=null){
			System.out.println(line);
		}
		bufr.myClose();
	}
```
### 答案3
```java
public class CopyMp3Test {
	public static void main(String[] args) throws IOException {
		copy_1();
	}
	
	public static void copy_1() throws IOException {
		//自定义一个缓冲区
		FileInputStream fis = new FileInputStream("c:\\0.mp3");		
		FileOutputStream fos = new FileOutputStream("c:\\1.mp3");
		
		byte[] buf = new byte[1024];
		int len = 0;
		while((len=fis.read(buf))!=-1){
			fos.write(buf,0,len);
		}
		fos.close();
		fis.close();
	}
	
	public static void copy_2() throws IOException {
		//使用自带的缓冲区
		FileInputStream fis = new FileInputStream("c:\\0.mp3");	
		BufferedInputStream bufis = new BufferedInputStream(fis);
		
		FileOutputStream fos = new FileOutputStream("c:\\2.mp3");
		BufferedOutputStream bufos = new BufferedOutputStream(fos);
		
		int ch = 0;
		while((ch=bufis.read())!=-1){
			bufos.write(ch);
		}
		bufos.close();
		bufis.close();
	}
	
	//建一个数组，能全部存进去。不建议。 
	public static void copy_3() throws IOException {
		FileInputStream fis = new FileInputStream("c:\\0.mp3");		
		FileOutputStream fos = new FileOutputStream("c:\\3.mp3");
		
		byte[] buf = new byte[fis.available()];
		fis.read(buf);
		fos.write(buf);
		fos.close();
		fis.close();
	}
	
//	读一个写一个。千万不要用，效率没有！
	public static void copy_4() throws IOException {
		FileInputStream fis = new FileInputStream("c:\\0.mp3");		
		FileOutputStream fos = new FileOutputStream("c:\\4.mp3");
		
		int ch = 0;
		while((ch =fis.read())!=-1){
			fos.write(ch);
		}
		fos.close();
		fis.close();
	}
}
```
### 答案4
```java
* 思路：
* 1，因为键盘录入只读取一个字节，要判断是否是over，需要将读取到的字节拼成字符串。
* 2，需要一个容器，StringBuilder.
* 3，在用户回车之前将录入的数据变成字符串判断即可。 是over就结束，不是就变大写*/
	
		//1，创建容器。
		StringBuilder sb = new StringBuilder();
		//2，获取键盘读取流。		
		InputStream in = System.in;
		//3，定义变量记录读取到的字节，并循环获取。 		
		int ch = 0;
		
		while((ch=in.read())!=-1){
			//	在存储之前需要判断是否是换行标记 ,因为换行标记不存储。 
			if(ch=='\r')
				continue;
			if(ch=='\n'){
				String temp = sb.toString();
				if("over".equals(temp))
					break;
				System.out.println(temp.toUpperCase());
				sb.delete(0, sb.length());
			}
			else
			//将读取到的字节存储到StringBuilder中。
			sb.append((char)ch);
		}
	}
	
上面的读取方法其实就是一次读一行，但是System.in不具备readLine()方法。
System.in是字节流，而readLine()方法是字符流的方法。
因此要先使用转换流，将字节流转换为字符流来操作。

public class TransStreamDemo {

	public static void main(String[] args) throws IOException {
		//字节流。 
		InputStream in = System.in;
		
		//将字节转成字符的桥梁--转换流。 
		InputStreamReader isr = new InputStreamReader(in);
		
		//装饰字符流。
		BufferedReader bufr = new BufferedReader(isr);
		
		//字符转为字节(System.out最终输出的是字节,属于OutputStream的子体系)
		OutputStream out = System.out;
		OutputStreamWriter osw = new OutputStreamWriter(out);
		BufferedWriter  bufw = new BufferedWriter(osw);//提高效率
		
		String line = null;
		while((line=bufr.readLine())!=null){
			if("over".equals(line))
				break;

			bufw.write(line.toUpperCase());
			bufw.newLine();
			bufw.flush();
		}		
	}
}

可将代码简化如下（重点）：
BufferedReader bufr = new BufferedReader(new InputStreamReader(System.in));//System.in可以变为其他的输入流
		
BufferedWriter bufw = new BufferedWriter(new OutputStreamWriter(System.out));//System.out可以变为其他的输出流
//可以将键盘录入的数据写到一个文件中
BufferedWriter bufw = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("b.txt")));
		
String line = null;
	while((line=bufr.readLine())!=null){
		if("over".equals(line))
			break;
		bufw.write(line.toUpperCase());
		bufw.newLine();
		bufw.flush();
	}

```
### 答案5
```java
public class FileTest {
	public static void main(String[] args) {
		File dir = new File("e:\\demodir");
		listAll(dir);
	}
//获取指定目录下当前的所有文件夹或者文件对象
	public static void listAll(File dir) {
		System.out.println(getSpace(level)+dir.getAbsolutePath());

		File[] files = dir.listFiles();
		
		for(int x=0; x<files.length; x++){//使用了递归的思想
			if(files[x].isDirectory()){
				listAll(files[x]);
			}
			else
				System.out.println(getSpace(level)+files[x].getAbsolutePath());
		}	
	}
}

可以对listAll方法进行改进，显示目录的层级
public class FileTest {
	public static void main(String[] args) {
		File dir = new File("e:\\demodir");
		listAll(dir,0);
	}

	public static void listAll(File dir,int level) {
		System.out.println(getSpace(level)+dir.getName());
		level++;
		File[] files = dir.listFiles();
		
		for(int x=0; x<files.length; x++){
			if(files[x].isDirectory()){
				listAll(files[x],level);
			}
			else
				System.out.println(getSpace(level)+files[x].getName());
		}	
	}

	private static String getSpace(int level) {
		StringBuilder sb = new StringBuilder();
		for(int x=0; x<level; x++){
			sb.insert(0,"|--");
		}
		return sb.toString();
	}
}
```
### 答案6
原理：必须从最里面往外删，需要进行深度遍历
```java
public class RemoveDirTest {
	public static void main(String[] args) {
		File dir  = new File("e:\\demodir");
		removeDir(dir);
	}

	public static void removeDir(File dir) {
		File[] files = dir.listFiles();
		
		for(File file : files){
			if(file.isDirectory()){
				removeDir(file);
			}else{
				System.out.println(file+":"+file.delete());
			}
		}
		System.out.println(dir+":"+dir.delete());
	}
}
```

### 答案7
```java
合并之前并不知道碎片文件的个数和原后缀名，因此需要在切割时添加配置文件，记录这些信息。
切割
public class SplitFileDemo {
	private static final int SIZE = 1024 * 1024;

	public static void main(String[] args) throws Exception {
		File file = new File("c:\\aa.mp3");
		splitFile_2(file);
	}

	private static void splitFile_2(File file) throws IOException {
		// 用读取流关联源文件。
		FileInputStream fis = new FileInputStream(file);

		// 定义一个1M的缓冲区。
		byte[] buf = new byte[SIZE];

		// 创建目的。
		FileOutputStream fos = null;
		
		int len = 0;
		int count = 1;
		
		File dir = new File("c:\\partfiles");
		if (!dir.exists())
			dir.mkdirs();

		while ((len = fis.read(buf)) != -1) {
			fos = new FileOutputStream(new File(dir, (count++) + ".part"));
			fos.write(buf, 0, len);
			fos.close();
		}
		
		/* 切割文件时，必须记录住被切割文件的名称，以及切割出来碎片文件的个数，以方便于合并。
		 * 这个信息为了进行描述，使用键值对的方式。用到了properties对象 */
		// 将被切割文件的信息保存到prop集合中。
		Properties prop  = new Properties();
		prop.setProperty("partcount", count+"");
		prop.setProperty("filename", file.getName());
		
		fos = new FileOutputStream(new File(dir,count+".properties"));
		
		//将prop集合中的数据存储到文件中。 
		prop.store(fos, "save file info");

		fos.close();
		fis.close();
	}

	public static void splitFile(File file) throws IOException {

		// 用读取流关联源文件。
		FileInputStream fis = new FileInputStream(file);

		// 定义一个1M的缓冲区。
		byte[] buf = new byte[SIZE];

		// 创建目的。
		FileOutputStream fos = null;

		int len = 0;
		int count = 1;

		File dir = new File("c:\\partfiles");
		if (!dir.exists())
			dir.mkdirs();

		while ((len = fis.read(buf)) != -1) {

			fos = new FileOutputStream(new File(dir, (count++) + ".part"));
			fos.write(buf, 0, len);
		}

		fos.close();
		fis.close();
	}

}

合并
public class MergeFile {

	/**
	 * @param args
	 * @throws IOException 
	 */
	public static void main(String[] args) throws IOException {
		File dir = new File("c:\\partfiles");
		mergeFile_2(dir);
	}
	
	public static void mergeFile_2(File dir) throws IOException {
		//	 获取指定目录下的配置文件对象。
		File[] files = dir.listFiles(new SuffixFilter(".properties"));
		if(files.length!=1)
			throw new RuntimeException(dir+",该目录下没有properties扩展名的文件或者不唯一");
		
		//  记录配置文件对象。
		File confile = files[0];
		
		//获取该文件中的信息================================================。
		Properties prop = new Properties();
		FileInputStream fis = new FileInputStream(confile);
		prop.load(fis);
		
		String filename = prop.getProperty("filename");		
		int count = Integer.parseInt(prop.getProperty("partcount"));
		
		//获取该目录下的所有碎片文件。 ==============================================
		File[] partFiles = dir.listFiles(new SuffixFilter(".part"));
		if(partFiles.length!=(count-1)){
			throw new RuntimeException(" 碎片文件不符合要求，个数不对!应该"+count+"个");
		}
		
		//将碎片文件和流对象关联 并存储到集合中。 
		ArrayList<FileInputStream> al = new ArrayList<FileInputStream>();
		for(int x=0; x<partFiles.length; x++){
			al.add(new FileInputStream(partFiles[x]));
		}
		
		//将多个流合并成一个序列流。 
		Enumeration<FileInputStream> en = Collections.enumeration(al);
		SequenceInputStream sis = new SequenceInputStream(en);
		
		FileOutputStream fos = new FileOutputStream(new File(dir,filename));
		byte[] buf = new byte[1024];
		int len = 0;
		while((len=sis.read(buf))!=-1){
			fos.write(buf,0,len);
		}
		fos.close();
		sis.close();
	}


过滤器
public class SuffixFilter implements FilenameFilter {
	private String suffix;
	public SuffixFilter(String suffix) {
		super();
		this.suffix = suffix;
	}
	@Override
	public boolean accept(File dir, String name) {
		return name.endsWith(suffix);
	}
}
```
