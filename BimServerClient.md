To connect to a BIMserver you can use one of the 3 protocols: [SOAP](SOAP), [JSON](JSON API) or [Protocol Buffers](Protocol Buffers). To make connecting to a BIMserver even easier, there also is a Java library you can use.

# Get the client library

From version 1.5 on we are using Maven for all dependency management. We suggest you do too when using the BIMserver Client library as it makes installing all the required dependencies a lot easier.

You can find the Maven XML snippet in the latest release notes (for example https://github.com/opensourceBIM/BIMserver/releases/tag/parent-1.5.51). Make sure you match the version of the client with the version of your BIMserver. It  looks something like this:
```xml
<dependency>
    <groupId>org.opensourcebim</groupId>
    <artifactId>bimserverclientlib</artifactId>
    <version>1.5.51</version>
</dependency>
```

If you are using Eclipse for development, and you are not familiar with Maven yet, use [this tutorial](https://github.com/opensourceBIM/BIMserver/wiki/BimServerClientMavenEclipse).

Of course you can also use the client from source code, in that case download a source zip file, or checkout the projects from GIT.

# Examples

This example connects to a locally running BIMserver on port 8080, authenticates with the default username/password and then creates a new project.

```java
package org.opensourcebim;

import org.bimserver.client.BimServerClient;
import org.bimserver.client.json.JsonBimServerClientFactory;
import org.bimserver.interfaces.objects.SProject;
import org.bimserver.shared.ChannelConnectionException;
import org.bimserver.shared.UsernamePasswordAuthenticationInfo;
import org.bimserver.shared.exceptions.BimServerClientException;
import org.bimserver.shared.exceptions.PublicInterfaceNotFoundException;
import org.bimserver.shared.exceptions.ServiceException;

public class ClientDemo {
	public static void main(String[] args) {
		try {
			JsonBimServerClientFactory factory = new JsonBimServerClientFactory("http://localhost:8080");
			BimServerClient client = factory.create(new UsernamePasswordAuthenticationInfo("admin@bimserver.org", "admin"));
			
			SProject newProject = client.getServiceInterface().addProject("Test Project", "ifc2x3tc1");
			System.out.println(newProject.getOid());
		} catch (BimServerClientException e) {
			e.printStackTrace();
		} catch (ServiceException e) {
			e.printStackTrace();
		} catch (ChannelConnectionException e) {
			e.printStackTrace();
		}
	}
}
```

BIMserver 1.5 examples:

### Connect to a BIMserver
https://github.com/opensourceBIM/BimServerJavaClientDemo/blob/master/BimServerJavaClientDemo/src/org/opensourcebim/clientdemo/Connecting.java

### Create a project
https://github.com/opensourceBIM/BimServerJavaClientDemo/blob/master/BimServerJavaClientDemo/src/org/opensourcebim/clientdemo/CreateProject.java

### Checkin IFC file
https://github.com/opensourceBIM/BimServerJavaClientDemo/blob/master/BimServerJavaClientDemo/src/org/opensourcebim/clientdemo/CheckinIfcFile.java

# Older examples (might not work anymore in 1.5)

Examples on how to use the client-library can be found [here](https://github.com/opensourceBIM/BIMserver/tree/master/Tests/test/org/bimserver/tests/serviceinterface).

Examples on how to use the client-side EMF model can be found [here]( https://github.com/opensourceBIM/BIMserver/tree/master/Tests/test/org/bimserver/tests/emf)

Examples on how to use the low-level-calls from the client library are [here](https://github.com/opensourceBIM/BIMserver/tree/master/Tests/test/org/bimserver/tests/lowlevel)