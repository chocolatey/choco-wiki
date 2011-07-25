# Installing Chocolatey
There are at least two ways to install chocolatey.  

## NuGet Package Manager
  
Chocolatey exists as a [nuget package](http://nuget.org/list/packages/chocolatey).  
  
 `Install-Package chocolatey`  
 `Initialize-Chocolatey`  
 `Uninstall-Package chocolatey`  

## NuGet.exe

Use nuget command line to download chocolatey:  
  
 `nuget install chocolatey`  
  
Once you download it, open powershell (remote unsigned) and run 

 `Initialize-Chocolatey`

## Alternative Download Method

This is probably the easiest method.

Download the two items from [chocolateyInstall](https://github.com/ferventcoder/chocolatey/tree/master/chocolateyInstall).

Run `installChocolatey.cmd` and it will install and update to the latest version of chocolatey.