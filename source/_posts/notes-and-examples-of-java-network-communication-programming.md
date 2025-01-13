---
title: Java 网络通信编程的笔记和实例
date: 2021-09-13 14:35:00
categories: Java
tags:
  - 编程
  - Java
  - 网络通信
  - 网络编程
  - TCP
  - IP
  - UDP
  - URL
comments: true
---

本文整理了学习 Java 网络通信编程的笔记，并分析了若干程序实例，以巩固学习成果。

<!--more-->

## 1 网络通信编程笔记

### 1.1 网络程序设计基础

#### 1.1.1 基本概念

- 局域网（LAN）、广域网（WAN）
- IP 协议，IP 地址（IPv4，4 个字节表示）
- TCP 协议（传输控制协议）：类似拨打电话，固接连线，可靠性高，有顺序
- UDP 协议（数据用户报协议）：类似发送信件，无连接通信，可靠性低，不保证顺序
- 端口（port）：假想的连接装置，计算机与网络的物理连接，为整数
- 套接字（Socket）：假想的连接装置，连接程序与端口

#### 1.1.2 网络通信的要素

- **通信双方地址：** IP、端口号
- **网络通信协议：** TCP/IP 协议

### 1.2 TCP 程序设计基础

#### 1.2.1 `InetAddress` 类

与 IP 地址相关的类，注意该类会抛 `UnknownHostException` 异常

- IP 地址：
  - 本机 `localhost`（127.0.0.1）
  - IPv4（4 个字节组成，42 亿），IPv6（128 位，8 个无符 16 进制整数）
  - 公网（互联网），私网（局域网），ABCD 类地址
- 无构造器，不可被 `new`，只可被自己的方法返回
- 常用方法：
  - `getByName(String host)` 获取与 Host 对应的 `InetAddress` 对象
  - `getHostAddress()` 获取对象所包含的 IP 地址，返回 String
  - `getHostName()` 获取 IP 主机名，返回 String
  - `getLocalHost()` 获取本地主机的 `InetAddress` 对象

#### 1.2.2 `ServerSocket` 类

服务器套接字，等待网络请求，注意该类会抛 `IOException` 异常

- 端口：
  - 0~65535
  - 不同的进程用不同的端口号，类似于门牌
  - 端口分类：
    - 公有端口 0~1023（`HTTP` 80、`HTTPS` 443、`FTP` 21、`Telent` 23）
    - 程序注册端口 1024~49151（`Tomcat` 8080、`MySQL` 3306、`Oracle` 1521）
    - 动态（私有）端口 49152~65535
- `InetSocketAddress` 类：与 `InetAddress` 类似，加入了端口，可以 `new`，传入 String 地址和 int 端口，有 `getPort()` 等方法
- `ServerSocket` 用于等待网络请求，构造方法：
  - `ServerSocket()` 非绑定服务器套接字
  - `ServerSocket(int port)` 绑定特定端口
  - `ServerSocket(int port, int backlog)` 指定本机端口、指定的 `backlog`
  - `ServerSocket(int port, int backlog, InetAddress bindAddress)` 指定端口、侦听 `backlog` 和绑定到的本地 IP 地址
- `ServerSocket` 的常用方法：
  - `accept()` 等待客户机连接，若连接返回一个 Socket 套接字
  - `isBound()` 判断绑定状态
  - `getInetAddress()` 返回本地地址的 `InetAddress`
  - `isClosed()` 返回关闭状态
  - `close()` 关闭服务器套接字
  - `bind(SocketAddress endpoint)` 绑定到特定地址（IP 和端口）
  - `getLocalPort()` 获取等待端口

#### 1.2.3 TCP 网络程序

- 通信协议：速率、传输码率、代码结构、传输控制等

  - TCP/IP 协议：协议簇，最出名的是 TCP 协议和 IP 协议
  - TCP：连接、稳定，三次握手四次挥手，客户端服务端架构，传输完成释放连接，效率低
  - UDP：不连接、不稳定，客户端服务端无明确界限，效率高

- 参见第 2 章的实例

#### 1.2.4 Tomcat 基础

- Tomcat 是一个服务端，客户端通过浏览器进入
- 一般使用 8080 端口

