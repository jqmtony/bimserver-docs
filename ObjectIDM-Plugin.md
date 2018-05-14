An ObjectIDM helps BIMserver to decide whether to follow a relation/reference or not and whether to include/exclude certain classes. This can be used to define subsets of models, based on a given starting point.

```java
public interface ObjectIDMPlugin extends Plugin {
	ObjectIDM getObjectIDM(PluginConfiguration pluginConfiguration);
}
```

```java
public interface ObjectIDM {
	boolean shouldFollowReference(EClass originalClass, EClass eClass, EStructuralFeature eStructuralFeature);
	boolean shouldIncludeClass(EClass originalClass, EClass eClass);
}
```