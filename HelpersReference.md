#Helper Reference
  
##Main Helpers
These helpers call other helpers and have error handling built in. When using just them, you don't need to put error handling in your [[chocolateyInstall.ps1 file|ChocolateyInstallPS1]]. These helpers call down to the other helpers and encapsulate everything nicely so that it is possible to have one line chocolateyInstall.ps1 files.  

[[Install-ChocolateyPackage|HelpersInstallChocolateyPackage]]  
[[Install-ChocolateyZipPackage|HelpersInstallChocolateyZipPackage]]  
  
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
  
[[Home]]