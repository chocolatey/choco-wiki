# Installing Chocolatey  
Chocolatey installs in seconds...  
  
There are a few ways to install chocolatey. Chocolatey exists as a [nuget package](http://nuget.org/list/packages/chocolatey) so virtually any way you can get a nuget package, you have the opportunity to then install it.  
  
If you have Visual Studio 2010 and the NuGet extension installed, perhaps the quickest method is to use NuGet Package Manager. Three commands in succession and you are done. See below.  
  
## PowerShell
This is the easiest method. Open a powershell command line and paste in the following and press &lt;Enter&gt;:  
  
```powershell
iex ((new-object net.webclient).DownloadString("http://bit.ly/psChocInstall"))
```  

It downloads and runs (https://raw.github.com/chocolatey/chocolatey/master/chocolateyInstall/InstallChocolatey.ps1).  
  
## PowerShell Through Batch Method
This is the best method if you want to repeat it or include it in source control. It requires no change to your existing powershell to allow for remote unsigned scripts.  

Download the two items from [chocolateyInstall](https://github.com/ferventcoder/chocolatey/tree/master/chocolateyInstall).  
  
Run `installChocolatey.cmd` and it will install and update to the latest version of chocolatey.  
  
## NuGet Package Manager Method
  
When you have Visual Studio 2010 and the NuGet extension installed, you can simply type the following three commands and you will have chocolatey installed on your machine.  
  
 `Install-Package chocolatey`  
 `Initialize-Chocolatey`  
 `Uninstall-Package chocolatey`  

## NuGet.exe + PowerShell Method

You can also use nuget command line to download chocolatey:  
  
 `nuget install chocolatey`  
  
Once you download it, open powershell (remote unsigned), navigate to the tools folder and run:  

`& 'chocolateyInstall.ps1'`
