# Introduction

Checkin is the term used to upload complete models to a BIMserver. Because there are several ways to do it, and because it might not be very intuitive this page intends to clarify a few things.

# Usual way

The usual way would be to call the [https://github.com/opensourceBIM/BIMserver/blob/master/PluginBase/src/org/bimserver/shared/interfaces/ServiceInterface.java#L547](checkin) method on the ServiceInterface. Just look at the documentation for the method. It will return a topicId/checkinId, of which the use is explained (here)[# TopicId].

If you are using JSON, you'll have to encode the actual data (the ``data`` argument) in base64. Because of this, using this method is not the most efficient way of checking-in a file in BIMserver. It is however the most consistent way, because this call works just like all other (300) BIMserver calls.

Any immediate exception will be in the return message as well, just as all other calls to BIMserver. See [https://github.com/opensourceBIM/BIMserver/wiki/JSON-API#exception] for the JSON interface.

# Other way

Because of the overhead of the different implementations of the Service Interfaces (SOAP, PB, JSON) and because of the nature of the checkin method (pushing a lot of data, which makes streaming more suitable) there is another way. You can (HTTP) POST the data to ```/upload```. The required fields are:
```
token: The token you are using, this is the token you get from calling login
deserializerOid: Id of the deserializer you want to use
comment: A comment for this checkin
merge: Whether to merge or not (not working properly, just keep it to false)
poid: Id of the project
sync: Whether to call this synchronous or not
file: The actual data of the file you are uploading, make sure this is the last field
```

The upload servlet will return a bit of json, the structure:
```
{
  topicId: The topicId // Just like the one you get from calling ServiceInterface.checkin
}
```

If something goes wrong, it will return:
```
{
  exception: A message
}
```

# CheckinId / TopicId

CheckinId and TopicId are the same thing, recently this has been renamed everywhere to TopicId.

Get progress
```
Bimsie1NotificationRegistryInterface.getProgress
{
topicId
}
```

This will return:
```
{
  state: "STARTED" | "DONE" | "NONE" | "AS_ERROR",
  stage: "Name of the stage",
  title: "Title",
  progress: Percentage
}
```

When something went wrong (for example, the IFC file is supposedly invalid) the state will be "AS_ERROR". A description will be in the title field. The "stage" field contains a string describing the current stage of the process, which for example can be "Generating geometry...".

# Using notification
Register to be notified on a change in progress:
```
Bimsie1NotificationRegistryInterface.registerProgressHandler
{
topicId: topicId
endPointId: othis.server.endPointId
}
```

Read https://github.com/opensourceBIM/BIMserver/wiki/Endpoints to learn how to acquire an endPointId.

When there is new progress, the Bimsie1NotificationInterface.progress method will be called on the client.
```
{
  topicId: "",
  state: The same state object that is being returned by the call to Bimsie1NotificationRegistryInterface.getProgress.
}
```

Don't forget to unregister
```
Bimsie1NotificationRegistryInterface.unregisterProgressHandler
{
topicId: topicId
endPointId: othis.server.endPointId
}
```