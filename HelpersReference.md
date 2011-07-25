#Helper Reference
  
##Main Helpers
These helpers call other helpers and have error handling built in. When using just them, you don't need to put error handling in your [[chocolateyInstall.ps1 file|ChocolateyInstallPS1]].  

[[Install-ChocolateyPackage|HelpersInstallChocolateyPackage]]  
[[Install-ChocolateyZipPackage|HelpersInstallChocolateyZipPackage]]  

##More Helpers
These helpers require you to wrap a try catch around your chocolateyInstall.ps1 file. Two of these helpers are strongly suggested every time you have to wrap.  


```powershell
try {
  
  #Your code here...

  Write-ChocolateySuccess 'gittfs'
} catch {
  Write-ChocolateyFailure 'gittfs' $($_.Exception.Message)
  throw 
}

```