System requirements for running a BIMserver version 1.5:
- A working (outgoing) internet connection (from your BIMserver). BIMserver needs this in order to
  - Install plugins that are hosted on the internet
  - Send emails (via SMTP)
  - Connect to other BIMservers (for example when running remote services)
- A working (incoming) internet connection (to allow other people to connect)
- Disk (to store the database, logs, plugins etc...)
- Java 8 (or higher)

# Memory

The amount of required heap memory depends on what plugins you install, the size of your models and the amount of concurrent users of BIMserver. A rule of thumb is that you need about 15 times the size of the largest (unzipped) IFC file you want to be able to upload, times the maximum number of concurrent users. You can find more information [here](Memory-usage).

# JRE / JDK

You can download a JRE or JDK [here](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

Make sure you install a 64bit JRE/JDK if you have a 64bit system. The main advantage will be that you are going to be able to reserve more than 1300MB of memory, which you probably will want to.

For advanced queries you will need to use a JDK, for all other features a JRE will suffice.

# WAR

  * A JRE or JDK 8
  * A Servlet Specification 3.0 or higher based Container with WebSocket support (Tomcat 8 or higher, Jetty 8 or higher)

# JAR
  * A JRE or JDK 8