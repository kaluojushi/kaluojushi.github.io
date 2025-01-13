---
title: Java 华容道小游戏源码分析
date: 2021-09-08 17:08:00
categories: Java
tags:
  - 编程
  - Java
  - 游戏
  - Swing
  - GUI
comments: true
---

利用 Java 的 Swing 编程、事件监听等知识写一个华容道小游戏。这个游戏的作者不是我，但是我根据所学的知识，分析一下游戏的源码，以巩固学习成果。

<!--more-->

## 1 界面分析

华容道游戏的界面如下图所示：

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/java/EX01-001.jpg)

一共 10 个人物，每个人物可以上下左右移动。所以应提前想好一个布局方式，以便在写游戏时设定坐标。

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/java/EX01-002.png)

程序所需知识：

- Swing 基础
- Swing 基本组件（`JFrame`、`JButton`）
- 事件监听器（焦点、动作、键盘、鼠标）

## 2 启动类

在 IDEA 中新建一个项目 `Klotski`，并新建启动类 `Start.java` 和游戏窗体类 `GameFrame.java`。根据启动类和游戏主体分离的原则，我们在启动类 `Start.java`中只新建游戏窗体就行，不做任何与游戏内容相关的操作。

```java
public class Start {
    public static void main(String[] args) {
        new GameFrame();
    }
}
```

## 3 人物类

在设计游戏窗体之前，我们先新建一个人物类 `Person.java`，这个类用来实例化游戏中每一个人物对象。由于每一个人物对象实际上可以看做窗体的一个按钮，所以使该类继承 `JButton` 类。

```java
import javax.swing.*;

public class Person extends JButton {
}
```

我们为人物类设计三个属性，分别是编号、颜色和字体。编号在后期用于定位人物，颜色和字体方便构造人物时初始化。

```java
int number;
Color color = new Color(255, 245, 170);
Font font = new Font("微软雅黑", Font.BOLD, 12);
```

### 3.1 构造方法

人物类的构造方法要传进两个参数，分别是编号和人物名字，并在下面对编号、姓名、颜色、字体等进行初始化。

```java
public Person(int number, String s) {
    super(s);	// 调用父类JButton的构造方法，为按钮命名
    this.number = number;
    setBackground(color);
    setFont(font);
}
```

### 3.2 人物变色监听

当我们想让某个人物移动时，我们需要让其从默认颜色变为别的颜色。这里可以为每个按钮添加一个监听器，并且焦点事件监听器是最适合的。当按钮被聚焦的时候，鼠标键盘也可以对指定按钮进行操作。

可以直接让该类实现 `FocusListener` 接口，并在构造方法加上焦点事件监听器，把自己传入即可。

```java
public class Person extends JButton implements FocusListener {	// 实现FocusListener接口
    ...

    public Person(int number, String s) {
        ...
        addFocusListener(this);	// 添加焦点事件监听器
    }

    @Override
    public void focusGained(FocusEvent e) {
        setBackground(Color.RED);	// 按钮获得焦点时，颜色变红
    }

    @Override
    public void focusLost(FocusEvent e) {
        setBackground(color);	// 按钮失去焦点时，颜色变回默认值
    }
}
```

### 3.3 人物类完整代码

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.FocusEvent;
import java.awt.event.FocusListener;

public class Person extends JButton implements FocusListener {
    int number;
    Color color = new Color(255, 245, 170);
    Font font = new Font("微软雅黑", Font.BOLD, 12);

    public Person(int number, String s) {
        super(s);
        this.number = number;
        setBackground(color);
        setFont(font);
        addFocusListener(this);
    }

    @Override
    public void focusGained(FocusEvent e) {
        setBackground(Color.RED);
    }

    @Override
    public void focusLost(FocusEvent e) {
        setBackground(color);
    }
}
```

## 4 游戏窗体类

游戏窗体类是游戏的主体部分，包含显示模块和逻辑模块。首先我们让该类继承 `JFrame` 类。

```java
import javax.swing.*;

