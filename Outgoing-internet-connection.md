BIMserver (> 1.5) needs a working outgoing internet connection for the following purposes.

> Note: If you don't have a working outgoing internet connection, have a look [here](https://github.com/opensourceBIM/BIMserver/wiki/Installing-without-internet-connection)

# Updating/installing plugins
Address: http://central.maven.org/maven2

# Sending triggers to other servers (optional)
It will need HTTP connectivity to the servers you select

# Extended Data Schemas (optional)

To load a list of known extended data schemas BIMserver will fetch a JSON file from the default repository.
The URL: [Repository Location]/extendeddataschemas.json

The default value for the repository is https://raw.githubusercontent.com/opensourceBIM/BIMserver-Repository/master, but you can change this in server settings.

> Alternative: Install extended data schemas by hand, BIMvie.ws provides a GUI to do so.
