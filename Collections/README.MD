# 集合框架工具类
  - [可变参数](#可变参数)
  
  
  
  
  


## 可变参数
```java
public static int newAdd(int a,int...  arr){// ...代表参数个数是可变的
		int sum = 0;
		for (int x:arr) {
			sum+=x
		}
		return sum;
	}
```
