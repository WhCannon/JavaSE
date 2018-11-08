# 基本数据类型
  - [练习](#练习)
  
  
  


## 练习
### [练习1](#答案1)
```java
下面的程序有问题吗，如果有，在哪里呢?
			byte b1 = 3;
			byte b2 = 4;
			byte b3 = b1 + b2;
			byte b4 = 3 + 4;
 ```
 
### [练习2](#答案2)
下面的操作结果是什么呢?  
byte b = (byte)130;


### [练习3](#答案3)
```java
class ArgsDemo {
	public static void main(String[] args) {
		int a = 10;
		int b = 20;
		System.out.println("a:"+a+",b:"+b); //a:10,b:20
		change(a,b);
		System.out.println("a:"+a+",b:"+b); //???	a:10,b:20

		int[] arr = {1,2,3,4,5}; 
		change(arr);
		System.out.println(arr[1]); //???	4
	}

	public static void change(int a,int b) { //a=10,b=20
		System.out.println("a:"+a+",b:"+b); //a:10,b:20
		a = b;	//a=20
		b = a + b; //b=40
		System.out.println("a:"+a+",b:"+b); //a:20,b:40
	}

	public static void change(int[] arr) { //arr={1,2,3,4,5};
		for(int x=0; x<arr.length; x++) {
			if(arr[x]%2==0) {
				arr[x]*=2;
			}
		}
		//arr={1,4,3,8,5};
	}
}
```
 
 
## 答案1
第3行出错，b1+b2会提升为int类型。  
第4行不出错，因为3+4是常量，会先计算出结果，判断是否在byte范围内


## 答案2
```java
分析过程：
计算机中数据的运算都是补码进行的。
而要得到补码，首先要计算出数据的二进制。
		A:获取130这个数据的二进制。
		00000000 00000000 00000000 10000010
			这是130的原码，也是反码，还是补码。
		B:做截取操作，截成byte类型的了。
			10000010 
			这个结果是补码。
		C:已知补码求原码。
				符号位		数值位
			补码：	1		0000010
			
			反码：	1		0000001
			
			原码：	1		1111110
			
所以结果就是11111110，即-126
```
