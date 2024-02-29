# java的IO&NIO

### 1.什么是IO流

它是一种数据的流从源头流向目的地。比如数据拷贝，输入流和输出流都考虑了。输入流从文件中读取数据存储到进程中，输出流从进程中读取文件然后写入目标文件。

### 2.java有几种类型的流

按照单位大小：字符流、字节流

按照流的方向：输出流、输入流

### 3.字节流和字符流应该怎么选择

1.缓存大多数采用字节流，因为字节流是字符流的包装，而大多数时候IO操作都是直接操作磁盘文件，所以这些流在传输时都是以字节的方式进行（图片等都是按字节存储）

2.对于操作需要通过IO在内存中频繁的处理字符串的情况下使用字符流，因为字符流具备缓冲区，提高了性能。

### 4.读取数据量很大的文件时，速度会很慢，如何选择流

字节流时，选择BufferedInputStream和BufferedOutputStream

字符流时，选择BufferedReader和BufferedWriter

### 5.IO模型有几种

阻塞IO、非阻塞IO、多路复用IO、信号驱动IO、异步IO

### 6.阻塞IO（blocking IO）

应用程序调用一个IO函数，导致应用程序阻塞，如果数据已经准备好，从内核拷贝大用户空间，否则一直等待下去。一个典型的读操作大致如下图，当用户进程调用recvfrom这个系统调用时，kernel就开始了IO的第一阶段：准备数据，就是数据被拷贝到内核缓冲区的一个进程（很多网络IO数据不会那么快到达，如没收一个完整的UDP包），等数据到操作系统内核缓冲区了，就到了第二阶段：将数据从内核缓冲区拷贝到用户内存，然后kernel返回结果，用户进程才会解除block状态，重写运行起来。

**blocking** **IO的特点在于IO执行的两个阶段用户进程都会block住**。

![image-20240229181305766](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240229181305766.png)

### 7.非阻塞I/O（nonblocking IO)

非阻塞IO模型，我们把一个套接口设置为非阻塞就是告诉内核，当所请求的IO操作无法完成时，不要将进程睡眠，而是返回一个错误。这样我么的IO操作函数将不断的测试数据是否已经准备好了。如果没准备好，继续测试，直到数据准备好了为止。在这个不断测试的过程中，会占用大量的CPU时间。

当用户进程发出read操作时，如果是kernel（内核）中数据还没准备好，那么并不会block用户进程，而是立即返回error，用户进程判断结果是error，就知道数据还没准备好，用户可以再次read，直到kernel中数据准备好，并且用户再一次法read操作，产生system call，那么kernel马上将数据拷贝到用户内存，然后返回。**nonblocking IO的特点是用户进程需要不断地主动询问Kernel数据准备好了没。**

阻塞IO一个线程只能处理一个IO流事件，要想同时处理多个IO流事件要么多线程，要么多进程，这样效率很低。而非阻塞IO可以一个进程处理多个流事件，只要不停地轮询所有流事件就行，但是当大多数流没数据时，也会造成浪费大量CPU资源。为了避免CPU空转，引进代理（select和poll）代理可以观察多个IO流事件，空闲时会把当前线程阻塞掉，当有一个或多个IO事件时，就从阻塞态醒来，把所以IO流轮询一遍，于是没有IO事件程序就阻塞在select方法处，问题是从select处，我们只能知道有IO事件发生，却不知道是哪几个流，依然需要轮询所有流，但是**epoll**这样的代理就可以通知那个流发生怎样的IO事件。

```
在许多并发连接中只有少数活路的场景下，epoll是Linux下I/O多路复用接口select/poll的增强版本，能有效提升系统CPU的使用率。区别于select和poll每次等待事件之前都需要重新设置监视的文件描述符集，epoll能复用文件描述符集来传递结果，减少了重复的准备工作。

获取事件时，epoll无需像select和poll一样遍历整个被侦听的描述符集，只需遍历被内核IO事件异步唤醒并加入到就绪队列的描述符集即可。这使得处理大量文件描述符时，只有实际产生活动的文件描述符才需要被处理，从而大大提升了效率。

当前，在大规模并发网络程序中，epoll已经成为首选模型。除了提供select/poll的IO事件电平触发（Level Triggered）模式，epoll还额外提供了边沿触发（Edge Triggered）模式，这使得用户空间程序可以缓存IO状态，减少epoll_wait/epoll_pwait的调用，从而进一步提升了程序的运行效率。
```

