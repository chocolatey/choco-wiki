#Helper Reference
  
##Main Helpers
These helpers call other helpers and have error handling built in. When using just them, you don't need to put error handling in your [[chocolateyInstall.ps1 file|ChocolateyInstallPS1]]. These helpers call down to the other helpers and encapsulate everything nicely so that it is possible to have one line chocolateyInstall.ps1 files.  

[[Install-ChocolateyPackage|HelpersInstallChocolateyPackage]]  
[[Install-ChocolateyZipPackage|HelpersInstallChocolateyZipPackage]]  
[[Install-ChocolateyPowershellCommand|HelpersInstallChocolateyPowershellCommand]]  
  
##Error/SuccessHelpers
  
[[Write-ChocolateySuccess|HelpersWriteChocolateySuccess]]  
[[Write-ChocolateyFailure|HelpersWriteChocolateyFailure]]  
  
If do anything besides use the main helpers, it is strong suggested you format your chocolateyInstall.ps1 as follows:  
  
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
[[Start-ChocolateyProcessAsAdmin|HelpersStartChocolateyProcessAsAdmin]]  
[[Get-ChocolateyWebFile|HelpersGetChocolateyWebFile]]  
[[Install-ChocolateyInstallPackage|HelpersInstallChocolateyInstallPackage]]  
[[Get-ChocolateyUnzip|HelpersGetChocolateyUnzip]]  
[[Install-ChocolateyPath|HelpersInstallChocolateyPath]]  
[[Install-ChocolateyDesktopLink|HelpersInstallChocolateyDesktopLink]]  

##Administrative Access Packages
When creating packages that need to run one of the following commands below, one should add the tag `admin` to the nuspec.  

* [[Install-ChocolateyPackage|HelpersInstallChocolateyPackage]]  
* [[Start-ChocolateyProcessAsAdmin|HelpersStartChocolateyProcessAsAdmin]]   
* [[Install-ChocolateyInstallPackage|HelpersInstallChocolateyInstallPackage]]  
* [[Install-ChocolateyPath|HelpersInstallChocolateyPath]] - when specifying machine path  
  
##Non-Administrator Safe Helpers
Some folks expressed a desire to have chocolatey not run as administrator to reach continuous integration and developers that are not administrators on their machines. Starting with chocolatey [[v0.9.8.3|ReleaseNotes]], this has been possible.  

These are the helpers from above as one list.    

* [[Install-ChocolateyZipPackage|HelpersInstallChocolateyZipPackage]]  
* [[Install-ChocolateyPowershellCommand|HelpersInstallChocolateyPowershellCommand]]  
* [[Write-ChocolateySuccess|HelpersWriteChocolateySuccess]]  
* [[Write-ChocolateyFailure|HelpersWriteChocolateyFailure]]  
* [[Get-ChocolateyWebFile|HelpersGetChocolateyWebFile]]  
* [[Get-ChocolateyUnzip|HelpersGetChocolateyUnzip]]  
* [[Install-ChocolateyPath|HelpersInstallChocolateyPath]] - when specifying user path  
* [[Install-ChocolateyDesktopLink|HelpersInstallChocolateyDesktopLink]]  
  
[[Home]]