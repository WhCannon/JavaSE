# IO流
- ## [字符流](#字符流)
   - ### [FileWriter](#filewriter)
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
		FileWriter fw = new FileWriter("demo.txt",true);//IO异常
		
		// 调用Writer对象中的write(string)方法，写入数据。 
		fw.write("abcde"+LINE_SEPARATOR+"hahaha");//数据写入到临时存储缓冲区中
		fw.write("xixi");
		
		// 进行刷新，将数据写到文件中。
		fw.flush();
		// 关闭流，关闭资源。在关闭前会先调用flush刷新缓冲中的数据到目的地。
		fw.close();
//	fw.write("haha");// 关闭流之后，不能继续写入。java.io.IOException: Stream closed
	}
}

```



## 字节流
