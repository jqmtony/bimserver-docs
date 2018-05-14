When querying an IFC model, a hard question to answer is which information to give back. For example, a user might want to query all IfcDoor objects. Independent of the query language used, what should this query return? If it only returns IfcDoor objects, the results are not very useful (all references will be empty). So we probably want a little more. For example the properties of the object, but maybe also the geometry. Now geometry is already an interesting topic.

In short, the query should not only define how to query the objects, but also how far it should go following references to include in the results.

## Geometry

To get the positions of the objects right, we usually have to follow a path of IfcObjectPlacement objects. Doors for example are usually positioned relative to the walls they are a part of. But we don't want the Walls. So the only solution would be to internalize/accumulate the positions.