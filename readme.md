Release is a simple bash-based release management script.

## Server Configuration

This assumes that all of your projects are in your /var/www directory (configure this on line 5). The project name is tha name of the directory it is hosted in.

Each project should have a "releases" subdirectory within it. Inside this directory, you should have named version releases as directories, these can be numbers or strings, depends how you want to do it.

You should configure your webserver so that the code used is a "live" subdirectory in the main project folder, for example "/var/www/<project>/live" - this will be created as described later.

This example shows a directory structure with 2 projects, "foo" and "bar". Each project has two releases, "foo" has release 1 and release 2, "bar" has "alpha" and "holding". You need not create the live directories. Each release directory has the entire set of code for that release.

* /var/www
	* foo
		* live
		* releases
			* 1
			* 2
	* bar
		* live
		* releases
			* alpha
			* holding
			
## Releasing

In order to release a particular code version, you need to somehow get that code into one of the release directories, it is usually best to create a new one. It could be each release directory is a different subversion tag, git branch etc., or you may wish to upload code yourself. Don't upload into the currently live release, as that will completely defeat the point of the system!

Release uses symbolic links to deploy a particular code version. As your webserver is always pointed at "/var/www/<project>/live", we can atomically switch the symlink to point from one release to another, making sure everything happens at the same time. If you have no release deployed (no live symlink) it will create one when you release.

To do a release, just run the script with the required arguments:

    release <project> <release>

Taking the example above, if the "foo" project currently has release 1 active, you can switch atomically to release 2 by running:

    release foo 2
	
You can roll back just as easily:

    release foo 1

## To Do

* More Robustness
* Multi-server
* A deploy script to help