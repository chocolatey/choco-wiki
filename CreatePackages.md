#Creating Chocolatey Packages

## What version of the software should I package?
The main release of a product versions are usually sufficient. If there are also beta versions available and you would rather have that, then please create both the official release and the beta (and set the beta as a prerelease when pushing the item to chocolatey.org). Regular users of packages may want to use official releases only and not betas.
  
## Okay, how do I create the packages?
There are four main elements to a chocolatey package. Only the nuspec is required (#1 below).  
  
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