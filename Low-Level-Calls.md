# Intro

To allow for more granular access to information stored in BIMserver, the "Low Level Calls" have been added.

# Workflow

## Start a transaction

First you call "startTransaction". The only parameter you have to give is the poid (ProjectID). This call will return a Transaction ID (tid). This Transaction ID will be used for all subsequent calls.

## Read/Write data

There are a lot of calls to read/write data. All of them can be found in the Bimsie1LowLevelInterfaceinterface. For example you can call 
```
getIntegerAttribute(tid, oid, "OverallWidth").
```

The oid is the ObjectID of the object of which you would like to get the attribute with the name "OverallWidth".

It's the same for changing attributes. For example you can call.
```
Bimsie1LowLevelInterface.setStringAttribute(tid, oid, "GlobalId", "GUID123455");
```

## Commit / Abort

After changing the model, you can decide to either commit the changes, or abort the transaction. When you abort the transaction all changes will be discarded, and there will not be a new revision. When you commit the changes, a new revision will be made.

```
bimsie1LowLevelInterface.commit(tid, "Comment");
```