> This was the design document, the actual implementation is documented [here](https://github.com/opensourceBIM/BIMserver/wiki/JSON-Queries)

BIMserver has had several ways of querying the stored models, but none have been very useful so far. This page will describe the requirements of a new query language that should replace all the other ways of querying data.

Requirements

- Should be easy to read/write, or easy to build higher-level languages on top of it
- Query by type (e.g. IfcDoor), with or without subtypes
- Query on exact values of fields (e.g. guid = "FHAJJAJJH", oid=20020)
- Use comparative expressions like >, <
- Use functions like substr etc...
- An efficient implementation should be possible (using indices, not loading the complete model in advance)
- Should basically be possible to create a query to get any sub-graph of any given model
- Should have the possibility to traverse a model (recursively)
- Should allow for having reusable pieces of query that are used a lot
- Should have the possibility to simplify for example the concept of Ifc properties via IfcPropertySet etc...
- It should be possible to "overlay" one query over another query. An example would be a model view definition, governing a specific view of the model, and a user querying within that domain.
- Programmatic extensibility, for complex query blocks
- Should at least be possible to have an idea of spatial queries

## Possible standards/influences
- http://www.jsoniq.org/
- XQuery
- OQL
- GraphQL (intro + good description of why REST sucks: https://facebook.github.io/react/blog/2015/05/01/graphql-introduction.html)

## GraphQL

Looks pretty cool. Main reasons for not choosing it:
- The actual query part seems to be quite limited, for example no range queries (>, <). We could implement "oid" and "guid" root calls server side. Maybe even as lists, and combined with type as well...
- Not only a new language, but also a new syntax, but there is a Java implementation of the parsing already (https://github.com/andimarek/graphql-java)

## Other considerations

- Will not be supplied as a plugin but integated in BIMserver. Code will be way to complex to support other "query engines" in a useful way.

## Current preload query in BIMvie.ws

This is not very intuitive, but can be used to query a model quite precisely.

The "defines" section declares reusable query-parts, the "queries" section the actual queries (which are all joined as OR)

The first query get's all IfcProject objects (usually just one). For every IfcProject, 2 includes are traversed. "IsDecomposedByDefine" will follow the "IsDecomposedBy" field, for every object it will follow "RelatedObjects", it will then recursively call itself, and "ContainsElementsDefine" and "Representation". And so on... This will basically read the whole decomposes-tree, with some sidesteps.

This example script is used in BIMvie.ws to preload the minimal amount of objects needed to create the tree/types/classifications/layers tabs on the left.

```javascript
var preLoadQuery = {
	defines: {
		Representation: {
			field: "Representation"
		},
		ContainsElementsDefine: {
			field: "ContainsElements",
			include: {
				field: "RelatedElements",
				include: [
					"IsDecomposedByDefine",
					"ContainsElementsDefine",
					"Representation"
				]
			}
		},
		IsDecomposedByDefine: {
			field: "IsDecomposedBy",
			include: {
				field: "RelatedObjects",
				include: [
					"IsDecomposedByDefine",
					"ContainsElementsDefine",
					"Representation"
				]
			}
		}
	},
	queries: [
	    {
			type: "IfcProject",
			include: [
				"IsDecomposedByDefine",
				"ContainsElementsDefine"
			]
	    },
	    {
	    	type: "IfcRepresentation",
	    	includeAllSubtypes: true
	    },
	    {
	    	type: "IfcProductRepresentation"
	    },
	    {
	    	type: "IfcPresentationLayerWithStyle"
	    },
	    {
	    	type: "IfcProduct",
	    	includeAllSubTypes: true
	    },
	    {
	    	type: "IfcProductDefinitionShape"
	    },
	    {
	    	type: "IfcPresentationLayerAssignment"
	    },
	    {
	    	type: "IfcRelAssociatesClassification",
	    	include: [
	    		{
	    			field: "RelatedObjects"
	    		},
	    		{
	    			field: "RelatingClassification"
	    		}
	    	]
	    },
	    {
	    	type: "IfcSIUnit"
	    },
	    {
	    	type: "IfcPresentationLayerAssignment"
	    }
	]
};
```

## JSON Based Query Language

Query on type
```
{
  type: "IfcProject"
}
```

Query on oid
```
{
  oid: 12345
}
```

Query on oids (OR)
```
{
  oids: [12346, 56789]
}
```

Query on GUID
```
{
  guid: "EBEBEBEBE"
}
```

Query on GUID's (OR)
```
{
  guids: ["EBEBEBEBEB", "CDCDCDCDCD"]
}
```

Using AND, OR, XOR
```
{
  or: [{
    type: "IfcWall"
  }, {
    type: "IfcWallStandardCase"
  }]
}
```

# Indices

Right now, BIMserver allows for indices on String fields, but only for the "Store" model, this is the model that contains all the meta-data like Project, Revision, User etc... With these indices it's a lot quicker to for example get a User by username, or Project by name.

This system will have to be extended for the IFC models and for more types.

## Types

- String, only exact matching for now, maybe add a case-insensitive one
- Integer, exact and comparative matching (=, >, >=, <, <=)
- Double, exact and comparative matching (=, >, >=, <, <=), have to look into the binary encoding of Java's double and weather it allows for lexicographic indexing
- Boolean, only exact matching (duh)
- References, to be determined

## Revisions

The complicating factor for the IFC models is that those are versioned.

## When to index

There are a few strategies
- Simply index everything (all types), this will slow down the write operations a lot, and also increase the database size.
- Add explicit indices to commonly queried fields in the code. Problem: how to determine the commonly queried fields. We could ask people to (automatically) share their query-patterns.
- Add dynamic indices based on usage pattern of a specific instance of BIMserver. This is the most complicated, although a very basic implementation (for example, just index every field that has ever been explicitly queried) should be possible.

Because deleting objects should not happen too often, we just leave the indices (we need them for previous revisions anyways). When a deleted object has been found via an expired index (for that particular revision), we just treat it as if there was no index at all.

## To figure out

- Do we want to be able to do queries over multiple projects? This has huge implication on the way indices are stored. For now I think no, user always has to provide the poid(s) and roid(s) that are to be queried.

# List of queries

- Get all IfcTask objects that are planned between date X and Y and get the related objects (IfcTask.OperatesOn)
- Get all IfcTask objects that are planned between Project start and now and get the related objects (IfcTask.OperatesOn)
- Get all IfcTask objects that are planned for today and get the related objects (IfcTask.OperatesOn)
- Get all objects linked to a given task with name/id X