For the BIMserver 1.5 release, plugins have changed quite a bit.

For starters they depend on maven now. In the long term the plan is to also support non-maven plugins, but for now it's a requirement. To make the transition for older plugins easier, and to help developers of new plugins, this document describes how it works.

BIMserver specific terminology:
- Plugin: A plugin (implementing org.bimserver.plugins.Plugin)
- PluginBundle: A collections of 1 or more plugins, for example the org.opensourcebim.ifcplugins bundle has 18 plugins, but a lot of bundles only have 1 (for example org.opensourcebim.ifcopenshellplugin)

# Create a maven project

Most Java IDEs should support this. There should be one pom.xml file per plugin bundle. In the next section all required parts in the pom.xml will be described, most of them are enforced by maven(relase plugin), ans some by BIMserver pluginmanagement.

## pom.xml

```xml
<groupId>org.opensourcebim</groupId>
```
For most plugins for bimserver this is org.opensourcebim, but this can be anything of course, make sure to use lowercase, which is the convention

```xml
<artifactId>ifcplugins</artifactId>
```
This should describe your plugin bundle, also make sure it is lowercase

```xml
<version>0.0.8-SNAPSHOT</version>
```
Make sure your version on SCM always ends in -SNAPSHOT and releases always omit -SNAPSHOT

```xml
<properties>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
```
This is just to make sure build results are repeatable on different platforms

```xml
<description>BIMserver plugin that uses IfcOpenShell to generate geometry from IFC files</description>
```
A description for the whole bundle.

```xml
	<scm>
		<url>https://github.com/opensourceBIM/IfcOpenShell-BIMserver-plugin.git</url>
		<connection>scm:git:https://github.com/opensourceBIM/IfcOpenShell-BIMserver-plugin.git</connection>
		<developerConnection>scm:git:https://github.com/opensourceBIM/IfcOpenShell-BIMserver-plugin.git</developerConnection>
	</scm>
```
This is used by the maven release plugin to push/pull to the SCM.

```xml
	<organization>
		<name>OpenSource BIM</name>
		<url>opensourcebim.org</url>
	</organization>
```
Some information about the organization, of course you should fill in the details about your organization.

```xml
	<issueManagement>
		<system>GitHub</system>
		<url>https://github.com/opensourceBIM/IfcOpenShell-BIMserver-plugin/issues</url>
	</issueManagement>
```

```xml
	<name>IfcOpenShellPlugin</name>
```

```xml
	<url>https://github.com/opensourceBIM/IfcOpenShell-BIMserver-plugin</url>
```

```xml
	<licenses>
		<license>
			<name>GNU Affero General Public License</name>
			<url>http://www.gnu.org/licenses/agpl-3.0.en.html</url>
			<distribution>repo</distribution>
		</license>
	</licenses>
```

```xml
	<developers>
		<developer>
			<email>ruben@logic-labs.nl</email>
			<name>Ruben de Laat</name>
		</developer>
	</developers>
```

## 