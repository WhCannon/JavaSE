# 基本数据类型对象包装类
- [自动装箱拆箱](#自动装箱拆箱)
- [练习](#练习)
### 自动装箱拆箱
```java
Integer ii = 100;//自动装箱
ii += 200; //自动拆箱，再自动装箱
System.out.println("ii:" + ii);

// 通过反编译后的代码
// Integer ii = Integer.valueOf(100); //自动装箱
// ii = Integer.valueOf(ii.intValue() + 200); //自动拆箱，再自动装箱
// System.out.println((new StringBuilder("ii:")).append(ii).toString());

Integer a = new Integer(128);
Integer b = new Integer(128);
System.out.println(a==b);//false
System.out.println(a.equals(b));//true
		
Integer x = 127;
Integer y = 127;
System.out.println(x==y);//true
System.out.println(x.equals(y));//true

Integer x = 128;
Integer y = 128;
System.out.println(x==y);//false
System.out.println(x.equals(y));//true
jdk1.5以后，自动装箱，如果装箱的是一个字节，那么该数据会被共享，不会重新开辟空间。
```
### 练习
#### 练习1 [答案1](#答案1)
对一个字符串中的数值进行从小到大的排序  
 "20 78 9 -7 88 36 29"  



#### 答案1
```java
public class WrapperTest {
	private static final String SPACE_SEPARATOR = " ";
	public static void main(String[] args) {
		String numStr = "20 78 9 -7 88 36 29";
		System.out.println(numStr);
		numStr = sortStringNumber(numStr);
		System.out.println(numStr);
	}
	
	public static String sortStringNumber(String numStr) {
		//1,将字符串变成字符串数组。
		String[] str_arr = stringToArray(numStr);
		
		//2,将字符串数组变成int数组。
		int[] num_arr = toIntArray(str_arr);
		
		//3,对int数组排序。
		mySortArray(num_arr);
		//4,将排序后的int数组变成字符串。
		String temp = arrayToString(num_arr);
		return temp;
	}

	public static String arrayToString(int[] num_arr) {
		StringBuilder sb = new StringBuilder();
		for(int x = 0; x<num_arr.length; x++){
			if(x!=num_arr.length-1)
				sb.append(num_arr[x]+SPACE_SEPARATOR);
			else
				sb.append(num_arr[x]);
		}
		return sb.toString();
	}

	public static void mySortArray(int[] num_arr) {
		Arrays.sort(num_arr);
	}

	public static int[] toIntArray(String[] str_arr) {
		int[] arr = new int[str_arr.length];
		for (int i = 0; i < arr.length; i++) {
			arr[i] = Integer.parseInt(str_arr[i]);
		}
		return arr;
	}

	public static String[] stringToArray(String numStr) {
		String[] str_arr = numStr.split(SPACE_SEPARATOR);
		return str_arr;
	}
}
```
