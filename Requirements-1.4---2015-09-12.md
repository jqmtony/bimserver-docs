System requirements for running a BIMserver version 1.4 starting with https://github.com/opensourceBIM/BIMserver/releases/tag/1.4.0-FINAL-2015-09-12

Before this build there were separate releases for Tomcat 7 and Tomcat 8, this was unmaintainable, hence the updated requirements.

# Memory

The amount of required heap memory depends on what plugins you install, the size of your models and the amount of concurrent users of BIMserver. A rule of thumb is that you need about 15 times the size of the largest (unzipped) IFC file you want to be able to upload, times the maximum number of concurrent users. You can find more information [here](Memory-usage).

# JRE / JDK

You can download a JRE or JDK [here](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

Make sure you install a 64bit JRE/JDK if you have a 64bit system. The main advantage will be that you are going to be able to reserve more than 1300MB of memory, which you probably will want to.

For advanced queries you will need to use a JDK, for all other features a JRE will suffice.

# WAR

  * A JRE or JDK 7
  * A Servlet Specification 3.0 or higher based Container with WebSocket JSR-356 (1.1) support (**Tomcat 7.0.56** or higher, Tomcat 8, Jetty 8 or higher)

# JAR
  * A JRE or JDK 7