...is not possible using the normal database upgrade scripts, e.a. is not automatic.

However there is a way to get the most important data migrated, but it involves more manual work.

1. Always make sure you backup first
2. This type of migration requires both the old and new server runnning at the same time, obviously they have to be able to find each other on the network
3. Make sure there is an admin account with the same name/password on both servers
4. In your local environment (Eclipse for example), run TriggerImportDataRemote with the right arguments (see documentation in file). This call will end quickly, but starts the migration on the new server.
5. Wait and read the logs of the new server

This has only been tested once!