# Map集合
接口Map<K,V>   也称为双列集合，存储的是键值对，K-键  V-值  
- ## [常用方法](#常用方法)
- ## 常见子类
	 - [HashTable](#hashtable)
	 	- [Properties](#properties)
	 - [HashMap](#hashmap)
	 	 - [LinkedHashMap](#linkedhashmap)
	 - [TreeMap](#treemap)
- ## [练习](#练-习)







## 常用方法
### 1、添加  
	value put(key,value);返回前一个与key关联的值，如果没有则返回null  
### 2、删除  
 void  clear();清空map集合  
	value remove(key);根据指定的key翻出这个键值对  
### 3、判断  
boolean containsKey(key);  
	boolean containsValue(value);  
	boolean isEmpty();  
### 4、获取  
value get(key);//通过键获取值，如果没有该键返回null。(可以通过返回null，来判断是否包含指定键)   
int size(); 获取键值对的个数  
Collection<V> values();返回映射中所有值的collection集合   

### 5、取出所有键值对  
#### Set<K> keySet()
返回此映射中键的Set集合    
通过keySet方法获取map中所有键的Set集合，再通过Set的迭代器获取到每一个键
再对每一个键通过map集合的get方法获取其对应的值即可 
#### Set<Map.Entry<K,V>> entrySet()
返回此映射包含的映射关系的Set集合  
该方法将键和值的映射关系作为对象存储到了Set集合中，而这个映射关系的类型就是Map.Entry类型。  
再通过getKey()和getValue()获取键值对。


```java
public static void main(String[] args) {
		
		Map<Integer,String> map = new HashMap<Integer,String>();
	}
  
public static void method(Map<Integer,String> map){//存储学号和姓名
		// 添加元素。
		System.out.println(map.put(8, "wangcai"));//返回null
		System.out.println(map.put(8, "xiaoqiang"));//返回wangcai 存相同键，值会覆盖。
		map.put(2,"zhangsan");
		map.put(7,"zhaoliu");
		
		//删除。
		System.out.println("remove:"+map.remove(2));//删除了2=zhangsan
		
		//判断。
		System.out.println("containskey:"+map.containsKey(7));
		
		//获取。 
		System.out.println("get:"+map.get(6));//null
		
		//获取所有值
		Collection<String> values = map.values();
		Iterator<String> it2 = values.iterator();
		while(it2.hasNext()){
			System.out.println(it2.next());
		}
		
    //取出map集合中所有的键值对1
    Set<Integer> k = map.keySet();
		Iterator<Integer> it = k.iterator();
		
		while(it.hasNext()){
			Integer key = it.next();
			String value = map.get(key);
			System.out.println(key+":"+value);	
		}
		
	//取出键值对2
		Set<Map.Entry<Integer, String>> entrySet = map.entrySet();
		Iterator<Map.Entry<Integer, String>> it = entrySet.iterator();

		while(it.hasNext()){
			Map.Entry<Integer, String> me = it.next();
			Integer key = me.getKey();
			String value = me.getValue();
			System.out.println(key+"::::"+value);
			
		}
	}
```

## 常见子类

### HashTable

#### Properties
```java
public static void propertiesDemo(){
		//创建一个Properties集合。
		Properties prop  = new Properties();
		
		//存储元素。 
		prop.setProperty("zhangsan","30");
		prop.setProperty("lisi","31");
		prop.setProperty("wangwu","36");
		prop.setProperty("zhaoliu","20");
		
		//修改元素。 
		prop.setProperty("wangwu","26");
		
		//取出所有元素。
		Set<String> names = prop.stringPropertyNames();
		for(String name : names){
			String value = prop.getProperty(name);//获取值
			System.out.println(name+":"+value);
		}
	}
	
list(PrintStream out);将属性列表输出到指定的输出流
Properties prop  = new Properties();
prop = System.getProperties();
prop.list(System.out);

将配置信息存储到硬盘上（持久化）
	public static void methodDemo_3() throws IOException {
		Properties prop  = new Properties();
		//存储元素。 
		prop.setProperty("zhangsan","30");
		prop.setProperty("lisi","31");
		prop.setProperty("wangwu","36");
		prop.setProperty("zhaoliu","20");
		
	//  想要将这些集合中的字符串键值信息持久化存储到文件中，需要关联输出流。
	//	字节流或字符流均可
		FileOutputStream fos = new FileOutputStream("info.txt");
		
		//将集合中数据存储到文件中，使用store方法，需要添加描述信息。
		prop.store(fos, "info");
		fos.close();
	}
	
获取文件中的配置信息（键值对数据）
public static void methodDemo_4() throws IOException {	
		Properties prop  = new Properties();
		
		//集合中的数据来自于一个文件。 
		//注意；必须要保证该文件中的数据是键值对。
		//需要使用到读取流（字节流、字符流均可） 
		FileInputStream fis = new FileInputStream("info.txt");
		
		//使用load方法。 
		prop.load(fis);
		prop.list(System.out);
	}
	//load方法原理。
	public static void myLoad() throws IOException{
		Properties prop  = new Properties();
		BufferedReader bufr = new BufferedReader(new FileReader("info.txt"));
		
		String line = null;
		while((line=bufr.readLine())!=null){
			if(line.startsWith("#"))
				continue;
			String[] arr = line.split("=");
			prop.setProperty(arr[0], arr[1]);
		}
		prop.list(System.out);
		bufr.close();
	}

对已有的配置文件中的信息进行修改
/* 步骤：
	 * 读取这个文件。
	 * 将这个文件中的键值数据存储到集合中。
	 * 再通过集合对数据进行修改。
	 * 再通过流将修改后的数据存储到文件中。 
	 */
	public static void test() throws IOException{
		//读取这个文件。
		File file = new File("info.txt");
		if(!file.exists()){
			file.createNewFile();
		}
		FileReader fr = new FileReader(file);
		//创建集合存储配置信息。
		Properties prop = new Properties();
		
		//将流中信息存储到集合中。
		prop.load(fr);
		prop.list(System.out);
		
		//修改
		prop.setProperty("wangwu", "16");
		FileWriter fw = new FileWriter(file);
		prop.store(fw,"");
		
		fw.close();
		fr.close();
	}

```


### HashMap
内部结构为哈希表，不同步。允许null作为键和值。
```java
public class HashMapDemo {
	public static void main(String[] args) {
		  //将学生对象和学生的归属地通过键与值存储到map集合中。
		HashMap<Student,String> hm = new HashMap<Student,String>();

		hm.put(new Student("lisi",38),"北京");
		hm.put(new Student("zhaoliu",24),"上海");
		hm.put(new Student("xiaoqiang",31),"沈阳");
		hm.put(new Student("wangcai",28),"大连");
		hm.put(new Student("zhaoliu",24),"铁岭");
		
//		Set<Student> keySet = hm.keySet();		
//		Iterator<Student> it = keySet.iterator();
		Iterator<Student> it = hm.keySet().iterator();
		while(it.hasNext()){
			Student key = it.next();
			String value = hm.get(key);
			System.out.println(key.getName()+":"+key.getAge()+"---"+value);
		}
	}
}

```

#### LinkedHashMap

### TreeMap
内部结构为二叉树，不同步。可以对Map集合中的键排序。
```java
public class TreeMapDemo {
	public static void main(String[] args) {
		TreeMap<Student,String> tm = new TreeMap<Student,String>(new ComparatorByName());
		
		tm.put(new Student("lisi",38),"北京");
		tm.put(new Student("zhaoliu",24),"上海");
		tm.put(new Student("xiaoqiang",31),"沈阳");
		tm.put(new Student("wangcai",28),"大连");
		tm.put(new Student("zhaoliu",24),"铁岭");
		
		Iterator<Map.Entry<Student, String>> it = tm.entrySet().iterator();
		while(it.hasNext()){
			Map.Entry<Student,String> me = it.next();
			Student key = me.getKey();
			String value = me.getValue();
			
			System.out.println(key.getName()+":"+key.getAge()+"---"+value);
		}
	}
}
```

## 练 习
### [练习1](#答案1)
“fdgavcbsacdfs”获取该字符串中每一个字母出现的次数。  
要求打印结果是：a(2)b(1)...

### [练习2](#答案2)
定义功能，获取一个应用程序运行的次数，如果超过5次，给出使用次数已到请注册的提示，并不要再运行程序  


### 答案1
```java
public class MapTest {

	public static void main(String[] args) {
		
		String str = "fdg+avAdc  bs5dDa9c-dfs";
		String s = getCharCount(str);
		System.out.println(s);
	}

	public static String getCharCount(String str) {
		//将字符串变成字符数组 
		char[] chs = str.toCharArray();
		
		//定义map集合表。
		Map<Character,Integer> map = new TreeMap<Character,Integer>();
		
		//只记录字母
		for (int i = 0; i < chs.length; i++) {
			if(!(chs[i]>='a' && chs[i]<='z' || chs[i]>='A' && chs[i]<='Z'))
				continue;
			
			//将数组中的字母作为键去查map表。			
			Integer value = map.get(chs[i]);
					
			//判断值是否为null.
			if(value==null){
				map.put(chs[i], 1);
			}else{
				map.put(chs[i], value+1);
			}
		}
		return mapToString(map);
	}

	private static String mapToString(Map<Character, Integer> map) {
		StringBuilder sb = new StringBuilder();
		Iterator<Character> it = map.keySet().iterator();
		
		while(it.hasNext()){
			Character key = it.next();
			Integer value = map.get(key);
			sb.append(key+"("+value+")");
		}
		return sb.toString();
	}
}

```

### 答案2
```java
思路：
 * 1、应该有计数器。 每次程序启动都需要计数一次,并且是在原有的次数上进行计数。
 * 2、计数器就是一个变量。 
 * 程序启动时进行计数，计数器必须存在于内存并进行运算。
 * 可是程序一结束，计数器消失了。那么再次启动该程序，计数器又重新被初始化了。
 * 而我们需要多次启动同一个应用程序，使用的是同一个计数器。 
 * 这就需要计数器的生命周期变长，从内存存储到硬盘文件中。
 * 3、如何使用这个计数器呢？
 * 首先，程序启动时，应该先读取这个用于记录计数器信息的配置文件。
 * 获取上一次计数器次数，并进行试用次数的判断。
 * 其次，对该次数进行自增，并将自增后的次数重新存储到配置文件中。 
 * 4、文件中的信息该如何进行存储并体现。
 * 	直接存储次数值可以，但是不明确该数据的含义，所以起名字就变得很重要。
 * 	这就有了名字和值的对应，所以可以使用键值对。
 * 	可是映射关系map集合搞定，又需要读取硬盘上的数据，所以map+io = Properties. 
 */

public class PropertiesTest {
	public static void main(String[] args) throws IOException  {
		getAppCount();
	}
	
	public static void getAppCount() throws IOException{
		//将配置文件封装成File对象。
		File confile = new File("count.properties");
		if(!confile.exists()){
			confile.createNewFile();
		}
		
		FileInputStream fis = new FileInputStream(confile);
		
		//加载配置信息
		Properties prop = new Properties();
		prop.load(fis);
		
		//从集合中通过键获取次数。		
		String value = prop.getProperty("time");
		//定义计数器，记录获取到的次数。
		int count =0;
		if(value!=null){
			count = Integer.parseInt(value);
			if(count>=5){
//				System.out.println("使用次数已到，请注册，给钱！");
//				return;
				throw new RuntimeException("使用次数已到，请注册，给钱！");
			}
		}
		count++;
		
		//将改变后的次数重新存储到集合中。
		prop.setProperty("time", count+"");
		
		FileOutputStream fos = new FileOutputStream(confile);
		prop.store(fos, "");
		
		fos.close();
		fis.close();
	}
}

```
