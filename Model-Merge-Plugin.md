A model merge plugin basically provides the functionality to the BIMserver to merge 2 or more models. Merging has been made pluggable because merging  models is a hard problem in programming and requirements vary a lot.

# Details

The plugin just has one method, which returns the actual ModelMerger instance.

```java
public interface ModelMergerPlugin extends Plugin {
	ModelMerger createModelMerger(PluginConfiguration pluginConfiguration);
}
```

The model merger instance looks like this:

```java
public interface ModelMerger {
	IfcModelInterface merge(Project project, IfcModelSet modelSet, ModelHelper modelHelper) throws MergeException;
}
```

The returned IfcModelInterface must be a new instance. You cannot move/link objects in the given models in the new model. You have to copy them. The ModelHelper can help you with that. The given Project can be used for more information about the model (units for example). The IfcModelSet contains all the models that should be merged.