### 1.4 UDP 程序设计基础

#### 1.4.1 UDP 通信

- 基本模式：
  - 数据打包（数据包），发往目的地
  - 接收数据包并查看
- 发送数据包步骤：
  - 创建数据报套接字（`DatagramSocket()`）
  - 创建发送的数据包（`DatagramPacket(byte[] buf, int offset, int length, InetAddress ip, int port)`）
  - 发送数据包（`DatagramSocket().send()`）
- 接收数据包步骤：
  - 创建数据报套接字并绑定到端口（`DatagramSocket(int port)`）
  - 创建字节数组接收数据包（`DatagramPacket(byte[] buf, int length)`）
  - 接收 UDP 包（`DatagramSocket().receive()`）

#### 1.4.2 `DatagramPacket` 类

- 表示数据包
- 构造方法：
  - `DatagramPacket(byte[] buf, int length)` 指定数据包的内存空间和大小
  - `DatagramPacket(byte[] buf, int length, InetAddress ip, int port)` 指定数据包的目标地址和端口

#### 1.4.3 `DatagramSocket` 类

- 表示发送和接收数据包的数据报套接字
- 构造方法：
  - `DatagramSocket()` 绑定到本地主机任何可用端口
  - `DatagramSocket(int port)` 绑定到本地主机指定端口
  - `DatagramSocket(int port, InetAddress ip)` 绑定到指定的本地地址

#### 1.4.4 UDP 网络程序

- 服务端、客户端没有明确的界限

- 参见第 3 章的实例


### 1.5 URL 类

统一资源定位器，通过地址定位互联网上的资源

- URL 的形式：`协议 : // ip 地址 : 端口 / 项目名 / 资源`

- 构造方法：传入字符串，`URL(String url)`

- 常用方法：

  - `getProtocol()` 获取协议名
  - `getHost()` 获取主机 IP
  - `getPort()` 获取端口
  - `getPath()` 获取路径
  - `getFile()` 获取完整路径
  - `getQuery()` 获取查询名

- 参见第 4 章的实例


## 2 TCP 网络程序示例

### 2.1 简单的接收器（服务端）、发送器（客户端）程序

接收器（`MyReceiver.java`）

```java
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class MyReceiver {
    public static void main(String[] args) {
        ServerSocket serverSocket = null;
        Socket socket = null;
        InputStream is = null;
        ByteArrayOutputStream baos = null;

        try {
            serverSocket = new ServerSocket(9005);	// 实例化一个服务器套接字，端口9005
            socket = serverSocket.accept();	// 使用套接字对象接收客户端连接该端口，连接后返回套接字，可以理解为“插座”
            is = socket.getInputStream();	// 获取“插座”套接字的输入流
            baos = new ByteArrayOutputStream();	// 实例化字节数组输出流
            byte[] buffer = new byte[1024];	// 字节数组
            int len;	//长度
            while ((len = is.read(buffer)) != -1) {	// 读取输入流到字节数组，若长度有效
                baos.write(buffer, 0, len);	// 则将字节数组写入到字节数组输出流中
            }
            System.out.println(baos);	// 打印字节数组输出流
        } catch (IOException e) {
            e.printStackTrace();
        } finally {	// 以下关闭流和套接字，注意顺序先打开的后关闭
            if (baos != null) {
                try {
                    baos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (is != null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (socket != null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (serverSocket != null) {
                try {
                    serverSocket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

发送器（`MyTransmitter.java`）

```java
import java.io.IOException;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;

