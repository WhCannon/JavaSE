# String类
  - [字符串缓冲区](#字符串缓冲区)
### [字符串定义方式](https://github.com/WhCannon/JavaSE/blob/master/String/定义方式.MD)
### [String类构造函数](https://github.com/WhCannon/JavaSE/blob/master/String/构造函数.MD)
### [常用方法](https://github.com/WhCannon/JavaSE/blob/master/String/常用方法.MD)
### [StringBuffer](https://github.com/WhCannon/JavaSE/blob/master/String/StringBuffer.MD)
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
