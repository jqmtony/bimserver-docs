To deserialize a stream of data to an object model.

```java
public interface DeserializerPlugin extends Plugin {
	Deserializer createDeserializer(PluginConfiguration pluginConfiguration);
	boolean canHandleExtension(String extension);
}
```

```java
public interface Deserializer {
	void init(SchemaDefinition schemaDefinition);
	IfcModelInterface read(File file) throws DeserializeException;
	IfcModelInterface read(InputStream inputStream, String fileName, long fileSize) throws DeserializeException;
}
```

You can subclass [EmfDeserializer](https://github.com/opensourceBIM/BIMserver/blob/master/Shared/src/org/bimserver/plugins/deserializers/EmfDeserializer.java) so you don't have to implement all methods.