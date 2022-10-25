# 【GUI编程】

> GUI（Graphical User Interface）图形用户界面，又称图形用户接口
>
> Java使用AWT和Swing相关的类可以完成图形化界面编程，其中AWT的全称是抽象窗口工具集(Abstract Window Toolkit),它是sun公司最早提供的GUI库，这个GUI库提供了一些基本功能，但这个GUI库的功能比较有限。
>
> 后来sun公司又提供了Swing库。通过使用AWT和Swing提供的图形化界面组件库，Java的图形化界面编程非常简单，程序只需要依次创建所需的图形组件，并以合适的方式将这些组件组织在一起，就可以开发出非常美观的用户界面。

# 【AWT】

## 简介

> AW（Abstract Window Tollkit）抽象窗口工具包
>
> 它为Java应用程序提供了基本的图形组件。AWT是窗口框架，它从不同平台的窗口系统中抽取出共同组件，当程序运行时，将这些组件的创建和动作委托给程序所在的运行平台。
>
> 简而言之，当使用AWT编写图形界面应用时，程序仅指定了界面组件的位置和行为，并未提供真正的实现，JVM调用操作系统本地的图形界面来创建和平台一致的对等体 。
>
> 使用AWT创建的图形界面应用和所在的运行平台有相同的界面风格 ， 比如在 Windows 操作系统上，它就表现出 Windows 风格; 在 UNIX 操作系统上，它就表现出 UNIX 风格 。 Sun希望采用这种方式来实现"Write Once, Run Anywhere"的目标 。
>
> 然而因为不同平台上的AWT用户界面库存在着不同的bug，所以最后变成了：
>
> “一次编写，到处调试“——《Java核心技术卷一》

## 组件层次

所有和AWT编程相关的类都放在`java.awt`包以及它的子包中，AWT编程中有两个基类：`Component`和`MenuComponent`。

- `Component`：代表一个能以图形化方式显示出来，并可与用户交互的对象，例如`Button`代表一个按钮，`TextField`代表一个文本框等；
- `MenuComponent`：则代表图形界面的菜单组件，包括`MenuBar`(菜单条)、`Menultem` (菜单项)等子类。

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/AWT%E7%BB%84%E4%BB%B6%E7%BB%A7%E6%89%BF%E4%BD%93%E7%B3%BB.png)

其中`Container`是一种特殊的`Component`，它代表一种容器，可以盛装普通的 `Component`。

AWT中还有一个非常重要的接口叫`LayoutManager`，如果一个容器中有多个组件，那么容器就需要使用`LayoutManager`来管理这些组件的布局方式。

## 容器

> Container：容器

### 继承体系

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/Container%E7%BB%A7%E6%89%BF%E4%BD%93%E7%B3%BB.png)

- `Window`是可以 **独立存在** 的顶级窗口,默认使用`BorderLayout`管理其内部组件布局;
- `Panel`可以容纳其他组件，但不能独立存在，它必须内嵌其他容器中使用，默认使用`FlowLayout`管理其内部组件布局；
- `ScrollPane`是 一个带滚动条的容器，它也不能独立存在，默认使用`BorderLayout`管理其内部组件布局；

### 常见API

`Component`作为基类，提供了如下常用的方法来设置组件的大小、位置、可见性等。

|                     方法签名                     |         方法功能         |
| :----------------------------------------------: | :----------------------: |
|           `setLocation(int x, int y)`            |      设置组件的位置      |
|         `setSize(int width, int height)`         |      设置组件的大小      |
| `setBounds(int x, int y, int width, int height)` | 同时设置组件的位置、大小 |
|             `setVisible(Boolean b)`              |    设置该组件的可见性    |

`Container`作为容器根类，提供了如下方法来访问容器中的组件

|                 方法签名                 |                           方法功能                           |
| :--------------------------------------: | :----------------------------------------------------------: |
|     `Component add(Component comp)`      | 向容器中添加其他组件 (该组件既可以是普通组件，也可以 是容器)，并返回被添加的组件 |
| `Component getComponentAt(int x, int y)` |                       返回指定点的组件                       |
|        `int getComponentCount()`         |                    返回该容器内组件的数量                    |
|      `Component[] getComponents()`       |                    返回该容器内的所有组件                    |

### 容器演示

#### Window

```java
public class WindowDemo {
    public static void main(String[] args) {
        // 1.创建窗口对象
        Frame frame = new Frame("这是第一个窗口容器");
        // 2.设置窗口的位置和大小
        frame.setBounds(100,100,500,300);
        // 3.设置窗口可见
        frame.setVisible(true);
    }
}
```

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/FrameDemo.jpg)

#### Panel

> Panel不能独立存在，需要依附Window存在

```java
public class PanelDemo {
    public static void main(String[] args) {
        // 1.创建Frame容器对象
        Frame frame = new Frame("这里测试Panel");
        // 2.创建Panel容器对象
        Panel panel = new Panel();
        // 3.往Panel容器中添加组件
        panel.add(new TextField("这是一个测试文本"));
        panel.add(new Button("这是一个测试按钮"));
        // 4.把Panel添加到Frame中
        frame.add(panel);
        // 5.设置Frame的位置和大小
        frame.setBounds(30, 30, 500, 300);
        // 6.设置Frame可见
        frame.setVisible(true);
    }
}
```

> 由于IDEA默认使用utf-8进行编码，但是当前我们执行代码是是在windows系统上，而windows操作系统的默认编码是gbk，所以会乱码，如果出现了乱码，那么只需要在运行当前代码前，设置一个jvm参数`-Dfile.encoding=gbk`即可
>
> IDEA在`Run --> Edit  Configuraitons`的`VM options`中添加参数

#### ScrollPane

```java
public class ScrollPaneDemo {
    public static void main(String[] args) {
        // 1.创建Frame窗口对象
        Frame frame = new Frame("这里测试ScrollPane");
        // 2.创建ScrollPane对象，并且指定默认有滚动条
        ScrollPane scrollPane = new ScrollPane(ScrollPane.SCROLLBARS_ALWAYS);
        // 3.往ScrollPane中添加组件
        scrollPane.add(new TextField("这是测试文本"));
        scrollPane.add(new Button("这是测试按钮"));
        // 4.把ScrollPane添加到Frame中
        frame.add(scrollPane);
        // 5.设置Frame的位置及大小
        frame.setBounds(30, 30, 500, 300);
        // 6.设置Frame可见
        frame.setVisible(true);
    }
}
```

![image-20220429154605167](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429154605167.png)

## 布局管理器

> LayoutManager：布局管理器
>
> 手动为组件设置位置和大小时，会造成程序的不通用性，例如：
>
> ```java
> Label label = new Label("你好，世界");
> ```
>
> 创建了一个lable组件，很多情况下，需要让lable组件的宽高和“你好，世界”这个字符串自身的宽高一致，这种大小称为**最佳大小**。
>
> 由于操作系统存在差异，例如在windows上，我们要达到这样的效果，需要把该Lable组件的宽和高分别设置为100px,20px,但是在Linux操作系统上，可能需要把Lable组件的宽和高分别设置为120px，24px，才能达到同样的效果。
>
> 如果要让我么的程序在不同的操作系统下，都有相同的使用体验，那么手动设置组件的位置和大小，无疑是一种灾难，因为有太多的组件，需要分别设置不同操作系统下的大小和位置。为了解决这个问题，Java提供了LayoutManager布局管理器，可以根据运行平台来自动调整组件大小，程序员不用再手动设置组件的大小和位置了，只需要为容器选择合适的布局管理器即可。

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/%E5%B8%B8%E7%94%A8%E5%B8%83%E5%B1%80%E7%AE%A1%E7%90%86%E5%99%A8.png)

### FlowLayout

> 在FlowLayout布局管理器中，组件像水流一样向某方向流动 (排列) ，遇到障碍(边界)就折回，重头开始排列。
>
> 在默认情况下，FlowLayout 布局管理器从左向右排列所有组件，遇到边界就会折回下一行重新开始。

|                 构造方法                  |                           方法功能                           |
| :---------------------------------------: | :----------------------------------------------------------: |
|              `FlowLayout()`               | 使用默认的对齐方式及默认的垂直间距、水平间距创建 FlowLayout 布局管理器 |
|          `FlowLayout(int align)`          | 使用指定的对齐方式及默认的垂直间距、水平间距创建 FlowLayout 布局管理器 |
| `FlowLayout(int align,int hgap,int vgap)` | 使用指定的对齐方式及指定的垂直问距、水平间距创建FlowLayout 布局管理器 |

> FlowLayout中组件的排列方向(从左向右、从右向左、从中间向两边等)，
>
> 该参数应该使用FlowLayout类的静态常量（默认左对齐） : 
>
> - FlowLayout. LEFT：左对齐
> - FlowLayout. CENTER ：中间对齐
> - FlowLayout. RIGHT：右对齐
>
> FlowLayout 中组件中间距通过整数设置，单位是像素，默认是5个像素。

**代码演示：**

```java
public class FlowLayoutDemo {
    public static void main(String[] args) {
        //1.创建Frame对象
        Frame frame = new Frame("这里测试FlowLayout");
        //2.修改Frame容器的布局管理器为FlowLayout
        frame.setLayout(new FlowLayout(FlowLayout.LEFT, 20, 20));
        //3.往Frame中添加100个button
        for (int i = 0; i < 100; i++) {
            frame.add(new Button("按钮" + i));
        }
        //4.设置Frame为最佳大小
        frame.pack();
        //5.设置Frame可见
        frame.setVisible(true);
    }
}
```

![image-20220429160214396](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429160214396.png)

### BorderLayout

> BorderLayout 将容器分为EAST、SOUTH、WEST、NORTH、CENTER五个区域，普通组件可以被放置在这5个区域的任意一个中。BorderLayout布局管理器的布局示意图如图：
>
> ![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/BorderLayout.png)
>
> 当改变使用BorderLayout的容器大小时，NORTH、SOUTH和CENTER区域水平调整，而EAST、WEST和CENTER区域垂直调整
>
> 使用BorderLayout 有如下两个注意点:
>
> 1. 当向使用 BorderLayout 布局管理器的容器中添加组件时，需要指定要添加到哪个区域中。如果没有指定添加到哪个区域中，则默认添加到中间区域中；
> 2. 如果向同一个区域中添加多个组件时，后放入的组件会覆盖先放入的组件；

|              构造方法              |                          方法功能                           |
| :--------------------------------: | :---------------------------------------------------------: |
|          `BorderLayout()`          | 使用默认的水平间距、垂直间距创建 BorderLayout 布局管理器 。 |
| `BorderLayout(int hgap,int vgap):` | 使用指定的水平间距、垂直间距创建 BorderLayout 布局管理器。  |

**代码演示：**

```java
public class BorderLayoutDemo {
    public static void main(String[] args) {
        // 1.创建Frame对象
        Frame frame = new Frame("这里测试BorderLayout");
        // 2.指定Frame对象的布局管理器为BorderLayout
        frame.setLayout(new BorderLayout(30,5));
        // 3.往Frame指定东西南北中各添加一个按钮组件
        frame.add(new Button("东侧按钮"), BorderLayout.EAST);
        frame.add(new Button("西侧按钮"), BorderLayout.WEST);
        frame.add(new Button("南侧按钮"), BorderLayout.SOUTH);
        frame.add(new Button("北侧按钮"), BorderLayout.NORTH);
        frame.add(new Button("中间按钮"), BorderLayout.CENTER);
        // 4.设置Frame为最佳大小
        frame.pack();
        // 5.设置Frame可见
        frame.setVisible(true);
    }
}
```

![image-20220429162454139](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429162454139.png)

> 如果不往某个区域中放入组件，那么该区域不会空白出来，而是会被其他区域占用

### GridLayout

> GridLayout布局管理器将容器分割成纵横线分隔的网格，每个网格所占的区域大小相同。
>
> 当向使用GridLayout布局管理器的容器中添加组件时，默认从左向右、从上向下依次添加到每个网格中。与FlowLayout不同的是，放置在GridLayout布局管理器中的各组件的大小由组件所处的区域决定(每个组件将自动占满整个区域) 。    

|                     构造方法                      |                           方法功能                           |
| :-----------------------------------------------: | :----------------------------------------------------------: |
|         `GridLayout(int rows,in t cols)`          | 采用指定的行数、列数，以及默认的横向间距、纵向间距将容器分割成多个网格 |
| `GridLayout(int rows,int cols,int hgap,int vgap)` | 采用指定的行数、列数，以及指定的横向间距、纵向间距将容器分割成多个网格。 |

**代码演示：** 使用Frame+Panel，配合FlowLayout和GridLayout完成一个计算器效果。

```java
public class GridLayoutDemo {
    public static void main(String[] args) {
        // 1.创建Frame对象，并且标题设置为计算器
        Frame frame = new Frame("计算器");
        // 2.创建一个Panel对象，并放入一个TextField组件
        Panel p1 = new Panel();
        p1.add(new TextField(30));
        // 3.把上述的Panel放入到Frame的北侧区域
        frame.add(p1, BorderLayout.NORTH);
        // 4.创建一个Panel对象，并且设置其布局管理器为GridLayout
        Panel p2 = new Panel();
        p2.setLayout(new GridLayout(3,5));
        // 5.往上述Panel中，放置计算器键盘
        for (int i = 0; i < 10; i++) {
            p2.add(new Button(i + ""));
        }
        p2.add(new Button("+"));
        p2.add(new Button("-"));
        p2.add(new Button("*"));
        p2.add(new Button("/"));
        p2.add(new Button("."));
        // 6.把上述Panel添加到Frame的中间区域
        frame.add(p2);
        // 7.设置Frame为最佳大小
        frame.pack();
        // 8.设置Frame可见
        frame.setVisible(true);
    }
}
```

![image-20220429163024685](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429163024685.png)

### GridBagLayout

> GridBagLayout布局管理器的功能最强大，但也最复杂，与GridLayout布局管理器不同的是，在GridBagLayout布局管理器中，一个组件可以跨越一个或多个网格，并可以设置各网格的大小互不相同，从而增加了布局的灵活性，当窗口的大小发生变化，GridBagLayout 布局管理器也可以准确地控制窗口各部分的拉伸 。
>
> ![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/GridBagLayout.png)
>
> 由于在GridBagLayout 布局中，每个组件可以占用多个网格，此时，我们往容器中添加组件的时候，就需要具体的控制每个组件占用多少个网格，Java提供的GridBagConstaints类，与特定的组件绑定，可以完成具体大小和跨越性的设置
>
> **太过复杂，不建议使用，Swing提供了更好的方法**

**GridBagConstraints API:**

|   成员变量   |                             含义                             |
| :----------: | :----------------------------------------------------------: |
|   `gridx`    |      设置受该对象控制的GUI组件左上角所在网格的横向索引       |
|   `gridy`    |      设置受该对象控制的GUI组件左上角所在网格的纵向索引       |
| `gridwidth`  | 设置受该对象控制的 GUI 组件横向跨越多少个网格,如果属性值为 GridBagContraints.REMAIND,则表明当前组件是横向最后一个组件，如果属性值为GridBagConstraints.RELATIVE,表明当前组件是横向倒数第二个组件。 |
| `gridheight` | 设置受该对象控制的 GUI 组件纵向跨越多少个网格，如果属性值为 GridBagContraints.REMAIND,则表明当前组件是纵向最后一个组件，如果属性值为GridBagConstraints.RELATIVE,表明当前组件是纵向倒数第二个组件。 |
|    `fill`    | 当"显示区域"大于"组件"的时候,如何调整组件 ：<br/> GridBagConstraints.NONE : GUI 组件不扩大<br/> GridBagConstraints.HORIZONTAL: GUI 组件水平扩大 以 占据空白区域<br/> GridBagConstraints.VERTICAL: GUI 组件垂直扩大以占据空白区域<br/> GridBagConstraints.BOTH: GUI 组件水平 、 垂直同时扩大以占据空白区域. |
|   `ipadx`    | 设置受该对象控制的 GUI 组件横向内部填充的大小，即 在该组件最小尺寸的基础上还需要增大多少. |
|   `ipady`    | 设置受该对象控制的 GUI 组件纵向内部填充的大小，即 在该组件最小尺寸的基础上还需要增大多少. |
|   `insets`   | 设置受该对象控制 的 GUI 组件的 外部填充的大小 ， 即该组件边界和显示区 域边界之间的 距离 . |
|  `weightx`   | 设置受该对象控制 的 GUI 组件占据多余空间的水平比例， 假设某个容器 的水平线上包括三个 GUI 组件， 它们的水平增加比例分别是 1 、 2 、 3 ， 但容器宽度增加 60 像素 时，则第一个组件宽度增加 10 像素 ， 第二个组件宽度增加 20 像素，第三个组件宽度增加 30 像 素。 如 果其增 加比例为 0 ， 则 表示不会增加 。 |
|  `weighty`   |      设置受该对象控制 的 GUI 组件占据多余空间的垂直比例      |
|   `anchor`   | 设置受该对象控制 的 GUI 组件在其显示区域中的定位方式:<br/>GridBagConstraints .CENTER (中间 )<br/>GridBagConstraints.NORTH (上中 ) <br/>GridBagConstraints.NORTHWEST (左上角)<br/>GridBagConstraints.NORTHEAST (右上角)<br/>GridBagConstraints.SOUTH (下中) <br/>GridBagConstraints.SOUTHEAST (右下角)<br/>GridBagConstraints.SOUTHWEST (左下角)<br/>GridBagConstraints.EAST (右中) <br/>GridBagConstraints.WEST (左中) |

