# GUI图形用户界面
Java为GUI提供的对象都在java.awt包和javax.swing包。
  * ## [java.awt](#java.awt)
  * ## [javax.swing](#javax.swing)




## java.awt

* 创建GUI演示
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

* 鼠标和键盘监听
```java
public class MouseAndKeyDemo {
	private Frame f;
	private TextField tf;
	private Button but;
	
	public MouseAndKeyDemo() {
		init();
	}

	public static void main(String[] args) {
		new MouseAndKeyDemo();
	}
	
	private void init() {
		f = new Frame("演示鼠标和键盘监听");
		f.setBounds(400,200,500,400);
		f.setLayout(new FlowLayout());
		
		tf = new TextField(35);
		but = new Button("一个按钮");
		f.add(tf);
		f.add(but);
		
		myEvent();
		f.setVisible(true);
	}
	private void myEvent() {
		//给文本框添加键盘监听。
		tf.addKeyListener(new KeyAdapter() {
			@Override
			public void keyPressed(KeyEvent e) {
				System.out.println("key run..."+KeyEvent.getKeyText(e.getKeyCode())+"::::"+e.getKeyCode());
				// code是键盘上每个键对应的值
				// int code = e.getKeyCode();
				// if(!(code>=KeyEvent.VK_0 && code<=KeyEvent.VK_9)){
				//		System.out.println("必须是数字");
				//		e.consume();
				// }
				
				if(e.isControlDown() && e.getKeyCode()==KeyEvent.VK_ENTER){
					//组合键
					System.out.println("enter run ...");
				}
			}
		});
		
		f.addWindowListener(new WindowAdapter() {
			//视窗关闭
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
		
		//在按钮上添加一个鼠标监听.
		but.addMouseListener(new MouseAdapter() {
			private int count = 1;
			@Override
			public void mouseClicked(MouseEvent e) {
				if(e.getClickCount()==2)//获取点击次数
				tf.setText("mouse double click..."+count++);
			}
		});
	}
}

```
