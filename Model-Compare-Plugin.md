A compare plugin basically provides the functionality to the BIMserver to compare 2 models. Comparing has been made pluggable because comparing models is a hard problem in programming and requirements vary a lot.

# Details

The plugin just has one method, which returns the actual ModelCompare instance.

```java
public interface ModelComparePlugin extends Plugin {
        ModelCompare createModelCompare(PluginConfiguration pluginConfiguration) throws ModelCompareException;
}
```

The model compare instance looks like this:

```java
public interface ModelCompare {
        CompareResult compare(IfcModelInterface model1, IfcModelInterface model2, CompareType compareType) throws ModelCompareException;
}
```