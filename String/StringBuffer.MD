# StringBuffer常用功能
添加
```java
StringBuffer sb = new StringBuffer();
sb.append(4).append(false);//4false
sb.insert(1, "haha");//4hahafalse
```
删除
```java
StringBuffer sb = new StringBuffer("abce");
sb.delete(1, 3);//ae
//清空缓冲区。
sb.delete(0,sb.length());

修改  
sb.replace(1, 3, "nba");//anbae
sb.setCharAt(2, 'q');anqae
```

#StringBuilder
功能和StringBuffer一摸一样，但线程不同步。由于不执行同步，所以单线程下速度更快。
