**Note:** BIMserver is a platform for others to build applications on. We provide a stable and flexible platform to create online (open)BIM tools.

### **Stand-alone BIMserver**

1. Read the wiki page for the relevant version of [Requirements Version] document, e.g. (https://github.com/opensourceBIM/BIMserver/wiki/Requirements-1.4)

2. Make sure you can execute a JAR file by double-clicking a JAR file. If not, check that [Java](http://www.java.com) is installed properly and the JAVA environment variables are setup correctly. On you mac Oracle advises to install the JDK (instead of the JRE). Make sure you have 64bit java installed.

3. Download the latest [file(s)](https://github.com/opensourceBIM/BIMserver/releases). If needed, read about [which type of files to download](https://github.com/opensourceBIM/BIMserver/wiki/Download).

4. Assuming this is your first installation, create a new folder in your system, e.g. C:\BIMserver
**Note**: avoid creating BIMserver subfolder with spaces like C:\Program Files or C:\Program Files (x86)

5. Copy the downloaded bimserver-[version].jar into the above folder, e.g. C:\BIMserver

6. Double-click on the bimserver-[version].jar file to execute it. This will extract the content of the jar file and create two subfolders, i.e. home and bimserver-[version].

7. Wait for BIMserver to expand all the files and configure itself and until the phrase "Server started successfully" appears.

8. Click the Stop button to stop the BIMserver.

9. For BIMserver before 1.5: 
- Download the latest open source BIMserver client, i.e. bimvie-[version].jar from [here](https://github.com/opensourceBIM/bimvie.ws/releases).
- Copy the bimvie-[version].jar file into the plugins subfolder under the bimserver-[version] folder
- Click the Start button to restart BIMserver. Once the BIMserver has restarted, click Launch Browser
(in BIMserver 1.5 you are given the option to download bimvie.ws and ifcopenshell plugins during setup / step 10)

**If the above steps are followed correctly, you should have BIMserver launched successfully on a browser.** If failed, restart BIMserver with another port number, e.g. http://localhost:8082

10. The first time BIMserver is launched, you will need to set up the administration login.


**Third party GUI:**

There are a few third party GUI available. Some are commercial products that you have to purchase a licence for, but there are a few that are free to try or shared freely by others, such as the open source bimvie.ws

**Checkin an IFC model**

1. Use http://localhost:[port]/admin/console.html or http://localhost:[port]/apps/console (depending on your version of BIMserver) and run the Checkin API to checkin a model, or
2. Use a GUI such as the bimvie.ws to checkin a model either into an existing project.

**View the model**

bimvie.ws allows viewing of the model. Alternatively, one can use bimsurfer.org

**Additional Info**

1. Read [Setup Guide](https://github.com/opensourceBIM/BIMserver/wiki/Setup)
2. Watch this [Open Source BIMserver](http://www.youtube.com/watch?v=greB5jHi6JQ) video.
3. As a rule of thumb, set the heap size according to: 15MB per 1MB of IFC file, e.g. for 30MB of IFC file, the heap size should be at least 4Gb for normal use with plugins.

### **Use BIMserver in your application**

See Developers' Guide.