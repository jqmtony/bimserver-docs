快速入门指南
============

**注意：** BIMserver是其他人在其上构建应用程序的平台。 我们提供了一个稳定而灵活的平台来创建在线（开放式）BIM工具。

### **独立的 BIMserver**

1. 阅读wiki页面以获取[Requirements Version]文档的相关版本, 例如: (https://github.com/opensourceBIM/BIMserver/wiki/Requirements-1.4)

2. 确保可以通过双击JAR文件来执行JAR文件。如果没有, 请检查是否正确安装了[Java]（http://www.java.com），并且JAVA环境变量安装正确。在你的mac上，Oracle建议安装JDK（而不是JRE）。确保你已经安装了64位的Java.

3. 下载最新的[file(s)](https://github.com/opensourceBIM/BIMserver/releases)。如果需要，请阅读[which type of files to download](https://github.com/opensourceBIM/BIMserver/wiki/Download)。

4. .假设这是您的第一次安装，请在您的系统中创建一个新文件夹，例如C:\BIMserver
**注意**: 避免使用C：\ Program Files或C：\ Program Files（x86）等空格创建BIMserver子文件夹。

5. 将下载的bimserver-[version].jar复制到上述文件夹中，例如C:\BIMserver

6. 双击bimserver-[version].jar文件执行它.这将提取jar文件的内容并创建两个子文件夹，即home和bimserver-[version]。

7. 等待BIMserver展开所有文件并进行自我配置，直到出现“Server started successfully”。

8. 单击停止按钮停止BIMserver.

9. 对于1.5之前的BIMserver: 
- 从[here](https://github.com/opensourceBIM/bimvie.ws/releases)下载最新的开源BIMserver客户端，即bimvie-[version] .jar
- Copy the bimvie-[version].jar file into the plugins subfolder under the bimserver-[version] folder
- Click the Start button to restart BIMserver. Once the BIMserver has restarted, click Launch Browser
(in BIMserver 1.5 you are given the option to download bimvie.ws and ifcopenshell plugins during setup / step 10)

**如果上述步骤正确执行，则应在浏览器上成功启动BIMserver.** 如果失败，请使用另一个端口号重新启动BIMserver, 例如 http://localhost:8082

10. 首次启动BIMserver时，您需要设置管理登录。


**第三方(Third party)GUI:**

There are a few third party GUI available. Some are commercial products that you have to purchase a licence for, but there are a few that are free to try or shared freely by others, such as the open source bimvie.ws

**签入IFC模型**

1. Use http://localhost:[port]/admin/console.html or http://localhost:[port]/apps/console (depending on your version of BIMserver) and run the Checkin API to checkin a model, or
2. Use a GUI such as the bimvie.ws to checkin a model either into an existing project.

**查看模型**

bimvie.ws允许查看模型。或者，可以使用bimsurfer.org

**附加信息**

1. 请阅读 [安装指南](https://github.com/opensourceBIM/BIMserver/wiki/Setup)
2. 观看此视频 [Open Source BIMserver](http://www.youtube.com/watch?v=greB5jHi6JQ) video.
3. 根据经验法则，根据以下公式设置堆大小：每1MB的IFC文件，例如15MB。对于30MB的IFC文件，堆大小应至少为4Gb，以便正常使用插件。

### **在应用程序中使用BIMserver**

请参阅开发人员指南.