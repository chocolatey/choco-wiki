# How To Host Your Own (Private) Feed

There are three types of feeds, [folder/unc share](#local-folder--unc-share), simple server, and advanced package gallery.

## Local Folder / UNC Share
Perhaps the easiest to set up and recommended for testing quick and dirty scenarios, local folder is easily a strong point when you need a quick source for packages.

**Advantages:**
* Speed of setup (can be setup almost immediately).
* Package store is filesystem.
* Can be easily upgrade to Simple Server.
* Permissions are based on file system/share permissions.

**Disadvantages:**
* Anyone with permission can push and overwrite packages.
* No HTTP/HTTPS pushing. Must be able to access the folder/share to push to it. 
* Starts to affect choco performance once the source has over 500 packages (maybe?).

## Simple Server

**Advantages:**
* Push over HTTP/HTTPS
* One API key for pushes
* Package store is file system.

**Disadvantages:**


## Package Gallery
This is like what Chocolatey.org (the community feed runs on). It is the most advanced, having both a file store for packages and a database for tracking all sorts of information.
It supports:

 * Database for information
 * Package stores (like S3 and Azure blobs)
 *