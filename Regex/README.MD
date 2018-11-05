# 正则表达式
  - [常用操作](#常用操作)



## 常用操作
* 匹配  
String类中的matches方法
```java
public static void functionDemo_1(){
		//匹配手机号码是否正确。 
		String tel = "15800001111";
		String regex = "1[358][0-9]{9}";//或写成"1[358]\\d{9}"
		
		boolean b = tel.matches(regex);
		System.out.println(tel+":"+b);
	}
```

* 切割  
String类中的split方法
```java
public static void functionDemo_2(){
		String str = "zhangsan    xiaoqiang    zhaoliu";
		String[] names = str.split(" +");//一个或多个空格
		for(String name : names){
			System.out.println(name);
		}
		
		String str = "zhangsan.xiaoqiang.zhaoliu";
		String[] names = str.split("\\.");//.在正则中代表任意字符，需要转译
		for(String name : names){
			System.out.println(name);
		}
		
		String str = "zhangsanttttxiaoqiangmmmmmmzhaoliu";
		String[] names = str.split("(.)\\1+");//正则中()代表复用
		//正则中()代表一个捕获组，()\\1代表第一组，()\\1+代表第一组内容出现了1次或多次
		for(String name : names){
			System.out.println(name);
		}

正则中 ()代表一个组，
组：((A)(B(C))) 组的顺序按左括号的顺序来排，
第一组((A)(B(C)))
第二组\A
第三组(B(C))
第四组(C)
```

* 替换  
String类中的replaceAll()方法
```java
public static void functionDemo_3() {
		String str = "zhangsanttttxiaoqiangmmmmmmzhaoliu";
		str = str.replaceAll("(.)\\1+", "$1");//多个t换成一个t
		System.out.println(str);
		
		String tel = "15800001111";//158****1111
		//将号码头尾分成2组，替换两个组中间的部分
		tel = tel.replaceAll("(\\d{3})\\d{4}(\\d{4})", "$1****$2");
		System.out.println(tel);
	}
```

* 获取
```java

```
