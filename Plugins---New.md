Just thinking out load how plugins should be working.

- BIMserver should come with no plugins at all, everything that cannot be missed, should be in BIMserver anyways
- Plugins should be on github releases, or any other downloadable URL
- Every plugin should have someone responsible for maintaining the plugin, testing with different BIMserver versions and making releases
- Plugin repositories should aggregate links to those plugins, there should be 1 main repository (maintained by BIMserver developers), but uses should be able to setup other repositories
- Github looks like a good place for a repository
- WebModule plugins should have no Java code at all, configration should be done with json files, this makes the build process easier for plugin developers. All web modules should also be able to run on a remote host of course.
- Already an open issue for a long time: Plugins should be running in a sandboxed environment
- There should be a "template" project developers can use to start with, which includes build scripts and script for uploading releases to github

# Dependencies

This is where stuff gets complicated. We make a distinction between WebModule plugins and Java-based plugins.

## Java-based plugins

For example the COBie plugin has dependencies on a separate Java project. This should work in Eclipse (when developing), but also when the plugin is running on a deployed server.
- Will dependencies be included in dependant plugins, or will they simply require the dependency-plugin to be available (and should BIMserver go and get it)?
- What happens when multiple plugins have the same dependencies? Load them multiple times?

- How to build plugins? They all depend on "Shared". Should we automatically download a specific version of "shared.jar" and compile agains that? Or checkout a specific revision/part of BIMserver? It's probably a good idea to make a minimal "shared.jar", it's including way too much at the moment. Looks like maven might actually be a good choice for this.

## WebModule plugins

Those have been added to BIMserver mainly for development ease, but have been used on deployments a lot as well (you don't need a separate webserver).

- WebModule plugins should work fine in development-mode, all source files should be individual and non-minified
- WebModule plugins should be optimized in deployment (we use grunt for this)
- WebModule plugins can have dependencies, we will manage those with package.json, but how will Eclipse/IDE be able to download those dependencies?
- Ideally we don't want to have to do releases of dependencies (for example BIMserver JavaScript API) in order to use changed code in for example BIMvie.ws, while developing
- We don't want any submodules as they are a pain in the ass in this usecase
- The BIMserver JavaScript API specifically should also work on NodeJS
