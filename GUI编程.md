 # GUI编程
 ## 组件
 1. 窗口
 2. 弹窗
 3. 面板
 4. 文本框
 5. 按钮
 6. 图片
 7. 监听事件
 8. 鼠标
 9. 键盘事件
 10. 破解工具
# 一. 简介
## Gui核心技术：Swing AWT，  
### 不流行原因：
1. 界面不美观
2. 需要jre环境
### 为什么学？
1. 写小工具  
2. 工作可能维护swing界面，概率极小！！
3. 了解mvc架构

# 二.AWT
## 1.组件和容器
## 1. Frame
## 2. 面板Panel
## 3. 布局管理器
 流式布局  
东西南北中  
表格布局   
## 4.事件监听
## 5.输入框TextField监听
## 6.简易计数器（组合+内部类复习）
oop原则：组合大于继承
（1. 组合：一个类的成员变量是另一个类，把一个对象直接直接传到类里 ，然后直接使用传进类的成员  
比较继承 减低了耦合性  
（2. 内部类：把一个类写在另一个类里方便直接使用外部类中的成员 
## 7. 画笔
```java
package com.kuang.lesson02;

import java.awt.*;

public class TestPaint {
    public static void main(String[] args) {
        new Mypaint().loadFrame();
    }
}
class Mypaint extends Frame{
    public void loadFrame(){
        setBounds(200,200,600,500);
        setVisible(true);
    }
    @Override
    // 画笔
    public void paint(Graphics g) {
        //画笔 颜色 画画
        g.setColor(Color.red);
        g.fillOval(100,100,100,100);
        // 画笔用完 还原到最初的颜色
        g.setColor(Color.black);
    }
}

```
paint方法是applet继承自awt中的Component的方法，
会在对象加载时自动调用，用来绘制该组件内部的所有内容。
如果想重新调用该方法中执行的操作可以使用repaint()方法。
## 8.鼠标监听

## 9.窗口监听
```jave
package com.kuang.lesson03;
import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class TestWindow {
}
class WindowFrame extends Frame {
    public WindowFrame()  {
        setBounds(200,200,300,300);
        setVisible(true);
        addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.out.println("windowOpened");
            }
            @Override//窗口激活
            public void windowActivated(WindowEvent e) {
                WindowFrame windowFrame = (WindowFrame) e.getSource();
                windowFrame.setTitle("被激活");
            }
        });
    }
}
```
## 10.键盘监听
```java
package com.kuang.lesson03;

import java.awt.*;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;

public class TestKeylistener {
    public static void main(String[] args) {
        new keyFrame();
    }
}
class keyFrame extends Frame{
    public keyFrame(){
        setBackground(Color.blue);
        setBounds(200,200,300,300);
        setVisible(true);
        this.addKeyListener(new KeyAdapter() {
            @Override
            public void keyPressed(KeyEvent e) {
                //键盘按下的是哪一个
                // 获得当前键盘的码

                int keyCode = e.getKeyCode();
                if (keyCode == KeyEvent.VK_UP){
                    System.out.println("你按下了上键");
                }
            }
        });
    }
}

```
# 三.Swing
## 1.窗口 (JFrame)
frame的子类
如果没有容器实例化 窗口是没有背景色的

注意点：
 JFrame的层次结构
Java窗口是指JFrame或者Frame

其次，窗口背景颜色是指直接调用JFrame或者Frame的setBackground(Color color)方法设置后显示出来的颜色。<u>其实,JFrame在你直接调用这个方法后，你的确设置了背景颜色，而你看到的却不是直接的JFrame或者Frame，而是JFrame.getContentPane()</u>.而JFrame上的contentPane默认是Color.WHITE的，所以，无论你对JFrame或者Frame怎么设置背景颜色，你看到的都只是contentPane.

最后，讲解决办法：

办法A:在完成初始化，调用getContentPane()方法得到一个contentPane容器，然后将其设置为不可见，即setVisible(false)。这样，你就可以看到JFrame的庐山真面貌啦！

核心代码this.getContentPane().setVisible(false);//得到contentPane容器，设置为不可见

方法B:将contentPane的颜色设置为你想要的颜色，而不搜索是对JFrame本身设置，



