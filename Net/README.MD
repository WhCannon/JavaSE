# 网络编程
  - ## 网络传输要素
      - [IP地址](#ip地址)
      - [端口](#端口)
      - [传输协议](#传输协议)
      
  - ## [练习](#练-习)
  
  
  
  
  
  
  
  
  
  ## IP地址
  网络中设备的标识。  
  InetAddress对象  
  ```java
  public static void main(String[] args) throws UnknownHostException {
		//获取本地主机ip地址对象。 
		InetAddress ip = InetAddress.getLocalHost();
		
		//获取其他主机的ip地址对象。
		ip = InetAddress.getByName("www.baidu.com");//InetAddress.getByName("my_think");
		
		System.out.println(ip.getHostAddress());
		System.out.println(ip.getHostName());
	}
  ```
  
  
  ## 端口
  用于标识不同进程的地址，有效端口0-65535，其中0-1024系统使用或保留。
  
  
  ## 传输协议
  ### Socket
  
  
  ### UDP协议  
  创建UDP传输的发送端
  ```java
  public static void main(String[] args) throws IOException {

		System.out.println("发送端启动......");
		/*
		 * 创建UDP传输的发送端。
		 * 步骤：
		 * 1，建立udp的socket服务。
		 * 2，将要发送的数据封装到数据包中。 
		 * 3，通过udp的socket服务将数据包发送出去。
		 * 4，关闭socket服务。
		 */
		//1,建立udpsocket服务，使用DatagramSocket对象。
		DatagramSocket ds = new DatagramSocket(8888);
		
		//2,将要发送的数据封装到数据包中。
		//String str = "udp传输演示：哥们来了！";
		
		//键盘持续录入数据
		BufferedReader bufr = new BufferedReader(new InputStreamReader(System.in));
		String line = null;
		while((line=bufr.readLine())!=null){
		
		//使用DatagramPacket将数据封装到包中。
		byte[] buf = line.getBytes();
		//以字节数组为数据包，将长度为length的包发送到指定主机上的指定端口号。
		DatagramPacket dp = 
				new DatagramPacket(buf,buf.length,InetAddress.getByName("192.168.1.100"),10000);
		
		//3，通过udp的socket服务将数据包发送出去。使用send方法。
		ds.send(dp);
		
		if("886".equals(line))
				break;
		}
		
		//4，关闭资源。
		ds.close();
	}
  ```
  
  创建UDP的接收端
  ```java
  public static void main(String[] args) throws IOException {

		System.out.println("接收端启动......");
		/*
		 * 建立UDP接收端的步骤。
		 * 1，建立udp socket服务,因为是要接收数据，必须要明确一个端口号。
		 * 2，创建数据包，用于存储接收到的数据。方便用数据包对象的方法解析这些数据。
		 * 3，使用socket服务的receive方法将接收的数据存储到数据包中。
		 * 4，通过数据包的方法解析数据包中的数据。
		 * 5，关闭资源 
		 */
		
		//1,建立udp socket服务，明确一个端口
		DatagramSocket ds = new DatagramSocket(10000);
		
		while(true){//持续接收
		//2,创建数据包。
		byte[] buf = new byte[1024];
		DatagramPacket dp = new DatagramPacket(buf,buf.length);
		
		//3,使用接收方法将数据存储到数据包中。
		ds.receive(dp);//阻塞式的。
		
		//4，通过数据包对象的方法，解析其中的数据,比如，地址，端口，数据内容。
		String ip = dp.getAddress().getHostAddress();
		int port = dp.getPort();
		String text = new String(dp.getData(),0,dp.getLength());
		
		System.out.println(ip+":"+port+":"+text);
		}
		
		//5,关闭资源。
		//ds.close();
	}
  ```
  
  
  
### TCP协议
客户端建立
```java
public static void main(String[] args) throws UnknownHostException, IOException {
		//需求：客户端发数据到服务端
		/*
		 * Tcp传输，客户端建立的过程。
		 * 1，创建tcp客户端socket服务。使用的是Socket对象。
		 * 		建议该对象一创建就明确目的地--要连接的主机。 
		 * 2，如果连接建立成功，说明数据传输通道已建立。
		 * 		该通道就是socket流 ,是底层建立好的。 既然是流，说明这里既有输入，又有输出。
		 * 		想要输入或者输出流对象，可以找Socket来获取。 
		 * 		可以通过getOutputStream(),和getInputStream()来获取两个字节流。
		 * 3，使用输出流，将数据写出。 
		 * 		这一步根据目的的不同，使用的流不同。
		 * 4，关闭资源。 
		 */
		
		//创建客户端socket服务。
		Socket socket = new Socket("192.168.1.100",10002);
		
		//获取socket流中的输出流。 
		OutputStream out = socket.getOutputStream();
		
		//使用输出流将指定的数据写出去。
		out.write("tcp演示：哥们又来了!".getBytes());
		
		//关闭资源。
		socket.close();
	}
```

服务端建立
```java
public class ServerDemo {
	public static void main(String[] args) throws IOException {
//		需求：服务端接收客户端发送过来的数据，并打印在控制台上。 
		/*
		 * 建立tcp服务端的思路：
		 * 1，创建服务端socket服务。通过ServerSocket对象。
		 * 		服务端必须对外提供一个端口，否则客户端无法连接。
		 * 2，获取连接过来的客户端对象。
		 * 3，通过客户端对象获取socket流读取客户端发来的数据 
		 * 		并打印在控制台//这里可以对数据做其他的处理
		 * 4，关闭资源。关客户端，关服务端。 
		 */
		
		//1创建服务端对象。
		ServerSocket ss = new ServerSocket(10002);
		
		//2,获取连接过来的客户端对象。
		Socket s = ss.accept();//阻塞式.
		String ip = s.getInetAddress().getHostAddress();//获取客户端IP地址
		
		//3，通过socket对象获取输入流，要读取客户端发来的数据
		InputStream in = s.getInputStream();
		
		byte[] buf = new byte[1024];
		int len = in.read(buf);
		String text = new String(buf,0,len);
		System.out.println(ip+":"+text);
		
		//4.关闭资源
		s.close();
		ss.close();//服务端通常不关闭
	}
}
```

服务端和客户端交互
```java
客户端
public static void main(String[] args) throws UnknownHostException, IOException {
		Socket socket = new Socket("192.168.1.100",10002);
		OutputStream out = socket.getOutputStream();	
		out.write("tcp演示：哥们又来了!".getBytes());
		
		//读取服务端返回的数据,使用socket读取流。 
		InputStream in = socket.getInputStream();
		byte[] buf = new byte[1024];
		int len = in.read(buf);
		String  text = new String(buf,0,len);
		System.out.println(text);
		
		//关闭资源。
		socket.close();
	}
	
服务端
public static void main(String[] args) throws IOException {
		//1创建服务端对象。
		ServerSocket ss = new ServerSocket(10002);
		
		//2,获取连接过来的客户端对象。
		Socket s = ss.accept();
		
		String ip = s.getInetAddress().getHostAddress();
		
		//3，通过socket对象获取输入流，要读取客户端发来的数据
		InputStream in = s.getInputStream();
		byte[] buf = new byte[1024];
		int len = in.read(buf);
		String text = new String(buf,0,len);
		System.out.println(ip+":"+text);
		
		//使用客户端socket对象的输出流给客户端返回数据
		OutputStream out = s.getOutputStream();
		out.write("收到".getBytes());
		
		s.close();
		ss.close();
	}
```



  
## 练 习
### [练习1](#答案-1)
聊天程序（是多线程的）

### [练习2](#答案2)
客户端输入字母数据，发送给服务端，  
服务端收到后显示在控制台，并将该数据转成大写返回给客户端   
直到客户端输入over.转换结束.   
也就是创建一个英文大写转换服务器.

### [练习3](#答案3)
上传文本文件到服务端。

### [练习4](#答案4)
创建一个能并发上传图片并存储的服务端。

## 答案 1
```java
public class ChatDemo {
	public static void main(String[] args) throws IOException {
		DatagramSocket send = new DatagramSocket();
		DatagramSocket rece = new DatagramSocket(10001);
		
		new Thread(new Send(send)).start();
		new Thread(new Rece(rece)).start();
	}
}

public class Send implements Runnable {
	private DatagramSocket ds;
	public Send(DatagramSocket ds){
		this.ds = ds;
	}
	@Override
	public void run() {
		try {
			BufferedReader bufr = new BufferedReader(new InputStreamReader(System.in));
			String line = null;
			
			while((line=bufr.readLine())!=null){
				byte[] buf = line.getBytes();
				DatagramPacket dp = 
						new DatagramPacket(buf,buf.length,InetAddress.getByName("192.168.1.255"),10001);
				ds.send(dp);
				if("886".equals(line))
					break;
			}
			ds.close();
		} catch (Exception e) {
		}
	}
}

public class Rece implements Runnable {
	private DatagramSocket ds;
	public Rece(DatagramSocket ds) {
		this.ds = ds;
	}
	@Override
	public void run() {
		try {
			while (true) {
				// 2,创建数据包。
				byte[] buf = new byte[1024];
				DatagramPacket dp = new DatagramPacket(buf, buf.length);

				// 3,使用接收方法将数据存储到数据包中。
				ds.receive(dp);// 阻塞式的。

				// 4，通过数据包对象的方法，解析其中的数据,比如，地址，端口，数据内容。
				String ip = dp.getAddress().getHostAddress();
				int port = dp.getPort();
				String text = new String(dp.getData(), 0, dp.getLength());
				
				System.out.println(ip + "::" + text);
				if(text.equals("886")){
					System.out.println(ip+"....退出聊天室");
				}
			}
		} catch (Exception e) {
		}
	}
}

```


## 答案2
分析：  
有客户端和服务端,使用tcp传输
```java
客户端
public static void main(String[] args) throws UnknownHostException, IOException {
		/*
		 * 思路：
		 * 客户端：
		 * 1，需要先有socket端点。
		 * 2，客户端的数据源：键盘。
		 * 3，客户端的目的：socket.
		 * 4，接收服务端的数据，源：socket。
		 * 5，将数据显示在打印出来：目的：控制台.
		 * 6，在这些流中操作的数据，都是文本数据。
		 * 
		 * 转换客户端:
		 * 1,创建socket客户端对象。
		 * 2,获取键盘录入。
		 * 3，将录入的信息发送给socket输出流。
		 */
		
		//1,创建socket客户端对象。
		Socket s = new Socket("192.168.1.100",10004);
		
		//2，获取键盘录入。
		BufferedReader bufr  =
			new BufferedReader(new InputStreamReader(System.in));
		
		//3,socket输出流。2种方法
//		new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
		PrintWriter out = new PrintWriter(s.getOutputStream(),true);
		
		//4,socket输入流，读取服务端返回的大写数据
		BufferedReader bufIn  = 
			new BufferedReader(new InputStreamReader(s.getInputStream()));
		
		String line = null;
		while((line=bufr.readLine())!=null){
			if("over".equals(line))
				break;
			out.println(line);
			
			//读取服务端发回的一行大写数。
			String upperStr = bufIn.readLine();
			System.out.println(upperStr);
		}
		s.close();
	}
	
服务端
public static void main(String[] args) throws IOException {
		/* 转换服务端。
		 * 分析：
		 * 1，serversocket服务。
		 * 2，获取socket对象。
		 * 3，源：socket，读取客户端发过来的需要转换的数据。
		 * 4，目的：显示在控制台上。
		 * 5，将数据转成大写发给客户端。 
		 */
		
		//1,
		ServerSocket ss = new ServerSocket(10004);
		
		//2,获取socket对象。
		Socket s = ss.accept();
		
		//获取ip.
		String ip = s.getInetAddress().getHostAddress();
		System.out.println(ip+"......connected");
		
		//3,获取socket读取流，并装饰。 
		BufferedReader bufIn = new BufferedReader(new InputStreamReader(s.getInputStream()));
		
		//4,获取socket的输出流，并装饰。
		PrintWriter out = new PrintWriter(s.getOutputStream(),true);
		
		String line = null;
		while((line=bufIn.readLine())!=null){
			System.out.println(line);
			out.println(line.toUpperCase());
		}
		s.close();
		ss.close();
	}
```

## 答案2
```java
客户端
public static void main(String[] args) throws UnknownHostException, IOException {

		System.out.println("上传客户端。。。。。。");
		
		File file = new File("c:\\client.txt");
		System.out.println(file.exists());
		
		//1
		Socket s = new Socket("192.168.1.100",10005);
		//2
		BufferedReader bufr =
				new BufferedReader(new FileReader(file));
		//3,
		PrintWriter out = new PrintWriter(s.getOutputStream(),true);
		
		String line = null;
		while((line=bufr.readLine())!=null){
			out.println(line);
		}
		
		//告诉服务端，客户端写完了。
		s.shutdownOutput();
		
		//4,
		BufferedReader bufIn =
			new BufferedReader(new InputStreamReader(s.getInputStream()));
		String str = bufIn.readLine();
		System.out.println(str);
		
		bufr.close();
		s.close();
	}
	
服务端
public static void main(String[] args) throws IOException {
		System.out.println("上传服务端。。。。。。。。。");
		
		//1
		ServerSocket ss = new ServerSocket(10005);
		//2,
		Socket s = ss.accept();
		System.out.println(s.getInetAddress().getHostAddress()+".....connected");
		
		//3,
		BufferedReader bufIn = 
			new BufferedReader(new InputStreamReader(s.getInputStream()));
		//4,
		BufferedWriter bufw = new BufferedWriter(new FileWriter("c:\\server.txt"));
		
		String line = null;
		while((line=bufIn.readLine())!=null){
			bufw.write(line);
			bufw.newLine();
			bufw.flush();
		}
		
		PrintWriter out = new PrintWriter(s.getOutputStream(),true);
		out.println("上传成功");
		
		bufw.close();
		s.close();
		ss.close();
	}
```

## 答案4
```java
客户端
public class UploadPicClient {
	public static void main(String[] args) throws UnknownHostException, IOException {
		//1,创建客户端socket。
		Socket s = new Socket("192.168.1.100",10006);
		
		//2,读取客户端要上传的图片文件。
		FileInputStream fis = new FileInputStream("c:\\0.bmp");
		
		//3,获取socket输出流，将读到图片数据发送给服务端。
		OutputStream out = s.getOutputStream();
		byte[] buf = new byte[1024];
		int len = 0;
		while((len=fis.read(buf))!=-1){
			out.write(buf,0,len);
		}
		
		//告诉服务端说：这边的数据发送完毕。让服务端停止读取。
		s.shutdownOutput();
		
		//读取服务端发回的内容。 		
		InputStream in  = s.getInputStream();
		byte[] bufIn = new byte[1024];
		int lenIn = in.read(buf);
		String text = new String(buf,0,lenIn);
		System.out.println(text);
		
		fis.close();
		s.close();
	}
}


服务端
public class UploadPicServer {
	public static void main(String[] args) throws IOException {
			
		//创建tcp的socket服务端。
		ServerSocket ss = new ServerSocket(10006);
		while(true){
			Socket s = ss.accept();			
			new Thread(new UploadTask(s)).start();		
		}
	}
}

服务端任务
public class UploadTask implements Runnable {
	private static final int SIZE = 1024*1024*2;
	private Socket s;
	public  UploadTask(Socket s) {
		this.s = s;
	}
	@Override
	public void run() {
		int count = 0;
		String ip = s.getInetAddress().getHostAddress();
		System.out.println(ip + ".....connected");
		
		try{
		// 读取客户端发来的数据。
		InputStream in = s.getInputStream();

		// 将读取到数据存储到一个文件中。
		File dir = new File("c:\\pic");
		if (!dir.exists()) {
			dir.mkdirs();
		}
		File file = new File(dir, ip + ".jpg");
		//如果文件已经存在于服务端 
		while(file.exists()){
			file = new File(dir,ip+"("+(++count)+").jpg");
		}
		
		FileOutputStream fos = new FileOutputStream(file);
		byte[] buf = new byte[1024];
		int len = 0;
		while ((len = in.read(buf)) != -1) {
			fos.write(buf, 0, len);
			if(file.length()>SIZE){
				System.out.println(ip+"文件体积过大");
				fos.close();
				s.close();
				
				System.out.println(ip+"...."+file.delete());
				return ;
			}
		}

		// 获取socket输出流，将上传成功字样发给客户端。
		OutputStream out = s.getOutputStream();
		out.write("上传成功".getBytes());

		fos.close();
		s.close();
		}catch(IOException e){
		}
	}
}
```
