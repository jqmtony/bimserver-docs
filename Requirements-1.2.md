System requirements for running a BIMserver version 1.2
运行BIMServer 1.2版的系统要求

# Memory(内存)

The amount of required heap memory depends on what plugins you install, the size of your models and the amount of concurrent users of BIMserver. A rule of thumb is that you need about 15 times the size of the largest (unzipped) IFC file you want to be able to upload, times the maximum number of concurrent users. You can find more information [here](Memory-usage).
所需的堆内存量取决于所安装的插件、模型的大小以及BIMServer的并发用户数量。经验法则是，您需要大约15倍的最大（解压的）IFC文件的大小，您希望能够上传，倍的最大并发用户数。您可以在[此处]找到更多信息（内存使用）。


# JRE / JDK

You can download a JRE or JDK [here](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
你能在这下载JRE或者JDK.

Make sure you install a 64bit JRE/JDK if you have a 64bit system. The main advantage will be that you are going to be able to reserve more than 1300MB of memory, which you probably will want to.
如果您有64位系统，请确保安装64位JRE/JDK。主要的优势是，您将能够保留超过1300MB的内存，这可能是您想要的。

For advanced queries you will need to use a JDK, for all other features a JRE will suffice.
对于高级查询，您需要使用JDK，对于所有其他特性，JRE就足够了。

# WAR

  * A JRE or JDK 6, update 4 or higher
  * A Servlet Specification 3.0 or higher based Container with WebSocket support (Tomcat 7.0.27 or higher, Jetty 8 or higher)

  * JRE或JDK 6，更新4或更高版本
  * 基于servlet规范3.0或更高版本的容器，带WebSocket支持（Tomcat 7.0.27或更高版本、Jetty 8或更高版本）

# JAR
  * A JRE or JDK 6, update 4 or higher
  * JRE或JDK 6，更新4或更高版本

Explanation:
  * A JRE or JDK version 6 update 4 or higher is required because the BIMserver makes extensive use of JAXB 2.1.3, which only JREs and JDKs after update 4 have a reference implementation of
  * The BIMserver makes use of WebSockets, which are not a standard yet, but they are implemented in Jetty 8 and Tomcat 7 (you will need to have 7.0.27 at least)

说明：

  * 需要JRE或JDK版本6更新4或更高版本，因为BIMServer广泛使用JAXB 2.1.3，只有更新4后的JRE和JDK才参考了
  * BIMServer使用尚未成为标准的WebSockets，但它们在Jetty 8和Tomcat 7中实现（至少需要7.0.27）。