**GridBagLayout使用步骤：**

- 创建GridBagLaout布局管理器对象，并给容器设置该布局管理器对象；


- 创建GridBagConstraints对象，并设置该对象的控制属性：


	gridx: 用于指定组件在网格中所处的横向索引；
	gridy: 用于执行组件在网格中所处的纵向索引；
	gridwidth: 用于指定组件横向跨越多少个网格；
	gridheight: 用于指定组件纵向跨越多少个网格；

- 调用GridBagLayout对象的`setConstraints(Component c, GridBagConstraints gbc)`方法，把即将要添加到容器中的组件`c`和`GridBagConstraints`对象关联起来；


- 把组件添加到容器中；

**代码演示：**

```java
public class GridBagLayoutDemo {
    public static void main(String[] args) {
        // 1.创建Frame对象
        Frame frame = new Frame("这里是GridBagLayout测试");
        // 2.创建GridBagLayout对象
        GridBagLayout gbl = new GridBagLayout();
        // 3.把Frame对象的布局管理器设置为GridBagLayout
        frame.setLayout(gbl);
        // 4.创建GridBagConstraints对象
        GridBagConstraints gbc = new GridBagConstraints();
        // 5.创建容量为10的Button数组
        Button[] bs = new Button[10];
        // 6.遍历数组，初始化每一个Button
        for (int i = 0; i < bs.length; i++) {
            bs[i] = new Button("按钮" + (i + 1));
        }
        // 7.设置所有的GridBagConstraints对象的fill属性为GridBagConstraints.BOTH,当有空白区域时，组件自动扩大占满空白区域
        gbc.fill = GridBagConstraints.BOTH;
        // 8.设置GridBagConstraints对象的weightx设置为1,表示横向扩展比例为1
        gbc.weightx = 1;
        // 9.往frame中添加数组中的前3个Button
        addComponent(frame, bs[0], gbl, gbc);
        addComponent(frame, bs[1], gbl, gbc);
        addComponent(frame, bs[2], gbl, gbc);
        // 10.把GridBagConstraints的gridwidth设置为GridBagConstraints.REMAINDER,则表明当前组件是横向最后一个组件
        gbc.gridwidth = GridBagConstraints.REMAINDER;
        // 11.把button数组中第四个按钮添加到frame中
        addComponent(frame, bs[3], gbl, gbc);
        // 12.把GridBagConstraints的weighty设置为1，表示纵向扩展比例为1
        gbc.weighty = 1;
        // 13.把button数组中第5个按钮添加到frame中
        addComponent(frame, bs[4], gbl, gbc);
        // 14.把GridBagConstaints的gridheight和gridwidth设置为2，表示纵向和横向会占用两个网格
        gbc.gridheight = 2;
        gbc.gridwidth = 2;
        // 15.把button数组中第6个按钮添加到frame中
        addComponent(frame, bs[5], gbl, gbc);
        // 16.把GridBagConstaints的gridheight和gridwidth设置为1，表示纵向会占用1个网格
        gbc.gridwidth = 1;
        gbc.gridheight = 1;
        // 17.把button数组中第7个按钮添加到frame中
        addComponent(frame, bs[6], gbl, gbc);
        // 18.把GridBagConstraints的gridwidth设置为GridBagConstraints.REMAINDER,则表明当前组件是横向最后一个组件
        gbc.gridwidth = GridBagConstraints.REMAINDER;
        // 19.把button数组中第8个按钮添加到frame中
        addComponent(frame, bs[7], gbl, gbc);
        // 20.把GridBagConstaints的gridwidth设置为1，表示纵向会占用1个网格
        gbc.gridwidth = 1;
        // 21.把button数组中第9、10个按钮添加到frame中
        addComponent(frame, bs[8], gbl, gbc);
        addComponent(frame, bs[9], gbl, gbc);
        // 22.设置frame为最佳大小
        frame.pack();
        // 23.设置frame可见
        frame.setVisible(true);
    }

    public static void addComponent(Container container, Component c, GridBagLayout gridBagLayout, GridBagConstraints gridBagConstraints) {
        gridBagLayout.setConstraints(c, gridBagConstraints);
        container.add(c);
    }
}
```

![image-20220429164035062](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429164035062.png)

### CardLayout

> CardLayout布局管理器以时间而非空间来管理它里面的组件，它将加入容器的所有组件看成一叠卡片（每个卡片其实就是一个组件），每次只有最上面的那个Component才可见。就好像一副扑克牌，它们叠在一起，每次只有最上面的一张扑克牌才可见

|              方法名称               |                           方法功能                           |
| :---------------------------------: | :----------------------------------------------------------: |
|           `CardLayout()`            |              创建默认的 CardLayout 布局管理器。              |
|   `CardLayout(int hgap,int vgap)`   | 通过指定卡片与容器左右边界的间距 (hgap) 、上下边界 (vgap) 的间距来创建 CardLayout 布局管理器. |
|      `first(Container target)`      |                显示target 容器中的第一张卡片.                |
|      `last(Container target)`       |               显示target 容器中的最后一张卡片.               |
|    `previous(Container target)`     |                显示target 容器中的前一张卡片.                |
|      `next(Container target)`       |                显示target 容器中的后一张卡片.                |
| `show(Container taget,String name)` |              显 示 target 容器中指定名字的卡片.              |

**代码演示：**

```java
public class CardLayoutDemo {
    public static void main(String[] args) {
        // 1.创建Frame对象
        Frame frame = new Frame("这里测试CardLayout");
        // 2.创建一个String数组，存储不同卡片的名字
        String[] names = {"第一张","第二张","第三张","第四张","第五张"};
        // 3.创建一个Panel容器p1，并设置其布局管理器为CardLayout,用来存放多张卡片
        CardLayout cardLayout = new CardLayout();
        Panel p1 = new Panel();
        p1.setLayout(cardLayout);
        // 4.往p1中存储5个Button按钮，名字从String数组中取
        for (int i = 0; i < 5; i++) {
            p1.add(names[i],new Button(names[i]));
        }
        // 5.创建一个Panel容器p2,用来存储5个按钮，完成卡片的切换
        Panel p2 = new Panel();
        // 6.创建5个按钮，并给按钮设置监听器
        ActionListener listener = new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String command = e.getActionCommand();
                switch (command){
                    case "上一张":
                        cardLayout.previous(p1);
                        break;
                    case "下一张":
                        cardLayout.next(p1);
                        break;
                    case "第一张":
                        cardLayout.first(p1);
                        break;
                    case "最后一张":
                        cardLayout.last(p1);
                        break;
                    case "第三张":
                        cardLayout.show(p1,"第三张");
                        break;
                }
            }
        };
        Button b1 = new Button("上一张");
        Button b2 = new Button("下一张");
        Button b3 = new Button("第一张");
        Button b4 = new Button("最后一张");
        Button b5 = new Button("第三张");
        b1.addActionListener(listener);
        b2.addActionListener(listener);
        b3.addActionListener(listener);
        b4.addActionListener(listener);
        b5.addActionListener(listener);
        // 7.把5个按钮添加到p2中
        p2.add(b1);
        p2.add(b2);
        p2.add(b3);
        p2.add(b4);
        p2.add(b5);
        // 8.把p1添加到frame的中间区域
        frame.add(p1);
        // 9.把p2添加到frame的底部区域
        frame.add(p2,BorderLayout.SOUTH);
        //10设置frame最佳大小并可见
        frame.pack();
        frame.setVisible(true);
    }
}
```

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/CardLayout.jpg)

### BoxLayout

> 为了简化开发，Swing引入了 一个新的布局管理器:BoxLayout
>
> BoxLayout 可以在垂直和水平两个方向上摆放GUI组件，BoxLayout提供了如下一个简单的构造器:

|                方法名称                 |                           方法功能                           |
| :-------------------------------------: | :----------------------------------------------------------: |
| `BoxLayout(Container target, int axis)` | 指定创建基于target容器的BoxLayout布局管理器，该布局管理器里的组件按axis方向排列。其中axis有BoxLayout.X_AXIS(横向)和 BoxLayout.Y _AXIS(纵向)两个方向。 |

**演示代码：**

```java
public class BoxLayoutDemo1 {
    public static void main(String[] args) {
        // 1.创建Frame对象
        Frame frame = new Frame("这里测试BoxLayout");
        // 2.创建BoxLayout布局管理器，并指定容器为上面的frame对象，指定组件排列方向为纵向
        BoxLayout boxLayout = new BoxLayout(frame, BoxLayout.Y_AXIS);
        frame.setLayout(boxLayout);
        // 3.往frame对象中添加两个按钮
        frame.add(new Button("按钮1"));
        frame.add(new Button("按钮2"));
        // 4.设置frame最佳大小，并可见
        frame.pack();
        frame.setVisible(true);
    }
}
```

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/BoxLayout1.jpg)

> 在java.swing包中，提供了一个新的容器Box，该容器的默认布局管理器就是BoxLayout,大多数情况下，使用Box容器去容纳多个GUI组件，然后再把Box容器作为一个组件，添加到其他的容器中，从而形成整体窗口布局。

| 方法名称                           | 方法功能                           |
| :--------------------------------- | ---------------------------------- |
| `static Box createHorizontalBox()` | 创建一个水平排列组件的 Box 容器 。 |
| `static Box createVerticalBox()`   | 创建一个垂直排列组件的 Box 容器 。 |

**演示代码：**

```java
public class BoxLayoutDemo2 {
    public static void main(String[] args) {
        // 1.创建Frame对象
        Frame frame = new Frame("这里测试BoxLayout");
        // 2.创建一个横向的Box,并添加两个按钮
        Box hBox = Box.createHorizontalBox();
        hBox.add(new Button("水平按钮一"));
        hBox.add(new Button("水平按钮二"));
        // 3.创建一个纵向的Box，并添加两个按钮
        Box vBox = Box.createVerticalBox();
        vBox.add(new Button("垂直按钮一"));
        vBox.add(new Button("垂直按钮二"));
        // 4.把box容器添加到frame容器中
        frame.add(hBox,BorderLayout.NORTH);
        frame.add(vBox);
        // 5.设置frame最佳大小并可见
        frame.pack();
        frame.setVisible(true);
    }
}
```

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/boxlayoutdemo2.jpg)

> 按钮之间没有间隔，可以把间隔作为一个组件添加到组件之间，只不过该组件没有内容，仅仅起到一种分隔的作用

**Box类中，提供了5个方便的静态方法来生成这些间隔组件：**

