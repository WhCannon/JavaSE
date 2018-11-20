# 常用的编程思想
  -[递归](#递-归)
  
  
  
  


## 递 归
```java
public  static int show(int n) {
		if(n==1) {
			return 1;
		}else {
		return n*show(n-1);
		}
	}
```
