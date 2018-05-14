# Introduction

The BIMserver project has a strong focus on interaction with other systems, that's what the (Service Interfaces)[Service Interfaces] are for (accessible via [SOAP](SOAP), [Protocol Buffers](Protocol Buffers) or [JSON](JSON-API)). However, some logic will only work (efficiently) when running within the BIMserver, that's where plugins come into play.

# Types of plugins

All plugins implement the Plugin interface:
```java
public interface Plugin {
	void init(PluginManager pluginManager) throws PluginException; // Initialization code, if your plugin requires other plugins, this is the time to check for them, be sure to throw a PluginException when something is wrong
	String getName(); // A short name of this plugin
	String getDescription(); // A short description of this plugin
	String getVersion(); // A version, not used for dependencies (yet)
	boolean isInitialized(); // Should return whether this plugin is successfully initialized
}
```

Do not implement the Plugin class directly, there are sub-interfaces for the different purposes plugins can have.

# Types of plugins

| Name | Functionality |
| ---- | ------------- |
| [Serializer](Serializer Plugin) | Create a serialized version of a model (can be text or binary)|
| [Deserializer](Deserializer Plugin) | Parse a serialized version of a model and store it in the database |
| [Render Engine](Render Engine Plugin) | Triangulates IFC geometry |
| [Query Engine](Query Engine Plugin) | Provides a way of querying a model |
| [Schema](Schema Plugin) | Provides the BIMserver with metadata about the models |
| [Object IDM](Object IDM Plugin) | Provides the BIMserver with a way of traversing objects |
| [Model Merge](Model Merge Plugin) | Merge multiple models into one model |
| [Model Compare](Model Compare Plugin) | Compare 2 models |
| [Service](Service Plugin) | Services can be triggered by certain events |

# So how to develop a plugin

> This tutorial assumes you use eclipse, but other IDEs should also work


> The easiest way to learn is to look how other people have done things, there are quite a few plugins already, so have a look at them.

  * Create a new java project for your plugin, for example "PluginTest"
![new project](http://bimserver.googlecode.com/svn/wiki/images/newproject.png)
  * Create your plugin class, this class must implement the plugin interface you want to write a plugin for, make sure you implement all methods correctly. For this example we will create a serializer and we will name the plugin "TestSerializerPlugin" in the package "test".
  * Create a plugin folder under your project
  * Create a plugin.xml file under the plugin folder, the content should like like this:
```xml
<?xml version="1.0" encoding="utf-8"?>
<PluginDescriptor>		
	<PluginImplementation>
		<interfaceClass>org.bimserver.plugins.serializers.SerializerPlugin</interfaceClass>
		<implementationClass>test.TestSerializerPlugin</implementationClass>
		<enabled>true</enabled>
	</PluginImplementation>
</PluginDescriptor>
```

Now to test your plugin locally you will have to tell the BIMserver where your plugin can be found. Edit "LocalDevPluginLoader.java" and  look for the lines with loadPluginFromEclipseProject. Add

```java
pluginManager.loadPluginsFromEclipseProject(new File("../PluginTest"));
```

Now you can start the BIMserver and your plugin should be available as yet another way to serialize models.

# JAR

To make your plugin available on deployed BIMservers (either WAR or JAR), you have to create a JAR file of your plugin. It should contain the compiled code, your plugin folder (+plugin.xml), and any required JAR files. The place of jar files doesn't matter, as long as they have the extension ".jar".

# License

BIMserver is an open framework that uses Plugins. Derivatives of bimserver.org code inherit the Affero GPL code. Plugins build from scratch can also be licensed under the GPL license (and used as plugin in BIMserver).  This does not go for snippets, GUIs and some separate (or remote) running services where BIMserver is a client to that service. Again: when in doubt, feel free to contact us.
More details and examples on [http://bimserver.org/license/](http://bimserver.org/license/)