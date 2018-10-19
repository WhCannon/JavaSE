# Map集合
接口Map<K,V>   也称为双列集合，存储的是键值对，K-键  V-值  
- ## [常用方法](#常用方法)







## 常用方法
1、添加  
	value put(key,value);返回前一个与key关联的值，如果没有则返回null  
2、删除  
void  clear();清空map集合  
	value remove(key);根据指定的key翻出这个键值对  
3、判断  
boolean containsKey(key);  
	boolean containsValue(value);  
	boolean isEmpty();  
4、获取  
value get(key);//通过键获取值，如果没有该键返回null。(可以通过返回null，来判断是否包含指定键)   
	int size(); 获取键值对的个数  
5、取出所有键值对
Set<K> keySet();     返回此映射中键的Set集合  
通过keySet方法获取map中所有键的Set集合，再通过Set的迭代器获取到每一个键
再对每一个键通过map集合的get方法获取其对应的值即可  

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
		
    //取出map集合中所有的键值对
    Set<Integer> k = map.keySet();
		Iterator<Integer> it = k.iterator();
		
		while(it.hasNext()){
			Integer key = it.next();
			String value = map.get(key);
			System.out.println(key+":"+value);	
		}
	}
```
