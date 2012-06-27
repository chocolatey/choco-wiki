##Creating Chocolatey Packages

## What version of the software should I package?
The main release of a product versions are usually sufficient. If there are also beta versions available and you would rather have that, then please create both the official release and the beta (and set the beta as a prerelease when pushing the item to chocolatey.org). Regular users of packages may want to use official releases only and not betas.
  
## Okay, how do I create the packages?
There are three main elements to a chocolatey package. Only the nuspec is required (#1 below).  
  
1. [Nuspec](https://github.com/ferventcoder/nugetpackages/blob/master/_template/chocolatey/__NAME__.nuspec) - [Nuspec Reference](http://docs.nuget.org/docs/reference/nuspec-reference)
1. [[chocolateyInstall.ps1|ChocolateyInstallPS1]] - check out the [[helper reference|HelpersReference]]
1. any tools to include (it is highly suggested that you are the author in this case or you have the right to [[distribute files|Legal]]). EXE files in the package/downloaded to package folder from chocolateyInstall.ps1 will get a link to the command line.
  
There is a video showing the creation of a package: [http://www.youtube.com/watch?v=Wt_unjS_SUo](http://www.youtube.com/watch?v=Wt_unjS_SUo)  
The video is a bit outdated in showing the contents of the chocolateyInstall.ps1. Have a look at what the [chocolateyInstall.ps1](https://github.com/ferventcoder/nugetpackages/blob/master/windirstat/tools/chocolateyInstall.ps1) looks like now:
  
```powershell
Install-ChocolateyPackage 'windirstat' 'exe' '/S' 'http://windirstat.info/wds_current_setup.exe'
```
  
##Dependency Chaining
You can make packages that depend on other packages just by adding those dependencies to the nuspec. Take a look at [ferventcoder.chocolatey.utilities nuspec](https://github.com/ferventcoder/nugetpackages/blob/master/ferventcoder.chocolatey.utilities/ferventcoder.chocolatey.utilities.nuspec)
  
##Naming your package
Name the package the same as the actual application/tool (or as close as you can get). This aids in discoverability. Folks love it when they try something like `cinst curl` and it just works.  
  
If you are going to offer a package that has both an installer and an archive (zip or executable only) version of the application, create three packages - see Rob's guidance on this: http://devlicio.us/blogs/rob_reynolds/archive/2012/02/25/chocolatey-guidance-on-packaging-apps-with-both-an-install-and-executable-zip-option.aspx  
  
##Versioning Recommendations
Versioning can be both simple and complicated. The best recommendation is to use the same versioning that the application/tool uses. With chocolatey you get four version segments. If the application/tool only uses 1, 2 or 3 version segments, follow suit.  
  
If the 4th segment is used, some folks like to drop the segment altogether and use that as only the package fix notation using one of the notations in the next section. There is no recommendations at this time.  
  
###Package Fix Version Notation
If you need to fix the package for some reason, you can use the fourth number for a package fix notation. There are two recommended methods of package fix version notation:  
  
 * **Date (Year/Month/Day)** - Some folks use year month day package fix notation (yyyyMMdd as in 20120627 seen as 1.2.0.20120627) 
 * Sequential - Some folks use sequential numbering (0, then 1, etc as in 0 for no fix, 1 for first fix and so on seen as 1.2.0.0 and 1.2.0.1).  
  
Date Package Fix Version Notation is recommended because one can ascertain what it is immediately upon seeing it.   
  
Package fix version notation is only acceptable in the fourth segment. Do not use any of the other segments for package fix notation. If an app/tool only uses 1 or 2 version segments, add zeros into the other segments until you get to the 4th segment (i.e. 1.0.0.20120627).   
  
## How do I exclude [executables from getting batch redirects](https://github.com/chocolatey/chocolatey/issues/106)?
If you have executables in the package or brought into the package folder during powershell run and you want to exclude them you need to create an empty file named exactly like (case sensitive) the executable with `.ignore` suffixed on the end in the same directory where the executable is or will be.  
  
Example: In the case of `Bob.exe` you would create a file named `Bob.exe.ignore` and that file would not get a redirect batch link. The chocolatey package has an example of that.  
  
## How do I set up batch redirects for [applications that have a GUI](https://github.com/chocolatey/chocolatey/issues/76)?
If you don't want to see a hanging window when you open an application from the command line that was set up with chocolatey, you want to create a file next to the executable that is named exactly the same (case sensitive) with `.gui` suffixed on the end.  
  
Example: In the case of `Bob.exe` you would create a file named `Bob.exe.gui` and that file would be set up as a GUI application so the window will call it and then move on without waiting for it to finish.  
  
##Build Your Package

Open a command line in the directory where the nuspec is and type [[cpack|CommandsPack]]. That's it.

##Testing Your Package

To test the package you just built, with the command line still open (and in the current working directory in the same folder as the newly created `*.nupkg` file) type:  

```cmd
 cinst packageName -source %cd%
```
  
This will install the package right out of your source. As you find things you may need to fix, you will want to delete the particular package folder out of the %ChocolateyInstall%\lib folder.

##Push Your Package

To push your package after you have built and tested it, you type `cpush packageName.nupkg` where *packageName.nupkg* is the name of the nupkg that was built with a version number as part of the package name.  You must have an api key for http://chocolatey.org/ set. You can do that with nuget.exe. You can install nuget.commandline so you can set this (notice it is on nuget.org and not on chocolatey.org - chocolatey installs packages from both sources!). 
Take a look at [[cpush|CommandsPush]]  
  
You can also log into chocolatey.org and upload your package from there.  

##Is there a SIMPLER way of creating packages?
I'm so glad you asked. Take a look at this repository - https://github.com/ferventcoder/chocolateytemplates (note the _templates folder).
Now open a command line, navigate to your source code top level folder and type the following:
  
```cmd
 cinst warmup
 warmup addTextReplacement __CHOCO_PKG_OWNER_NAME__ "Your Name"
 warmup addTextReplacement __CHOCO_PKG_OWNER_REPO__ "Your Repository"
 warmup addTextReplacement __CHOCO_AUTO_PKG_OWNER_REPO__ "Your Choco Automatic Packages Repository (could be same as other)"
 git clone git://github.com/ferventcoder/chocolateytemplates.git
```
  
 * The package owner name (__CHOCO_PKG_OWNER_NAME__) would be you. 
 * Your packages repository (__CHOCO_PKG_OWNER_REPO__) is part of a github repo just **ferventcoder/nugetpackages** if your repository is https://github.com/ferventcoder/nugetpackages. This is only used for image urls.
 * Your chocolatey automatic packages repository (__CHOCO_AUTO_PKG_OWNER_REPO__) could be the same as your regular packages repository. This is also the same as package owner repo. This is only used for image urls.  

Automatic package repositories? It's coming and it will make chocolatey one of the most up to date package repositories in the world (if not the most up to date)...stay tuned to the chocolatey google group for more information.