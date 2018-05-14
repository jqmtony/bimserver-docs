JSON is one of the three available channels to access the methods of the [Service Interfaces](Service-Interfaces).

The JSON interface is mainly there to facilitate connecting to the BIMserver from web applications/web sites. It is however also very useful when connecting from other applications like for example web servers or BIM/CAD software.

# Connecting from a web application/web site

For this purpose we have created a small JavaScript library, this library is being served by all BIMservers on [address]/js/bimserverapi.js. Your code should download it, and then instantiate the API like this: 
```
  var bimServerApi = new BimServerApi("http://examplebimserver:port");
```

Have a look at the [http://code.google.com/p/bimserver/wiki/JavaScriptApi JavaScriptApi] or [https://github.com/opensourceBIM/BIMserver/wiki/JavaScriptClient] page for more details on the JavaScript API.

# Connecting from non-javascript applications

For every call you should send an HTTP POST request to http://[address]:[port]/[optional context]/json. The body of this request should be an UTF-8 encoded JSON message. The structure of this message is the following:
```
{
  token: "ABCDEF1234567890",
  request: {
    interface: "ServiceInterface",
    method: "getAllProjects",
    parameters: {
      onlyTopLevel: true
    }
  }
}
```

Of course the required parameters depend on the actual method you are calling. All parameters are always required. For almost all calls you have to include a valid token. The token can be obtained by 2 methods: "login" or "autologin". Those 2 methods do not require a token themselves.

A typical response for a "login" call looks like this:
```
{
  response: {
    result: "ABCDEF012345567890"
  }
}
```

#Exception
When an exception occurs the message looks like this:
```
{
  response: {
    exception: {
      __type: "UserException",
      message: "User account has been deleted"
    }
  }
}
```

The "__type" parameters here helps client-side libraries to construct and throw appropriate exception.

# Multiple calls in one request

Sometimes it's useful to call multiple remote methods in one request. For example in asynchronous languages like javascript you sometimes want to combine the results of multiple calls, which would result in a sequence of chronologically executed calls, resulting in acummulated latency and unreadable code.

```
{
  token: "ABCDEF1234567890"
  requests: [{
    interface: "ServiceInterface",
    method: "getAllProjects",
    parameters: {
      onlyTopLevel: true
    }
  }, {
    ...another call...
  }]
}
```

Notice that the same token is used for all calls. The response looks like this:
```
{
  responses: [{
    result: "ABCDEF012345567890"
  }, {
    exception: {
      __type: "UserException",
      message: "User account has been deleted"
    }
  }]
}
```

In this response the first call executed successfully, and the second call failed. Note that all responses have the same array index as their corresponding request objects.