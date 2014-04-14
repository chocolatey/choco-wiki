##Main Helpers
These helpers call other helpers and have error handling built in. When using just them, you don't need to put error handling in your [[chocolateyInstall.ps1 file|ChocolateyInstallPS1]]. These helpers call down to the other helpers and encapsulate everything nicely so that it is possible to have one line chocolateyInstall.ps1 files.  

[[Install-ChocolateyPackage|HelpersInstallChocolateyPackage]]  
[[Install-ChocolateyZipPackage|HelpersInstallChocolateyZipPackage]]  
[[Install-ChocolateyPowershellCommand|HelpersInstallChocolateyPowershellCommand]]  
[[Install-ChocolateyVsixPackage|HelpersInstallChocolateyVsixPackage]] - v0.9.8.20+
  
##Error/SuccessHelpers
  
[[Write-ChocolateySuccess|HelpersWriteChocolateySuccess]]  
[[Write-ChocolateyFailure|HelpersWriteChocolateyFailure]]  
  
If do anything besides use the main helpers, it is strongly suggested you format your chocolateyInstall.ps1 as follows:  
  
```powershell
try {
  
  #Your code here...

  Write-ChocolateySuccess '__NAME__'
} catch {
  Write-ChocolateyFailure '__NAME__' $($_.Exception.Message)
  throw 
}
```  
  
##More Helpers
These helpers require you to wrap a try catch around your chocolateyInstall.ps1 file. See the example script above.  

###Administrative Access Packages
When creating packages that need to run one of the following commands below, one should add the tag `admin` to the nuspec.  

* [[Install-ChocolateyPackage|HelpersInstallChocolateyPackage]]  
* [[Start-ChocolateyProcessAsAdmin|HelpersStartChocolateyProcessAsAdmin]]   
* [[Install-ChocolateyInstallPackage|HelpersInstallChocolateyInstallPackage]]  
* [[Install-ChocolateyPath|HelpersInstallChocolateyPath]] - when specifying machine path  
* [[Install-ChocolateyEnvironmentVariable|HelpersInstallChocolateyEnvironmentVariable]] - when specifying machine path v0.9.8.20+
* [[Install-ChocolateyExplorerMenuItem|HelpersInstallChocolateyExplorerMenuItem]] - v0.9.8.20+
* [[Install-ChocolateyFileAssociation|HelpersInstallChocolateyFileAssociation]] - v0.9.8.20+
* [[Update-SessionEnvironment|HelpersUpdateSessionEnvironment]] - v0.9.8.20+

###Non-Administrator Safe Helpers
Some folks expressed a desire to have chocolatey not run as administrator to reach continuous integration and developers that are not administrators on their machines. Starting with chocolatey [[v0.9.8.3|ReleaseNotes]], this has been possible.  

These are the helpers from above as one list.    

* [[Install-ChocolateyZipPackage|HelpersInstallChocolateyZipPackage]]  
* [[Install-ChocolateyPowershellCommand|HelpersInstallChocolateyPowershellCommand]]  
* [[Write-ChocolateySuccess|HelpersWriteChocolateySuccess]]  
* [[Write-ChocolateyFailure|HelpersWriteChocolateyFailure]]  
* [[Get-ChocolateyWebFile|HelpersGetChocolateyWebFile]]  
* [[Get-ChocolateyUnzip|HelpersGetChocolateyUnzip]]  
* [[Install-ChocolateyPath|HelpersInstallChocolateyPath]] - when specifying user path
* [[Install-ChocolateyEnvironmentVariable|HelpersInstallChocolateyEnvironmentVariable]] - when specifying user path v0.9.8.20+
* [[Install-ChocolateyDesktopLink|HelpersInstallChocolateyDesktopLink]]  
* [[Install-ChocolateyPinnedTaskBarItem|HelpersInstallChocolateyPinnedTaskBarItem]] - v0.9.8.20+
  
##Overview

###Helpers in alphabetical order
_(needs updating)_

* __Get-BinRoot__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-BinRoot.ps1)  
Gets the path to where binaries should be installed. Either by environmental variable `ChocolateyBinRoot` or by default. E.g. `C:\Tools`
```powershell
$installDir = Get-BinRoot;
```
* __Get-CheckSumValid__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-CheckSumValid.ps1)
* __Get-ChocolateyUnzip__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-ChocolateyUnzip.ps1)
* __Get-ChocolateyWebFile__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-ChocolateyWebFile.ps1)
* __Get-FtpFile__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-FtpFile.ps1)
* __Get-ProcessorBits__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-ProcessorBits.ps1)  
Get the system architecture address width; return the system architecture address width (`32` or `64`). Optionally return `true` or `false` by specifying a width to test against.
```powershell
$architecture = Get-ProcessorBits; # 64
$is32bit = Get-ProcessorBits 32; # false
```
* __Get-UACEnabled__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-UACEnabled.ps1)
* __Get-VirusCheckValid__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-VirusCheckValid.ps1)
* __Get-WebFile__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-WebFile.ps1)
* __Get-WebHeaders__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-WebHeaders.ps1)
* __Install-ChocolateyDesktopLink__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyDesktopLink.ps1)
* __Install-ChocolateyEnvironmentVariable__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyEnvironmentVariable.ps1)
* __Install-ChocolateyExplorerMenuItem__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyExplorerMenuItem.ps1)
* __Install-ChocolateyFileAssociation__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyFileAssociation.ps1)
* __Install-ChocolateyInstallPackage__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyInstallPackage.ps1)
* __Install-ChocolateyPackage__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyPackage.ps1)
* __Install-ChocolateyPath__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyPath.ps1)
* __Install-ChocolateyPinnedTaskBarItem__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyPinnedTaskBarItem.ps1)
* __Install-ChocolateyPowershellCommand__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyPowershellCommand.ps1)
* __Install-ChocolateyShortcut__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyShortcut.ps1)
* __Install-ChocolateyVsixPackage__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyVsixPackage.ps1)
* __Install-ChocolateyZipPackage__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyZipPackage.ps1)
* __Start-ChocolateyProcessAsAdmin__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Start-ChocolateyProcessAsAdmin.ps1)
* __UnInstall-ChocolateyZipPackage__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/UnInstall-ChocolateyZipPackage.ps1)
* __Uninstall-ChocolateyPackage__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Uninstall-ChocolateyPackage.ps1)
* __Update-SessionEnvironment__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Update-SessionEnvironment.ps1)
* __Write-ChocolateyFailure__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Write-ChocolateyFailure.ps1)
* __Write-ChocolateySuccess__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Write-ChocolateySuccess.ps1)
* __Write-Debug__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Write-Debug.ps1)
* __Write-Error__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Write-Error.ps1)
* __Write-FileUpdateLog__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Write-FileUpdateLog.ps1)
* __Write-Host__ [src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Write-Host.ps1)

[[Home]]