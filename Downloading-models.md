Downloading models from a BIMserver is a two-step process.

> Note, when you are using a BIMserver client API, you probably don't need to implement any of this

# Step 1, initiate the download

The method you call is [ServiceInterface.download](https://thisisanexperimentalserver.com/apps/console/?interface=ServiceInterface&method=download). This method returns a TopicId (Long). This TopicId can be used to get the actual data described in [step2](#step-2-downloading-the-data). This process has been split over 2 methods because the process potentially takes a long time and could produce errors along the way. The TopicId can also be used to get information about the progress.

The download method has 4 parameters.
- roids (A set/list of roid). A roid can be acquired by called .oid on a [Revision](https://github.com/opensourceBIM/BIMserver/wiki/SRevision), .lastRevisionId on a [Project]([https://github.com/opensourceBIM/BIMserver/wiki/SProject]), also see [Revision](https://github.com/opensourceBIM/BIMserver/wiki/SRevision)
- query (A valid JSON query) When you are using the BIMserver API over JSON, you need to base64 this
- serializerOid. Tell BIMserver how to serialize the results of the query. See (Acquire serializer)
- sync. Whether this method should return right away (async) or wait for the process to finsh (sync)

More information about [Projects](https://github.com/opensourceBIM/BIMserver/wiki/SProject)
More information about [Revisions](https://github.com/opensourceBIM/BIMserver/wiki/SRevision)
More information about [Serializers](https://github.com/opensourceBIM/BIMserver/wiki/SSerializerPluginConfiguration)

# Step 2, downloading the data

## Using JSON/SOAP/Protocol Buffers

This is the most consistent way, because all other communication with BIMserver happens in the same way.

```
SDownloadResult result = ServiceInterface.getDownloadData(topicId);
DataHandler dataHandler = result.file;
```

## Using the download servlet (direct HTTP)

This is the preferred way.

There are two reasons why this alternative method exists:
- To allow the models to be downloaded by browsers, in a way that the downloaded file does not have to be "extracted" from another file (for example JSON).
- For efficiency reasons (for example when using the API over JSON, data would have to be encoded in base64, SOAP would have to do some mtom magic etc...)

The way to use this method is to send a HTTP GET to [yourbimserver]/download. The required parameters:

| Name | Description | Required |
|---|---|---|
| token | Your BIMserver auth token | Yes |
| topicId | The TopicId returned by the download method | Yes |
| zip | Whether to download the content in a ZIP file. Even if this argument is not "on" or not supplied, the content might still be compressed, this depends on the HTTP headers sent/received | No |