|                      方法名称                       |                           方法功能                           |
| :-------------------------------------------------: | :----------------------------------------------------------: |
|      `static Component createHorizontalGlue()`      |       创建一条水平 Glue (可在两个方向上同时拉伸的间距)       |
|       `static Component createVerticalGlue()`       |      创建一条垂直 Glue (可在两个方向上同时拉伸的间距）       |
| `static Component createHorizontalStrut(int width)` | 创建一条指定宽度(宽度固定了，不能拉伸)的水平Strut (可在垂直方向上拉伸的间距) |
| `static Component createVerticalStrut(int height)`  | 创建一条指定高度(高度固定了，不能拉伸)的垂直Strut (可在水平方向上拉伸的间距) |

## 其他组件

### 常用组件

> 这些AWT组件的用法比较简单，可以查阅API文档来获取它们各自的构方法、成员方法等详细信息。

|     组件名      | 功能                                                         |
| :-------------: | :----------------------------------------------------------- |
|    `Button`     | 按钮                                                         |
|    `Canvas`     | 用于绘图的画布                                               |
|   `Checkbox`    | 复选框组件（也可当做单选框组件使用）                         |
| `CheckboxGroup` | 用于将多个Checkbox组件组合成一组，一组Checkbox组件将只有一个可以被选中，即全部变成单选框组件 |
|    `Choice`     | 下拉选择框                                                   |
|     `Frame`     | 窗口，在GUI程序里通过该类创建窗口                            |
|     `Label`     | 标签类，用于放置提示性文本                                   |
|     `List`      | 列表表框组件，可以添加多项条目                               |
|     `Panel`     | 不能单独存在基本容器类，必须放到其他容器中                   |
|   `Scrollbar`   | 滑动条组件。如果需要用户输入位于某个范围的值，就可以使用滑动条组件，比如调色板中设置 RGB 的三个值所用的滑动条。当创建一个滑动条时，必须指定它的方向、初始值、 滑块的大小、最小值和最大值。 |
|  `ScrollPane`   | 带水平及垂直滚动条的容器组件                                 |
|   `TextArea`    | 多行文本域                                                   |
|   `TextField`   | 单行文本框                                                   |

**案例：**

![image-20220429190253052](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429190253052.png)

```java
public class BasicComponentDemo {
    Frame frame = new Frame("这里测试基本组件");
    // 定义一个按钮
    Button ok = new Button("确认");
    // 定义一个复选框
    CheckboxGroup cbg = new CheckboxGroup();
    // 定义一个单选框，初始化处于被选中状态，并添加到cbg组
    Checkbox male = new Checkbox("男", cbg, true);
    // 定义一个单选框，初始处于未被选中状态,并添加到cbg组中
    Checkbox female = new Checkbox("女", cbg, false);
    // 定义一个单选框，初始处于未被选中状态
    Checkbox married = new Checkbox("是否已婚？", false);
    // 定义一个下拉选择框
    Choice colorChooser = new Choice();
    // 定义一个列表选择框
    List colorList = new List(6, true);
    // 定义一个5行，20列的多行文本域
    TextArea ta = new TextArea(5, 20);
    // 定义一个50列的单行文本域
    TextField tf = new TextField(50);

    public void init() {
        // 往下拉选择框中添加内容
        colorChooser.add("红色");
        colorChooser.add("绿色");
        colorChooser.add("蓝色");
        // 往列表选择框中添加内容
        colorList.add("红色");
        colorList.add("绿色");
        colorList.add("蓝色");
        // 创建一个装载按钮和文本框的Panel容器
        Panel bottom = new Panel();
        bottom.add(tf);
        bottom.add(ok);
        // 把bottom添加到Frame的底部
        frame.add(bottom,BorderLayout.SOUTH);
        // 创建一个Panel容器，装载下拉选择框，单选框和复选框
        Panel checkPanel = new Panel();
        checkPanel.add(colorChooser);
        checkPanel.add(male);
        checkPanel.add(female);
        checkPanel.add(married);
        // 创建一个垂直排列的Box容器，装载多行文本域和checkPanel
        Box topLeft = Box.createVerticalBox();
        topLeft.add(ta);
        topLeft.add(checkPanel);
        // 创建一个水平排列的Box容器，装载topLeft和列表选择框
        Box top = Box.createHorizontalBox();
        top.add(topLeft);
        top.add(colorList);
        // 将top添加到frame的中间区域
        frame.add(top);
        // 设置frame最佳大小并可见
        frame.pack();
        frame.setVisible(true);
    }
    public static void main(String[] args) {
        new BasicComponentDemo().init();
    }
}
```

### 对话框

> 对话框：Dialog
>
> Dialog是Window 类的子类，是一个容器类，属于特殊组件。 
>
> 对话框是可以独立存在的顶级窗口，因此用法与普通窗口的用法几乎完全一样，但是使用对话框需要注意下面两点：
>
> - 对话框通常依赖于其他窗口，就是通常需要有一个父窗口；
> - 对话框有非模式(non-modal)和模式(modal)两种，当某个模式对话框被打开后，该模式对话框总是位于它的父窗口之上，在模式对话框被关闭之前，父窗口无法获得焦点。

**演示代码**

![image-20220429191116449](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429191116449.png)

```java
public class DialogDemo {
    public static void main(String[] args) {
        Frame frame = new Frame("这里测试Dialog");
        Dialog d1 = new Dialog(frame, "模式对话框", true);
        Dialog d2 = new Dialog(frame, "非模式对话框", false);
        Button b1 = new Button("打开模式对话框");
        Button b2 = new Button("打开非模式对话框");
        // 设置对话框的大小和位置
        d1.setBounds(20,30,300,400);
        d2.setBounds(20,30,300,400);
        // 给b1和b2绑定监听事件
        b1.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                d1.setVisible(true);
            }
        });
        b2.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                d2.setVisible(true);
            }
        });
        // 把按钮添加到frame中
        frame.add(b1);
        frame.add(b2,BorderLayout.SOUTH);
        // 设置frame最佳大小并可见
        frame.pack();
        frame.setVisible(true);
    }
}
```

### 文件对话框

> 文件对话框：FileDialog
>
> FileDialog代表一个文件对话框，用于打开或者保存文件，需要注意的是FileDialog无法指定模态或者非模态，这是因为 FileDialog 依赖于运行平台的实现，如果运行平台的文件对话框是模态的，那么FileDialog也是模态的，否则就是非模态的 。

|                      方法名称                      |                           方法功能                           |
| :------------------------------------------------: | :----------------------------------------------------------: |
| `FileDialog(Frame parent, String title, int mode)` | 创建一个文件对话框：<br/>parent:指定父窗口<br/>title:对话框标题<br/>mode:文件对话框类型，如果指定为`FileDialog.LOAD`，用于打开文件，如果指定为`FileDialog.SAVE`,用于保存文件 |
|              `String getDirectory()`               |                获取被打开或保存文件的绝对路径                |
|                 `String getFile()`                 |                 获取被打开或保存文件的文件名                 |

**演示代码：**

![image-20220429234942342](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429234942342.png)

```java
public class FileDialogTest {
    public static void main(String[] args) {
        Frame frame = new Frame("这里测试FileDialog");
        FileDialog d1 = new FileDialog(frame, "选择需要加载的文件", FileDialog.LOAD);
        FileDialog d2 = new FileDialog(frame, "选择需要保存的文件", FileDialog.SAVE);
        Button b1 = new Button("打开文件");
        Button b2 = new Button("保存文件");
        // 给按钮添加事件，练习一下lambda表达式
        b1.addActionListener(e -> {
            d1.setVisible(true);
            //打印用户选择的文件路径和名称
            System.out.println("用户选择的文件路径:" + d1.getDirectory());
            System.out.println("用户选择的文件名称:" + d1.getFile());
        });

        System.out.println("-------------------------------");
        b2.addActionListener(e -> {
            d2.setVisible(true);
            //打印用户选择的文件路径和名称
            System.out.println("用户选择的文件路径:" + d2.getDirectory());
            System.out.println("用户选择的文件名称:" + d2.getFile());
        });
        //添加按钮到frame中
        frame.add(b1);
        frame.add(b2, BorderLayout.SOUTH);
        //设置frame最佳大小并可见
        frame.pack();
        frame.setVisible(true);
    }
}
```

## 事件处理

> 通过放置各种组件，可以得到丰富多彩的图形界面，但这些界面还不能响应用户的任何操作：比如单击前面所有窗口右上角的“X”按钮，但窗口依然不会关闭
>
> 因为在 AWT 编程中，所有用户的操作，都必须都需要经过一套事件处理机制来完成，而 Frame 和组件本身并没有事件处理能力 。

### 事件处理机制

> **定义：** 当在某个组件上发生某些操作的时候，会自动的触发一段代码的执行。
>
> **4个重要的概念：**
>
> - **事件源(Event Source):** 操作发生的场所，通常指某个组件，例如按钮、窗口等；
> - **事件(Event):** 在事件源上发生的操作可以叫做事件，GUI会把事件都封装到一个Event对象中，如果需要知道该事件的详细信息，就可以通过Event对象来获取。
> - **事件监听器(Event Listener):** 当在某个事件源上发生了某个事件，事件监听器就可以对这个事件进行处理。
>
> - **注册监听:** 把某个事件监听器(A)通过某个事件(B)绑定到某个事件源(C)上，当在事件源C上发生了事件B之后，那么事件监听器A的代码就会自动执行。
>
> ![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/%E4%BA%8B%E4%BB%B6%E5%A4%84%E7%90%86%E6%9C%BA%E5%88%B6.png)

**使用步骤：**

1. 创建事件源组件对象；
2. 自定义类，实现`XxxListener`接口，重写方法；
3. 创建事件监听器对象(自定义类对象)
4. 调用事件源组件对象的`addXxxListener`方法完成注册监听

**代码演示：**

完成下图效果，点击确定按钮，在单行文本域内显示`hello world`

![image-20220430000524938](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220430000524938.png)

```java
public class EventDemo1 {
    Frame frame = new Frame("这里测试事件处理");
    // 事件源
    Button button = new Button("确定");
    // 文本域
    TextField tf = new TextField(30);
    public void init() {
        // 注册监听，这里实现了ActionListener接口
        button.addActionListener(e -> {
            System.out.println("用户点击了确定按钮");
            tf.setText("hello world");
        });
        // 添加组件到frame中
        frame.add(tf);
        frame.add(button, BorderLayout.SOUTH);
        // 设置frame最佳大小并可见
        frame.pack();
        frame.setVisible(true);
    }
    public static void main(String[] args) {
        new EventDemo1().init();
    }
}
```

### 常见事件

**AWT把事件分为了两大类：**

- **低级事件:** 这类事件是基于某个特定动作的事件。比如进入、点击、拖放等动作的鼠标事件，再比如得到焦点和失去焦点等焦点事件。

  |       事件       |                           触发时机                           |
  | :--------------: | :----------------------------------------------------------: |
  | `ComponentEvent` | 组件事件，当组件尺寸发生变化、位置发生移动、显示/隐藏状态发生改变时触发该事件。 |
  | `ContainerEvent` |   容器事件，当容器里发生添加组件、删除组件时触发该事件 。    |
  |  `WindowEvent`   | 窗口事件，当窗口状态发生改变 ( 如打开、关闭、最大化、最 小化)时触发该事件 。 |
  |   `FocusEvent`   |      焦点事件，当组件得到焦点或失去焦点时触发该事件 。       |
  |    `KeyEvent`    |       键盘事件，当按键被按下、松开、单击时触发该事件。       |
  |   `MouseEvent`   | 鼠标事件，当进行单击、按下、松开、移动鼠标等动作时触发该事件。 |
  |   `PaintEvent`   | 组件绘制事件，该事件是一个特殊的事件类型，当GUI组件调用 `update/paint`方法来呈现自身时触发该事件，该事件并非专用于事件处理模型 。 |

- **高级事件:** 这类事件并不会基于某个特定动作，而是根据功能含义定义的事件

  |       事件       |                           触发时机                           |
  | :--------------: | :----------------------------------------------------------: |
  |  `ActionEvent`   | 动作事件，当按钮、菜单项被单击，在TextField中按Enter键时触发 |
  | `AjustmentEvent` |     调节事件，在滑动条上移动滑块以调节数值时触发该事件。     |
  |   `ItemEvent`    |   选项事件，当用户选中某项，或取消选中某项时触发该事件 。    |
  |   `TextEvent`    |   文本事件，当文本框、文本域里的文本发生改变时触发该事件。   |

### 常见监听器

不同的事件需要使用不同的监听器监听，不同的监听器需要实现不同的监听器接口，当指定事件发生后，事件监听器就会调用所包含的事件处理器(实例方法)来处理事件。

| 事件类别          | 描述信息                 | 监听器接口名          |
| ----------------- | ------------------------ | --------------------- |
| `ActionEvent`     | 激活组件                 | `ActionListener`      |
| `ItemEvent`       | 选择了某些项目           | `ItemListener`        |
| `MouseEvent`      | 鼠标移动                 | `MouseMotionListener` |
| `MouseEvent`      | 鼠标点击等               | `MouseListener`       |
| `KeyEvent`        | 键盘输入                 | `KeyListener`         |
| `FocusEvent`      | 组件收到或失去焦点       | `FocusListener`       |
| `AdjustmentEvent` | 移动了滚动条等组件       | `AdjustmentListener`  |
| `ComponentEvent`  | 对象移动缩放显示隐藏等   | `ComponentListener`   |
| `WindowEvent`     | 窗口收到窗口级事件       | `WindowListener`      |
| `ContainerEvent`  | 容器中增加删除了组件     | `ContainerListener`   |
| `TextEvent`       | 文本字段或文本区发生改变 | `TextListener`        |

### 代码案例

**案例1：**

​	通过ContainerListener监听Frame容器添加组件；

​	通过TextListener监听TextFiled内容变化；

​	通过ItemListener监听Choice条目选中状态变化；

![image-20220430001835859](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220430001835859.png)

```java
public class ListenerDemo1 {
    public static void main(String[] args) {
        Frame frame = new Frame("这里测试监听器");
        // 创建一个单行文本域
        TextField tf = new TextField(30);
        // 给文本域添加TextListener，监听内容的变化
        tf.addTextListener(new TextListener() {
            @Override
            public void textValueChanged(TextEvent e) {
                System.out.println("当前内容："+tf.getText());;
            }
        });
        // 给frame注册ContainerListener监听器，监听容器中组件的添加
        frame.addContainerListener(new ContainerAdapter() {
            @Override
            public void componentAdded(ContainerEvent e) {
                Component child = e.getChild();
                System.out.println("容器中添加了新组件："+child);
            }
        });
        // 添加tf到frame
        frame.add(tf);
        // 设置frame最佳大小并可见
        frame.pack();
        frame.setVisible(true);
    }
}
```

**案例2：**

​	给Frame设置WindowListner，监听用户点击X的动作，如果用户点击X，则关闭当前窗口

![image-20220430001950703](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220430001950703.png)

```java
public class ListenerDemo2 {
    public static void main(String[] args) {
        Frame frame = new Frame("这里测试WindowListener");
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
        frame.setBounds(200,200,500,300);
        frame.setVisible(true);
    }
}
```

## 菜单组件

> 构建GUI界面，其实就是把一些GUI的组件，按照一定的布局放入到容器中展示就可以了。
>
> 在实际开发中，除了主界面，还有一类比较重要的内容就是菜单相关组件，可以通过菜单相关组件很方便的使用特定的功能。
>
> 在AWT中，菜单相关组件的使用和之前学习的组件是一模一样的，只需要把菜单条、菜单、菜单项组合到一起，按照一定的布局，放入到容器中即可。
>
> ![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/%E8%8F%9C%E5%8D%951.png)

**常见的菜单相关组件：**

| 菜单组件名称       | 功能                                                         |
| ------------------ | ------------------------------------------------------------ |
| `MenuBar`          | 菜单条，菜单的容器 。                                        |
| `Menu`             | 菜单组件，菜单项的容器。它也是Menultem的子类，所以可作为菜单项使用，也就是说菜单项可以添加到菜单项里形成二级菜单 |
| `PopupMenu`        | 上下文菜单组件(右键菜单组件)                                 |
| `Menultem`         | 菜单项组件                                                   |
| `CheckboxMenuItem` | 复选框菜单项组件                                             |

**常见菜单相关组件集成体系图：**

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/%E8%8F%9C%E5%8D%95%E9%A1%B9%E7%BB%84%E4%BB%B6%E7%BB%A7%E6%89%BF%E4%BD%93%E7%B3%BB.png)

**菜单相关组件使用：**

1. 准备菜单项组件，这些组件可以是MenuItem及其子类对象
2. 准备菜单组件Menu或者PopupMenu(右击弹出子菜单)，把第一步中准备好的菜单项组件添加进来；
3. 准备菜单条组件MenuBar，把第二步中准备好的菜单组件Menu添加进来；
4. 把第三步中准备好的菜单条组件添加到窗口对象中显示。

> 菜单条(窗口上方的条框)显示菜单组件，菜单组件下拉框(比如点击文件出现的保存、打开等)里是菜单项，
>
> 所以菜单组件放入菜单条，菜单项放入菜单组件，菜单组件也可以放入菜单组件形成二级菜单

> **小技巧：**
>
> 1. 如果要在某个菜单的菜单项之间添加分割线，那么只需要调用Menu的`add(new MenuItem(-))`即可。
> 2. 如果要给某个菜单项关联快捷键功能，那么只需要在创建菜单项对象时设置即可，例如给菜单项关联ctrl+shif+/ 快捷键，只需要：`new MenuItem("菜单项名字", new MenuShortcut(KeyEvent.VK_Q, true)`

**代码演示：**

​	使用awt中常用菜单组件，完成下图效果

![image-20220430004542879](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220430004542879.png)

```java
public class SimpleMenu {
    // 创建窗口
    private Frame frame = new Frame("这里测试菜单相关组件");

    // 创建菜单条组件，容纳所有菜单组件
    private MenuBar menuBar = new MenuBar();

    // 创建文件菜单组件
    private Menu fileMenu = new Menu("文件");
    // 创建编辑菜单组件
    private Menu editMenu = new Menu("编辑");
    // 创建格式菜单组件，格式菜单包括在编辑菜单组件里
    private Menu formatMenu = new Menu("格式");

    // 文件菜单的菜单项
    private MenuItem newItem = new MenuItem("新建");
    private MenuItem saveItem = new MenuItem("保存");
    private MenuItem exitItem = new MenuItem("退出");

    // 编辑菜单的菜单项
    private CheckboxMenuItem autoWrap = new CheckboxMenuItem("自动换行", true);
    private MenuItem copyItem = new MenuItem("复制");
    private MenuItem pasteItem = new MenuItem("粘贴");

    // 格式菜单的菜单项，给注释菜单项添加快捷键 Ctrl+Shift+Q
    private MenuItem commentItem = new MenuItem("注释", new MenuShortcut(KeyEvent.VK_Q, true));
    private MenuItem cancelItem = new MenuItem("取消注释");

    //创建一个文本域
    private TextArea ta = new TextArea(6, 40);

    public void init() {
        // 使用内部类，定义菜单事件监听器
        ActionListener listener = new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                // 获得单击的的菜单项的名字
                String command = e.getActionCommand();
                // 在文本域中显示单击的菜单项
                ta.append("单击" + command + "菜单\n");
                // 如果是单击的是退出，就退出
                if (command.equals("退出")) {
                    System.exit(0);
                }
            }
        };
        // 为注释菜单项和退出菜单项注册监听器
        commentItem.addActionListener(listener);
        exitItem.addActionListener(listener);

        // 为文件菜单fileMenu添加菜单项
        fileMenu.add(newItem);
        fileMenu.add(saveItem);
        fileMenu.add(exitItem);

        // 为编辑菜单editMenu添加菜单项
        editMenu.add(autoWrap);
        editMenu.add(copyItem);
        editMenu.add(pasteItem);

        // 为格式化菜单formatMenu添加菜单项
        formatMenu.add(commentItem);
        formatMenu.add(cancelItem);

        // 将格式化菜单添加到编辑菜单中，作为二级菜单，顺便添一个分隔符
        editMenu.add(new MenuItem("-"));
        editMenu.add(formatMenu);

        // 将文件菜单和编辑菜单添加到菜单条中
        menuBar.add(fileMenu);
        menuBar.add(editMenu);

        // 把文本域和菜单条添加到frame中
        frame.add(ta);
        frame.setMenuBar(menuBar);

        //设置frame最佳大小并可见
        frame.pack();
        frame.setVisible(true);
    }
    public static void main(String[] args) {
        new SimpleMenu().init();
    }
}
```

## 绘图

> 很多程序如各种小游戏都需要在窗口中绘制各种图形，除此之外，即使在开发JavaEE项目时，有时候也必须"动态"地向客户端生成各种图形、图表，比如图形验证码、统计图等，这都需要利用AWT的绘图功能。

### 绘图原理

> 例如Button、Frame、Checkbox等等，不同的组件，展示出来的图形都不一样，其实这些组件展示出来的图形，其本质就是用AWT的绘图来完成的。
>
> 在AWT中，真正提供绘图功能的是Graphics对象，在Component类中，提供了下列三个方法来完成组件图形的绘制与刷新：
>
> - `paint(Graphics g)`: 绘制组件的外观；
> - `update(Graphics g)`: 内部调用paint方法，刷新组件外观；
> - `repaint()`: 调用`update`方法，刷新组件外观；
>
> **组件绘图图形流程：**
>
> ![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/%E7%BB%84%E4%BB%B6%E7%BB%98%E5%88%B6%E5%9B%BE%E5%BD%A2%E6%B5%81%E7%A8%8B.png)
>
> 一般情况下，update和paint方法是由AWT系统负责调用，如果程序要希望系统重新绘制组件，可以调用repaint方法完成。

### 绘图步骤

> 程序中绘图和现实中绘图一样，需要画布，画笔，颜料等等。A
>
> WT中提供了Canvas类充当画布，提供了Graphics类来充当画笔，通过调用Graphics对象的setColor()方法可以给画笔设置颜色。

**画图的步骤：**

1. 自定义类，继承`Canvas`类，重写`paint(Graphics g)`方法完成画图；
2. 在`paint`方法内部，真正开始画图之前调用`Graphics`对象的`setColor()`、`setFont()`等方法设置画笔的颜色、字体等属性；
3. 调用`Graphics`画笔的`drawXxx()`方法开始画图。

> 其实画图的核心就在于使用Graphics画笔在Canvas画布上画出什么颜色、什么样式的图形，所以核心在画笔(Graphics)上

**Graphics类中常用的一些方法：**

|       方法名称       |        方法功能        |
| :------------------: | :--------------------: |
| `setColor(Color c)`  |        设置颜色        |
| `setFont(Font font)` |        设置字体        |
|     `drawLine()`     |        绘制直线        |
|     `drawRect()`     |        绘制矩形        |
|  `drawRoundRect()`   |      绘制圆角矩形      |
|     `drawOval()`     |       绘制椭圆形       |
|   `drawPolygon()`    |       绘制多边形       |
|     `drawArc()`      |        绘制圆弧        |
|   `drawPolyline()`   |        绘制折线        |
|     `fillRect()`     |      填充矩形区域      |
|  `fillRoundRect()`   |    填充圆角矩形区域    |
|     `fillOval()`     |      填充椭圆区域      |
|   `fillPolygon()`    |     填充多边形区域     |
|     `fillArc()`      | 填充圆弧对应的扇形区域 |
|    `drawImage()`     |        绘制位图        |

