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
  
  
## 练 习
### [练习1](#答案-1)
聊天程序（是多线程的）




## 答案 1
```java

```
