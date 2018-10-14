# 集合框架Collection
- [常用方法](#常用方法)
- [迭代器](#迭代器)


## 常用方法
### 添加
boolean add(Object obj);  
boolean addAll(Collection coll);  
### 删除
boolean remove(object obj);//remove会改变集合的长度  
boolean removeAll(Collection coll);//将内部与coll集合中相同的元素删除  
void clear();//清空
### 判断
boolean contains(object obj);  
boolean containsAll(Colllection coll);  
boolean isEmpty();//判断集合中是否有元素。  
### 获取
int size();//获取长度  
Iterator iterator();//迭代器(取出集合中元素的方式称为迭代器)  
### 其他
boolean retainAll(Collection coll);//取交集（相同的元素）  
Object[] toArray();//将集合转成数组。  

方法示例：
```java
//创建两个集合
Collection c1 = new ArrayList();
Collection c2 = new ArrayList();

//添加
//给c1添加元素。
c1.add("abc1");
c1.add("abc2");
c1.add("abc3");
c1.add("abc4");//c1=[abc1,abc2,abc3,abc4]
//给c2添加元素。
c2.add("abc2");
c2.add("abc6");
c2.add("abc7");//c2=[abc2,abc6,abc7]

c1.addAll(c2);//c1=[abc1,abc2,abc3,abc4,abc2,abc6,abc7]

//删除 
//删除会改变集合的长度
c1.removeAll(c2);//c1=[abc1,abc3,abc4]  将两个集合中相同的元素删除

//判断
boolean b =ci.containsAll(c2);//false

//其他
c1.retainAll(c2);//c1=[abc2],取交集
```
## 迭代器
```java
Collection coll = new ArrayList();
coll.add("abc1");
coll.add("abc2");
coll.add("abc3");
coll.add("abc4");
//使用Collection中的iterator()方法, 调用集合中的迭代器方法，是为了获取集合中的迭代器对象。
Iterator it = coll.iterator();//创建一个迭代器对象		
System.out.println(it.next());//abc1   返回迭代的下一个元素
System.out.println(it.next());//abc2
System.out.println(it.next());//abc3
System.out.println(it.next());//abc4
System.out.println(it.next());//java.util.NoSuchElementException
//改为循环
for(Iterator it = coll.iterator(); it.hasNext(); ){
			System.out.println(it.next());
		}

