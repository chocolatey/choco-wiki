#Creating Chocolatey Packages

There are four main elements to a chocolatey package. Only the nuspec is required (#1 below).  
  
1. [nuspec](https://github.com/ferventcoder/nugetpackages/blob/master/_template/chocolatey/__NAME__.nuspec)
1. [nuget blocker with chocolatey note](https://github.com/ferventcoder/nugetpackages/blob/master/_template/chocolatey/tools/install.ps1)
1. [[chocolateyInstall.ps1|ChocolateyInstallPS1]] - check out the [[helper reference|HelpersReference]]
1. any tools to include (it is highly suggested that you are the author in this case or you have the right to [[distribute files|Legal]]). EXE files in the package/downloaded to package folder from chocolateyInstall.ps1 will get a link to the command line.
  
There is a video showing the creation of a package: [http://www.youtube.com/watch?v=Wt_unjS_SUo](http://www.youtube.com/watch?v=Wt_unjS_SUo)  
The video is a bit outdated in showing the contents of the chocolateyInstall.ps1. Have a look at what the [chocolateyInstall.ps1](https://github.com/ferventcoder/nugetpackages/blob/master/windirstat/tools/chocolateyInstall.ps1) looks like now:

```powershell
Install-ChocolateyPackage 'windirstat' 'exe' '/S' 'http://windirstat.info/wds_current_setup.exe'
```
  
##Dependency Chaining
You can make packages that depend on other packages just by adding those dependencies to the nuspec. Take a look at [ferventcoder.chocolatey.utilities nuspec](https://github.com/ferventcoder/nugetpackages/blob/master/ferventcoder.chocolatey.utilities/ferventcoder.chocolatey.utilities.nuspec)