![image-20240229183703718](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240229183703718.png)

![image-20240229184859395](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240229184859395.png)

### 8.I/O多路复用模型（IO multiplexing)

IO多路复用就在于单个进程可以同时处理多个网络连接IO，基本原理就是select、poll、epoll这些函数会不断轮询所负责的所有socket，当某个socket有数据到达了，就通知用户进程，这三个function会阻塞进程，但和IO阻塞不同，这些函数可以同时阻塞多个IO操作，而且可以同时对多个读操作、写操作，IO检验，直到有数据到达，才真正调用IO操作函数。**IO多路复用的特点是通过一种机制一个进程能同时等待多个文件描述符，而这些文件描述符（套接字描述符）其中任意一个进入就绪状态，select函数就可以返回。**

IO多路复用的优势在于并发数较高的IO操作情况，可以同时处理多个连接，和blocking IO一样socket是被阻塞的，只不过在多路复用中socket是被select阻塞，而在阻塞IO中时被socket IO给阻塞。

![image-20240229184807905](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240229184807905.png)

### 9.信号驱动I/O模型

可以用信号，让内核在描述符就绪时发生SIGIO信号通知我们，通过sigaction（ **sigaction()*是一个 Unix 系统调用,用于改变进程接收到特定信号时的行为*。这是一个用于处理信号的非常强大的工具,它提供了对信号处理的更多控制,**）系统调用安装一个信号处理函数。该系统调用将立即返回，我们的进程继续工作，也就是说没有被阻塞。当数据报准备好读取时，内核就为该进程产生一个SIGIO信号。随后我们可在信号处理函数中调用recvfrom读取数据报，并通过主循环数据报已经准备好待处理。**特点：等待数据报到达期间进程不被堵塞。主循环可以继续执行，只要等待来自信号处理函数的通知：既可以是数据已准备好被处理，也可以是数据报已准备好被读取。**

![image-20240229185530105](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240229185530105.png)

### 10.异步I/O（asynchronous IO)

异步IO告知内核启动某个操作，并让内核在整个操作（包括将内核数据复制到我们自己的缓冲区）完成后通知我们，调用调用aio_read（Posix异步I/O函数以aio_或lio_开头）函数，给内核传递描述字、缓冲区指针、缓冲区大小（与read相同的3个参数）、文件偏移以及通知的方式，然后系统立即返回。我们的进程不阻塞于等待I/0操作的完成。当内核将数据拷贝到缓冲区后，再通知应用程序。 

用户进程发起read操作之后，立刻就可以开始去做其它的事。而另一方面，从kernel的角度，当它受到一个asynchronous  read之后，首先它会立刻返回，所以不会对用户进程产生任何block。然后，kernel会等待数据准备完成，然后将数据拷贝到用户内存，当这一切都完成之后，kernel会给用户进程发送一个signal，告诉它read操作完成了。

![image-20240229185748522](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240229185748522.png)

### 11.NIO与IO的区别

NIO就是New IO，这个库是在jdk1.4中引入。NIO和IO有相同的作用和目的，但实现方式不同，NIO主要用到的是块，所以NIO的效率较高。在JAVA API中提供了两套NIO，一套是针对标准输入输出NIO，另一套是网络编程NIO。

![image-20240229190342537](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240229190342537.png)

### 12.NIO和IO适用场景

NIO是为了弥补传统IO的不足而诞生的，NIO也有缺点，因为NIO是面向缓冲区的操作，每一次的数据处理都是对缓冲区进行的，在数据处理之前必须要判断缓冲区的数据是否完整或者已经读取完毕，如果没有，假设只读取了一部分，那么对不完整的数据处理没有任何意义。所以每次数据处理之前都要检测缓冲区数据。

