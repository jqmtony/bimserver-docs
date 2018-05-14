To make communication with the BIMserver easier we have made a simple API library you can use. Of course you can also implement your own API in JavaScript.

# Requirements

From 2015-10-14 on, the old bimserverapi.js file has been splitted into multiple files. Here is a little bit of boilerplate code to load it, make sure you include "require.js":

Note: This is not yet working on NodeJS, if you figure out how to get it working, please let us know!

```
	requirejs.config({
	    baseUrl: address + "/js", // address should be the address of your bimserver, including optional port
	    urlArgs: "bust=" + version // the version you use here will be used for caching
	});

	requirejs(["bimserverapi_BimServerApi", "bimserverapi_BimServerApiPromise"], function(BimServerApi, BimServerApiPromise) {
		if (address.endsWith("/")) {
			address = address.substring(0, address.length - 1);
		}
		if (BimServerApi != null) {
			var bimServerApi = new BimServerApi(address, notifier);
			bimServerApi.init(function(){
				// This function gets called on success, you can use bimServerApi now
			});
		} else {
			// error
		}
	}, function (err) {
		// Error
	});
```

This new API is not depending on jquery anymore.

# Communication

The JavaScript API will communicate via JSON for most of the functions. Only downloading and uploading will sometimes be done without using JSON for performance reasons.

# Using the API

All methods in the [Service Interfaces](Service-Interfaces) can be called (make sure you look at the right documentation for your version). You must always use all parameters that are defined. Also when sending complex objects, you should define all the properties of the objects. The first call you will usually do is to login.

```javascript
	bimServerApi.login(username, password, rememberme, function(data){
		// Success
	});
```

Not all calls have been implemented in the JavaScript API, but it is very easy to use them anyways. The following example will list all the available projects:

```javascript
	bimServerApi.call("Bimsie1ServiceInterface", "getAllProjects", {onlyTopLevel: true, onlyActive: true}, function(data){
		data.forEach(function(project){
			console.log(project);
		});
	});
```

> TODO: Document the use of the Model class