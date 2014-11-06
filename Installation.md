## Installing Chocolatey  
Chocolatey installs in seconds...  
  
There are a few ways to install chocolatey. Chocolatey exists as a [nuget package](http://nuget.org/list/packages/chocolatey) so virtually any way you can get a nuget package, you have the opportunity to then install it.  
  
If you have Visual Studio 2010 and the NuGet extension installed, perhaps the quickest method is to use NuGet Package Manager. Three commands in succession and you are done. See below.  
 
## Why does chocolatey install where it does by default?
Great question - [[why chocolatey installs where it does|DefaultChocolateyInstallReasoning]]  
  
## Before You Install  
**Can I install chocolatey to another location?** Yes  
  
1. Create a __machine__ level (__user__ level will also work) environment variable named ```ChocolateyInstall``` and set it to the folder you want chocolatey to install to prior to installation (this environment variable must be set globally or available to PowerShell- it is not enough to simply make it available to your current command prompt session).  Keep in mind the [[restrictions|DefaultChocolateyInstallReasoning]] though!  
1. Create the folder manually.
1. If you have already installed (and want to change the location after the fact):
  * Follow the above steps. 
  * Install chocolatey again. 
  * Copy/Move over the items from the old lib/bin directory.
  * Delete your old install directory.
  
## Non-Administrative Install

1. You must choose a different location than the default. The default is a more secure location that only administrators can write to.
1. Follow that with the command line / powershell methods of installation.

## Command Line
This really is the easiest method because it requires no configuration of powershell prior to executing it. Open a command line, paste the following and press &lt;Enter&gt;:  
  
```cmd
@powershell -NoProfile -ExecutionPolicy unrestricted -Command "(iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))) >$null 2>&1" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
```  
  
## PowerShell
This is the second-most easy method. Open a powershell command line and paste in the following and press &lt;Enter&gt;:  
  
```powershell
(iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1')))>$null 2>&1
```  
  
**<font color="red">Note: You must have your execution policy set to unrestricted (or at least in bypass) for this to work (`Set-ExecutionPolicy Unrestricted`). There have been [reports](https://github.com/chocolatey/chocolatey/issues/70) that RemoteSigned is enough for the install to work.</font>**  
It downloads and runs (https://chocolatey.org/install.ps1).  
  
## PowerShell Through Batch Method
`NOTE: This method will give you a deprecated warning.`
This is the best method if you want to repeat it or include it in source control. It requires no change to your existing powershell to allow for remote unsigned scripts.  

Download the two items from [chocolateyInstall](https://github.com/chocolatey/chocolatey/tree/master/chocolateyInstall).  
  
Run `installChocolatey.cmd` and it will install and update to the latest version of chocolatey.  
  
## NuGet Package Manager Method
  
When you have Visual Studio 2010 and the NuGet extension installed, you can simply type the following three commands and you will have chocolatey installed on your machine.  
  
 `Install-Package chocolatey`  
 `Initialize-Chocolatey`  
 `Uninstall-Package chocolatey`  

## NuGet.exe + PowerShell Method

You can also use nuget command line to download chocolatey:  
  
 `nuget install chocolatey` or `nuget install chocolatey -pre`  
  
Once you download it, open powershell (remote unsigned), navigate to the tools folder and run:  

`& 'chocolateyInstall.ps1'`