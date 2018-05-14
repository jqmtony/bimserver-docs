To make it easier to evaluate BIMserver, a cross platform executable build is created that allows you to run a BIMserver on your desktop computer. The executable is a JAR file, most operating systems allow you to double click the file to start running it.

> (Windows) Make sure you do not put the .jar file in C:\Program Files or C:\Program Files (x86) because Windows plays funky tricks with those directories [http://www.hanselman.com/blog/VistasShowCompatibilityFilesAndTheScrumptiousWonderThatIsFileVirtualization.aspx]

  * Make sure you have a recent version of a Java Runtime Environment (JRE) or Java Development Kit (JDK), a JRE can be downloaded here http://java.com.
  * Download the latest JAR build from https://github.com/opensourceBIM/BIMserver/releases
  * Some browsers rename the JAR file, make sure it ends with ".jar"
  * Start the program by double clicking
  * Normally you wont have to change any settings and you can simply start the BIMserver by clicking "Start"

> (OSX) You can change the default JVM under Applications | Utilities | Java Preferences, on some OSX installations this somehow defaults to an older version of Java where the BIMserver needs version 8._

![JAR Runner](https://github.com/opensourceBIM/BIMserver/raw/master/Documentation/img/jar.png)

| Settings | Meaning |
| ---------------------------- |-------------|
| JVM | Allows for a JVM other than the default JVM to be selected (This feature seems to not be working for some people) |
| Home directory | Location of the home directory, this is where the database, log files etc... are stored. If you have ran a previous version of BIMserver on the same home directory, the database cannot always be migrated successfully |
| Address | The address the server will be binding on, if you want the BIMserver to be available on other machines than your own, you will have to change this to a real IP address or a hostname pointing to the right IP address |
| Port | The port must be free, and the firewall must be configured to allow listening on this port |
| Max heap size | The amount of heap memory appointed to the instance JVM of the BIMserver, more heap means larger models can be stored/retrieved. The amount of heap is limited by the amount of memory available on your machine, but be sure to always save a few hundred of MB's to your OS and other applications. On 32-bit Windows computers, the limit is around 1500MB. A Typical BIMserver will need at least 2GB |
| Max Perm Size | 256MB should be enough, if you are deploying a lot of plugins you might need more |
| Stack size | The amount of stack size available for every thread, you are probably not ever going to need more than 512KB. With a stack size that is too low, you will be getting StackOverflowError messages |
| Force IPv4 | On some operating systems binding will happen automatically on the IPv6 address of a machine, even if the user is not using IPv6. With this option you can override to use IPv4, only use this option if you have problems with this spcific issue |
| Use proxy server | You can check this option if you need to use a proxy server for _outgoing_ connections |
| Proxy Host | The host of your proxy server |
| Proxy Port | The port of your proxy server |