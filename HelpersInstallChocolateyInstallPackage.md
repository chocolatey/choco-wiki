#Install-ChocolateyInstallPackage
###NOTE: This command will assert UAC/Admin privileges on the machine.  
  
`Install-ChocolateyInstallPackage $packageName $fileType $silentArgs $file`  
  
##Description
This will run a native installer to perform an install/upgrade on your machine.  
  
##Parameters
###$packageName
This is an arbitrary name.  
Example: `'7zip'`  
  
###$fileType (very important)
Pick only one : 'exe' or 'msi'  
Example: `'exe'` or `'msi'`  
  
###$silentArgs
Silent and other arguments to pass to the native installer.  
Example: `'/S'`  
If there are no silent arguments, pass this as `''`  
  
###$file  
This is the file to install. This is a full path to the file.  
Example: `'c:\somepath\someinstaller.msi'`  
  
##Examples
`Install-ChocolateyInstallPackage '7zip' 'exe' '/S' 'c:\somepath\7zipInstaller.msi'`  
  
[[Helper Reference|HelpersReference]]  