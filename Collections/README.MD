# 集合框架工具类
  - [Collections](#collections)
  - [可变参数](#可变参数)
  - [练习](#练习)
  
  
  
## Collections
* 排序
```java
public static void demo_1(){
		List<String> list = new ArrayList<String>();
		list.add("abcde");
		list.add("cba");
		list.add("aa");
		list.add("zzz");
		list.add("cba");
		list.add("nbaa");
		//对list集合进行指定顺序的排序。
//		Collections.sort(list);自然排序
		Collections.sort(list,new ComparatorByLength());//根据比较器排序
		System.out.println(list);
	}
//实现原理，比较器方法
	public static <T> void mySort(List<T> list,Comparator<? super T> comp){
		for (int i = 0; i < list.size()-1; i++) {
			for (int j = i+1; j < list.size(); j++) {
				if(comp.compare(list.get(i), list.get(j))>0){
					Collections.swap(list, i, j);
				}
			}
		}
	}
//实现原理，自然排序方法
	public static <T extends Comparable<? super T>> void mySort(List<T> list){
		for (int i = 0; i < list.size()-1; i++) {
			for (int j = i+1; j < list.size(); j++) {
				if(list.get(i).compareTo(list.get(j))>0){
//					T temp = list.get(i);
//					list.set(i, list.get(j));
//					list.set(j, temp);
					Collections.swap(list, i, j);
				}
			}
		}
	}
}
public static void demo_3() {
		/*逆转排序
		TreeSet<String> ts = new TreeSet<String>(new Comparator<String>(){
			public int compare(String o1, String o2) {
				int temp = o2.compareTo(o1);
				return temp;
			}
			
		});
		*/逆转
		TreeSet<String> ts = new TreeSet<String>(Collections.reverseOrder(new ComparatorByLength()));	
		ts.add("abc");
		ts.add("hahaha");
		ts.add("zzz");
		ts.add("aa");
		ts.add("cba");
		
		System.out.println(ts);
	}

```
* 交换
```java

```
* 获取
```java

```
* 其他
```java

```


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

## 练习
### [练习1](#答案1)
模拟斗地主洗牌发牌



## 答案2
```java

```
