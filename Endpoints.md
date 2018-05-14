When connecting to BIMserver from another machine/process, you usually use the JSON, ProtocolBuffers or SOAP calls. Those are RPC-like calls that do or do not return something in a synchronous way.

Sometimes you want the communication to be issued the other way around. You want BIMserver to initiate. For this to work, the remote side has to tell the BIMserver it is interested.

# Step 1 - Register an endpoint

The remote side has to register an EndPoint. For now the only type of EndPoint available is a WebSocket. You have to setup a WebSocket to http://[YOUR HOST]:[OPTIONAL PORT]/[OPTIONAL CONTEXT]/stream. BIMserver will send a JSON message:
```
{
  welcome: 12345
}

The number in this message is the current time as UTC milliseconds from the epoch.
```
You have to send your token next, this is the same token you use for normal calls to BIMserver
```
{
  token: ABCDE...
}
```
Then BIMserver will send you an EndPointID, this EndPoint will be matched with the User linked to the token you sent earlier.
```
{
  endpointid: 12345...
}
```

# Step 2 - Register events

Now you can use this EndPointID when registering for certain events, such as progress on some action:
``` 
Bimsie1NotificationRegistryInterface.registerProgressHandler(topicId, endPointId);
```

In the previuos example the topicId is being returned by for example the Bimsie1ServiceInterface.checkin method.

# JavaScript

Are you using the BIMserver JavaScript API? Then you can register events a bit easier. The API will setup the WebSocket for you.

```
bimServerApi.registerProgressHandler(topicId, handler, callback);
```

In this example, *handler* should be a function reference to the function that should be executed when the progress changes, the *callback* will be called after the *handler* is successfully registered.

# Unregistering

To make sure no memory and processing power is wasted, please remember to unregister any unused handlers.