public class GameFrame extends JFrame {
}
```

该类首先要有 10 个人物，那么我们实例化一个 `Person` 数组；以及在属性中添加几个按钮，为边界按钮（处理游戏边界）和重新开始按钮。

```java
Person[] people = new Person[10];
JButton left, right, above, below;	// 左、右、上、下边界按钮
JButton restart = new JButton("重新开始");
```

### 4.1 构造方法

构造方法中只写与窗体默认设置有关的部分，具体的按钮布置放到一个初始化方法里面去写。

```java
public GameFrame() {
    init();	// 初始化方法
    setDefaultCloseOperation(DISPOSE_ON_CLOSE);
    setBounds(100, 100, 320, 500);
    setVisible(true);
    validate();	// 验证布局
}
```

### 4.2 初始化布置

在初始化方法 `init()` 中，我们一步步放置所需要的组件。首先把窗体设定为绝对布局，并放置重新开始按钮。

```java
public void init() {
    setLayout(null);
    add(restart);
    restart.setBounds(100, 320, 120, 35);
}
```

启动 `Start.java`，查看效果。

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/java/EX01-003.png)

接下来放置每个人物。先写一个 `String` 数组，放置每个人物的名字，再将名字传入本类的属性 `people` 数组中。

```java
String[] name = {"曹操", "关羽", "马", "黄", "赵", "张", "兵", "兵", "兵", "兵"};
for (int i = 0; i < name.length; i++) {
    people[i] = new Person(i, name[i]);
    add(people[i]);
}
```

设定每个人物按钮的大小和位置。观察第 1 章界面分析中的图，如果设置每个格子的大小为 50 像素 × 50 像素，那么曹操就是 100 像素 × 100 像素；如果游戏左上角的边界为 `(54, 54)`，那么曹操所在的位置应为 `(104, 54)`。依次类推，设置每个人物的大小和位置。

```java
// 以左上角的位置为(54, 54)，每个格子大小为50*50，设定每个按钮的位置
people[0].setBounds(104, 54, 100, 100);
people[1].setBounds(104, 154, 100, 50);
people[2].setBounds(54, 154, 50, 100);
people[3].setBounds(204, 154, 50, 100);
people[4].setBounds(54, 54, 50, 100);
people[5].setBounds(204, 54, 50, 100);
people[6].setBounds(54, 254, 50, 50);
people[7].setBounds(204, 254, 50, 50);
people[8].setBounds(104, 204, 50, 50);
people[9].setBounds(154, 204, 50, 50);
```

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/java/EX01-004.png)

再添加游戏边界，游戏边界也可以看做是按钮，我们将这 4 个按钮设定为宽为 5 的长条，并根据添加好的人物的位置确定长条位置。

```java
left = new JButton();
right = new JButton();
above = new JButton();
below = new JButton();
add(left);
add(right);
add(above);
add(below);
// 以宽为5设定每个长条的大小
left.setBounds(49, 49, 5, 260);
right.setBounds(254, 49, 5, 260);
above.setBounds(49, 49, 210, 5);
below.setBounds(49, 304, 210, 5);
validate();
```

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/java/EX01-005.png)

### 4.3 人物移动操作

写一个 `go()` 方法，使人物进行上下左右移动。传进去的参数肯定是人物和方向，人物放 `Person` 类对象就行，方向其实把已有的 4 个边界按钮放进去就行了，因为它们就代表上下左右，并且可以直接拿它们的边界。

```java
public void go(Person man, JButton direction) {
    
}
```

我们的操作判断分为以下几步：

1. 先试着移动一下，获取移动后的位置
2. 看是否与其他人物的位置或游戏边界的位置相撞
3. 如果相撞，则不移动；否则进行移动

先试着移动一下，为了方便判断相撞，我们先拿一个矩形类 `Rectangle` 放这个人物的边界，并获得坐标。

```java
Rectangle manRect = man.getBounds();	// 人物边界矩形
int x = manRect.x;
int y = manRect.y;
```

再试着移动一下，这里我们只操作 `x` 和 `y`，所以人物、矩形都是实际上没有移动的。

```java
if (direction == above) {
    y -= 50;
} else if (direction == below) {
    y += 50;
} else if (direction == left) {
    x -= 50;
} else if (direction == right) {
    x += 50;
}
```

再把矩形移动过去（人物还是未动）。

```java
manRect.setLocation(x, y);
```

从这时开始，我们要进行判断，先建立一个布尔型变量 `move` 并初始化为 `true`，并拿矩形类 `Rectangle` 放其他人物的边界、游戏边界的边界，使用 `intersects()` 方法判断是否相撞。

```java
boolean move = true;	// 移动判断变量
for (int i = 0; i < 10; i++) {
    Rectangle personRect = people[i].getBounds();	// 每个人物的边界
    if ((manRect.intersects(personRect) && (man.number != i))) {	// 操作人物的边界和某个人物边界相撞，且不是自己
        move = false;	// 不移动
    }
}
Rectangle directionRect = direction.getBounds();	// 游戏边界的边界
if (manRect.intersects(directionRect)) {	// 与游戏边界相撞
    move = false;	// 不移动
}
```

再根据 `move` 的结果，对该人物进行移动。

```java
if (move) {
    man.setLocation(x, y);
}
```

### 4.4 游戏重启监听器

点击重新开始按钮，游戏重启。我们让本类实现 `ActionListener` 动作事件监听器接口，并在 `init()` 方法中为 `restart` 添加监听器。然后重写 `actionPerformed()` 方法，可以直接销毁游戏窗体，再重新新建实例就行了。

```java
public class GameFrame extends JFrame implements ActionListener {	// 实现ActionListener接口
    ...
    public void init() {
        ...
        restart.addActionListener(this);	// 添加动作事件监听器
        ...
    }
	...