**代码演示：**

​	使用AWT绘图API，完成下图效果

![image-20220430011732407](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220430011732407.png)

```java
public class GraphicsTest {
    // 矩形
    private final String RECT_SHAPE = "rect";
    // 椭圆形
    private final String OVAL_SHAPE = "oval";

    private Frame frame = new Frame("这里测试绘图");
    private Button drawRectBtn = new Button("绘制矩形");
    private Button drawOvalBtn = new Button("绘制椭圆");

    // 用来保存当前用户需要绘制什么样的图形
    private String shape = "";
    // 自己定义的画布，重写了paint方法
    MyCanvas drawArea = new MyCanvas();

    // 自定义类，继承Canvas类，重写paint方法
    private class MyCanvas extends Canvas {
        @Override
        public void paint(Graphics g) {
            Random r = new Random();
            if (shape.equals(RECT_SHAPE)) {
                // 用黑笔在画布的随机(x,y)坐标处画一个指定大小的矩形
                g.setColor(Color.BLACK);
                g.drawRect(r.nextInt(200), r.nextInt(100), 40, 60);
            }

            if (shape.equals(OVAL_SHAPE)) {
                // 用红笔在画布的随机(x,y)坐标出画一个指定大小的椭圆
                g.setColor(Color.RED);
                g.drawOval(r.nextInt(200), r.nextInt(100), 60, 40);
            }
        }
    }

    public void init() {
        // 为按钮添加点击事件
        drawRectBtn.addActionListener(e -> {
            shape = RECT_SHAPE;
            drawArea.repaint();
        });
        drawOvalBtn.addActionListener(e -> {
            shape = OVAL_SHAPE;
            drawArea.repaint();
        });
        // 定义一个Panel，装载两个按钮
        Panel p = new Panel();
        p.add(drawRectBtn);
        p.add(drawOvalBtn);
        // 把panel添加到frame底部
        frame.add(p, BorderLayout.SOUTH);
        // 设置画布的大小
        drawArea.setPreferredSize(new Dimension(300, 200));
        // 把画布添加到frame中
        frame.add(drawArea);
        frame.pack();
        frame.setVisible(true);
    }

    public static void main(String[] args) {
        new GraphicsTest().init();
    }
}
```

### 绘制动画

> Java也可用于开发一些动画。所谓动画，就是间隔一定的时间(通常小于0.1秒)重新绘制新的图像，两次绘制的图像之间差异较小，肉眼看起来就成了所谓的动画 。
>
> 为了实现间隔一定的时间就重新调用组件的repaint()方法，可以借助于Swing提供的Timer类，Timer类是一个定时器， 它有如下一个构造器：
> Timer(int delay, ActionListener listener): 每间隔delay毫秒，系统自动触发ActionListener监听器里的事件处理器方法，在方法内部我们就可以调用组件的repaint方法，完成组件重绘。
>
> ```java
> //定义ActionListener
> ActionListener timerTask = new ActionListener() {
>     @Override
>     public void actionPerformed(ActionEvent e) {
>         // 监听器时间里可以选择重绘组件
>     }
> }
> /** 
>  * 设置定时器，每隔100毫秒执行依次定时任务，也就是上面的监听器事件
>  * 通过间隔时间地重绘组件就可以表现出动画效果
>  */
> timer = new Timer(100,timerTask);
> timer.start();
> ```

### 处理位图

> 如果仅仅绘制一些简单的几何图形，程序的图形效果依然比较单调。AWT也允许在组件上绘制位图，Graphics提供了drawlmage()方法用于绘制位图，该方法需要一个Image参数一一代表位图，通过该方法就可以绘制出指定的位图 。

**位图使用步骤：**

1. 创建Image的子类对象`BufferedImage(int width,int height,int ImageType)`,创建时需要指定位图的宽高及类型属性，此时相当于在内存中生成了一张图片；
2. 调用BufferedImage对象的`getGraphics()`方法获取画笔，此时就可以往内存中的这张图片上绘图了，绘图的方法和之前学习的一模一样；
3. 调用组件的`drawImage()`方法，一次性的内存中的图片BufferedImage绘制到特定的组件上。

> **使用位图绘制组件的好处：**
>
> 使用位图来绘制组件，相当于实现了图的缓冲区，此时绘图时没有直接把图形绘制到组件上，而是先绘制到内存中的BufferedImage上，等全部绘制完毕，再一次性的图像显示到组件上即可，这样用户的体验会好一些。

**代码演示：**

​	通过BufferedImage实现一个简单的手绘程序：通过鼠标可以在窗口中画图

![image-20220430014033217](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220430014033217.png)

```java
public class HandDraw {
    // 定义画图区的宽高
    private final int AREA_WIDTH = 500;
    private final int AREA_HEIGHT = 400;

    // 定义变量，保存上一次鼠标拖动时，鼠标的坐标
    private int preX = -1;
    private int preY = -1;

    // 定义一个右键菜单，用于设置画笔的颜色
    private PopupMenu colorMenu = new PopupMenu();
    private MenuItem redItem = new MenuItem("红色");
    private MenuItem greenItem = new MenuItem("绿色");
    private MenuItem blueItem = new MenuItem("蓝色");

    // 定义一个BufferedImage对象，参数分别是绘画的缓冲区的宽高与图像的类型
    private BufferedImage image =
            new BufferedImage(AREA_WIDTH,AREA_HEIGHT, BufferedImage.TYPE_INT_RGB);
    // BufferedImage对象关联的画笔
    private Graphics g = image.getGraphics();

    //定义窗口对象
    private Frame frame = new Frame("简单手绘程序");

    //定义画布对象
    private Canvas drawArea =  new Canvas(){
        @Override
        public void paint(Graphics g) {
            // 把缓冲区的位图image绘制到(0,0)坐标点
            g.drawImage(image,0,0,null);
        }
    };

    // 定义一个Color对象，用来保存用户设置的画笔颜色,默认为黑色
    private Color forceColor = Color.BLACK;

    public void init(){
        // 定义颜色菜单项单击监听器
        ActionListener menuListener = new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String command = e.getActionCommand();
                switch (command){
                    case "红色":
                        forceColor=Color.RED;
                        break;
                    case "绿色":
                        forceColor = Color.GREEN;
                        break;
                    case "蓝色":
                        forceColor = Color.BLUE;
                        break;
                }
            }
        };

        // 为三个菜单项添加点击事件
        redItem.addActionListener(menuListener);
        greenItem.addActionListener(menuListener);
        blueItem.addActionListener(menuListener);

        // 把菜单项添加到右键菜单中
        colorMenu.add(redItem);
        colorMenu.add(greenItem);
        colorMenu.add(blueItem);

        // 把右键菜单添加到绘图区域drawArea
        drawArea.add(colorMenu);

        // 将iamge图片背景设置为白色
        g.fillRect(0,0,AREA_WIDTH,AREA_HEIGHT);

        // 设置绘图区域drawArea的大小
        drawArea.setPreferredSize(new Dimension(AREA_WIDTH,AREA_HEIGHT));

        // 绘图区域drawArea设置鼠标移动监听器
        drawArea.addMouseMotionListener(new MouseMotionAdapter() {
            // 用于绘制图像
            @Override
            // 按下鼠标键并拖动会触发
            public void mouseDragged(MouseEvent e) {
                // 如果上次鼠标的坐标在绘图区域，才开始绘图
                if (preX>0 && preY>0){
                    // 设置当前选中的画笔颜色
                    g.setColor(forceColor);
                    // 绘制线条，需要有两组坐标，一组是上一次鼠标拖动鼠标时的坐标，一组是现在鼠标的坐标
                    g.drawLine(preX,preY,e.getX(),e.getY());
                }
                //更新preX和preY, 也就是当前鼠标的位置
                preX = e.getX();
                preY = e.getY();
                // 重新绘制drawArea组件
                drawArea.repaint();
            }
        });
        drawArea.addMouseListener(new MouseAdapter() {
            // 用于弹出右键菜单
            @Override
            public void mouseReleased(MouseEvent e) {
                //松开鼠标键会触发
                boolean popupTrigger = e.isPopupTrigger();
                if (popupTrigger){
                    // 三个参数：菜单项显示的父窗口，菜单项显示的位置(这里坐标取鼠标当前位置)
                    colorMenu.show(drawArea, e.getX(), e.getY());
                }
                // 当鼠标松开时，把preX和preY重置为-1
                preX = -1;
                preY = -1;
            }
        });
        //把drawArea添加到frame中
        frame.add(drawArea);
        //设置frame最佳大小并可见
        frame.pack();
        frame.setVisible(true);
    }
    public static void main(String[] args) {
        new HandDraw().init();
    }
}
```

### 处理图片文件

> 在实际生活中，很多软件都支持打开本地磁盘已经存在的图片，然后进行编辑，编辑完毕后，再重新保存到本地磁盘。如果使用AWT要完成这样的功能，那么需要使用到ImageIO这个类，可以操作本地磁盘的图片文件。

|                           方法名称                           |         方法功能         |
| :----------------------------------------------------------: | :----------------------: |
|           `static BufferedImage read(File input)`            |   读取本地磁盘图片文件   |
|        `static BufferedImage read(InputStream input)`        |   读取本地磁盘图片文件   |
| `static boolean write(RenderedImage im, String formatName, File output)` | 往本地磁盘中输出图片文件 |

**代码演示：**

​	编写图片查看程序,支持另存操作

![image-20220430015249865](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220430015249865.png)

```java
public class ReadAndSaveImage {
    // 画布
    private Frame frame = new Frame("图片查看器");

    // 绘画缓冲区对象
    private BufferedImage image;

    // 画布
    private class MyCanvas extends Canvas{
        @Override
        public void paint(Graphics g) {
            // 如果缓冲区不为空，就画上去
            if (image != null){
                g.drawImage(image, 0, 0, image.getWidth(), image.getHeight(), null);
            }
        }
    }
    private MyCanvas imageCanvas = new MyCanvas();

    public void init() throws Exception{
        // 设置菜单项
        MenuBar mb = new MenuBar();
        Menu fileMenu = new Menu("文件");
        MenuItem openItem = new MenuItem("打开");
        MenuItem saveItem = new MenuItem("另存为");
        // 打开文件的事件
        openItem.addActionListener(e -> {
            // 弹出对话框，选择本地图片
            FileDialog oDialog = new FileDialog(frame);
            oDialog.setVisible(true);
            // 读取用户选择的图片
            String dir = oDialog.getDirectory();
            String file = oDialog.getFile();
            try {
                // 把读取到的文件写入绘画缓冲区
                image = ImageIO.read(new File(dir,file));
                // 画布执行重画方法
                imageCanvas.repaint();
            } catch (IOException e1) {
                e1.printStackTrace();
            }
        });
        // 保存文件的事件
        saveItem.addActionListener(e -> {
            // 弹出对话框，另存为
            FileDialog sDialog = new FileDialog(frame, "保存图片", FileDialog.SAVE);
            sDialog.setVisible(true);
            String dir = sDialog.getDirectory();
            String file = sDialog.getFile();
            try {
                ImageIO.write(image, "JPEG", new File(dir, file));
            } catch (IOException e1) {
                e1.printStackTrace();
            }
        });
        // 添加菜单项到菜单
        mb.add(fileMenu);
        fileMenu.add(openItem);
        fileMenu.add(saveItem);
        // 添加组件到窗口
        frame.setMenuBar(mb);
        frame.add(imageCanvas);
        frame.setBounds(200,200,800,600);
        frame.setVisible(true);
        // 设置关闭窗口事件
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
    }
    public static void main(String[] args) throws Exception {
        new ReadAndSaveImage().init();
    }
}
```



# 【Swing】

## 简介

> 实际使用Java开发图形界面程序时，很少使用AWT组件，绝大部分时候都是用Swing组件开发的。
>
> **Swing是由100%纯Java实现的，不再依赖于本地平台的GUI ，因此可以在所有平台上都保持相同的界面外观。**
>
> - 独立于本地平台的Swing组件被称为 **轻量级组件** 
>
> - 而依赖于本地平台的AWT组件被称为 **重量级组件**
>
> 由于Swing的所有组件完全采用Java实现，不再调用本地平台的GUI，所以导致Swing图形界面的显示速度要比AWT图形界面的显示速度慢一些，但相对于快速发展的硬件设施而言，这种微小的速度差别无妨大碍。
>
> **使用 Swing 的优势:**
>
> 1. Swing组件不再依赖于本地平台的GUI，无须采用各种平台的GUI交集，因此Swing提供了大量图形界面组件，远远超出 AWT 所提供的图形界面组件集。
> 2. Swing组件不再依赖于本地平台GUI，因此不会产生与平台相关的bug 。
> 3. Swing组件在各种平台上运行时可以保证具有相同的图形界面外观。
>
> Swing提供的这些优势，让Java图形界面程序真正实现了"一次编译，处处运行"的目标。
>
> **Swing的特征：**
>
> 1. **Swing组件采用MVC(Model-View-Controller，即模型一视图一控制器)设计模式：**
>
>    ```
>    模型(Model): 用于维护组件的各种状态；
>    视图(View): 是组件的可视化表现；
>    控制器(Controller): 用于控制对于各种事件、组件做出响应。
>    
>    当模型发生改变时，它会通知所有依赖它的视图，视图会根据模型数据来更新自己。
>    Swing使用UI代理来包装视图和控制器，还有一个模型对象来维护该组件的状态。
>    例如，按钮JButton有一个维护其状态信息的模型ButtonModel对象。Swing组件的模型是自动设置的，因此一般都使用JButton，而无须关心ButtonModel对象。
>    ```
>
> 2. **Swing在不同的平台上表现一致，并且有能力提供本地平台不支持的显示外观。**
>
>    由于Swing采用MVC模式来维护各组件，所以当组件的外观被改变时，对组件的状态信息(由模型维护)没有任何影响。
>
>    因此，Swing可以使用插拔式外观感觉 (Pluggable Look And Feel，PLAF)来控制组件外观，使得Swing图形界面在同一个平台上运行时能拥有不同的外观，用户可以选择自己喜欢的外观。
>
>    相比之下，在AWT图形界面中，由于控制组件外观的对等类与具体平台相关，因此AWT组件总是具有与本地平台相同的外观 。    

## 组件层次

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/Swing%E7%BB%84%E4%BB%B6%E7%BB%A7%E6%89%BF%E4%BD%93%E7%B3%BB.png)

> 大部分Swing组件都是JComponent抽象类的直接或间接子类(并不是全部的Swing组件)，JComponent类定义了所有子类组件的通用方法，JComponent 类是AWT里 java.awt. Container类的子类 ，这也是AWT和Swing的联系之一。绝大部分Swing组件类继承了 Container类，所以Swing组件都可作为容器使用 (JFrame继承了Frame类)。

**Swing组件和AWT组件的对应关系：**

​	大部分情况下，只需要在AWT组件的名称前面加个J，就可以得到其对应的Swing组件名称，但有几个例外：

1. `JComboBox`: 对应于AWT里的Choice组件，但比Choice组件功能更丰富 。
2. `JFileChooser`: 对应于AWT里的FileDialog组件 。
3. `JScrollBar`: 对应于AWT里的Scrollbar组件，注意两个组件类名b字母的大小写差别。
4. `JCheckBox`: 对应于AWT里的Checkbox组件，注意两个组件类名中b字母的大小写差别 
5. `JCheckBoxMenultem`: 对应于AWT里的CheckboxMenuItem 组件，注意两个组件类名中 b字母的大小写差别。

**Swing组件按照功能来分类：**

 	1. 顶层容器：`JFrame`、`JApplet`、`JDialog`和`JWindow`
 	2. 中间容器：`JPanel`、`JScrollPane`、`JSplitPane`、`JToolBar`等 
 	3. 特殊容器：在用户界面上具有特殊作用的中间容器，如`JIntemalFrame`、`JRootPane`、`JLayeredPane`和`JDestopPane`等
 	4.  基本组件：实现人机交互的组件，如`JButton`、`JComboBox`、`JList`、`JMenu`、`JSlider`等 
 	5. 不可编辑信息的显示组件：向用户显示不可编辑信息的组件，如`JLabel`、`JProgressBar`和`JToolTip`等
 	6. 可编辑信息的显示组件：向用户显示能被编辑的格式化信息的组件，如`JTable`、`JTextArea`和`JTextField`等
 	7. 特殊对话框组件：可以直接产生特殊对话框的组件，如`JColorChooser`和`JFileChooser`等

## 基本使用

### AWT组件的Swing实现

