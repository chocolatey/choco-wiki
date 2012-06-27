#Creating Chocolatey Packages

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

##Build Your Package

Open a command line in the directory where the nuspec is and type [[cpack|CommandsPack]]. That's it.

##Testing Your Package

To test the package you just built, with the command line still open (and in the current working directory in the same folder as the newly created `*.nupkg` file) type  

```cmd
 cinst packageName -source %cd%
```
  
This will install the package right out of your source. As you find things you may need to fix, you will want to delete the particular package folder out of the %ChocolateyInstall%\lib folder.

##Push Your Package

To push your package after you have built and tested it, you type `cpush packageName.nupkg` where *packageName.nupkg* is the name of the nupkg that was built with a version number as part of the package name.  You must have an api key for http://chocolatey.org/ set. You can do that with nuget.exe. You can install nuget.commandline so you can set this (notice it is on nuget.org and not on chocolatey.org - chocolatey installs packages from both sources!). 
Take a look at [[cpush|CommandsPush]]