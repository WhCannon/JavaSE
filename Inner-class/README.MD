# 内部类
- [定义内部类](https://github.com/WhCannon/JavaSE/tree/master/Inner-class/定义内部类.MD)
- [内部类私有](#内部类私有)
- [匿名内部类](https://github.com/WhCannon/JavaSE/tree/master/Inner-class/匿名内部类.MD)
- [练习题](https://github.com/WhCannon/JavaSE/tree/master/Inner-class/练习题.MD)




## 内部类私有
```java
我有一个人(人有身体，身体内有心脏。)
		class Body {
			private class Heart {
				public void operator() {
					System.out.println("心脏搭桥");
				}
			}
			public void method() {
				if(如果你是外科医生) {
					Heart h = new Heart();
					h.operator();
				}
			}
		}
```