> Swing为除Canvas之外的所有AWT组件提供了相应的实现，Swing组件比AWT组件的功能更加强大
>
> **相对于AWT组件，Swing组件具有如下4个额外的功能 :**
>
> 1. 可以为Swing组件设置提示信息。使用setToolTipText()方法，为组件设置对用户有帮助的提示信息
> 2. 很多Swing组件如按钮、标签、菜单项等，除使用文字外，还可以使用图标修饰自己。为了允许在Swing组件中使用图标，Swing为Icon接口提供了一个实现类：Imagelcon，该实现类代表一个图像图标。
> 3. 支持插拔式的外观风格。每个JComponent对象都有一个相应的ComponentUI对象，为它完成所有的绘画、事件处理、决定尺寸大小等工作。ComponentUI 对象依赖当前使用的PLAF，使用UIManager.setLookAndFeel()方法可以改变图形界面的外观风格 。
> 4. 支持设置边框。Swing组件可以设置一个或多个边框。Swing 中提供了各式各样的边框供用户使用，也能建立组合边框或自己设计边框。一种空白边框可以用于增大组件，同时协助布局管理器对容器中的组件进行合理的布局。

> 每个Swing组件都有一个对应的UI类，例如JButton组件就有一个对应的ButtonUI类来作为UI代理。
>
> 每个Swing组件的UI代理的类名总是将该Swing组件类名的J去掉，然后在后面添加UI后缀。
>
> UI代理类通常是一个抽象基类，不同的PLAF会有不同的UI代理实现类。Swing类库中包含了几套UI代理,分别放在不同的包下，每套UI代理都几乎包含了所有Swing组件的ComponentUI实现，每套这样的实现都被称为一种PLAF 实现。
>
> 以JButton为例，其UI代理的继承层次下图：
>
> ![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/ComponentUI.png)
>
> 如果需要改变程序的外观风格，则可以使用如下代码：
>
> ```java
> //容器：
> JFrame jf = new JFrame();
> try {
>     //设置外观风格
>     UIManager.setLookAndFeel("com.sun.java.swing.plaf.windows.WindowsLookAndFeel");
>     //刷新jf容器及其内部组件的外观
>     SwingUtilities.updateComponentTreeUI(jf);
> } catch (Exception e) {
>     e.printStackTrace();
> }
> ```

**代码演示：**

​	使用Swing组件，实现下图中的界面效果：

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/swing_c_1.jpg)

```java
public class SwingComponentDemo {
    JFrame f = new JFrame("测试swing基本组件");
    // 定义一个按钮，并为其指定图标
    Icon okIcon = new ImageIcon(ImagePathUtil.getRealPath("2\\ok.png"));
    JButton ok = new JButton("确定",okIcon);

    // 定义一个单选按钮，初始处于选中的状态
    JRadioButton male = new JRadioButton("男",true);
    // 定义一个单选按钮，初始处于选中状态
    JRadioButton female = new JRadioButton("女",false);

    // 定义一个ButtonGroup，把male和female组合起来，实现单选
    ButtonGroup bg  = new ButtonGroup();

    // 定义一个复选框，初始处于没有选中状态
    JCheckBox married = new JCheckBox("是否已婚？",false);

    // 定义一个数组存储颜色
    String[] colors = { "红色", "绿色 " , "蓝色 " };

    // 定义一个下拉选择框，展示颜色
    JComboBox<String> colorChooser = new JComboBox<String>(colors);

    // 定一个列表框，展示颜色
    JList<String> colorList = new JList<String>(colors);

    // 定义一个8行20列的多行文本域
    JTextArea ta = new JTextArea(8,20);

    // 定义一个40列的单行文本域
    JTextField name = new JTextField(40);

    // 定义菜单条
    JMenuBar mb = new JMenuBar();

    // 定义菜单
    JMenu file = new JMenu("文件");
    JMenu edit = new JMenu("编辑");

    // 创建菜单项，并指定图标
    JMenuItem newItem = new JMenuItem("新建",new ImageIcon(ImagePathUtil.getRealPath("2\\new.png")));
    JMenuItem saveItem = new JMenuItem("保存",new ImageIcon(ImagePathUtil.getRealPath("2\\save.png")));
    JMenuItem exitItem = new JMenuItem("退出",new ImageIcon(ImagePathUtil.getRealPath("2\\exit.png")));

    JCheckBoxMenuItem autoWrap = new JCheckBoxMenuItem("自动换行");
    JMenuItem copyItem = new JMenuItem("复制",new ImageIcon(ImagePathUtil.getRealPath("2\\copy.png")));
    JMenuItem pasteItem = new JMenuItem("粘贴",new ImageIcon(ImagePathUtil.getRealPath("2\\paste.png")));

    // 定义二级菜单，将来会添加到编辑中
    JMenu format = new JMenu("格式");
    JMenuItem commentItem = new JMenuItem("注释");
    JMenuItem cancelItem = new JMenuItem("取消注释");

    // 定义一个右键菜单，用于设置程序的外观风格
    JPopupMenu pop = new JPopupMenu();

    // 定义一个ButtongGroup对象，用于组合风格按钮，形成单选
    ButtonGroup flavorGroup = new ButtonGroup();

    // 定义五个单选按钮菜单项，用于设置程序风格
    JRadioButtonMenuItem metalItem = new JRadioButtonMenuItem("Metal 风格",true);
    JRadioButtonMenuItem nimbusItem = new JRadioButtonMenuItem("Nimbus 风格",true);
    JRadioButtonMenuItem windowsItem = new JRadioButtonMenuItem("Windows 风格",true);
    JRadioButtonMenuItem classicItem = new JRadioButtonMenuItem("Windows 经典风格",true);
    JRadioButtonMenuItem motifItem = new JRadioButtonMenuItem("Motif 风格",true);



    // 初始化界面
    public void init(){

        // ------------------------组合主区域------------------------
        // 创建一个装载文本框和按钮的JPanel
        JPanel bottom = new JPanel();
        bottom.add(name);
        bottom.add(ok);

        f.add(bottom, BorderLayout.SOUTH);

        // 创建一个装载下拉选择框、三个JChekBox的JPanel
        JPanel checkPanel = new JPanel();
        checkPanel.add(colorChooser);
        bg.add(male);
        bg.add(female);

        checkPanel.add(male);
        checkPanel.add(female);
        checkPanel.add(married);

        // 创建一个垂直排列的Box，装载checkPanel和多行文本域
        Box topLeft = Box.createVerticalBox();

        // 使用JScrollPane作为普通组件的JViewPort
        JScrollPane taJsp = new JScrollPane(ta);
        topLeft.add(taJsp);
        topLeft.add(checkPanel);

        // 创建一个水平排列的Box，装载topLeft和colorList
        Box top = Box.createHorizontalBox();
        top.add(topLeft);
        top.add(colorList);

        // 将top Box 添加到窗口的中间
        f.add(top);

        // ---------------------------组合菜单条----------------------------------------------
        // 为newItem添加快捷键 ctrl+N
        newItem.setAccelerator(KeyStroke.getKeyStroke('N', InputEvent.CTRL_MASK));
        newItem.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                ta.append("用户点击了“新建”菜单\n");
            }
        });


        // 为file添加菜单项
        file.add(newItem);
        file.add(saveItem);
        file.add(exitItem);

        // 为edit添加菜单项
        edit.add(autoWrap);
        edit.addSeparator();
        edit.add(copyItem);
        edit.add(pasteItem);
        // 为commentItem添加提示信息
        commentItem.setToolTipText("将程序代码注释起来");

        // 为format菜单添加菜单项
        format.add(commentItem);
        format.add(cancelItem);

        // 给edit添加一个分隔符
        edit.addSeparator();

        // 把format添加到edit中形成二级菜单
        edit.add(format);

        // 把edit file 添加到菜单条中
        mb.add(file);
        mb.add(edit);

        // 把菜单条设置给窗口
        f.setJMenuBar(mb);

        // ------------------------组合右键菜单-----------------------------

        flavorGroup.add(metalItem);
        flavorGroup.add(nimbusItem);
        flavorGroup.add(windowsItem);
        flavorGroup.add(classicItem);
        flavorGroup.add(motifItem);

        // 给5个风格菜单创建事件监听器
        ActionListener flavorLister = new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String command = e.getActionCommand();
                try {
                    changeFlavor(command);
                } catch (Exception e1) {
                    e1.printStackTrace();
                }
            }
        };

        // 为5个风格菜单项注册监听器
        metalItem.addActionListener(flavorLister);
        nimbusItem.addActionListener(flavorLister);
        windowsItem.addActionListener(flavorLister);
        classicItem.addActionListener(flavorLister);
        motifItem.addActionListener(flavorLister);

        pop.add(metalItem);
        pop.add(nimbusItem);
        pop.add(windowsItem);
        pop.add(classicItem);
        pop.add(motifItem);
 
        // 调用ta组件的setComponentPopupMenu即可设置右键菜单，无需使用事件
        ta.setComponentPopupMenu(pop);

        // 设置关闭窗口时推出程序
        f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        // 设置jFrame最佳大小并可见
        f.pack();
        f.setVisible(true);


    }

    // 定义一个方法，用于改变界面风格
    private void changeFlavor(String command) throws Exception{
        switch (command){
            case "Metal 风格":
                UIManager.setLookAndFeel("javax.swing.plaf.metal.MetalLookAndFeel");
                break;
            case "Nimbus 风格":
                UIManager.setLookAndFeel("javax.swing.plaf.nimbus.NimbusLookAndFeel");
                break;
            case "Windows 风格":
                UIManager.setLookAndFeel("com.sun.java.swing.plaf.windows.WindowsLookAndFeel");
                break;
            case "Windows 经典风格":
                UIManager.setLookAndFeel("com.sun.java.swing.plaf.windows.WindowsClassicLookAndFeel");
                break;
            case "Motif 风格":
                UIManager.setLookAndFeel("com.sun.java.swing.plaf.motif.MotifLookAndFeel");
                break;
        }

        // 更新f窗口内顶级容器以及所有组件的UI
        SwingUtilities.updateComponentTreeUI(f.getContentPane());
        // 更新mb菜单条及每部所有组件UI
        SwingUtilities.updateComponentTreeUI(mb);
        // 更新右键菜单及内部所有菜单项的UI
        SwingUtilities.updateComponentTreeUI(pop);
    }

    public static void main(String[] args) {
        new SwingComponentDemo().init();
    }
}
```

> **注意细节：**
>
> 1. Swing菜单项指定快捷键时必须通过`组件名.setAccelerator(keyStroke.getKeyStroke("大写字母",InputEvent.CTRL_MASK))`方法来设置，其中KeyStroke代表一次击键动作，可以直接通过按键对应字母来指定该击键动作 
> 2. 更新JFrame的风格时，调用了` SwingUtilities.updateComponentTreeUI(f.getContentPane());`这是因为如果直接更新JFrame本身 ，将会导致JFrame也被更新，JFrame是一个特殊的容器，JFrame一些部分依赖于本地平台的图形组件。如果强制JFrame更新，则有可能导致该窗口失去标题栏和边框 。 
> 3. 给组件设置右键菜单，不需要使用监听器，只需要调用`setComponentPopupMenu()`方法即可，更简单。
> 4. 关闭JFrame窗口，也无需监听器，只需要调用`setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE)`方法即可，更简单。
> 5. 如果需要让某个组件支持滚动条，只需要把该组件放入到JScrollPane中，然后使用JScrollPane即可。

### 边框

> Swing可以给不同的组件设置边框，从而让界面的层次感更明显，它提供了Border对象来代表一个边框。
>
> Border的继承体系图：
>
> ![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/Border%E7%BB%A7%E6%89%BF%E4%BD%93%E7%B3%BB.png)

**特殊的Border：**

- `TitledBorder`：它的作用并不是直接为其他组件添加边框，而是为其他边框设置标题，创建该类的对象时，需要传入一个其他的Border对象；
- `ComoundBorder`：用来组合其他两个边框，创建该类的对象时，需要传入其他两个Border对象，一个作为内边框，一个座位外边框

**给组件设置边框步骤：**

1. 使用BorderFactory或者XxxBorder创建Border的实例对象；
2. 调用Swing组件的`setBorder(Border b)`方法为组件设置边框；

**代码演示：**

![image-20220430175238196](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220430175238196.png)

```java
public class BorderTest {
    JFrame jf  = new JFrame("测试边框");
    public void init(){
        // 组装视图
        // 1.JFrame的布局修改为GridLayout
        jf.setLayout(new GridLayout(2,4));

        // 2.往网格中填充不同的JPanel组件，并且设置边框和内容

        // 创建BevelBorder
        Border bevelBorder = BorderFactory.createBevelBorder(BevelBorder.RAISED, Color.RED, Color.GREEN, Color.BLUE, Color.GRAY);
        jf.add(getJPanelWithBorder(bevelBorder,"BevelBorder"));

        // 创建LineBorder
        Border lineBorder = BorderFactory.createLineBorder(Color.ORANGE, 10);
        jf.add(getJPanelWithBorder(lineBorder,"LineBorder"));

        // 创建EmptyBorder
        Border emptyBorder = BorderFactory.createEmptyBorder(10, 5, 20, 10);
        jf.add(getJPanelWithBorder(emptyBorder,"EmptyBorder"));

        // 创建EchtedBorder
        Border etchedBorder = BorderFactory.createEtchedBorder(EtchedBorder.RAISED, Color.RED, Color.GREEN);
        jf.add(getJPanelWithBorder(etchedBorder,"EtchedBorder"));

        // 创建TitledBorder
        TitledBorder titledBorder = new TitledBorder(new LineBorder(Color.ORANGE,10),"测试标题",TitledBorder.LEFT,TitledBorder.BOTTOM,new Font("StSong",Font.BOLD,18),Color.BLUE);
        jf.add(getJPanelWithBorder(titledBorder,"TitledBorder"));

        // 创建MatteBorder
        MatteBorder matteBorder = new MatteBorder(10, 5, 20, 10, Color.GREEN);
        jf.add(getJPanelWithBorder(matteBorder,"MatteBorder"));

        // 创建CompoundBorder
        CompoundBorder compoundBorder = new CompoundBorder( new LineBorder(Color.RED, 10),titledBorder);
        jf.add(getJPanelWithBorder(compoundBorder,"CompoundBorder"));

        // 3.设置窗口最佳大小、设置窗口可见，处理关闭操作
        jf.pack();
        jf.setVisible(true);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

    }
    // 为组件组装边框，调用这个方法返回一个组装好边框的JPanel组件
    public JPanel getJPanelWithBorder(Border border,String content){
        JPanel jPanel = new JPanel();
        jPanel.add(new JLabel(content));
        //设置边框
        jPanel.setBorder(border);
        return jPanel;
    }

    public static void main(String[] args) {
        new BorderTest().init();
    }
}
```

### 工具条

> Swing提供了JToolBar类来创建工具条，并且可以往JToolBar中添加多个工具按钮。

**JToolBar API：**

|                 方法名称                 |                           方法功能                           |
| :--------------------------------------: | :----------------------------------------------------------: |
| `JToolBar(String name, int orientation)` | 创建一个名字为name，方向为orientation的工具条对象，其orientation的是取值可以是SwingConstants.HORIZONTAL或SwingConstants.VERTICAL |
|             `add(Action a)`              |       通过Action对象为JToolBar工具条添加对应的工具按钮       |
|      `addSeparator(Dimension size)`      |                向工具条中添加指定大小的分隔符                |
|        `setFloatable(boolean b)`         |                   设定工具条是否可以被拖动                   |
|          `setMargin(Insets m)`           |                  设置工具条与工具按钮的边距                  |
|         `setOrientation(int o)`          |                       设置工具条的方向                       |
|     `setRollover(boolean rollover)`      |                  设置此工具条的rollover状态                  |

> `add(Action a)`：
>
> Action接口是ActionListener的一个子接口，它代表一个事件监听器，
>
> 这里add方法给工具条添加一个工具按钮，传递的是一个事件监听器
>
> 因为不管是菜单条中的菜单项还是工具条中的工具按钮，最终肯定是需要点击来完成一些操作，所以JToolBar以及JMenu都提供了更加便捷的添加子组件的方法add(Action a),在这个方法的内部会做如下几件事：
>
> 1. 创建一个适用于该容器的组件(例如，在工具栏中创建一个工具按钮)；
> 2. 从Action对象中获得对应的属性来设置该组件(例如，通过name来设置文本，通过 lcon来设置图标)；
> 3. 把Action监听器注册到刚才创建的组件上；

**代码演示：**

![image-20220430214825636](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220430214825636.png)

