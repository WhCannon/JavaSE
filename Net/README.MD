# 网络编程
  - ## 网络传输要素
      - [IP地址](#ip地址)
      - [端口](#端口)
      - [传输协议](#传输协议)
      
  
  
  
  
  
  
  
  
  
  
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
  * Socket
  
  
  
  
  
  
