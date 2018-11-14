# String类
  - [字符串缓冲区](#字符串缓冲区)
  - [字符串定义方式](#字符串定义方式)
  - [String类构造函数](#构造函数)
  - [常用方法](#常用方法)
  - [StringBuffer](#stringbuffer)
  - [StringBuilder](#stringbuilder)
  - [练习](#练习)



## 字符串缓冲区
```java
    String s = "hello";//创建了hello
		s += "world";     //先创建world，再创建一个helloworld
		System.out.println("s:" + s); // helloworld
```
字符串如果是变量相加，先开空间，再拼接。  
字符串如果是常量相加，是先加，然后在常量池找，如果有就直接返回，没有就创建。
```java
public static void main(String[] args) {
		String s1 = "hello";
		String s2 = "world";
		String s3 = "helloworld";
		System.out.println(s3 == s1 + s2);// false
		System.out.println(s3.equals((s1 + s2)));// true
		System.out.println(s3 == "hello" + "world");//true
		System.out.println(s3.equals("hello" + "world"));// true
	}
```

## 字符串定义方式
### 第一种定义方式
```java
String s = "abc";
String s1 = "abc";		
System.out.println(s==s1);
```
返回结果为true  
创建一个对象"abc"存储在字符串缓冲区，首次出现时创建，随后可以被重复调用。
### 第二种定义方式
```java
String s1 = new String("abc");
String s = "abc"
System.out.println(s==s1)
System.out.println(s.equals(s1))
```
返回结果：false true  
用new的方法创建字符串对象，存储位置在堆内存中，实际创建了两个对象，一个new，一个字符串对象。  
string类中的equals复写了Object中的equals，建立了string类自己的判断字符串对象是否相同的依据。  
其实就是比较字符串内容。


## 构造函数
```java
public class StringConstructorDemo {
	public static void main(String[] args) {
		/*
		 * 将字节数组或者字符数组转成字符串可以通过String类的构造函数完成。
		 */
		stringConstructorDemo2();
	}

	private static void stringConstructorDemo2() {
		char[] arr = {'w','a','p','q','x'};
		String s = new String(arr,1,3);
		System.out.println("s="+s);
	}

	public static void stringConstructorDemo() {
		String s = new String();//等效于String s = "";  不等效String s = null;
		byte[] arr = {97,66,67,68};
		String s1 = new String(arr);
		System.out.println("s1="+s1);
	}
}
```


## 常用方法
### 获取
```java
String  s = "abcdae";
System.out.println("length:"+s.length());//6
System.out.println("char:"+s.charAt(2));//c
System.out.println("index:"+s.indexOf('k'));//-1 我们可以根据-1，来判断该字符或者字符串是否存在。
System.out.println("lastIndex:"+s.lastIndexOf('a'));//4
System.out.println("substring:"+s.substring(2,4));//cd
```
### 转换
```java
String  s = "张三,李四,王五";
String[] arr = s.split(",");//张三 李四 王五

char[] chs = s.toCharArray();//张 三 李 四 王 五
    
s = "ab你";
byte[] bytes = s.getBytes();//97 98 -60 -29

System.out.println("Abc".toUpperCase());//ABC
		
String s1 = "java";
String s2 = s1.replace('q', 'z');//“java”
System.out.println(s1==s2);//true
    
System.out.println("-"+"    ab  c    ".trim()+"-");//-ab  c-

System.out.println("abc".concat("kk"));abckk
```
### 判断
```java
String s = "abc";
System.out.println(s.equalsIgnoreCase("ABC"));//true
System.out.println(s.contains("cc"));//false
		
String str  = "ArrayDemo.java";
System.out.println(str.startsWith("Array"));
System.out.println(str.endsWith(".java"));
System.out.println(str.contains("Demo"));
```




## StringBuffer
### StringBuffer常用功能
* 添加
```java
StringBuffer sb = new StringBuffer();
sb.append(4).append(false);//4false
sb.insert(1, "haha");//4hahafalse
```
* 删除
```java
StringBuffer sb = new StringBuffer("abce");
sb.delete(1, 3);//ae
//清空缓冲区。
sb.delete(0,sb.length());
```

* 修改  
```java
sb.replace(1, 3, "nba");//anbae
sb.setCharAt(2, 'q');anqae
```

* String与StringBuffer的相互转换
```java
// String -- StringBuffer
		String s = "hello";
		// 方式1:通过构造方法
		StringBuffer sb = new StringBuffer(s);
		// 方式2：通过append()方法
		StringBuffer sb2 = new StringBuffer();
		sb2.append(s);

// StringBuffer -- String
		StringBuffer buffer = new StringBuffer("java");
		// 方式1:通过构造方法
		String str = new String(buffer);
		// 方式2：通过toString()方法
		String str2 = buffer.toString();
```
## StringBuilder
功能和StringBuffer一摸一样，但线程不同步。由于不执行同步，所以单线程下速度更快。




## 练习
### 练习1  [答案](#练习1)
给定一个字符串数组，按照字典顺序进行从小到大的排序。  
{"nba","abc","cba","zz","qq","haha"}
### 练习2  [答案](#练习2)
一个子串在整串中出现的次数。  
"nbaernbatynbauinbaopnba"
### 练习3  [答案](#练习3)
两个字符串中最大相同的子串。  
"qwerabcdtyuiop"  
"xcabcdvbn"  
### 练习4  [答案](#练习4)
模拟一个trim功能一致的方法。去除字符串两端的空白   


### 练习1
```java
public class StringTest_1 {
	public static void main(String[] args) {
		String[] arr = { "nba", "abc", "cba", "zz", "qq", "haha" };
		printArray(arr);
		sortString(arr);
		printArray(arr);
	}

	public static void sortString(String[] arr) {
		for (int i = 0; i < arr.length - 1; i++) {
			for (int j = i + 1; j < arr.length; j++) {
				if(arr[i].compareTo(arr[j])>0)//字符串比较用compareTo方法
					swap(arr,i,j);
			}
		}
	}

	private static void swap(String[] arr, int i, int j) {
		String temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
	}

	public static void printArray(String[] arr) {
		System.out.print("[");
		for (int i = 0; i < arr.length; i++) {
			if (i != arr.length - 1)
				System.out.print(arr[i] + ", ");
			else
				System.out.println(arr[i] + "]");
		}
	}
}
```

### 练习2
```java
public class StringTest_2 {
	  public static void main(String[] args) {
		String str = "nbaernbatnbaynbauinbaopnba";
		String key = "nba";
		int count = getKeyStringCount_2(str,key);
		System.out.println("count="+count);
	}
	public static int getKeyStringCount_2(String str, String key) {
		int count = 0;
		int index = 0;
		while((index = str.indexOf(key,index))!=-1){
			index = index + key.length();
			count++;
		}
		return count;
	}

	public static int getKeyStringCount(String str, String key) {
		
		//1,定义计数器。 
		int count = 0;
		
		//2，定义变量记录key出现的位置。
		int index = 0;
		while((index = str.indexOf(key))!=-1){
			str = str.substring(index+key.length());
			count++;
		}
		return count;
	}
}
```

### 练习3
```java
public class StringTest_3 {
	public static void main(String[] args) {
		String s1 = "qwerabcdtyuiop";
		String s2 = "xcabcdvbn";
		String s = getMaxSubstring(s1, s2);
		System.out.println("s=" + s);
	}
  
	public static String getMaxSubstring(String s1, String s2) {
		String max = null,min = null;
		if(s1.length()>=s2.length())
		{
			max=s1;
			min=s2;
		}
		else 
		{
			min=s1;
			max=s2;
		}
	
		for (int i = 0; i < min.length(); i++) 
    {
			for(int a = 0,b = min.length()-i; b != min.length()+1; a++,b++)
      {
				String sub = min.substring(a, b);
				if(max.contains(sub))
					return sub;
			}
		}
		return null;
	}
}
```

### 练习4
```java
public class StringTest_4 {
	public static void main(String[] args) {
		String s = "    ab   c     ";
		s = myTrim(s);
		System.out.println("-" + s + "-");
	}

	public static String myTrim(String s) {
		int start = 0, end = s.length() - 1;
		while (start <= end && s.charAt(start) == ' ') 
    {
			start++;
		}
		while (start <= end && s.charAt(end) == ' ') 
    {
			end--;
		}
		return s.substring(start, end + 1);
	}
}
```