```java
public class JToolBarTest {

    JFrame jf = new JFrame("测试工具条");
    JTextArea jta = new JTextArea(6,35);

    // 声明工具条相关内容
    JToolBar jToolBar = new JToolBar("播放工具条",SwingConstants.HORIZONTAL);

    // 创建3个Action对象
    // 传递的参数，name和icon，最终在添加到工具条中时，会被拿出来作为按钮的名称和图标
    Action pre = new AbstractAction("上一曲",new ImageIcon("img\\component\\pre.png")) {
        @Override
        public void actionPerformed(ActionEvent e) {
            jta.append("上一曲.\n");
        }
    };

    Action pause = new AbstractAction("暂停",new ImageIcon("img\\component\\pause.png")) {
        @Override
        public void actionPerformed(ActionEvent e) {
            jta.append("暂停播放.\n");
        }
    };

    Action next = new AbstractAction("下一曲",new ImageIcon("img\\component\\next.png")) {
        @Override
        public void actionPerformed(ActionEvent e) {
            jta.append("下一曲.\n");
        }
    };


    public void init(){
        // 组装视图

        // 通过Action对象来创建JButton
        JButton preBtn = new JButton(pre);
        JButton pauseBtn = new JButton(pause);
        JButton nextBtn = new JButton(next);
        jToolBar.add(preBtn);
        jToolBar.addSeparator();
        jToolBar.add(pauseBtn);
        jToolBar.addSeparator();
        jToolBar.add(nextBtn);

        // 让工具条可以拖动
        jToolBar.setFloatable(true);
        jf.add(jToolBar,BorderLayout.NORTH);

        // 文本框默认不支持滚动条
        // 把文本框组件设置到JScrollPane中，那么该组件就支持滚动条了
        JScrollPane jScrollPane = new JScrollPane(jta);
        jf.add(jScrollPane);

        // 设置窗口关闭、最佳大小和可见
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        jf.pack();
        jf.setVisible(true);
    }

    public static void main(String[] args) {
        new JToolBarTest().init();
    }
}
```

### 选择框

> Swing提供了JColorChooser和JFileChooser这两种对话框，可以很方便地完成颜色的选择和本地文件的选择。

#### JCholor

> JColorChooser 用于创建颜色选择器对话框 

**快速生成一个颜色选择对话框：**

```java
/**
 *	参数：
 *	componet:指定当前对话框的父组件
 *	title：当前对话框的名称
 *	initialColor：指定默认选中的颜色
 *	返回值：返回用户选中的颜色
 */
public static Color showDialog(Component component, String title,Color initialColor)
```

#### JFile

> JFileChooser的功能与AWT中的FileDialog基本相似，也是用于生成"打开文件"、"保存文件"对话框。与FileDialog不同的是，JFileChooser无须依赖于本地平台的GUI，它由100%纯Java实现，在所有平台上具有完全相同的行为，并可以在所有平台上具有相同的外观风格。

**JFileChooser使用步骤：**

- 创建JFileChooser对象：

```java
//指定默认打开的本地磁盘路径
JFileChooser chooser = new JFileChooser("D:\\a");
```

- 调用JFileChooser的一系列可选方法，进行初始化

```java
setSelectedFile(File file)
setSelectedFiles(File[] selectedFiles):设定默认选中的文件
setMultiSelectionEnabled(boolean b)：设置是否允许多选，默认是单选
setFileSelectionMode(int mode)：设置可以选择内容，例如文件、文件夹等，默认只能选择文件
```

- 打开文件对话框

```java
showOpenDialog(Component parent):打开文件加载对话框，并指定父组件
showSaveDialog(Component parent):打开文件保存对话框，并指定父组件
```

- 获取用户选择的结果

```java
File getSelectedFile():获取用户选择的一个文件
File[] getSelectedFiles():获取用户选择的多个文件
```

### 对话框

> 通过JOptionPane可以非常方便地创建一些简单的对话框
>
> Swing已经为这些对话框添加了相应的组件，无须程序员手动添加组件。 

**JOptionPane提供了如下4个方法来创建对话框：**

|                        方法名称                         |                           方法功能                           |
| :-----------------------------------------------------: | :----------------------------------------------------------: |
| `showMessageDialog()`<br/>`showInternalMessageDialog()` | 消息对话框，告知用户某事己发生，用户只能单击"确定"按钮，类似于JavaScript的alert函数 。 |
| `showConfirmDialog()`<br/>`showInternalConfirmDialog()` | 确认对话框，向用户确认某个问题，用户可以选择yes、no ~ cancel等选项。类似于JavaScript的comfirm函数。该方法返回用户单击了哪个按钮 |
|   `showInputDialog()`<br/>`showInternalInputDialog()`   | 输入对话框，提示要求输入某些信息，类似于JavaScript的prompt函数。该方法返回用户输入的字符串 。 |
|  `showOptionDialog()`<br>`showInternalOptionDialog()`   | 自定义选项对话框，允许使用自定义选项，可以取代showConfirmDialog所产生的对话框，只是用起来更复杂 。 |

**上述方法都有都有很多重载形式，选择其中一种最全的形式，参数解释如下：**

```java
showXxxDialog(Component parentComponent,
		Object message, 
		String title, 
		int optionType, 
		int messageType,
        Icon icon,
		Object[] options, 
		Object initialValue)
--参数解释：
parentComponent：当前对话框的父组件
message：对话框上显示的信息，信息可以是字符串、组件、图片等
title：当前对话框的标题
optionType：当前对话框上显示的按钮类型：DEFAULT_OPTION、YES_NO_OPTION、YES_NO_CANCEL_OPTION、OK_CANCEL_OPTION
messageType:当前对话框的类型:ERROR_MESSAGE、INFORMATION_MESSAGE、WARNING_MESSAGE、QUESTION_MESSAGE、PLAIN_MESSAGE
icon:当前对话框左上角的图标
options:自定义下拉列表的选项
initialValue:自定义选项中的默认选中项
```

**当用户与对话框交互结束后，不同类型对话框的返回值如下：**

- `showMessageDialog`: 无返回值 。
- `showlnputDialog`: 返回用户输入或选择的字符串 。
- `showConfirmDialog`: 返回一个整数代表用户选择的选项 。
- `showOptionDialog`: 返回一个整数代表用户选择的选项，如果用户选择第一项，则返回 0; 如果选择第二项，则返回1……依此类推 。

**对showConfirmDialog所产生的对话框，有如下几个返回值：**

- `YES OPTION`: 用户 单击了"是"按钮后返回 。
- `NO OPTION`: 用 户单击了"否"按钮后返回 。
- `CANCEL OPTION`: 用户单击了"取消"按钮后返回 。
- `OK OPTION`: 用户单击了"确定"按钮后返回 。
- `CLOSED OPTION`: 用户单击了对话框右上角的" x" 按钮后返回。

## 特殊容器

> Swing提供了一些具有特殊功能的容器 ， 这些特殊容器可以用于创建一些更复杂的用户界面。

### JSplitPane

> JSplitPane用于创建一个分割面板，它可以将一个组件(通常是一个容器)分割成两个部分，并提供一个分割条，用户可以拖动该分割条来调整两个部分的大小
>
> ![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/JSplitPaneDemo.jpg)

**JSplitPane使用步骤：**

- 创建JSplitPane对象

```java
通过如下构造方法可以创建JSplitPane对象
JSplitPane(int newOrientation, Component newLeftComponent,Component newRightComponent)
    
-newOrientation：指定JSplitPane容器的分割方向：
    如果值为JSplitPane.VERTICAL_SPLIT,为纵向分割；
    如果值为JSplitPane.HORIZONTAL_SPLIT，为横向分割；    	
-newLeftComponent：左侧或者上侧的组件；
-newRightComponent：右侧或者下侧的组件；
```

- 设置是否开启连续布局的支持(可选)

```java
setContinuousLayout(boolean newContinuousLayout):
默认是关闭的，如果设置为true，则打开连续布局的支持，但由于连续布局支持需要不断的重绘组件，所以效率会低一些
连续布局：拖动分隔条时组件大小会相应改变，支持连续布局就会在拖动的时候就开始重绘组件，不支持就是松开分隔条时才重绘
```

- 设置是否支持"一触即展"的支持(可选)

```java
setOneTouchExpandable(boolean newValue):
默认是关闭的，如果设置为true，则打开"一触即展"的支持
```

- 其他设置

```java
setDividerLocation(double proportionalLocation):设置分隔条的位置为JSplitPane的某个百分比
setDividerLocation(int location): 通过像素值设置分隔条的位置
setDividerSize(int newSize):通过像素值设置分隔条的大小
设置指定位置的组件    
setLeftComponent(Component comp)
setTopComponent(Component comp)
setRightComponent(Component comp)
setBottomComponent(Component comp)
```

### JTablePane

> JTabbedPane可以很方便地在窗口上放置多个标签页，每个标签页相当于获得了一个与外部容器具有相同大小的组件摆放区域。
>
> 通过这种方式，就可以在一个容器里放置更多的组件，例如右击桌面上的"我的电脑"图标，在弹出的快捷菜单里单击"属性"菜单工页，就可以看到一个"系统属性"对话框，这个对话框里包含了若干个标签页。
>
> ![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/JTabbedPane.jpg)

**JTabledPane使用步骤:**

- 创建JTabbedPane对象

```java
 JTabbedPane(int tabPlacement, int tabLayoutPolicy):
	tabPlacement:
		指定标签标题的放置位置，可以选择 SwingConstants中的四个常量：TOP、LEFT、BOTTOM、RIGHT
	tabLaoutPolicy:
		指定当窗口不能容纳标签页标题时的布局策略，可以选择JTabbedPane.WRAP_TAB_LAYOUT和JTabbedPane.SCROLL_TAB_LAYOUT
```

- 通过JTabbedPane对象堆标签进行增删改查

```java
addTab(String title, Icon icon, Component component, String tip):添加标签
	title:标签的名称
	icon:标签的图标
	component:标签对应的组件
	tip:光标放到标签上的提示
	
insertTab(String title, Icon icon, Component component, String tip, int index):插入标签页
	title:标签的名称
	icon:标签的图标
	component:标签对应的组件
	tip:光标放到标签上的提示
	index:在哪个索引处插入标签页
        
setComponentAt(int index, Component component):修改标签页对应的组件
	index:修改哪个索引处的标签
	component:标签对应的组件
        
removeTabAt(int index):
	index:删除哪个索引处的标签
```

- 设置当前显示的标签页

```java
setSelectedIndex(int index):设置哪个索引处的标签被选中
```

- 设置JTabbedPane的其他属性

```java
setDisabledIconAt(int index, Icon disabledIcon): 将指定位置的禁用图标设置为icon，该图标也可以是null表示不使用禁用图标。
setEnabledAt(int index, boolean enabled): 设置指定位置的标签页是否启用。
setTitleAt(int index, String title): 设置指定位置标签页的标题为title,该title可以是null,这表明设置该标签页的标题为空。
setToolTipTextAt(int index, String toolTipText): 设置指定位置标签页的提示文本 。
```

- 为JTabbedPane设置监听器

```java
addChangeListener(ChangeListener l)
```

### JLayeredPane

> JLayeredPane是一个代表有层次深度的容器，它允许组件在需要时互相重叠。当向JLayeredPane容器中添加组件时，需要为该组件指定一个深度索引，其中层次索引较高的层里的组件位于其他层的组件之上。
>
> ![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/JLayeredPane1.gif)

 JLayeredPane还将容器的层次深度分成几个默认层，程序只是将组件放入相应的层，从而可以更容易地确保组件的正确重叠，无须为组件指定具体的深度索引。

 **JLayeredPane 提供了如下几个默认层：**

 1. `DEFAULT_LAYER`:大多数组件位于标准层，这是最底层；
 2. `PALETTE_LAYER`: 调色板层位于默认层之上。该层对于浮动工具栏和调色板很有用，因此可以位于其他组件之上 。
 3. `MODAL_LAYER`: 该层用于显示模式对话框。它们将出现在容器中所有工具栏 、调色板或标准组件的上面 。
 4. `POPUP_LAYER`: 该层用于显示右键菜单，与对话框、工具提示和普通组件关联的弹出式窗口将出现在对应的对话框、工具提示和普通组件之上。
 5. `DRAG_LAYER`: 该层用于放置拖放过程中的组件，拖放操作中的组件位于所有组件之上。 一旦拖放操作结束后，该组件将重新分配到其所属的正常层。

**JLayeredPane方法：**

- `moveToBack(Component c)`：把当前组件c移动到所在层的所有组件的最后一个位置；

- `moveToFront(Component c)`：把当前组件c移动到所在层的所有组件的第一个位置；

- `setLayer(Component c, int layer)`：更改组件c所处的层；

需要注意的是，往JLayeredPane中添加组件，如果要显示，则必须手动设置该组件在容器中显示的位置以及大小。

### JDesktopPane和JInternalFrame

> JDesktopPane是JLayeredPane的子类，这种容器在开发中会更常用。
>
> 很多应用程序都需要启动多个内部窗口来显示信息（典型的比如IDEA、NotePad++），这些内部窗口都属于同一个外部窗口，当外部窗口最小化时，这些内部窗口都被隐藏起来。
>
> 在 Windows 环境中，这种用户界面被称为多文档界面(Multiple Document Interface, MDI) 
>
> 使用 Swing 可以非常简单地创建出这种 MDI 界面，通常，内部窗口有自己的标题栏、标题、图标、三个窗口按钮，并允许拖动改变内部窗口的大小和位置，但内部窗口不能拖出外部窗口。
>
> JDesktopPane需要和JIntemalFrame结合使用，其中JDesktopPane代表一个虚拟桌面 ，而JIntemalFrame则用于创建内部窗口。

**使用JDesktopPane和JIntenalFrame创建内部窗口步骤：**

- 创建一个JDesktopPane对象，代表虚拟桌面

```java
JDesktopPane()
```

- 使用JIntemalFrame创建一个内部窗口

```java
JInternalFrame(String title, boolean resizable, boolean closable, boolean maximizable, boolean iconifiable):
	title: 内部窗口标题
	resizable:是否可改变大小
	closeble: 是否可关闭
	maximizable: 是否可最大化
	iconifiable:是否可最小化
```

- 一旦获得了内部窗口之后，该窗口的用法和普通窗口的用法基本相似，一样可以指定该窗口的布局管理器，一样可以向窗口内添加组件、改变窗口图标等。

- 将该内部窗口以合适大小、在合适位置显示出来。

  与普通窗口类似的是，该窗口默认大小是0x0像素，位于(0,0)位置(虚拟桌面的左上角处)，并且默认处于隐藏状态，程序可以通过如下代码将内部窗口显示出来:

```java
reshape(int x, int y, int width, int height):设置内部窗口的大小以及在外部窗口中的位置；
show():设置内部窗口可见
```

5. 将内部窗口添加到JDesktopPane容器中，再将JDesktopPane容器添加到其他容器中。

## 进度条

> 进度条是图形界面中广泛使用的GUI组件
>
> 当复制一个较大的文件时，操作系统会显示一个进度条，用于标识复制操作完成的比例 
>
> 当启动Eclipse等程序时，因为需要加载较多的资源，故而启动速度较慢，程序也会在启动过程中显示一个进度条，用以表示该软件启动完成的比例 ……
>
> Swing中使用JProcessBar、ProcessMonitor和BoundedRangeModel实现进度条

### 创建进度条

**使用JProgressBar创建进度条的步骤：**

- 创建JProgressBar对象

```java
public JProgressBar(int orient, int min, int max):
	orint:方向
	min:最小值
	max:最大值
```

- 设置属性

```java
setBorderPainted(boolean b):设置进度条是否有边框
setIndeterminate(boolean newValue):设置当前进度条是不是进度不确定的进度条，如果是，则将看到一个滑块在进度条中左右移动
setStringPainted(boolean b)：设置进度条是否显示当前完成的百分比
```

- 获取和设置当前进度条的进度状态

```java
setValue(int n):设置当前进度值
double getPercentComplete():获取进度条的完成百分比
String  getStrin():返回进度字符串的当前值
```

> 实际开发中通过不断地检测一个耗时任务的完成情况，去更新进度条的进度。

**代码演示：**

![image-20220501004450151](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220501004450151.png)

```java
public class JProgressTest {

    JFrame jf = new JFrame("测试进度条");

    JCheckBox indeterminate = new JCheckBox("不确定进度");
    JCheckBox noBorder = new JCheckBox("不绘制边框");

    // 创建进度条
    JProgressBar bar = new JProgressBar(JProgressBar.HORIZONTAL,0,100);


    public void init(){
        //组装视图

        // 处理复选框的点击行为
        indeterminate.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                // 获取一下indeterminate复选框有没有选中
                boolean selected = indeterminate.isSelected();
                // 设置当前进度条不确定进度
                bar.setIndeterminate(selected);
                bar.setStringPainted(!selected);
            }
        });

        noBorder.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                // 获取一下noBorder复选框有没有选中
                boolean selected = noBorder.isSelected();
                bar.setBorderPainted(!selected);
            }
        });

        Box vBox = Box.createVerticalBox();
        vBox.add(indeterminate);
        vBox.add(noBorder);

        // 设置进度条的属性
        bar.setStringPainted(true);
        bar.setBorderPainted(true);

        // 把当前窗口的布局方式修改为FlowLayout
        jf.setLayout(new FlowLayout());
        jf.add(vBox);
        jf.add(bar);

        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        jf.pack();
        jf.setVisible(true);

        // 通过循环模拟修改进度条的进度，实际开发中要检测任务完成情况来更新进度
        for (int i = 0; i <= 100; i++) {
            // 修改已经完成的工作量，也就是百分比
            bar.setValue(i);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        new JProgressTest().init();
    }
}
```



