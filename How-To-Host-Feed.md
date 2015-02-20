# How To Host Your Own (Private) Feed

There are three types of feeds, [folder/unc share](#local-folder--unc-share), [simple server](#simple-server), and the sophisticated [package gallery](#package-gallery). 

**Alternative Hosting Options (that require less work/maintenance):**
* [MyGet](https://www.myget.org/) - MyGet has free public and paid for public/private cloud-hosting options if you don't want to handle all of the pain of setup and administration. MyGet offers some stellar options like multi-feed aggregation, mirroring, and source package build services!
* [ProGet](http://inedo.com/proget/overview) - ProGet gives you a ready to go On-Premise option ([installable with Chocolatey!](https://chocolatey.org/packages/proget)).
* Artifactory

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

### Local Folder Share Setup

No really, it's that easy. Just set your permissions appropriately and put packages in the folder. You are already done.

## Simple Server

**Advantages:**
* Setup is still really simple - just a website and IIS.
* There are at least two pure Java versions, one of which is [part of Team City](http://blog.jetbrains.com/teamcity/2011/12/setting-up-teamcity-as-a-native-nuget-server/) - Windows not required.
* Push over HTTP/HTTPS.
* One API key for pushes.
* Package store is file system.

**Disadvantages:**
* Only one API key, so no multi user scenarios.
* Starts to affect choco performance once the source has over 500 packages (maybe?).
* No moderation.
* No website to view packages.
* No package statistics.

#### Simple Server Setup

Many google searches will throw out good ways to set up your own feed (hint: search for host your own nuget server feed)

Some notable references:
 * Nuget Docs [Host Your Own Remote Feed](https://docs.nuget.org/Create/Hosting-Your-Own-NuGet-Feeds#creating-remote-feeds)

##### Chocolatey Server Setup
[Chocolatey Server](https://chocolatey.org/packages/chocolatey.server) is a simple Nuget.Server that is ready to rock and roll. It has already completed Steps 1-3 of NuGet's [host your own remote feed](https://docs.nuget.org/Create/Hosting-Your-Own-NuGet-Feeds#creating-remote-feeds).

 1. Install the [Chocolatey Server](https://chocolatey.org/packages/chocolatey.server) package - `choco install chocolatey.server -y`
 1. Configure IIS appropriately for the package.
 1. Be sure to give the app pool modify permissions to the `App_Data` folder.

Alternative means of installation? If you have [Puppet](https://docs.puppetlabs.com/puppet/), you can take a look at the [chocolatey server module](https://github.com/ferventcoder/puppet-chocolatey-presentation/blob/master/demo/puppet/modules/chocolateyserver/manifests/init.pp) - has not been fully fledged out into a module on the forge yet, so its [dependencies are listed in a ReadMe](https://github.com/ferventcoder/puppet-chocolatey-presentation/blob/master/demo/puppet/modules/ReadMe.md). 

## Package Gallery
This is like what Chocolatey.org (the community feed runs on). It is the most advanced, having both a file store for packages and a database for tracking all sorts of information.

**Advantages:**
* Can deal with thousands of packages with no performance issues.
* Package store can be File system, Azure blobs, or AWS S3 (**Chocolatey Package Gallery only**).
* Multiple users each having their own API keys.
* User registration with email confirmation.
* Interaction and collaboration based. 
* Can have administrators.
* Can take advantage of moderation (**Chocolatey Package Gallery only**).
* Package statistics (download counts, etc)
* A website to view package information.
* Can be configured to send email.

**Disadvantages:**
* Speed of setup (can take longer than the rest). There are many moving parts to configure.
* Requires Windows/IIS/SQL Server/SMTP (hopefully with the proper licenses on each of those).

#### Package Gallery Setup
Only approach this if you are a Windows Admin with significant experience in setting up SQL Server databases and IIS for ASP.NET MVC sites. We don't have resources to help support the setup, but we can point you to [NuGet Gallery Setup](https://github.com/NuGet/NuGetGallery/wiki/Hosting-the-NuGet-Gallery-Locally-in-IIS).

##### Chocolatey Package Gallery Setup

At this time we don't have setup instructions and won't answer questions specifically surrounding the setup of a Chocolatey specific gallery. Watch for this to change as Chocolatey for Business becomes a thing.