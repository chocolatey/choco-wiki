#Creating Chocolatey Packages

There are four main elements to a chocolatey package.  
  
1. nuspec
1. nuget blocker with chocolatey note
1. [[chocolateyInstall.ps1|ChocolateyInstallPS1]]
1. any tools to include (it is highly suggested that you are the author in this case or you have the right to [[distribute rights|Legal]]). EXE files will get a link to the command line.
  
There is a video showing the creation of a package: [http://www.youtube.com/watch?v=Wt_unjS_SUo](http://www.youtube.com/watch?v=Wt_unjS_SUo)  
The video is a bit outdated in showing the contents of the chocolateyInstall.ps1. Have a look at what the [chocolateyInstall.ps1](https://github.com/ferventcoder/nugetpackages/blob/master/windirstat/tools/chocolateyInstall.ps1) looks like now:

```powershell
Install-ChocolateyPackage 'windirstat' 'exe' '/S' 'http://windirstat.info/wds_current_setup.exe'
```