    @Override
    public void actionPerformed(ActionEvent e) {
        dispose();	// 销毁游戏窗体
        new GameFrame();	// 重新实例化
    }
}
```

### 4.5 键盘操作监听器

键盘是可以操作人物的移动的。我们让本类实现 `KeyListener` 键盘事件监听器接口，并在 `init()` 方法中为每个人物添加监听器。

```java
public class GameFrame extends JFrame implements ActionListener, KeyListener {	// 实现KeyListener接口
    ...
    public void init() {
        ...
        for (int i = 0; i < name.length; i++) {
            people[i] = new Person(i, name[i]);
            add(people[i]);
            people[i].addKeyListener(this);	// 添加键盘事件监听器
        }
        ...
    }
    ...

    @Override
    public void keyTyped(KeyEvent e) {
        
    }

    @Override
    public void keyPressed(KeyEvent e) {

    }

    @Override
    public void keyReleased(KeyEvent e) {

    }
}
```

我们只需要重写 `keyPressed()` 方法。先拿到键盘事件资源（当前聚焦的人物）和键盘按键，并指挥人物按一定方向移动。

```java
@Override
public void keyPressed(KeyEvent e) {
    Person man = (Person) e.getSource();	// 获得资源（人物）
    int keyCode = e.getKeyCode();	// 获得按键
    if (keyCode == KeyEvent.VK_UP) {	// 按上键
        go(man, above);	// 向上移动，下同
    }
    if (keyCode == KeyEvent.VK_DOWN) {
        go(man, below);
    }
    if (keyCode == KeyEvent.VK_LEFT) {
        go(man, left);
    }
    if (keyCode == KeyEvent.VK_RIGHT) {
        go(man, right);
    }
}
```

### 4.6 鼠标操作监听器

鼠标不仅可以聚焦人物，也可以操作人物移动。我们让本类实现 `MouseListener` 鼠标事件监听器接口，并在 `init()` 方法中为每个人物添加监听器。

```java
public class GameFrame extends JFrame implements ActionListener, KeyListener, MouseListener {	// 实现MouseListener接口
    ...
    public void init() {
        ...
        for (int i = 0; i < name.length; i++) {
            people[i] = new Person(i, name[i]);
            people[i].addKeyListener(this);
            people[i].addMouseListener(this);	// 添加鼠标事件监听器
            add(people[i]);
        }
        ...
    }
    ...
    
    @Override
    public void mouseClicked(MouseEvent e) {

    }

    @Override
    public void mousePressed(MouseEvent e) {

    }

    @Override
    public void mouseReleased(MouseEvent e) {

    }

    @Override
    public void mouseEntered(MouseEvent e) {

    }

