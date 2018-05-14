To give users and developers the possibility to tweak the objects included in serialized versions of models, the ObjectIDM concept has been introduced in version 1.1. In previous versions of the BIMserver a similar concept called IgnoreFile has been present but those have been superseded by the ObjectIDMs.

# Plugin

An ObjectIDM is a plugin for the BIMserver. This is the interface:

```java
public interface ObjectIDM {

	boolean shouldFollowReference(EClass originalClass, EClass eClass, EStructuralFeature eStructuralFeature);
	boolean shouldIncludeClass(EClass eClass);
}
```

# shouldFollowReference

This can be quite tricky though in essense you just have to return whether a certain reference should be followed or not. The eClass variable contains a reference to the EClass of the object that is currently being evaluated, the originalClass contains a reference to the EClass of the object that is originally queried:
  * When getting an object by oid/guid this is the EClass of that object (not necessarily the currently being evaluated object!)
  * For getAllOfType queries this will be the EClass of the queried type (actually the type of every found object which could technically be a subtype of the queried type)
  * For all other cases including lazily loaded objects this is always the same as eClass

# shouldIncludeClass

You simply decide whether the given eClass should be included or not.

Example (include all furniture + geometry):
```java
	@Override
	public boolean shouldIncludeClass(EClass eClass) {
		return !Ifc2x3Package.eINSTANCE.getIfcRoot().isSuperTypeOf(eClass) || eClass == Ifc2x3Package.eINSTANCE.getIfcFurnishingElement(); 
	}
```

Beware, in the previous example you cannot just return "eClass == Ifc2x3Package.eINSTANCE.getIfcFurnishingElement()" because only objects of type IfcFurnishingElement will be included but none of the other classes, so for example no geometry will be included.

# For non-developers

For non-Java developers we have also created an XML based implementation of the ObjectIDM functionality. Checkout the FileBasedObjectIDM project in SVN.

## objectidm.xml
The objectidm.xml file should be structured the following way:

### PackageDefinition
Should be the root element<br/>
| Name | Description | Required | Default value |
| name | Name of the package | true | none |

### ClassDefinition
Child of PackageDefinition<br/>
|Name | Description | Required | Default value |
|name | Name of the class | true | none |
|include||"true" or "false" (Include this class and all its subclasses or not)||false||"true"||
|defaultFollow||"follow" or "ignore" (Follow all references by default, Ignore all references by default) | false | "follow"|
|followInverses | Whether to include inverse relations by default | false | "false" |
|origin | This ClassDefinition element is only taken into account when the origin class is the same | false | none |

### FieldDefinition
Child of ClassDefinition<br/>
|Name | Description | Required | Default value |
|name | Name of the field | true | none |
|follow | "true" or "false" (Follow this reference, Ignore this reference) | false | "true" |

When defining fields on a class, the settings for the fields apply to all subclasses of the given class, except when overridden in an explicitly defined subclass.

Precedence rules:
  * Explicitly defined ClassDefinitions will overrule the defaultInclude settings of PackageDefinition, so if PackageDefinition.defaultInclude = "exclude" and a ClassDefinition is created with no "include" attribute, it wil still be included because the default value of the include attribute within ClassDefinition is "true"

Examples (note: All redundant and optional attributes have been omitted)

Example 1 (include all types, do not follow the "LayerAssignments" and "StyledByItem" references from "Ifc2DCompositeCurve" and it's descendents)
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?> 
<PackageDefinition name="Ifc2x3">
    <ClassDefinition name="Ifc2DCompositeCurve">
        <FieldDefinition name="LayerAssignments" follow="false"/>
        <FieldDefinition name="StyledByItem" follow="false"/>
    </ClassDefinition>
</PackageDefinition>
```

Example 2 (include no types except IfcProject and IfcOwnerHistory, follow all references from both types, most won't be followed by the way because the types are not included)
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?> 
<PackageDefinition name="Ifc2x3" defaultInclude="exclude">
    <ClassDefinition name="IfcProject"/>
    <ClassDefinition name="IfcOwnerHistory"/>
</PackageDefinition>
```

Example 3 (This is the default in the BIMserver)<br/>
By default all inverses are ignored, but only the HasOpenings reference of IfcElement (and its descendents) will be followed.
Also the (inverse) references "IsDecomposedBy" and "ContainsElements" will be followed on every class that has them ONLY IF the origin class equals - or is a subclass of - "IfcBuildingStorey"
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?> 
<PackageDefinition name="Ifc2x3">
    <ClassDefinition name="IfcElement">
        <FieldDefinition name="HasOpenings" follow="true"/>
    </ClassDefinition>
    <ClassDefinition name="IfcObjectDefinition" origin="IfcBuildingStorey">
        <FieldDefinition name="IsDecomposedBy" follow="true"/>
    </ClassDefinition>
    <ClassDefinition name="IfcSpatialStructureElement" origin="IfcBuildingStorey">
        <FieldDefinition name="ContainsElements" follow="true"/>
    </ClassDefinition>
</PackageDefinition>
```