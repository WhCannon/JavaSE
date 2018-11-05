# 正则表达式
  - [常用操作](#常用操作)
  - [练习](#练习)



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
public  static void functionDemo_4() {
		String str = "da jia hao,ming tian bu fang jia!";
		String regex = "\\b[a-z]{3}\\b";
		
		//1,将正则封装成对象。
		Pattern p = Pattern.compile(regex);
		//2通过正则对象的matcher方法和字符串相关联。获取要对字符串操作的匹配器对象Matcher
		Matcher m = p.matcher(str);
		
		//使用Matcher对象的方法对字符串进行操作。
		//既然要获取三个字母组成的单词,使用查找find();
		System.out.println(str);
		while(m.find()){//先查找
			System.out.println(m.group());//再获取匹配的子序列
			System.out.println(m.start()+":"+m.end());
		}
```

## 练习
### [练习1](#答案1)
对IP地址排序

### [练习2](#答案2)
对邮件地址校验




## 答案1
```java
public static void test_2(){
		String ip_str = "192.168.10.34  127.0.0.1  3.3.3.3  105.70.11.55";
		
		//为了让ip可以按照字符串顺序比较，只要让ip的每一段的位数相同。
		//所以，补零，按照每一位所需做多0补充。每一段都加两个0.
		ip_str = ip_str.replaceAll("(\\d+)", "00$1");
		System.out.println(ip_str);
		
		//然后每一段保留数字3位。
		ip_str = ip_str.replaceAll("0*(\\d{3})", "$1");
		System.out.println(ip_str);
		
		//将ip地址切出。
		String[] ips = ip_str.split(" +");
		TreeSet<String> ts = new TreeSet<String>();
		
		for(String  ip : ips){
			ts.add(ip);
		}
		for(String ip : ts){
			//输出时将多余的0去掉
			System.out.println(ip.replaceAll("0*(\\d+)", "$1"));
		}
	}
```

## 答案2
```java
public static void test_3() {
		String mail = "abc1@sina.com.cn";
		String regex = "[a-zA-Z0-9_]+@[a-zA-Z0-9]+(\\.[a-zA-Z]{1,3})+";
		
		regex = "\\w+@\\w+(\\.\\w+)+";//1@1.1
		
		boolean b = mail.matches(regex);
		System.out.println(mail+":"+b);
	}
```
