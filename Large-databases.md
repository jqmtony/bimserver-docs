This page tries to explain why your database grows the way it does.

# Models

Obviously Building Information Models take up most of the space. BIMserver does versioning based on revisions. This means that every object will be stored with information about the project it belongs to, the revision it belongs to and it's own identifier.

When you are uploading multiple IFC files to the same project, all data will be stored. Even if only small parts of the model have changed, every object is stored again. The reason for this is that it's impossible for BIMserver to figure out what has changed, and which object have stayed the same.

That's why we also added the [low level calls](https://github.com/opensourceBIM/BIMserver/wiki/Low-Level-Calls), to actually change models by telling what has changed, instead of telling what the new complete model looks like.

# Geometry
In IFC geometry can be stored in a lot of different ways (extrusion, bounding box, b-splines, triangles etc...). Most software (and especially 3D viewers) can only handle one type of geometry: triangles. The process of converting the IFC geometry to triangles has been offloaded to the "render engine" in BIMserver, in most cases this is (https://github.com/IfcOpenShell/IfcOpenShell). Because this process can take quite a while we only want to go through it once and not every time a user for example visualizes a model. Therefore the triangles are also stored in the database. The amount of data this requires is very dependent on the types of geometry in the IFC file.

# Other data

Apart from the Building Information Models BIMserver also stores information about projects, revisions, users, checkouts, history etc... This data is also being versioned, but on a per-object basis. So if you change the name of a project, the project (object) will be stored again in the database with a new revision number. Usually these objects don't take up too much space, but certain access patterns can definitely make this explode.