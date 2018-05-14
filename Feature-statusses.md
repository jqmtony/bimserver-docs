This page should reflect the current status of BIMserver features

If you test a functionality and it works, but it says "test" in this table, please let us know and we will update this document. If you are missing features, please let us know too. If you do not agree on certain decisions, also please let us know! If you want to adopt a feature, well you get the idea...

| Feature | Status | Future plans |
| ------------- | ------------- | ----- | ------ | 
| [https://github.com/opensourceBIM/CityGML](CityGML Serializer) | Not working | Moved to own repository |
| [https://github.com/opensourceBIM/Collada](Collada Serializer)| Not working | Moved to own repository |
| [https://github.com/opensourceBIM/ClashDetectionService](Clash Detection) | Not working | Moved to own repository. Maybe move BCF classes to Shared |
| [https://github.com/opensourceBIM/bimql](BIMQL) | Working, but not as documented (e.a. no geometry) | Decide whether to improve/fix or create a new query language |
| SceneJS Serializers | Not working, not updated | Have been removed entirely, not used anymore for SceneJS-based tools (BIMsurfer, BIMvie.ws). Geometry can now be downloaded with https://github.com/opensourceBIM/BinarySerializers |
| Java Query Engine | Working | Moved to own repository. Remove entirely, not used, too complex, inefficient, easier to write a plugin |
| AdminGUI | Removed | Removed |
| (https://github.com/opensourceBIM/Charting)[Charting] | Working | Moved to own repository |
| (https://github.com/opensourceBIM/JavaModelChecker)[JavaModelChecker] | Working | Moved to own repository. Remove, same reason as JavaQueryEngine |
| FileBasedObjectIDM | Removed | Removed |
| (https://github.com/opensourceBIM/IfcEngine)[IfcEngine] | Not working| Move to own repository |
| XSLT Serializer | Unknown|Remove, not used, inefficient |
| IFC 2x3tc1 Step Serializer | Working | Stable |
| IFC 4 Step Serializer | Working | Stable |
| IFC 2x3tc1 XML Serializer | Unknown | Test |
| IFC 4 XML Serializer | Unknown | Test |
| IFC 2x3tc1 Step Deserializer | Working | Stable |
| IFC 4 Step Deserializer | Working | Stable |
| IFC 2x3tc1 XML Deserializer | Unknown | Test |
| IFC 4 XML Deserializer | Unknown | Test |
| Basic Model Merger | Unknown | Test |
| GUID Based Model Merger | Unknown | Test |
| Name Based Model Merger | Unknown | Test |
| Sending Emails | Working | Remove, never seen a database server that sends emails |
| JSON API | Working | Stable |
| SOAP API | Unknown | |
| Protocol Buffers API | Unknown | |
| org.bimserver.database.query.conditions.* | Working | Will be removed in favor of a new query language |
| Database migrations | Working | Stable |
| BimServerClient Library Basics | Working | Stable |
| BimServerClient Library - Model modification| Unknown | Test |
| LowLevelCalls | More tests required | |
| JAR Starter | Working | Stable |
| WAR Build | Working | Stable |
| Generating Geometry | Working | Stable |
| Logging of events | Not working | Update |
| Plugin Manager | Working | Stable |
| Running services | Working | Stable, but not intuitive |
| [https://github.com/opensourceBIM/Console](Console) | Working | Moved to own repository |
| User/rights management | Not fully implemented | Check all actions |
| IFC Schema | Working | Will remove as plugin and integrate directly because there is only one implementation and interface is (too) large. Maybe even move .exp parsing logic to build process, and store serialized version of schema |
| GeoTag | Unknown | Test, maybe add to bimvie.ws |
| Revision tags | Unknown | Test, maybe add to bimvie.ws |
| Revision summary | Working | Maybe replace by new query function, for now stays supported |
| Checkouts | Unknown | Think about the usefulness, was meant for people working on the same model... |
| Model Checkers | Working | Stable, more implementations needed |
| Checkin via URL | Unknown | Test |