## Proposal to deprecate

### Query plugin interface

This has never really worked. Complete model is first loaded into memory, and then the query algorithms do their thing. BimQL seems to be dead and the JavaQueryEngine is too complicated for non-developers.

Replacement: New query language (https://github.com/opensourceBIM/BIMserver/wiki/New-query-langage)

### Query AST

This is very old code, not used a lot anymore. The few places where this is used, it can also be replaced by the more efficient New query language (https://github.com/opensourceBIM/BIMserver/wiki/New-query-langage)

### XSLT serializer

Will never perform on realistic models, nobody ever used it

Reason to remove: Create expectation that XSLT is good for this

### SceneJS Project

Not used anymore, more generic BinaryGeometrySerializer and BinaryGeometryMessagingSerializer are now used.

Reason: People keep asking why it does not work. Will always be in git history, so let's remove it

### ObjectIDM's

Reason: Makes a lot of code needlesslee complex, can be replaced by new query language

### ClashDetection service

Reason: It's not working, as class detection has been removed from the RenderEngine interface