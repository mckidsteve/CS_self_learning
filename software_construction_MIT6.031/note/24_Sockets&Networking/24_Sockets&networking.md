# socket套接字 与 网络

## 客户端/服务端 设计模式
经典的C/S设计模式

## 套接字与流
- IP 地址
- 域名Hostname
- Port端口号
- 网络套接字 : 
    - 监听套接字 : 用于服务器创建监听来自客户端的连接`ServerSocket`
    - 连接套接字 : 用于向另一端的进程发送与接受消息`Socket`
- 缓冲区: 网络的发送端存入待发送数据,接受端放入待接受数据的地方
- 字节流(byte streams): 套接字程序的IO数据都是字节流
- 字符流(char streams): 将IO数据流转为RW数据流(允许Unicode)

### 阻塞
输入输出IO流也存在阻塞:
- 当入套接的缓冲为空,`read`调用阻塞,直到有数据
- 当出套接的缓冲满,`write`调用阻塞,直到有空

## network sockets in java
[zip](./ex24-echo.zip)
[code](./24code.md)
- 创建socket:
  ```java
  Socket socket = new(Socket(hostname , port))
  ```
- 创建IO流:
  ```java
  OutputStream outToServer = socket.getOutputStream();
  InputStream inFromServer = socket.getInputStream();
  ```
- RW流:
  ```java
  PrintWriter writeToServer = new PrintWriter(new OutputStreamWriter(outToServer , StandardCharsets.UTF_8));
  BufferedReader readFromServer = new BufferedReader(new InputStreamReader(inFromServer , StandardCharsets.UTF_8));
  ```
- 逻辑:
  ```java
  while(true){
    String Message = readFromUser.readLine();
    // readline可能会返回null,如果服务器关闭了连接
    ...
    writeToServer.println(message);
    // println会将信息存入printwriter的内部缓存,满了才会发到服务器
    ...
    String reply = readFromServer.readLine();
    ...
  }
  // 这三段代码都有可能阻塞
  ```

- 多线程服务端
如果想要用阻塞IO同时处理多个客户端,就需要用一个新的线程来处理新的IO,然后使用一个主线程专门接受连接申请

- 通过try-with-resources关闭数据流和套接字
适用于任何使用后应该关闭的对象: 字节流IO,字符流RW,fileIO,socket
```java
try{
    Socket socket = new Socket(hostname , port);
}{
    // read and write to socket
} catch(IOException ioe){
    ioe.printStackTrace();
} // socket.close() automatically
finally {
    // finally clause
}
```

## 有线协议
一组以字符序列表示的信息,例如许多应用程序都使用基于ASCII的协议
> [telnet](http://en.wikipedia.org/wiki/telnet)

- [HTTP](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)
- [SMTP](http://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol)
  
### 有线协议设计
- 减少不同信息的数量
- 每条信息都有明确的目的和连贯的行为
- 消息集必须足以满足客户端的要求,并允许服务器交付结果

### 有线协议规范指定
消息的前提条件是什么？例如，如果消息中的某个字段是一串数字，那么任何数字都有效吗？还是必须是服务器已知记录的 ID 号？

在什么情况下可以发送信息？某些信息是否只有在按特定顺序发送时才有效？

后置条件是什么？服务器将根据信息采取什么行动？哪些服务器端数据会被更改？服务器将向客户端发送什么回复？

## 测试C/S code
- 从数据结构与算法中分离网络代码
- 从流中分离套接字代码
