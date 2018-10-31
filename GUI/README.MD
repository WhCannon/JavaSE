# GUI图形用户界面
Java为GUI提供的对象都在java.awt包和javax.swing包。
  * ## [java.awt](#java.awt)
  * ## [javax.swing](#javax.swing)




## java.awt

创建GUI演示
```java
public class FrameDemo {
	public static void main(String[] args) {

		Frame f = new Frame("my frame");
//		f.setSize(500, 400);//设置横轴和纵轴像素长度		
//		f.setLocation(400, 200);//窗体出现的位置
		f.setBounds(400, 200, 500, 400);//可以一起设置
		
		f.setLayout(new FlowLayout());//设置流式布局
		Button but  = new Button("一个按钮");//建立组件
		f.add(but);//将按钮添加到窗体中。 
		
		//在窗体上加一个监听
		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
		
		//在按钮上加上一个监听。
		but.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				//处理方式
				System.exit(0);
			}
		});
		f.setVisible(true);//可视化
		System.out.println("over");
	}
}
```