    @Override
    public void mouseExited(MouseEvent e) {

    }
}
```

我们只需要重写 `MousePressed()` 方法。先拿到鼠标事件资源（鼠标点击的人物）和鼠标坐标，再拿到这个人物的边界大小，根据这个鼠标点击的位置（如点击人物上半部分，则向上移动），指挥人物按一定方向移动。

```java
@Override
public void mousePressed(MouseEvent e) {
    Person man = (Person) e.getSource();	// 获得资源（人物）
    int x = e.getX();
    int y = e.getY();	// 鼠标坐标（相对于人物）
    int w = man.getBounds().width;
    int h = man.getBounds().height;	// 人物边界大小
    if (y < h / 2) {	// 点击的是上半部分
        go(man, above);	// 向上移动，下同
    }
    if (y > h / 2) {
        go(man, below);
    }
    if (x < w / 2) {
        go(man, left);
    }
    if(x > w / 2) {
        go(man, right);
    }
}
```

注意，键盘事件和鼠标事件不用 `else-if`，可以让人物斜向运动，运动顺序由程序书写顺序决定。

### 4.7 游戏窗体类完整代码

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class GameFrame extends JFrame implements ActionListener, KeyListener, MouseListener {
    Person[] people = new Person[10];
    JButton left, right, above, below;
    JButton restart = new JButton("重新开始");

    public GameFrame() {
        init();
        setDefaultCloseOperation(DISPOSE_ON_CLOSE);
        setBounds(100, 100, 320, 500);
        setVisible(true);
        validate();
    }

    public void init() {
        setLayout(null);
        add(restart);
        restart.setBounds(100, 320, 120, 35);
        restart.addActionListener(this);

        String[] name = {"曹操", "关羽", "马", "黄", "赵", "张", "兵", "兵", "兵", "兵"};
        for (int i = 0; i < name.length; i++) {
            people[i] = new Person(i, name[i]);
            people[i].addKeyListener(this);
            people[i].addMouseListener(this);
            add(people[i]);
        }

        people[0].setBounds(104, 54, 100, 100);
        people[1].setBounds(104, 154, 100, 50);
        people[2].setBounds(54, 154, 50, 100);
        people[3].setBounds(204, 154, 50, 100);
        people[4].setBounds(54, 54, 50, 100);
        people[5].setBounds(204, 54, 50, 100);
        people[6].setBounds(54, 254, 50, 50);
        people[7].setBounds(204, 254, 50, 50);
        people[8].setBounds(104, 204, 50, 50);
        people[9].setBounds(154, 204, 50, 50);

        left = new JButton();
        right = new JButton();
        above = new JButton();
        below = new JButton();
        add(left);
        add(right);
        add(above);
        add(below);
        left.setBounds(49, 49, 5, 260);
        right.setBounds(254, 49, 5, 260);
        above.setBounds(49, 49, 210, 5);
        below.setBounds(49, 304, 210, 5);
        validate();
    }

    public void go(Person man, JButton direction) {
        Rectangle manRect = man.getBounds();
        int x = manRect.x;
        int y = manRect.y;
        if (direction == above) {
            y -= 50;
        } else if (direction == below) {
            y += 50;
        } else if (direction == left) {
            x -= 50;
        } else if (direction == right) {
            x += 50;
        }
        manRect.setLocation(x, y);

        boolean move = true;
        for (int i = 0; i < 10; i++) {
            Rectangle personRect = people[i].getBounds();
            if ((manRect.intersects(personRect) && (man.number != i))) {
                move = false;
            }
        }
        Rectangle directionRect = direction.getBounds();
        if (manRect.intersects(directionRect)) {
            move = false;
        }

        if (move) {
            man.setLocation(x, y);
        }
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        dispose();
        new GameFrame();
    }

    @Override
    public void keyTyped(KeyEvent e) {

    }

    @Override
    public void keyPressed(KeyEvent e) {
        Person man = (Person) e.getSource();
        int keyCode = e.getKeyCode();
        if (keyCode == KeyEvent.VK_UP) {
            go(man, above);
        }
        if (keyCode == KeyEvent.VK_DOWN) {
            go(man, below);
        }
        if (keyCode == KeyEvent.VK_LEFT) {
            go(man, left);
        }
        if (keyCode == KeyEvent.VK_RIGHT) {
            go(man, right);
        }
    }

    @Override
    public void keyReleased(KeyEvent e) {

    }

    @Override
    public void mouseClicked(MouseEvent e) {

    }

    @Override
    public void mousePressed(MouseEvent e) {
        Person man = (Person) e.getSource();
        int x = e.getX();
        int y = e.getY();
        int w = man.getBounds().width;
        int h = man.getBounds().height;
        if (y < h / 2) {
            go(man, above);
        }
        if (y > h / 2) {
            go(man, below);
        }
        if (x < w / 2) {
            go(man, left);
        }
        if (x > w / 2) {
            go(man, right);
        }
    }

    @Override
    public void mouseReleased(MouseEvent e) {

    }

    @Override
    public void mouseEntered(MouseEvent e) {

    }

    @Override
    public void mouseExited(MouseEvent e) {

    }
}
```

## 5 扩展与总结

- **步数统计：** 可以新写一个属性 `step`，每 `go()` 一次就 `step++`，统计游戏步数。
- **成功判定：** 当曹操走到某个坐标的时候，游戏就成功并结束了。
- **不同关卡：** 华容道有横刀立马、齐头并前、兵分三路等多种摆放方法，可以只改变人物坐标，实现不同的关卡。
- **记录进度：** 把步数、人物坐标存档到文件里，下次可以直接读档，接着来。
- **平台迁移：** 把代码换成 JavaScript 版，并写到网页上，游戏基本逻辑是一样的。

这个游戏应该不难，逻辑上没有其他游戏那么多，主要是巩固 Swing 编程的基础。

走了好多遍终于成功了：

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/java/EX01-006.png)

[完整代码点这里获取](https://github.com/kaluojushi/MyJavaPersonalDevelopment/tree/main/Klotski)