- 如果需要管理同时打开成千上万个连接，这些连接每次只是发送少量的数据，例如聊天服务器，这时候用NIO处理数据。
- 如果是少量连接，而这些连接每次要发送大量的数据，这时候采用IO更合适。
- 使用哪种处理数据，需要再数据的响应等待时间和检查缓冲区数据的时间上作对比。

### 13.NIO核心组件

cahnnel、buffer、selector

### 14.什么是channel

一个channel代表和某一实体的连接，这个实体可以是文件、网络套接字等。通道是JAVA NIO提供的一座桥梁，用于程序和操作系统底层IO服务进行交互。

通道是一种很基本很抽象的描述，和不同的IO服务交互，执行不同的IO操作，实现不一样，因此具体的有FileChannel、SocketChannel等。

通道使用起来跟Stream比较像，可以读取数据到buffer，也可以把buffer中的数据写入通道。

区别：1.一个通道，既可以读又可以写，而一个Stream是单向的（分InputStream和OutputStream)

​            2.通道有非阻塞IO模式

![image-20240229191430386](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240229191430386.png)

### 15.java NIO中最常用的通道实现

FileChannel:读写文件

DatagramChannel:UDP协议网络通信

SocketChannel:TCP协议通信

ServerSocketChannel:监听TCP连接

### 16.Buffer是什么

NIO中所使用的缓冲区不是一个简单的byte数组，而是封装过的Buffer类，通过它提供的API，可以灵活的操纵数据。

与java的基本类型相对应，NIO提供了多种Buffer类，如ByteBuffer、CharBuffer、IntBuffer等，区别就是读写缓冲区时的单位长度不一样（以对应类型的变量为单位进行读写）

### 17.核心Buffer实现有哪些

ByteBuffer、CharBuffer、DoubleBuffer、FloatBuffer、IntBuffer、LongBuffer、ShortBuffer，涵盖了所有的基本数据类型（4类8种，除了Boolean).也有其他的buffer如MappedByteBuffer.

### 18.buffer读写操作基本操作

1. 将数据写入buffer
2. 调用buffer.flip()
3. 将数据从buffer中读取出来
4. 调用buffer.clear()或者buffer.compact()

在写buffer的时候，buffer会跟踪写入了多少数据，需要读buffer的时候，需要调用flip()来将buffer从写模式切换成读模式，读模式中只能读取写入的数据，而非整个buffer。

当数据都读完了，需要清空buffer以供下次使用：调用clear()或者调用compact()

二者区别 :clear方法清空整个buffer，compact方法只清除已经读取的数据，未读取的数据会被一到buffer开头，此时写入数据会从当前数据的末尾开始。

```
//创建一个容量为48的ByteBuffer
ByteBuffer buf = ByteBuffer.allocate(48);
//从channel中读（取数据再读）入buffer
int byteRead = inChannel.read(buf);
//读取buffer
while(byteRead != -1) {
   buf.flip();//转换buffer为读模式
   System.out.print((char)buf.get());//一次读取一个byte
   buf.clear();//清空buffer准备下一次写入
}
```

### 19.Selector是什么

选择器是一种特殊的组件，用于采集各个通道的状态（或者事件）。先将通道注册到选择器，并设置好关心的事件，然后通过调用select()方法，静静地等待事件发生。

### 20.通道可以监听几件事

- Accept:有可以接受的连接
- Connect:连接成功
- Read:有数据可读
- Write:可以写入数据了

### 21.为什么要用Selector

如果用阻塞IO，需要多线程（浪费内存），如果用非阻塞IO，需要不断重试（耗费CPU）。Selector的出现解决了这些问题，非阻塞模式下，通过Selector,线程只为已就绪的通道工作，不用盲目的重试。ex：当所有的通道都没有数据到达时，也就没有Read事件发生，线程会在select()方法处被挂起，从而让出CPU资源。

### 22.Selector处理多channel图文说明

要使用一个Selector,要先注册这个Selector的Channels.然后调用Selector的select()方法。这个方法会被阻塞，直到它注册的Channels当中有一个准备好了的事件发生。当select()方法返回的时候，线程可以处理这些事件，入新的连接到来，数据收到了等待。

![image-20240229202533600](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240229202533600.png)