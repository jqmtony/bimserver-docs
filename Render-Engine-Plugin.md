A Render Engine makes it possible to convert an IFC file to triangulated geometry.

```java
public interface RenderEnginePlugin extends Plugin {
	RenderEngine createRenderEngine(PluginConfiguration pluginConfiguration) throws RenderEngineException;
}
```

```java
public interface RenderEngine {
	RenderEngineModel openModel(File ifcFile) throws RenderEngineException;
	RenderEngineModel openModel(InputStream inputStream, int size) throws RenderEngineException;
	RenderEngineModel openModel(byte[] bytes) throws RenderEngineException;
	void close() throws RenderEngineException;
	void init() throws RenderEngineException;
}
```