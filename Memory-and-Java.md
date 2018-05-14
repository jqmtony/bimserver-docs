Since a lot of people ask questions about BIMserver and configuring it's Heap Size, this page hopes to clarify some details.

Java, unlike most native software, needs to know the maximum amount of memory it can use upfront. You have to tell the JVM with the following argument: 


For 4GB
```
-Xmx4g
```

For 16GB
```
-Xmx16g
```

- This amount of memory cannot be more than the physical (of virtual) memory of your machine. It has to be less then that because other software (including the OS) will also use memory. Depending on what OS/software is installed, keep at least 1GB free for the other software.
- Java will not necessarily use all the memory that is given, it is just a maximum amount it can use
- BIMserver needs at least 1GB, but depending on your usage and plugins might also need 100GB!
- Your OS can be 32bit or 64bit, 32bit systems are limited to 4GB of physical memory.
- 32bit Java is limited to a variable amount of memory usually close to 1300MB, which is not much!
- You can still install a 32bit JVM on a 64bit OS, this makes no sense, remove the 32bit version.