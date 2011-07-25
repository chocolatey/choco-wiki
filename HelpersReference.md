#Helper Reference
  
##Main Helpers
These helpers call other helpers and have error handling built in. When using just them, you don't need to put error handling in your [[chocolateyInstall.ps1 file|ChocolateyInstallPS1]].  

[[Install-ChocolateyPackage|HelpersInstallChocolateyPackage]]  
[[Install-ChocolateyZipPackage|HelpersInstallChocolateyZipPackage]]  
  
##Error/SuccessHelpers
  
[[Write-ChocolateySuccess|HelpersWriteChocolateySuccess]]  
[[Write-ChocolateyFailure|HelpersWriteChocolateyFailure]]  
  
If you use the helpers in the next section, it is strong suggested you format your chocolateyInstall.ps1 as follows:  
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
  
  
[[Home]]