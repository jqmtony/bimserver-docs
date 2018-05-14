System requirements for running a BIMserver version 1.2

# Memory

The amount of required heap memory depends on what plugins you install, the size of your models and the amount of concurrent users of BIMserver. A rule of thumb is that you need about 15 times the size of the largest (unzipped) IFC file you want to be able to upload, times the maximum number of concurrent users. You can find more information [here](Memory-usage).

# JRE / JDK

You can download a JRE or JDK [here](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

Make sure you install a 64bit JRE/JDK if you have a 64bit system. The main advantage will be that you are going to be able to reserve more than 1300MB of memory, which you probably will want to.

For advanced queries you will need to use a JDK, for all other features a JRE will suffice.

# WAR

  * A JRE or JDK 6, update 4 or higher
  * A Servlet Specification 3.0 or higher based Container with WebSocket support (Tomcat 7.0.27 or higher, Jetty 8 or higher)

# JAR
  * A JRE or JDK 6, update 4 or higher

Explanation:
  * A JRE or JDK version 6 update 4 or higher is required because the BIMserver makes extensive use of JAXB 2.1.3, which only JREs and JDKs after update 4 have a reference implementation of
  * The BIMserver makes use of WebSockets, which are not a standard yet, but they are implemented in Jetty 8 and Tomcat 7 (you will need to have 7.0.27 at least)