### 创建进度对话框

>  ProgressMonitor的用法与JProgressBar的用法基本相似，只是ProgressMonitor可以直接创建一个进度对话框，**它提供了下面的构造器完成对话框的创建：**
>
> ```java
> public ProgressMonitor(Component parentComponent, Object message, String note, int min, int max):
> 	parentComponent:对话框的父组件
> 	message:对话框的描述信息
> 	note:对话框的提示信息
> 	min:进度条的最小值
> 	max:进度条的最大值
> ```
>
> 使用ProgressMonitor创建的对话框里包含的进度条是非常固定的，程序甚至不能设置该进度条是否包含边框(总是包含边框)，不能设置进度不确定，不能改变进度条的方向(总是水平方向) 。
>

## 列表框

> Swing中使用JList和JComboBox可以实现列表框
>
> 无论从哪个角度来看，JList和JComboBox都是极其相似的：
>
> - 它们都有一个列表框，只是 JComboBox的列表框需要以下拉方式显示出来; 
>
> - JList和JComboBox都可以通过调用setRenderer()方法来改变列表项的表现形式。
> - 维护这两个组件的Model都是相似的，JList使用ListModel, JComboBox使用ComboBoxModel，而ComboBoxModel是ListModel 的子类 。

### 简单列表框

使用JList或JComboBox实现简单列表框的步骤：

- 创建JList或JComboBox对象

```java
JList(final E[] listData):创建JList对象，把listData数组中的每项内容转换成一个列表项展示
JList(final Vector<? extends E> listData):创建JList对象，把listData数组中的每项内容转换成一个列表项展示
JComboBox(E[] items):
JComboBox(Vector<E> items):
```

- 设置JList或JComboBox的外观行为

```java
---------------------------JList----------------------------------------------
addSelectionInterval(int anchor, int lead):在已经选中列表项的基础上，增加选中从anchor到lead索引范围内的所有列表项
setFixedCellHeight(int height)/setFixedCellWidth(int width):设置列表项的高度和宽度
setLayoutOrientation(int layoutOrientation)：设置列表框的布局方向
setSelectedIndex(int index)：设置默认选中项
setSelectedIndices(int[] indices)：设置默认选中的多个列表项
setSelectedValue(Object anObject,boolean shouldScroll)：设置默认选中项，并滚动到该项显示
setSelectionBackground(Color selectionBackground)：设置选中项的背景颜色
setSelectionForeground(Color selectionForeground)：设置选中项的前景色
setSelectionInterval(int anchor, int lead):设置从anchor到lead范围内的所有列表项被选中
setSelectionMode(int selectionMode)：设置选中模式，默认没有限制，也可以设置为单选或者区域选中
setVisibleRowCount(int visibleRowCount)：设置列表框的可是高度足以显示多少行列表项
    
---------------------------JComboBox---------------------------------------------- 
setEditable(boolean aFlag)：设置是否可以直接修改列表文本框的值，默认为不可以
setMaximumRowCount(int count)：设置列表框的可是高度足以显示多少行列表项
setSelectedIndex(int anIndex)：设置默认选中项
setSelectedItem(Object anObject)：根据列表项的值，设置默认选中项
```

- 设置监听器，监听列表项的变化，JList通过addListSelectionListener完成，JComboBox通过addItemListener完成

**代码演示：**

![image-20220501005039287](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220501005039287.png)

```java
public class ListTest {
    JFrame jf = new JFrame("列表框测试");
    String[] books = {"java自学宝典","轻量级javaEE企业应用实战","Android基础教程","jQuery实战教程","SpringBoot企业级开发"};

    // 定义布局选择按钮所在的面板
    JPanel layoutPanel = new JPanel();
    ButtonGroup layoutGroup = new ButtonGroup();

    // 定义选择模式按钮所在的面板
    JPanel selectModePanel = new JPanel();
    ButtonGroup selectModeGroup = new ButtonGroup();

    JTextArea favorite = new JTextArea(4,40);

    // 用一个字符串数组来创建一个JList对象
    JList<String> bookList ;
    JComboBox<String> bookSelector;

    public void init(){
        // 组装视图

        // 组装Jlist相关内容
        bookList = new JList<>(books);

        addBtn2LayoutPanel("纵向滚动",JList.VERTICAL);
        addBtn2LayoutPanel("纵向换行",JList.VERTICAL_WRAP);
        addBtn2LayoutPanel("横向换行",JList.HORIZONTAL_WRAP);

        addBtn2SelectModelPanel("无限制",ListSelectionModel.MULTIPLE_INTERVAL_SELECTION);
        addBtn2SelectModelPanel("单选",ListSelectionModel.SINGLE_SELECTION);
        addBtn2SelectModelPanel("单范围",ListSelectionModel.SINGLE_INTERVAL_SELECTION);

        // 对JList做设置
        bookList.setVisibleRowCount(3);
        bookList.setSelectionInterval(2,4);

        // 处理条目选中事件
        bookList.addListSelectionListener(new ListSelectionListener() {
            @Override
            public void valueChanged(ListSelectionEvent e) {
                // 获取当前选中的条目
                List<String> selectedValuesList = bookList.getSelectedValuesList();
                // 把当前条目的内容设置到文本域中。
                favorite.setText("");
                for (String str : selectedValuesList) {
                    favorite.append(str+"\n");
                }
            }
        });

        Box bookListVBox = Box.createVerticalBox();
        bookListVBox.add(new JScrollPane(bookList));
        bookListVBox.add(layoutPanel);
        bookListVBox.add(selectModePanel);

        // 组装JComboBox
        Vector<String> vector = new Vector<>();
        List<String> list = List.of("java自学宝典", "轻量级javaEE企业应用实战", "Android基础教程", "jQuery实战教程", "SpringBoot企业级开发");
        vector.addAll(list);

        bookSelector = new JComboBox<>(vector);

        bookSelector.setEditable(true);

        bookSelector.setMaximumRowCount(4);

        bookSelector.addItemListener(new ItemListener() {
            @Override
            public void itemStateChanged(ItemEvent e) {
                // 获取当前已经选中的条目，把内容设置到文本域中
                Object selectedItem = bookSelector.getSelectedItem();
                favorite.setText(selectedItem.toString());
            }
        });

        // 组装顶部的左右两部分
        Box topBox = Box.createHorizontalBox();
        topBox.add(bookListVBox);
        JPanel bookSelectPanel = new JPanel();
        bookSelectPanel.add(bookSelector);
        topBox.add(bookSelectPanel);

        // 组装底部
        JPanel bottomPanel = new JPanel();
        bottomPanel.setLayout(new BorderLayout());

        bottomPanel.add(new JLabel("您最喜欢的图书："),BorderLayout.NORTH);
        bottomPanel.add(favorite);

        // 组装整体
        Box holeBox = Box.createVerticalBox();
        holeBox.add(topBox);
        holeBox.add(bottomPanel);

        jf.add(holeBox);


        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        jf.pack();
        jf.setVisible(true);

    }

    // 封装方法，往LayoutPanel中添加单选按钮
    public void addBtn2LayoutPanel(String name,int layoutType){
        // 设置标题边框
        layoutPanel.setBorder(new TitledBorder(new EtchedBorder(),"确定选项布局"));
        // 创建单选按钮
        JRadioButton button = new JRadioButton(name);
        layoutPanel.add(button);
        // 让第一个按钮默认选中
        if (layoutGroup.getButtonCount()==0){
            button.setSelected(true);
        }
        layoutGroup.add(button);
        button.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                bookList.setLayoutOrientation(layoutType);
            }
        });
    }

    // 封装方法，给SelectModelPanel添加按钮
    public void addBtn2SelectModelPanel(String name,int selectionModel){
        // 设置标题边框
        selectModePanel.setBorder(new TitledBorder(new EtchedBorder(),"确定选择模式"));
        // 创建单选按钮
        JRadioButton button = new JRadioButton(name);
        selectModePanel.add(button);
        // 让第一个按钮默认选中
        if (selectModeGroup.getButtonCount()==0){
            button.setSelected(true);
        }
        selectModeGroup.add(button);
        button.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
               bookList.setSelectionMode(selectionModel);
            }
        });
    }

    public static void main(String[] args) {
        new ListTest().init();
    }
}
```



### Model

> 与JProgressBar一样，JList和JComboBox也采用了MVC的设计模式，JList和JComboBox只负责外观的显示，而组件底层的状态数据则由对应的Model来维护。
>
> JList对应的Model是ListModel接口，JComboBox对应的Model是ComboBox接口，其代码如下：
>
> ```java
> public interface ListModel<E>{
>   int getSize();
>   E getElementAt(int index);
>   void addListDataListener(ListDataListener l);
>   void removeListDataListener(ListDataListener l);
> }
> 
> public interface ComboBoxModel<E> extends ListModel<E> {
>   void setSelectedItem(Object anItem);
>   Object getSelectedItem();
> }
> ```
>
> 从上面接口来看，这个ListModel不管JList里的所有列表项的存储形式，它甚至不强制存储所有的列表项，只要 ListModel的实现类提供了getSize()和 getElementAt()两个方法，JList就可以根据该ListModel对象来生成列表框。ComboBoxModel 继承了 ListModel ，它添加了"选择项"的概念，选择项代表 JComboBox 显示区域内可见的列表项 。 
>
> 在使用JList和JComboBox时，除了可以使用jdk提供的Model实现类，程序员自己也可以根据需求，自己定义Model的实现类，实现对应的方法使用。

### DefaultModel

> 当调用JList和JComboBox构造方法时，传入数组或Vector作为参数，这些数组元素或集合元素将会作为列表项。
>
> 当使用JList或JComboBox时，常常还需要动态地增加、删除列表项。
>
> 例如JCombox提供了下列方法完成增删操作：
>
> ```java
> addItem(E item):添加一个列表项
> insertItemAt(E item, int index)：向指定索引处插入一个列表项
> removeAllItems()：删除所有列表项
> removeItem(Object anObject)：删除指定列表项
> removeItemAt(int anIndex)：删除指定索引处的列表项
> ```
>
> JList并没有提供这些类似的方法。
>
> 如果需要创建一个可以增加、删除列表项的JList对象，则应该在创建JList时显式使用 DefaultListModel作为构造参数。
>
> 因为DefaultListModel作为JList的Model，它负责维护JList组件的所有列表数据，所以可以通过向DefaultListModel中添加、删除元素来实现向JList对象中增加、删除列表项。
>
> DefaultListModel提供了如下几个方法来添加、删除元素:
>
> ```java
> add(int index, E element): 在该 ListModel 的指定位置处插入指定元素
> addElement(E obj): 将指定元素添加到该 ListModel 的末尾
> insertElementAt(E obj, int index): 在该 ListModel 的指定位置处插入指定元素
> Object remove(int index): 删除该 ListModel 中指定位置处的元素 
> removeAllElements(): 删 除该 ListModel 中的所有元素，并将其的大小设置为零
> removeElement(E obj): 删除该 ListModel 中第一个与参数匹配的元素
> removeElementAt(int index): 删除该 ListModel 中指定索引处的元素
> removeRange(int 企omIndex ， int toIndex): 删除该 ListModel 中指定范围内的所有元素
> set(int index, E element) : 将该 ListModel 指定索引处的元素替换成指定元素
> setElementAt(E obj, int index): 将该 ListModel 指定索引处的元素替换成指定元素
> ```

### 改变列表外观

> Swing中使用ListCellRenderer可以改变列表外观
>
> JList和JComboBox不仅支持简单的字符串列表，还可以支持图标列表项，如果在创建 JList或JComboBox时传入图标数组，则创建的JList和JComboBox的列表项就是图标 
>
> 如果希望列表项是更复杂的组件，例如希望像 QQ 程序那样每个列表项既有图标，此时需要使用ListCellRenderer接口的实现类对象，自定义每个条目组件的渲染过程：
>
> ```java
> public interface ListCellRenderer<E> {
>     Component getListCellRendererComponent(
>         JList<? extends E> list, // 列表组件
>         E value, // 当前列表项的值额索引
>         int index, // 当前列表项d
>         boolean isSelected, // 当前列表项是否被选中
>         boolean cellHasFocus); // 当前列表项是否获取了焦点
> }
> ```
>
> 通过JList的`setCellRenderer(ListCellRenderer<? super E> cellRenderer)`方法，把自定义的ListCellRenderer对象传递给JList，就可以按照自定义的规则绘制列表项组件了。

**代码演示：**

![image-20220501002925066](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220501002925066.png)

```java
public class ListCellRendererTest {
    private JFrame mainWin = new JFrame("好友列表");
    private String[] friends = {
            "李清照",
            "苏格拉底",
            "李白",
            "弄玉",
            "虎头"
    };
    // 定义一个JList对象
    JList friendsList = new JList(friends);
    public void init() {
        // 组装视图
        // 给JList设置ListCellRenderer对象,指定列表项绘制的组件
        friendsList.setCellRenderer(new MyRenderer());
        
        mainWin.add(friendsList);
        mainWin.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        mainWin.pack();
        mainWin.setVisible(true);

    }
	
    // 自定义的ListCellRenderer接口实现类对象
    private class MyRenderer extends JPanel implements ListCellRenderer{
        private String name;
        private ImageIcon icon;
        // 记录背景色
        private Color backGround;
        // 记录前景色：文字的颜色
        private Color forceGround;

        @Override
        public Component getListCellRendererComponent(JList list, Object value, int index, boolean isSelected, boolean cellHasFocus) {
            // 重置成员变量的值
           this.name = value.toString();
           this.icon = new ImageIcon("img\\list\\"+name+".gif");
           this.backGround = isSelected ? list.getSelectionBackground() : list.getBackground();
           this.forceGround = isSelected ? list.getSelectionForeground() : list.getForeground();
            return this;
        }

        @Override
        public Dimension getPreferredSize() {
            return new Dimension(60,80);
        }

        // 绘制列表项的内容
        @Override
        public void paint(Graphics g) {
            int imageWidth = icon.getImage().getWidth(null);
            int imageHeight = icon.getImage().getHeight(null);
            // 填充背景矩形
            g.setColor(backGround);
            g.fillRect(0,0,getWidth(),getHeight());
            // 绘制头像
            g.drawImage(icon.getImage(), this.getWidth()/2 -imageWidth/2, 10, null);
            // 绘制昵称
            g.setColor(forceGround);
            g.setFont(new Font("StSong", Font.BOLD, 18));
            g.drawString(this.name, this.getWidth()/2 - this.name.length()*20/2, imageHeight+30);

        }
    }

    public static void main(String[] args) {
        new ListCellRendererTest().init();
    }
}
```

## 树

> 树也是图形用户界面中使用非常广泛的GUI组件，例如使用Windows资源管理器时，可以看到目录树
>
> 使用Swing里的Jtree、TreeModel及其相关的辅助类可以很轻松地开发出计算机世界里的树。

### 创建树

> Swing使用JTree对象来代表一棵树，JTree树中结点可以使用TreePath来标识，该对象封装了当前结点及其所有的父结点。
>
> **当一个结点具有子结点时，该结点有两种状态：**
>
> - 展开状态：当父结点处于展开状态时，其子结点是可见的；
>
>
> - 折叠状态：当父结点处于折叠状态时，其子结点都是不可见的 。
>
>
> 如果某个结点是可见的，则该结点的父结点(包括直接的、间接的父结点)都必须处于展开状态，只要有任意一个父结点处于折叠状态，该结点就是不可见的 。

**JTree常用构造方法：**

```java
JTree(TreeModel newModel):
	使用指定的数据模型创建JTree对象，它默认显示根结点
JTree(TreeNode root):
	使用root作为根结点创建JTree对象，它默认显示根结点
JTree(TreeNode root, boolean asksAllowsChildren): 
	使用root作为根结点创建JTree对象，它默认显示根结点。
	asksAllowsChildren 参数控制怎样的结点才算叶子结点，如果该参数为true，则只有当程序使用 setAllowsChildren(false) 显式设置某个结点不允许添加子结点时(以后也不会拥有子结点)，该结点才会被JTree当成叶子结点；如果该参数为false，则只要某个结点当时没有子结点(不管以后是否拥有子结点)，该结点都会被JTree当成叶子结点。
```

**TreeNode继承体系及其使用：**

![TreeNode](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/TreeNode.png)

> 在构建目录树时，可以先创建很多DefaultMutableTreeNode对象，并调用他们的add方法构建好子父级结构，最后根据根结点构建一个JTree即可。

**代码演示：**

![image-20220501004303125](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220501004303125.png)

