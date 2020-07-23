## IO



- InputStream/Reader: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- OutputStream/Writer: 所有输出流的基类，前者是字节输出流，后者是字符输出流。

File对象本身可以是目录。
调用file.mkdirs()即可创建目录。

如果是文件或者空目录，可以直接删除。
但如果目录中有文件或者子目录，则必须递归删除。

- 字节数组char[] 作为输入源的InputStream类是————ByteArrayInputStream
- 用文件作为输入源的InputStream类是？————FileInputStream
- 用字符串作为输入源的是？————StringBufferInputStream
- 用于多线程之间管道通信的输入源是————PipeInputStream

BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。

阻塞：一个线程读操作的时候不能去做其他操作。

NIO：Non IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用。
AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制。