```java
package com.kuang.lesson04;

import javax.swing.*;
import java.awt.*;

public class JFrameDeom  {
    public void init() {
        //init初始化
        //JFrame是一个顶级窗口
        JFrame jf = new JFrame("这是一个JFrame窗口");
        jf.setVisible(true);
        jf.setBounds(200,200,300,300);
        jf.setBackground(Color.blue);

        //设置文字 Jlabet 标签
        JLabel jLabel = new JLabel("欢迎来到对抗路");

        //设置水平居中
        jLabel.setHorizontalAlignment(SwingConstants.CENTER);
        
        jf.add(jLabel);

        //容器实例化
        Container container = jf.getContentPane();
        container.setBackground(Color.red);


        //关闭事件
       jf.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);


    }

    public static void main(String[] args) {
        //建立窗口
        new JFrameDeom().init();
    }
```
## 2.弹窗JDialog

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;

public class DialogDemo extends JFrame {
    public DialogDemo(){
      this.setVisible(true);
      this.setBounds(300,300,300,300);
      this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);//fy:设置默认关闭操作 窗口常量
        //Jframe 放东西 需要容器
        Container contentPane = this.getContentPane();
        //绝对定位
        contentPane.setLayout(null);
        //按钮
        JButton jButton = new JButton("点击出现弹窗");
        jButton.setBounds(0,0,200,50);

        contentPane.add(jButton);

        //点击按钮 弹出事件
        jButton.addMouseListener(new MouseAdapter() {
            @Override
            public void mouseClicked(MouseEvent e) {
                new MyDialogdemo();
            }
        });
        //发现上下效果一样
//        jButton.addActionListener(new ActionListener(){
//            @Override
//            public void actionPerformed(ActionEvent e) {
//               new MyDialogdemo();
//            }
//        });
   }

    public static void main(String[] args) {
            new DialogDemo();
    }
}
//弹窗
class MyDialogdemo extends JDialog{
    public MyDialogdemo() {
        this.setVisible(true);
        this.setBounds(100,100,500,500);
        //this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE); 默认有关闭操作

        Container container = this.getContentPane(); //子类没有重写 会调父类的
        container.setLayout(null);

        container.add(new Label("王者"));
    }
}
```



## 3.标签label
```java
new JLable("文字")；
```

1.）图标Icon
```java
package com.kuang.lesson04;

import javax.swing.*;
import java.awt.*;

//图标接口 需要实现类
public class iconDemo extends JFrame implements Icon {
    private int width;
    private int heigth;

    public iconDemo(){
    }

    public iconDemo(int width, int heigth){
        this.width = width;
        this.heigth = heigth;
    }


    public void init(){
        iconDemo iconDemo = new iconDemo(15, 15);//图标 可以放在 标签 按钮等等上

        JLabel jLabel = new JLabel("adc",iconDemo,SwingConstants.CENTER);

        Container contentPane = iconDemo.getContentPane();//容器
        contentPane.add(jLabel);

        iconDemo.setVisible(true);
    }
    public static void main(String[] args) {
        new iconDemo().init();
    }

    @Override
    public void paintIcon(Component c, Graphics g, int x, int y) {
        g.fillOval(x,y,20,20);
    }

    @Override
    public int getIconWidth() {
        return this.width;
    }

    @Override
    public int getIconHeight() {
        return this.heigth;
    }
}

```
2.）图片icon

## 4.面板（Jpanel）
滚动框(JScrollPanel)

```java
package com.kuang.lesson05;

import javax.swing.*;
import java.awt.*;

public class JScrollPanelDemo extends JFrame {
    public JScrollPanelDemo(){
        Container container = this.getContentPane();

        //文本域
        JTextArea textArea = new JTextArea(20,50);//行列字数
        textArea.setText("abcdefgh");

        //Scroll面板
        JScrollPane scrollPane = new JScrollPane(textArea);
        container.add(scrollPane);

        this.setVisible(true);
        this.setBounds(200,200,300,350);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new JScrollPanelDemo();

    }
}

```



## 5.按钮(jbutton)
1. 图片按钮
```java
package com.kuang.lesson05;

import javax.swing.*;
import java.awt.*;
import java.net.URL;