public class MyTransmitter {
    public static void main(String[] args) {
        Socket socket = null;
        OutputStream os = null;
        try {
            InetAddress serverIP = InetAddress.getByName("127.0.0.1");	// 获取本地主机的IP对象
            int port = 9005;	// 端口9005
            socket = new Socket(serverIP, port);	// 使用IP和端口实例化套接字，可以理解为“插头”，并连接到“插座”
            os = socket.getOutputStream();	// 获取“插头”套接字的输出流
            os.write("你好！".getBytes());	// 输出流写入字符串转换的字节数组
        } catch (IOException e) {
            e.printStackTrace();
        } finally {	// 以下关闭流和套接字
            if (os != null) {
                try {
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (socket != null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

说明：

1. 服务端和客户端都需要有套接字，分别是 `ServerSocket` 和 `Socket`，并且绑定到统一端口。服务端还有一个 `Socket`，是连接端口的返回结果。
2. 注意输入流和输出流的使用，客户端使用 **输出流**，通过套接字输出；服务端使用 **输入流**，通过套接字输入，而这股输入流需要输出的话，还需要一个输出流。

先启动 `MyReceiver.java`，再启动 `MyTransmitter.java`，观察到 `MyReceiver.java` 输出：

```out
你好！
```

### 2.2 较复杂的服务端、客户端程序

服务端（`MyTCPServer.java`）

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

public class MyTCPServer {
    private ServerSocket server;
    private Socket socket;
    private BufferedReader reader;

    public static void main(String[] args) {
        MyTCPServer tcp = new MyTCPServer();	// 实例化服务端
        tcp.getServer();	// 服务端启动服务
    }

    void getServer() {	// 服务端启用服务方法
        try {
            server = new ServerSocket(8998);	// 实例化一个服务器套接字，端口8998
            System.out.println("服务器套接字创建成功！");
            while (true) {
                System.out.println("等待客户端连接...");
                socket = server.accept();	// 使用套接字对象接收客户端连接该端口，连接后返回“插座”套接字
                System.out.println("客户端连接成功！");
                reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));	// 将“插座”套接字的输入流放到缓存读取器中
                getClientMessage();	// 获取客户端信息
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private void getClientMessage() {	// 获取客户端信息方法
        try {
            while (true) {
                System.out.println("客户端：" + reader.readLine());	// 将缓存读取器的读取内容输出
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            if (reader != null) {
                reader.close();
            }
            if (socket != null) {
                socket.close();
            }
            if (server != null) {
                server.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

客户端（`MyTCPClient.java`）

```java
import javax.swing.*;
import javax.swing.border.BevelBorder;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.IOException;
import java.io.PrintWriter;
import java.net.Socket;

public class MyTCPClient extends JFrame {
    Container c;
    private final JTextArea ta = new JTextArea();
    private final JTextField tf = new JTextField();
    Socket socket;
    private PrintWriter writer;	// 这是一个字符输出流过滤器，可以对字符流进行处理

    public MyTCPClient(String title) {
        super(title);	// 标题
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        c = getContentPane();
        final JScrollPane sp = new JScrollPane();	// 滚动面板
        sp.setBorder(new BevelBorder(BevelBorder.RAISED));	// 设置滚动面板的边框为斜角边框，且为凸起斜面
        c.add(sp, BorderLayout.CENTER);	// 把滚动面板放到窗体中间
        sp.setViewportView(ta);	// 创建视口并设置视图为文本域（即把文本域放到滚动面板中）
        ta.setFont(new Font("微软雅黑", Font.PLAIN, 16));
        ta.setEditable(false);	// 文本域不可编辑
        c.add(tf, BorderLayout.SOUTH);	// 把文本框放到窗体底部
        tf.setFont(new Font("微软雅黑", Font.PLAIN, 16));
        tf.addActionListener(new ActionListener() {	// 监听文本框回车事件
            @Override
            public void actionPerformed(ActionEvent e) {
                writer.println(tf.getText());	// 将文本框的信息写入流，这股流会通过端口传输到服务端
                ta.append(tf.getText() + '\n');	// 在文本域中，把文本框写入的文本加进去
                ta.setSelectionEnd(ta.getText().length());	// 把文本域选择的结束位置放到文本域文本末尾
                tf.setText("");	// 清空文本框
            }
        });
    }

    public static void main(String[] args) {
        MyTCPClient client = new MyTCPClient("客户端系统");	// 实例化客户端
        client.setBounds(600, 300, 400, 400);
        client.setVisible(true);
        client.connect();	// 客户端连接
    }

    private void connect() {	// 客户端连接方法
        ta.append("尝试连接中...\n");
        try {
            socket = new Socket("127.0.0.1", 8998);	// 使用IP和端口实例化“插头”套接字，并连接到“插座”
            writer = new PrintWriter(socket.getOutputStream(), true);	// 将“插头”套接器的输出流放到过滤器中，true表示自动行刷新
            ta.append("完成连接！\n");
        } catch (IOException e) {
            ta.append("连接失败！请检查服务器端\n");
        }
    }
}
```

说明：

1. 服务端将获取客户端的方法放在无限循环中，以便无限接收客户端的信息。
2. 根据屏幕提示的信息，确定客户端是何时连接上服务端的。

先启动 `MyTCPServer.java`，再启动 `MyTCPClient.java`，在窗体输入文字，观察到窗体变化与 `MyTCPServer.java` 输出：

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/java/007.png)

```out
服务器套接字创建成功！
等待客户端连接...
客户端连接成功！
客户端：你好！
客户端：我是客户端
```

### 2.3 简单的文件传输程序

服务端（`FileReceiver.java`）

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class FileReceiver {
    public static void main(String[] args) throws IOException {
        ServerSocket server = new ServerSocket(9000);	// 端口9000
        Socket socket = server.accept();	// 监听客户端连接
        InputStream is = socket.getInputStream();	// 获得套接字输入流
        FileOutputStream fos = new FileOutputStream("receive.png");	// 实例化文件输出流
        byte[] buffer = new byte[1024];
        int len;
        while ((len = is.read(buffer)) != -1) {	// 读取套接字输入流
            fos.write(buffer, 0, len);	// 写入到文件输出流
        }

        BufferedWriter endBw = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));	// 获得套接字输出流，并放到缓存字符写入器中
        endBw.write("接收完毕");	// 写入字符串到输出流

        endBw.close();	// 关闭流与套接字
        fos.close();
        is.close();
        socket.close();
        server.close();
    }
}
```

客户端（`FileSender.java`）

```java
import java.io.*;
import java.net.Socket;

public class FileSender {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("127.0.0.1", 9000);	// 连接端口
        OutputStream os = socket.getOutputStream();	// 获得套接字输出流
        FileInputStream fis = new FileInputStream("send.png");	// 实例化文件输入流
        byte[] buffer = new byte[1024];
        int len;
        while ((len = fis.read(buffer)) != -1) {	// 读取文件输入流
            os.write(buffer, 0, len);	// 写入到套接字输出流
        }

        socket.shutdownOutput();	// 单向关闭客户端的套接字输出流

        BufferedReader endBr = new BufferedReader(new InputStreamReader(socket.getInputStream()));	// 获得套接字输入流，并放到缓存字符读取器中
        System.out.println(endBr.readLine());	// 打印读取器读取到的字符串

        endBr.close();	// 关闭流与套接字
        fis.close();
        os.close();
        socket.close();
    }
}
```

说明：

1. 示例中涉及到的输入输出流分别有（以输入流为例）：`InputStream`（接收套接字流）、`FileInputStream`（用于文件与系统间的传输流）、`InputStreamReader`（管道）、`BufferedReader`（用于处理服务端的回传字符）。
2. 注意客户端第 15 行的`socket.shutdownOutput();`，若不写这一行，即使客户端已写入完毕（并开始进行输入流的等待），服务端仍在继续读取（因为服务端第 13 行的 `is.read(buffer)`，读取的字节数已为 0，因此循环无法退出，处于读取 0 字节写入 0 字节的状态），因此要单向关闭客户端的套接字输出流，保证服务端结束读取。

先启动 `FileReceiver.java`，再启动 `FileSender.java`，观察到文件 `receive.png` 生成，以及 `FileSender.java` 输出：

```java
接收完毕
```

## 3 UDP 网络程序示例

### 3.1 简单的发送器、接收器程序

发送器（`MyUDPSender.java`）

```java
import java.net.*;

public class MyUDPSender {
    public static void main(String[] args) throws Exception {
        DatagramSocket socket = new DatagramSocket();	// 创建数据报套接字
        String msg = "你好！";
        byte[] buffer = msg.getBytes();
        InetAddress localhost = InetAddress.getByName("localhost");
        int port = 9090;
        DatagramPacket packet = new DatagramPacket(buffer, 0, buffer.length, localhost, port);	// 创建要发送的数据包，并绑定内容和发送地址
        socket.send(packet);	// 发送数据包
        socket.close();	// 关闭套接字
    }
}
```

接收器（`MyUDPReceiver.java`）

```java
import java.net.*;

public class MyUDPReceiver {
    public static void main(String[] args) throws Exception {
        DatagramSocket socket = new DatagramSocket(9090);	// 创建数据报套接字，绑定到9090端口
        byte[] buffer = new byte[1024];	// 创建缓存数组
        DatagramPacket packet = new DatagramPacket(buffer, 0, buffer.length);	// 创建要接收的数据包载体
        socket.receive(packet);	// 接收数据包（程序阻塞）
        System.out.println(packet.getAddress().getHostAddress());	// 获取数据包的地址
        System.out.println(new String(packet.getData(), 0, packet.getLength()));	// 获取数据包的数据
        socket.close();	// 关闭套接字
    }
}
```

说明：

1. 发送器和接收器都有数据报套接字 `DatagramSocket` 和数据包 `DatagramPacket`，但使用方式不同。
   - 发送器的 `DatagramSocket` 不用绑定端口，因为它只用 `send()` 方法，无需绑定发送者的端口；接收器的 `DatagramSocket` 需要绑定端口，因为它用 `receive()` 方法需要明确自己的地址。
   - 发送器的 `DatagramPacket` 需要绑定发送地址，因为它已有内容并打包好，需要向特定地址传递；接收器的 `DatagramPacket` 不用绑定地址，因为它是一个空的容器，只起接收载体作用。
2. 接收器第 10 行，`packet.getLength()` 不能用 `packet.getData().length` 代替，数据包大小与数据字节数组的大小是不同的。

先启动 `MyUDPReceiver.java`，观察到程序开始等待，再启动 `MyUDPSender.java`，观察到 `MyUDPReceiver.java` 输出：

```out
127.0.0.1
你好！
```

### 3.2 较复杂的聊天器程序（多线程）

聊天发送端线程（`MyChatSender.java`）

```java
import java.io.*;
import java.net.*;

public class MyChatSender implements Runnable {
    DatagramSocket socket = null;	// 数据报套接字
    BufferedReader reader = null;	// 缓存输入流
    private final String toIP;	// 发送的IP地址（理解为“写信时的对方地址”）
    private final int toPort;	// 发送的端口（理解为“写信时的对方门牌号”）

    public MyChatSender(String toIP, int toPort) {
        this.toIP = toIP;
        this.toPort = toPort;

        try {
            socket = new DatagramSocket();	// 创建发送端的套接字
            reader = new BufferedReader(new InputStreamReader(System.in));	// 缓存输入流放入系统输入
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    @Override
    public void run() {
        while (true) {
            try {
                String data = reader.readLine();	// 读取输入流的一行字符串
                byte[] dataBytes = data.getBytes();	// 字符串转换为字节数组
                DatagramPacket packet = new DatagramPacket(dataBytes, 0, dataBytes.length, new InetSocketAddress(toIP, toPort));	// 将字节数组打包，并绑定到对应IP和端口
                socket.send(packet);	// 发送数据包
                if ("bye".equals(data)) {	// 如果输入"bye"，退出发送循环
                    break;
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        try {
            reader.close();	// 关闭流
        } catch (IOException e) {
            e.printStackTrace();
        }
        socket.close();	// 关闭套接字
    }
}
```

聊天接收端线程（`MyChatReceiver.java`）

```java
import java.io.*;
import java.net.*;

public class MyChatReceiver implements Runnable {
    DatagramSocket socket = null;	// 数据报套接字
    private final String fromName;	// 来源的名字（理解为“写信时的寄件人姓名”）

    public MyChatReceiver(int port, String fromName) {
        this.fromName = fromName;
        try {
            socket = new DatagramSocket(port);	// 创建接收端的套接字，并绑定到一个接收端口
        } catch (SocketException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void run() {
        while (true) {
            try {
                byte[] container = new byte[1024];	// 字节数组容器
                DatagramPacket packet = new DatagramPacket(container, 0, container.length);	// 创建数据包容器
                socket.receive(packet);	// 阻塞式接收数据包
                byte[] dataBytes = packet.getData();	// 把数据包的缓存数据放到字节数组里
                String data = new String(dataBytes, 0, packet.getLength());	// 将缓存数据转换为字符串
                System.out.println(fromName + "：" + data);	// 输出
                if ("bye".equals(data)) {// 如果收到"bye"，退出接收循环
                    break;
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        socket.close();	// 关闭套接字
    }
}
```

聊天者 A（`ChatterA.java`）

```java
public class ChatterA {
    public static void main(String[] args) {
        new Thread(new MyChatSender("localhost", 9999)).start();	// 启动A的发送端线程
        new Thread(new MyChatReceiver(8888, "B")).start();	// 启动A的接收端线程
    }
}
```

聊天者 B（`ChatterB.java`）

```java
public class ChatterB {
    public static void main(String[] args) {
        new Thread(new MyChatSender("localhost", 8888)).start();	// 启动B的发送端线程
        new Thread(new MyChatReceiver(9999, "A")).start();	// 启动B的接收端线程
    }
}
```

说明：每个聊天者有两个线程，以及两个所需端口。A 向 `localhost` 的 9999 端口发信息，同时接收发送到 8888 端口名为 B 发来的信息；B 向 `localhost` 的 8888 端口发信息，同时接收发送到 9999 端口名为 A 发来的信息。

分别启动 `ChatterA.java` 和 `ChatterB.java`，并输入文字，观察到两个程序分别输出：

```out
你好！
B：你好啊！
今天天气真不错。
B：是啊，今天天气晴朗。
bye
B：bye
```

```out
A：你好！
你好啊！
A：今天天气真不错。
是啊，今天天气晴朗。
A：bye
bye
```

### 3.3 较复杂的广播、收音机程序

广播（`MyBroadcast.java`）

```java
import java.net.*;

public class MyBroadcast extends Thread {	// 实现线程
    String msg = "欢迎收听广播节目。";	// 广播信息
    int port = 9898;	// 端口9898
    InetAddress ipGroup;	//IP组
    MulticastSocket socket;	// 多点广播套接字

    MyBroadcast() throws Exception {
        ipGroup = InetAddress.getByName("224.255.10.0");	// 指定地址
        socket = new MulticastSocket(port);	// 创建一个指定端口的套接字
        socket.setTimeToLive(1);	// 指定发送范围是本地网络
        socket.joinGroup(ipGroup);	// 加入广播组
    }

    @Override
    public void run() {
        while (true) {
            byte[] data = msg.getBytes();	// 字节数组
            DatagramPacket packet = new DatagramPacket(data, data.length, ipGroup, port);	//数据打包并绑定地址端口
            System.out.println(new String(data));	// 输出广播信息
            try {
                socket.send(packet);	// 发送数据包
                sleep(3000);	// 休眠3秒
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) throws Exception {
        MyBroadcast broadcast = new MyBroadcast();	// 创建广播
        broadcast.start();	// 启动广播
    }
}
```

收音机（`MyRadio.java`）

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.net.*;

public class MyRadio extends JFrame implements Runnable, ActionListener {
    int port;	// 端口
    InetAddress ipGroup;	// IP组
    MulticastSocket socket;	// 多点广播套接字
    JButton start = new JButton("开始接收");	// 开始按钮
    JButton stop = new JButton("停止接收");	// 停止按钮
    JTextArea startArea = new JTextArea(10, 10);	// 广播信息区域（显示目前接收到的一条信息）
    JTextArea receivedArea = new JTextArea(10, 10);	// 接收信息区域（显示历史接收到的所有信息）
    Thread thread;	// 线程
    boolean isStopped = false;	// 是否停止

    public MyRadio() throws Exception {
        super("数据报收音机");	// 标题
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        thread = new Thread(this);	// 代理自己
        start.addActionListener(this);
        stop.addActionListener(this);	// 按钮添加监听器
        startArea.setForeground(Color.BLUE);
        JPanel north = new JPanel();	// 顶部面板
        north.add(start);
        north.add(stop);
        add(north, BorderLayout.NORTH);
        JPanel center = new JPanel(new GridLayout(1, 2));	// 中部面板
        center.add(startArea);
        center.add(receivedArea);
        add(center, BorderLayout.CENTER);
        validate();	// 刷新布局

        port = 9898;	// 端口9898
        ipGroup = InetAddress.getByName("224.255.10.0");	// IP组指定地址
        socket = new MulticastSocket(port);	// 套接字绑定端口
        socket.joinGroup(ipGroup);	// 套接字加入广播组

        setBounds(100, 50, 360, 380);
        setVisible(true);
    }

    @Override
    public void run() {
        while (true) {
            byte[] data = new byte[1024];	// 字节数组
            DatagramPacket packet = new DatagramPacket(data, data.length, ipGroup, port);	// 数据包容器
            try {
                socket.receive(packet);	// 接收数据包
                String msg = new String(packet.getData(), 0, packet.getLength());	// 缓存数据转换为字符串
                startArea.setText("正在接收的内容：\n" + msg);	// 打印在广播区域上
                receivedArea.append(msg + "\n");	// 添加在接收区域上
            } catch (Exception e) {
                e.printStackTrace();
            }
            if (isStopped) {	// 如果停止接收，退出循环
                break;
            }
        }
    }

    @Override
    public void actionPerformed(ActionEvent e) {	// 监听
        if (e.getSource() == start) {
            start.setBackground(Color.RED);
            stop.setBackground(Color.YELLOW);
            if (!(thread.isAlive())) {	// 如果线程不处于新建状态
                thread = new Thread(this);	// 新建线程
            }
            thread.start();	// 启动线程
            isStopped = false;
        }
        if (e.getSource() == stop) {
            start.setBackground(Color.YELLOW);
            stop.setBackground(Color.RED);
            isStopped = true;
        }
    }

    public static void main(String[] args) throws Exception {
        MyRadio radio = new MyRadio();	// 创建广播
        radio.setSize(460, 200);
    }
}
```

说明：

1. 发出广播和接收广播的地址必须位于同一个组内，地址范围是 224.0.0.0~224.255.255.255，该地址不代表某个特定主机的位置。
2. 加入同一个组的主机可以在某个端口广播信息，也可以在某个端口接收信息。

启动 `MyBroadcast.java`，观察到其开始输出；启动 `MyRadio.java` 并点击开始接收按钮，观察到其开始接收广播信息：

```java
欢迎收听广播节目。
欢迎收听广播节目。
欢迎收听广播节目。
```

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/java/008.png)

## 4 利用 URL 下载网络资源示例

```java
import javax.net.ssl.HttpsURLConnection;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.net.URL;

public class Main {
    public static void main(String[] args) throws Exception {
        URL url = new URL("https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png");	// 新建URL，并初始化为网络资源地址
        HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();	// URL开启连接
        InputStream is = connection.getInputStream();	// 获取连接输入流
        FileOutputStream fos = new FileOutputStream("logo.png");	// 新建文件输出流
        byte[] buffer = new byte[1024];
        int len;
        while ((len = is.read(buffer)) != -1) {	// 读入连接输入流
            fos.write(buffer, 0, len);	// 写入到文件输入流
        }
        fos.close();	// 关闭流与连接
        is.close();
        connection.disconnect();
    }
}
```

观察到文件 `logo.png` 的生成。

## 5 I/O 补充

**使用输入流、输出流时以下这段代码的原理：**

```java
FileInputStream fis = new FileInputStream("xxx");	// 文件输入流
OutputStream os = new OutputStream();	// 输出流
byte[] buffer = new byte[1024];
int len;
while ((len = fis.read(buffer)) != -1) {
    os.write(buffer, 0, len);
}
```

- `buffer` 是一个字节数组，长度为 1024，类似于一个缓冲区。
- `fis.read(buffer)`，这一步从输入流读取 `buffer` 大小的字节（即 1024），把这些字节赋给 `buffer`，并返回读取的字节数。
- 把读取的字节数赋值给 `len`，然后对 `len` 进行判断。
- 把 `buffer` 数组的 0 到 `len` 位置的字节写入输出流。
- 每次循环，`buffer` 就被重新赋值一次，因此文件大小与 `buffer` 的长度 1024 是没有关系的。
- 读取到输入流末尾时，`read()` 方法返回 -1，循环结束。
- 注意：缓冲区的大小不应太小，否则会涉及到编码问题，部分字节被强行拆分到两个缓冲区时可能会出现乱码。

