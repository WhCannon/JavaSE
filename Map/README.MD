# Map集合
接口Map<K,V>   也称为双列集合，存储的是键值对，K-键  V-值  
- ## [常用方法](#常用方法)
- ## 常见子类
	 - [HashMap](#hashmap)
	 	 - [LinkedHashMap](#linkedhashmap)
	 - [TreeMap](#treemap)







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
