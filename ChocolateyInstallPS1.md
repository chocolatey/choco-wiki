## ChocolateyInstall.ps1
Chocolatey uses powershell and will look for this file in the package. If it finds it, it will execute the contents of the file, attaching the helper modules.  Check out the [[Helper Reference|HelpersReference]] for more information on each of the helpers you can include.  

It really is just powershell, so you can use regular powershell here and it should run fine. 

### Example? 
This is what it takes to install [StExBar](https://github.com/ferventcoder/nugetpackages/blob/master/StExBar/tools/chocolateyInstall.ps1):  
  
```powershell
Install-ChocolateyPackage 'StExBar' 'msi' '/quiet' 'http://stexbar.googlecode.com/files/StExBar-1.8.3.msi' 'http://stexbar.googlecode.com/files/StExBar64-1.8.3.msi'
```  
  
The Install-ChocolateyPackage helper uses the url, msi, and silent args to download and silently install and update stexbar.  
  

### Templates?  
 
The latest templates are in [Chocolatey/ChocolateyTemplates](https://github.com/chocolatey/chocolateytemplates).  

Check out the [main one](https://github.com/chocolatey/chocolateytemplates/blob/master/_templates/chocolatey/tools/chocolateyInstall.ps1)
  
For getting these setup with WarmuP for moving really fast, review the [Templates ReadMe](https://github.com/chocolatey/chocolateytemplates/blob/master/README.md)