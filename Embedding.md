# Introduction

> Updated for BIMserver 1.4

Sometimes it is useful to embed BIMserver in another application, this page describes how to do this.

## Details

### Create a BimServerConfig instance
(documentation: https://github.com/opensourceBIM/BIMserver/blob/master/BimServer/src/org/bimserver/BimServerConfig.java)
```java
// Example
BimServerConfig config = new BimServerConfig();
config.setStartEmbeddedWebServer(false);
config.setHomeDir(new File("[LOCATION]"));
config.setResourceFetcher(new LocalDevelopmentResourceFetcher(new File("[LOCATION]")));
config.setClassPath(System.getProperty("java.class.path"));
config.setPort(8080);
config.setStartCommandLine(false);
config.setLocalDev(true);
config.setAutoMigrate(false);
```
### Create a BIMserver instance
```java
BimServer bimServer = new BimServer(config);
```

### Load plugins
```java
// Example, if you point [LOCATION] to the BIMserver workspace directory, all plugins delivered with BIMserver will be loaded
LocalDevPluginLoader.loadPlugins(bimServer.getPluginManager(), new File[]{new File("[LOCATION]")});
```

### Start the server
```java
bimServer.start();
```

### Use the BIMserver. All useful objects are available with getters on the BimServer object.
```java
BimDatabase bimDatabase = bimServer.getDatabase();
SettingsManager settingsManager = bimServer.getSettingsManager();
ServiceInterface systemService = bimServer.getSystemService();
EmfSerializerFactory emfSerializerFactory = bimServer.getEmfSerializerFactory();
DiskCacheManager diskCacheManager = bimServer.getDiskCacheManager();
VersionChecker versionChecker = bimServer.getVersionChecker();
ServiceFactory serviceFactory = bimServer.getServiceFactory();
ServerInfo serverInfo = bimServer.getServerInfo();
MailSystem mailSystem = bimServer.getMailSystem();
PluginManager pluginManager = bimServer.getPluginManager();
MergerFactory mergerFactory = bimServer.getMergerFactory();
LongActionManager longActionManager = bimServer.getLongActionManager();
// Etcetera
```

The main interfaces (that are also available via SOAP/ProtocolBuffers/JSON):
```java
// Example getting the ServiceInterface
ServiceInterface si = bimServer.getServiceFactory().get(AccessMethod.INTERNAL).getServiceInterface();
```