public class JbttonDemo extends JFrame {
    public JbttonDemo()  {
        Container container = this.getContentPane();
        //图片变图标
        URL url = JbttonDemo.class.getResource("eyykl8.jpg");//url路径
        ImageIcon icon = new ImageIcon(url);

        //图标放在按钮上
        JButton button = new JButton();
        button.setIcon(icon);
        button.setToolTipText("这是一个图标按钮");

        container.add(button);
        this.setVisible(true);
        this.setSize(500,500);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);

    }

    public static void main(String[] args) {
        new JbttonDemo();
    }
}

```
2.单选按钮JRadioButton

```java
mport javax.swing.*;
import java.awt.*;
import java.net.URL;

public class JbttonDemo extends JFrame {
    public JbttonDemo()  {
        Container container = this.getContentPane();
        //单选框
        JRadioButton radioButton01 = new JRadioButton("A");
        JRadioButton radioButton02 = new JRadioButton("B");
        JRadioButton radioButton03 = new JRadioButton("C");
        // 由于单选框 只能选一个 要分组 一个组只能选一个
        ButtonGroup buttonGroup = new ButtonGroup();
        buttonGroup.add(radioButton01);
        buttonGroup.add(radioButton02);
        buttonGroup.add(radioButton03);

        container.add(radioButton01,BorderLayout.NORTH);
        container.add(radioButton02,BorderLayout.CENTER);
        container.add(radioButton03,BorderLayout.SOUTH);

        this.setVisible(true);
        this.setSize(500,500);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);

    }

    public static void main(String[] args) {
        new JbttonDemo();
    }
}
```
3. 复选按钮JCheckBox
```java
   import javax.swing.*;
   import java.awt.*;
   import java.net.URL;
   
   public class JbttonDemo extends JFrame {
       public JbttonDemo()  {
           Container container = this.getContentPane();
           //多选框
           JCheckBox checkBox1 = new JCheckBox("a");
           JCheckBox checkBox2 = new JCheckBox("b");
           JCheckBox checkBox3 = new JCheckBox("c");
   
           container.add(checkBox1,BorderLayout.NORTH);
           container.add(checkBox2,BorderLayout.CENTER);
           container.add(checkBox3,BorderLayout.SOUTH);
   
   
   
           this.setVisible(true);
           this.setSize(500,500);
           this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
   
       }
   
       public static void main(String[] args) {
           new JbttonDemo();
       }
   }
```
## 6. 列表

1）下拉框



2）列表框

## 7.文本框

# 四.贪吃蛇说明
实现思想：帧 1秒30帧够 60帧足够，图片连起来是动画！

信息储存在了服务器数据库，使用了DBCP连接池

#### 结构目录

![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211206222620358.png)





### Snack类

+ #####  键盘监听

实现键盘监听接口 并重写方法

```java
SnackPanel extends JPanel implements KeyListener
```

+ ##### 定时器Timer

```java
Timer(int delay, ActionListener listener)
```

listener 是实现ActionListener接口的类 并且重写了actionPerformed方法（implements  ActionListener）

Timer timer = new Timer 是固定时间自动相当于发出一次动作  收到动作会调一次actionPerformed

#### 加新功能的思路
1. 定义数据
2. 画上去
3. 监听：键盘监听和事件监听

#### 源码：

下载源码 注释特别清晰

[https://gitee.com/dong2645981073/javaSwing]: https://gitee.com/dong2645981073/javaSwing	"源码"

数据库两个表结构：

![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211206221112216.png)

![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211206221138449.png)

#### 注意

paintComponent什么时候被调用

1. 当java认为需要重新绘制组件的时候由java调用。
    例如在程序中repaint();或者程序窗口最小化，然后恢复。或者程序窗口被遮挡，又显现的时候。
    注意观察，这个方法是个受保护的方法，这就是说平常并不用管这个方法，这个方法只在需要继承paintComponent(一般是JFrame)的时候，重写方法，（也可以不重新方法，如果不需要改变绘制组件动作的话）。
2. paintComponent()是swing的一个方法，相当于图形版的main()，是会自执行的。如果一个class中有构造函数，则执行顺序是先执行构造函数，再执行这个。

  
