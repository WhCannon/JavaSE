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
思路：
A:创建一个HashMap集合
B:创建一个ArrayList集合
C:创建花色数组和点数数组
D:从0开始往HashMap里面存储编号，并存储对应的牌
  同时往ArrayList里面存储编号即可。
E:洗牌(洗的是编号)
F:发牌(发的也是编号，为了保证编号是排序的，就创建TreeSet集合接收)
G:看牌(遍历TreeSet集合，获取编号，到HashMap集合找对应的牌)

public class PokerDemo {
	public static void main(String[] args) {
		// 创建一个HashMap集合
		HashMap<Integer, String> hm = new HashMap<Integer, String>();

		// 创建一个ArrayList集合
		ArrayList<Integer> array = new ArrayList<Integer>();

		// 创建花色数组和点数数组
		// 定义一个花色数组
		String[] colors = { "♠", "♥", "♣", "♦" };
		// 定义一个点数数组
		String[] numbers = { "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q",
				"K", "A", "2", };

		// 从0开始往HashMap里面存储编号，并存储对应的牌,同时往ArrayList里面存储编号即可。
		int index = 0;
		for (String number : numbers) {
			for (String color : colors) {
				String poker = color.concat(number);
				hm.put(index, poker);
				array.add(index);
				index++;
			}
		}
		hm.put(index, "小王");
		array.add(index);
		index++;
		hm.put(index, "大王");
		array.add(index);

		// 洗牌(洗的是编号)
		Collections.shuffle(array);

		// 发牌(发的也是编号，为了保证编号是排序的，就创建TreeSet集合接收)
		TreeSet<Integer> fengQingYang = new TreeSet<Integer>();
		TreeSet<Integer> linQingXia = new TreeSet<Integer>();
		TreeSet<Integer> liuYi = new TreeSet<Integer>();
		TreeSet<Integer> diPai = new TreeSet<Integer>();

		for (int x = 0; x < array.size(); x++) {
			if (x >= array.size() - 3) {
				diPai.add(array.get(x));
			} else if (x % 3 == 0) {
				fengQingYang.add(array.get(x));
			} else if (x % 3 == 1) {
				linQingXia.add(array.get(x));
			} else if (x % 3 == 2) {
				liuYi.add(array.get(x));
			}
		}

		// 看牌(遍历TreeSet集合，获取编号，到HashMap集合找对应的牌)
		lookPoker("风清扬", fengQingYang, hm);
		lookPoker("林青霞", linQingXia, hm);
		lookPoker("刘意", liuYi, hm);
		lookPoker("底牌", diPai, hm);
	}

	// 写看牌的功能
	public static void lookPoker(String name, TreeSet<Integer> ts,
			HashMap<Integer, String> hm) {
		System.out.print(name + "的牌是：");
		for (Integer key : ts) {
			String value = hm.get(key);
			System.out.print(value + " ");
		}
		System.out.println();
	}
}
```
