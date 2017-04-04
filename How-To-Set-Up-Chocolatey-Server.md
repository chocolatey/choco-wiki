# How To Setup the Chocolatey.Server Package

<!-- TOC -->

- [What is Chocolatey.Server?](#what-is-chocolateyserver)
- [Setup with Puppet](#setup-with-puppet)
- [Setup Normally](#setup-normally)
- [Additional Configuration](#additional-configuration)

<!-- /TOC -->

## What is Chocolatey.Server?
The [Chocolatey.Server package](https://chocolatey.org/packages/chocolatey.server) contains the binaries for a fully ready to go Chocolatey NuGet Server where you can serve packages over HTTP using a NuGet-compatible OData feed.

[Chocolatey Server](https://chocolatey.org/packages/chocolatey.server) is a simple Nuget.Server that is ready to rock and roll. It has already completed Steps 1-3 of NuGet's [host your own remote feed](https://docs.nuget.org/Create/Hosting-Your-Own-NuGet-Feeds#creating-remote-feeds). Version 0.1.2 has the following additional adds:

* Uses same enhanced NuGet that Chocolatey uses so you can see more information in search if you choose to use those things.
* Allows packages up to 2GB. Package size can be controlled through [maxAllowedContentLength](https://msdn.microsoft.com/en-us/library/ms689462(v=vs.90).aspx) and [maxRequestLength](https://msdn.microsoft.com/en-us/library/e1f13641(v=vs.100).aspx).

When you install it, it will install the website typically to `c:\tools\chocolatey.server`.

## Setup with Puppet
If you are using the Puppet module [chocolatey/chocolatey_server](https://forge.puppet.com/chocolatey/chocolatey_server), it will do all of the additional setup for this package and allow some customization.

The module works with Windows Server 2008/2012.
For a simple `include chocolatey_server` it does the following automatically:

 * Ensures IIS is installed
 * Ensures ASP.NET is installed
 * Ensures the chocolatey.server package is installed
 * Ensures an app pool for the chocolatey.server site
 * Ensures the IIS website is setup for chocolatey.server
 * Ensures permissions for the site are set correctly.

## Setup Normally

 * Install or upgrade the package - `choco upgrade chocolatey.server -y`
 * Ensure IIS is installed. You can try `choco install IIS-WebServer --source windowsfeatures`
 * Ensure that ASP.NET is installed. Try `choco install IIS-ASPNET45 --source windowsfeatures` (`IIS-ASPNET` for Windows 2008).
 * Disable or remove the Default website
 * Set up an app pool for Chocolatey.Server. Ensure 32-bit is enabled and the managed runtime version is `v4.0`.
 * Set up an IIS website pointed to the install location and set it to use the app pool.
 * Go to explorer and right click on `c:\tools\chocolatey.server` and add the following permissions:
   * `IIS_IUSRS` - Read
   * `IUSR` - Read
   * `IIS APPPOOL\<app pool name>` - Read
 * Right click on the `App_Data` subfolder and add the following permissions:
   * `IIS_IUSRS` - Modify
   * `IIS APPPOOL\<app pool name>` - Modify

## Additional Configuration

Looking for where the apikey is and how it is changed, that is all done through the web.config. The config is pretty well-documented on each of the appSettings key values.

To update the apiKey, it is kept in the web.config, search for that value and update it. If you reach out to the server on https://localhost (it will show you what the apikey is, only locally though).