```java
public class SimpleJTree {
    JFrame jf = new JFrame("简单树");
    public void init(){
        // 创建DefaultMutableTreeNode对象代表结点
        DefaultMutableTreeNode root = new DefaultMutableTreeNode("中国");
        DefaultMutableTreeNode guangDong = new DefaultMutableTreeNode("广东");
        DefaultMutableTreeNode guangXi = new DefaultMutableTreeNode("广西");
        DefaultMutableTreeNode foShan = new DefaultMutableTreeNode("佛山");
        DefaultMutableTreeNode shanTou = new DefaultMutableTreeNode("汕头");
        DefaultMutableTreeNode guiLin = new DefaultMutableTreeNode("桂林");
        DefaultMutableTreeNode nanNing = new DefaultMutableTreeNode("南宁");

        // 组装结点之间的关系
        root.add(guangDong);
        root.add(guangXi);
        guangDong.add(foShan);
        guangDong.add(shanTou);
        guangXi.add(guiLin);
        guangXi.add(nanNing);

        // 创建JTree对象
        JTree tree = new JTree(root);

        //把JTree放入到窗口中进行展示
        jf.add(tree);

        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        jf.pack();
        jf.setVisible(true);

    }
    
    public static void main(String[] args) {
        new SimpleJTree().init();
    }
}
```

### 拖动、编辑树

> JTree生成的树默认是不可编辑的，不可以添加、删除结点，也不可以改变结点数据
>
> 如果想让某个JTree对象变成可编辑状态，则可以调用JTree的setEditable(boolean b)方法，传入true即可把这棵树变成可编辑的树(可以添加、删除结点，也可以改变结点数据)

**编辑树结点的步骤：**

- 获取当前被选中的结点：

```java
获取当前被选中的结点，会有两种方式：
	一：
		通过JTree对象的某些方法，例如 TreePath getSelectionPath() 等，得到一个TreePath对象，包含了从根结点到当前结点路径上的所有结点；
		调用TreePath对象的 Object getLastPathComponent()方法，得到当前选中结点
	二：
		调用JTree对象的 Object getLastSelectedPathComponent() 方法获取当前被选中的结点
```

- 调用DefaultTreeModel数据模型有关增删改的一系列方法，完成编辑，再方法执行完后，会自动重绘JTree

### 监听结点事件

> JTree专门提供了一个TreeSelectionModel对象来保存该JTree选中状态的信息，也就是说，JTree组件背后隐藏了两个model对象，其中TreeModel用于保存该JTree的所有结点数据，而TreeSelectionModel用于保存该JTree的所有选中状态的信息 。
>
> 程序可以改变JTree的选择模式，但必须先获取该JTree对应的TreeSelectionModel对象，再调用该对象的setSelectionMode(int mode)方法来设置该JTree的选择模式，
>
> 其中model可以有如下3种取值：
>
> 1. `TreeSelectionModel.CONTIGUOUS_TREE_SELECTION`: 可以连续选中多个TreePath。
> 2. `TreeSelectionModel.DISCONTIGUOUS_TREE_SELECTION`: 该选项对于选择没有任何限制 。
> 3. `TreeSelectionModel.SINGLE_TREE_SELECTION`: 每次只能选择一个 TreePath 。
>
> **为JTree添加监听器:**
>
> 1. `addTreeExpansionListener(TreeExpansionListener tel)` : 添加树结点展开/折叠事件的监听器。
> 2. `addTreeSelectionListener(TreeSelectionListener tsl)` : 添加树结点选择事件的监听器。

**代码演示：**

![image-20220501005830507](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220501005830507.png)

```java
public class SelectJTree {
    JFrame jf = new JFrame("监听树的选择事件");
    JTree tree;

    DefaultMutableTreeNode root = new DefaultMutableTreeNode("中国");
    DefaultMutableTreeNode guangdong = new DefaultMutableTreeNode("广东");
    DefaultMutableTreeNode guangxi = new DefaultMutableTreeNode("广西");
    DefaultMutableTreeNode foshan = new DefaultMutableTreeNode("佛山");
    DefaultMutableTreeNode shantou = new DefaultMutableTreeNode("汕头");
    DefaultMutableTreeNode guilin = new DefaultMutableTreeNode("桂林");
    DefaultMutableTreeNode nanning = new DefaultMutableTreeNode("南宁");

    JTextArea eventTxt = new JTextArea(5, 20);

    public void init() {

        // 通过add()方法建立父子层级关系
        guangdong.add(foshan);
        guangdong.add(shantou);
        guangxi.add(guilin);
        guangxi.add(nanning);
        root.add(guangdong);
        root.add(guangxi);

        tree = new JTree(root);

        // 设置选择模式，这里设置为每次只能选中一个结点
        TreeSelectionModel selectionModel = tree.getSelectionModel();
        selectionModel.setSelectionMode(TreeSelectionModel.SINGLE_TREE_SELECTION);

        // 设置监听器
        tree.addTreeSelectionListener(new TreeSelectionListener() {
            @Override
            public void valueChanged(TreeSelectionEvent e) {
                // 把当前选中结点的路径显示到文本域中
                TreePath newLeadSelectionPath = e.getNewLeadSelectionPath();
                eventTxt.append(newLeadSelectionPath.toString()+"\n");
            }
        });

        Box box = Box.createHorizontalBox();
        box.add(new JScrollPane(tree));
        box.add(new JScrollPane(eventTxt));

        jf.add(box);
        jf.pack();
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        jf.setVisible(true);
    }

    public static void main(String[] args) {
        new SelectJTree().init();
    }
}
```

### 改变结点外观

> JTree默认的外观是比较单一的，它提供了如下几种改变结点外观的方式：
>
> 1. 使用DefaultTreeCellRenderer直接改变结点的外观，这种方式可以改变整棵树所有结点的字体、颜色和图标
> 2. 为JTree指定DefaultTreeCellRenderer的扩展类对象作为JTree的结点绘制器，该绘制器负责为不同结点使用不同的字体、颜色和图标，通常使用这种方式来改变结点的外观 
> 3. 为JTree指定一个实现TreeCellRenderer接口的结点绘制器，该绘制器可以为不同的结点自由绘制任意内容，这是最复杂但最灵活的结点绘制器。
>
> 第一种方式最简单，但灵活性最差，因为它会改变整棵树所有结点的外观。在这种情况下，Jtree的所有结点依然使用相同的图标，相当于整体替换了Jtree中结点的所有默认图标。用户指定的结点图标未必就比JTree默认的图标美观 。

#### 第一种方法

> **使用DefaultTreeCellRenderer直接改变结点的外观**

**DefaultTreeCellRenderer提供了如下几个方法来修改结点的外观：**

```java
setBackgroundNonSelectionColor(Color newColor): 设置用于非选定结点的背景颜色
setBackgroundSelectionColor(Color newColor): 设置结点在选中状态下的背景颜色
setBorderSelectionColor(Color newColor): 设置选中状态下结点的边框颜色
setClosedIcon(Icon newIcon): 设置处于折叠状态下非叶子结点的图标
setFont(Font font) : 设置结点文本的字体
setLeaflcon(Icon newIcon): 设置叶子结点的图标
setOpenlcon(Icon newlcon): 设置处于展开状态下非叶子结点的图标
setTextNonSelectionColor(Color newColor): 设置绘制非选中状态下结点文本的颜色
setTextSelectionColor(Color newColor): 设置绘制选中状态下结点文本的颜色
```

#### 第二种方法

> **使用DefaultTreeCellRenderer的扩展类对象来改变结点外观**
>
> DefaultTreeCellRenderer实现类实现了TreeCellRenderer接口，该接口里只有一个用于绘制结点内容的方法: getTreeCellRendererComponent()
>
> DefaultTreeCellRenderer类继承了JLabel，实现getTreeCellRendererComponent()方法时返回this，即返回一个特殊的JLabel对象。
>
> 如果需要根据结点内容来改变结点的外观，则可以再次扩展DefaultTreeCellRenderer类，并再次重写它提供的getTreeCellRendererComponent()方法。

**代码演示：**

![image-20220501010741662](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220501010741662.png)

```java
public class ExtendsDefaultCellTreeRenderer {
    JFrame jf = new JFrame("根据结点类型定义图标");
    JTree tree;

    // 初始化5个图标
    ImageIcon rootIcon = new ImageIcon("img\\tree\\root.gif");
    ImageIcon databaseIcon = new ImageIcon("img\\tree\\database.gif");
    ImageIcon tableIcon = new ImageIcon("img\\tree\\table.gif");
    ImageIcon columnIcon = new ImageIcon("img\\tree\\column.gif");
    ImageIcon indexIcon = new ImageIcon("img\\tree\\index.gif");

    // 定义一个NodeData类，用于封装结点数据
    class NodeData{
        public ImageIcon icon;
        public String name;

        public NodeData(ImageIcon icon, String name) {
            this.icon = icon;
            this.name = name;
        }


    }

    // 定义几个初始结点
    DefaultMutableTreeNode root = new DefaultMutableTreeNode(new NodeData(rootIcon,"数据库导航"));
    DefaultMutableTreeNode salaryDb = new DefaultMutableTreeNode(new NodeData(databaseIcon,"公司工资数据库"));
    DefaultMutableTreeNode customerDb = new DefaultMutableTreeNode(new NodeData(databaseIcon,"公司客户数据库"));
    DefaultMutableTreeNode employee = new DefaultMutableTreeNode(new NodeData(tableIcon,"员工表"));
    DefaultMutableTreeNode attend = new DefaultMutableTreeNode(new NodeData(tableIcon,"考勤表"));
    DefaultMutableTreeNode concat = new DefaultMutableTreeNode(new NodeData(tableIcon,"联系方式表"));
    DefaultMutableTreeNode id = new DefaultMutableTreeNode(new NodeData(indexIcon,"员工ID"));
    DefaultMutableTreeNode name = new DefaultMutableTreeNode(new NodeData(columnIcon,"姓名"));
    DefaultMutableTreeNode gender = new DefaultMutableTreeNode(new NodeData(columnIcon,"性别"));

    public void init(){
        // 通过结点的add方法，建立结点的父子关系
        root.add(salaryDb);
        root.add(customerDb);
        salaryDb.add(employee);
        salaryDb.add(attend);
        customerDb.add(concat);
        concat.add(id);
        concat.add(name);
        concat.add(gender);
        // 创建树
        tree = new JTree(root);
        
        // 通过扩展的DefaultTreeCellRenderer修改外观
        tree.setCellRenderer(new MyRenderer());

        jf.add(new JScrollPane(tree));
        jf.pack();
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        jf.setVisible(true);
    }

    // 自定义类，继承DefaultTreeCellRenderer，完成结点的绘制
    private class MyRenderer extends DefaultTreeCellRenderer{
        // 重写方法
        @Override
        public Component getTreeCellRendererComponent(JTree tree, Object value, boolean sel, boolean expanded, boolean leaf, int row, boolean hasFocus) {
            // 当前类间接的继承了JLabel这个组件类，展示一张图片和一些配套的文字
            // Object value这个参数，代表的就是即将要绘制的结点
            // 获取当前结点
            DefaultMutableTreeNode node = (DefaultMutableTreeNode)value;
            // 获取到当前即将绘制的结点的名称和图标
            NodeData nodeData = (NodeData)node.getUserObject();
            // 通过setText方法和setIcon方法完成设置
            this.setText(nodeData.name);
            this.setIcon(nodeData.icon);
            return this;
        }
    }
    public static void main(String[] args) {
        new ExtendsDefaultCellTreeRenderer().init();
    }
}
```

#### 第三种方法

> **实现TreeCellRenderer接口改变结点外观**
>
> 这种改变结点外观的方式是最灵活的，程序实现TreeCellRenderer接口时同样需要实现getTreeCellRendererComponent()方法，该方法可以返回任意类型的组件，该组件将作为JTree的结点。
>
> 通过这种方式可以最大程度的改变结点的外观。

**代码演示：**

![image-20220501011336666](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220501011336666.png)

```java
public class CustomerTreeNode {
    JFrame jf = new JFrame("定制树的结点");
    JTree tree;

    //定义几个初始结点
    DefaultMutableTreeNode friends = new DefaultMutableTreeNode("我的好友");
    DefaultMutableTreeNode qingzhao = new DefaultMutableTreeNode("李清照");
    DefaultMutableTreeNode suge = new DefaultMutableTreeNode("苏格拉底");
    DefaultMutableTreeNode libai = new DefaultMutableTreeNode("李白");
    DefaultMutableTreeNode nongyu = new DefaultMutableTreeNode("弄玉");
    DefaultMutableTreeNode hutou = new DefaultMutableTreeNode("虎头");

    public void init() {
        // 组装视图
        friends.add(qingzhao);
        friends.add(suge);
        friends.add(libai);
        friends.add(nongyu);
        friends.add(hutou);

        tree = new JTree(friends);

        // 设置结点绘制器
        MyRenderer renderer = new MyRenderer();
        tree.setCellRenderer(renderer);

        jf.add(new JScrollPane(tree));
        jf.pack();
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        jf.setVisible(true);

    }

    // 自定义类，实现TreeCellRenderer接口，绘制组件
    private class MyRenderer extends JPanel implements TreeCellRenderer{
        private ImageIcon icon;
        private String name;
        private Color background;
        private Color foreground;

        @Override
        public Component getTreeCellRendererComponent(JTree tree, Object value, boolean selected, boolean expanded, boolean leaf, int row, boolean hasFocus) {
            // 给成员变量设置值
            this.icon = new ImageIcon("img\\tree\\"+value.toString()+".gif");
            this.name = value.toString();
            this.background = hasFocus? new Color(144,200,225) : new Color(255,255,255);
            this.foreground = hasFocus? new Color(255,255,3) : new Color(0,0,0);
            return this;
        }

        // 通过重写getPreferenceSize方法，指定当前Jpanel组件的大小
        @Override
        public Dimension getPreferredSize() {
            return new Dimension(80,80);
        }

        @Override
        public void paint(Graphics g) {
            // 绘制组件内容
            int iconWidth = this.icon.getIconWidth();
            int iconHeight = this.icon.getIconHeight();

            // 填充背景
            g.setColor(background);
            g.fillRect(0,0,getWidth(),getHeight());

            // 绘制头像
            g.drawImage(this.icon.getImage(),getWidth()/2 - iconWidth/2,10,null);

            // 绘制昵称
            g.setColor(foreground);

            g.setFont(new Font("StSong",Font.BOLD,18));
            g.drawString(this.name,getWidth()/2-this.name.length()*20/2,iconHeight+30);

        }
    }

    public static void main(String[] args) {
        new CustomerTreeNode().init();
    }
}
```

## 表格

### 创建简单表格

> **使用JTable创建简单表格步骤:**
>
> 1. 创建一个一维数组，存储表格中每一列的标题
> 2. 创建一个二维数组，存储表格中每一行数据，其中二维数组内部的每个一维数组，代表表格中的一行数据
> 3. 根据第一步和第二步创建的一维数组和二维数组，创建JTable对象
> 4. 把JTable添加到其他容器中显示

**代码演示：**

![image-20220501011532862](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220501011532862.png)

```java
public class SimpleTable {
    JFrame jf = new JFrame("简单表格");

    // 创建一维数组，存储标题
    Object[] titles = {"姓名","年龄","性别"};

    // 创建二维数组，存储数据
    Object[][] data = {
            {"李清照",29,"女"},
            {"苏格拉底",56,"男"},
            {"李白",35,"男"},
            {"弄玉",18,"女"},
            {"虎头",2,"男"}

    };

    public void init(){
        // 组装视图
        JTable jTable = new JTable(data,titles);
        jf.add(new JScrollPane(jTable));
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        jf.pack();
        jf.setVisible(true);
    }

    public static void main(String[] args) {
        new SimpleTable().init();
    }
}
```

### TableModel

> 与JList、JTree类似，JTable采用了TableModel来保存表格中的所有状态数据 : 
>
> 与ListModel类似，TableModel也不强制保存该表格显示的数据。 
>
> 虽然在前面程序中看到的是直接利用一个二维数组来创建JTable对象，但也可以通过TableModel对象来创建表格。

**使用TableModel步骤：**

1. 自定义类，继承AbstractTableModel抽象类，重写下面几个方法：

   ```java
   int getColumnCount():返回表格列的数量
   int getRowCount()：返回表格行的数量
   Object getValueAt(int rowIndex, int columnIndex)：返回rowIndex行，column列的单元格的值
   String getColumnName(int columnIndex)：返回columnIndex列的列名称
   boolean isCellEditable(int rowIndex, int columnIndex)：设置rowIndex行，columnIndex列单元格是否可编辑
   setValueAt(Object aValue, int rowIndex, int columnIndex)：设置rowIndex行，columnIndex列单元格的值
   ```

2. 创建自定义类对象，根据该对象，创建JTable对象

### DefaultTableModel

> Swing也为AbstractTableModel提供了一个DefaultTableModel实现类，程序可以通过这个实现类创建JTable对象。
>
> 通过DefaultTableModel创建JTable对象后，就可以调用它提供的方法来添加数据行、插入数据行、删除数据行和移动数据行

**DefaultTableModel提供了如下几个方法来控制数据行操作：**

```java
// 添加一列
addColumn(Object columnName)
addColumn(Object columnName, Object[] columnData)
// 在指定位置插入一行    
insertRow(int row, Object[] rowData)
// 移动指定范围内的数据行，将start和end表示的行移动到to表示的位置
moveRow(int start, int end, int to)
```

