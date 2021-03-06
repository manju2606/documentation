# Upgrading Go

## Introduction

To upgrade from a previous version of Go, it is only necessary to upgrade the Server. It is not necessary to stop or backup the Go Agents. Agents will automatically update to the correct version of Go.

### Before you start

Since Cruise 1.1 (legacy version of Go), we do not include a bundled version of the Subversion version control system. This means that if you use Subversion for your projects the server and all agents need to have Subversion installed and available on the system path.

Since Cruise 1.2 (legacy version of Go), we do not include a bundled version of ANT. This means that if you use ANT for your projects the server and all agents need to have ANT installed and available on the system path.

### Backing up your data

#### Configuration Backup

As part of the configuration two files need to be backed up:

-   Go's configuration is saved in the **cruise-config.xml** file
-   Cipher file for password encryption.

Based on the OS your Go server is running on, both these files can be found at:

| Operating System 	| Location                                            	|
|:----------------	|:----------------------------------------------------- |
| Linux            	| /etc/go                                             	|
| Windows          	| [Go install directory]\\config                      	|
| Mac OS X         	| < user-home >/Library/Application Support/Go Server 	|

#### Database backup

It is critical that the Go server be stopped before taking a backup of the database. If the Go server is not stopped, the backup may be corrupted. The database directory will be located at any one of the following locations based on what OS you're running on:

| Operating System 	| Location                                            	|
|:----------------	|:----------------------------------------------------- |
| Linux            	| /var/lib/go-server/db                                 |
| Windows          	| [Go install directory]\\db                      	    |
| Mac OS X         	| < user-home >/Library/Application Support/Go Server/db|

#### Build Artifacts Backup

The Go server acts as a repository for all your build artifacts. While it is not essential to backup the artifacts before an upgrade, it is good practice to make regular backups of this directory.

You can configure where Go stores build artifacts. The following are the default locations of the artifacts if you have not customized its location:

| Operating System 	| Location                                            	
|:----------------	|:----------------------------------------------------- 
| Linux            	| /var/lib/go-server/artifacts                                           	
| Windows          	| [Go install directory]\\artifacts                      	
| Mac OS X         	| < user-home >/Library/Application Support/Go Server/artifacts 	

### Upgrading to the new version

You do not need to stop the Agents to perform an upgrade. Go agents will automatically update to the correct version of the software. You do not need to upgrade the Go agents. Any builds in progress will be rescheduled, and the existing pipelines will complete as expected.

If you are upgrading from a pre-2.1 release, the agent's directory structure will continue to be called "cruise-agent" and will not be renamed to "go-agent". This is normal and will not cause any issues.

On linux, you can ignore any permission errors logged as part of the upgrade of the cruise agent from a pre-2.1 release

Go will perform upgrades of its internal data structures when it starts. This process may take some time for installations with a large number of historical builds (10 to 15 minutes on very large installations). If you suspect that there is a problem with the upgrade, check the go-server.log to see if there are any reported errors. This is a one-time migration and subsequent restarts will be much faster.

#### Windows

Run the Go installer. Make sure that you specify the same directory as your previously installed version.

If you have changed the Go Server Windows service to run as a different user, you will need to repeat this configuration change.

The installer will automatically start the service. Once Go completes its internal data changes, you should be able to see the Go webpage. Any existing Agents should automatically reconnect. Any builds in progress should continue, or be rescheduled.

#### Linux

#### Debian based distributions (i.e. Ubuntu)

Run the Go installer as described 'sudo dpkg -i [go-server-package-name]'.

#### RPM based distributions (i.e. RedHat)

Run the Go installer as described 'sudo rpm -U [go-server-package-name]'.

#### Macintosh OSX

The Macintosh OSX edition of Go does not support upgrades. You should follow the steps above to backup your data, uninstall Go/Cruise (by dragging the application into trash), and then perform a fresh installation.

#### Solaris

The Solaris edition of Go does not support upgrades. You should follow the steps above to backup your data, uninstall Go/Cruise, and then perform a fresh installation.

### Notes

Use the notes from this section when upgrading to a particular version of Go.

#### Version 2.3

As part of the 2.3 upgrade, the "dest" attribute of a material configured in a pipeline is case-insensitive. This would mean that if you have a pipeline P1 with two materials, say material M1 with dest = "foo" and material M2 with dest = "Foo", after the upgrade the dest folders will be automatically changed to M1 (dest = "foo\_(random\_string)") and M2 (dest = "Foo\_(random\_string)")