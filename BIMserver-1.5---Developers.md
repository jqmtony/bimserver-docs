BIMserver 1.5 setup in Eclipse will be different than for 1.4, the current GIT master branch will be the 1.5 release.

- Some projects will always generate an error in Eclipse, this is because at the moment there is a maven-plugin which has been written for maven 2, but we use maven 3. You can safely ignore this error (it's not a compile error).

```
Plugin execution not covered by lifecycle configuration: org.codehaus.mojo:build-helper-maven-plugin:1.10:add-source (execution: add-source, phase: generate-sources)

```

Note: Please let us know if you are a Maven expert and know how to fix this

# Maven | Update Project...

This will criple the eclipse project configuration. After doing maven update, you will have to manually fix a few dependencies.

## PluginBase project

Change the "Java Build Path | Source", remove the "PluginBase/genclasses" altogether, and clear the "Excluded" part of "PluginBase/generated"

![PluginBase deps](https://github.com/opensourceBIM/BIMserver/raw/master/Documentation/img/pluginbasedeps.png)

Then go to "Libraries" and add a class-folder, "PluginBase/genclasses".

![PluginBase deps](https://github.com/opensourceBIM/BIMserver/raw/master/Documentation/img/pluginbasedeps2.png)

Note: Please let us know if you know how to make this work automatically