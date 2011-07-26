#Install-ChocolateyPackage
###NOTE: This command will assert UAC/Admin privileges on the machine.  

`Install-ChocolateyPackage $packageName $fileType $silentArgs $url $url64bit`  
  
##Description
This will download a native installer from a url and install it on your machine. Has error handling built in. You do not need to surround this with try catch if it is the only thing in your [[chocolateyInstall.ps1|ChocolateyInstallPS1]].  
  
##Parameters
###$packageName
This is an arbitrary name.  
Example: `'7zip'`  
  
###$fileType (very important)
Pick only  one to leave here.  
Example: `'exe'` or `'msi'`  
  
###$silentArgs
Silent and other arguments to pass to the native installer.  
Example: `'/S'`  
If there are no silent arguments, pass this as `''`  
  
###$url (very important)
The Url to the native installer.  
Example: `'http://stexbar.googlecode.com/files/StExBar-1.8.3.msi'`  
  
###$url64bit (optional)
If there is a 64 bit installer available, put the link next to the other url. Chocolatey will automatically determine if the user is running a 64bit machine or not and adjust accordingly.  
Example: `'http://stexbar.googlecode.com/files/StExBar64-1.8.3.msi'`  
Defaults to the 32bit url.  
  
##Examples
`Install-ChocolateyPackage 'StExBar' 'msi' '/quiet' 'http://stexbar.googlecode.com/files/StExBar-1.8.3.msi' 'http://stexbar.googlecode.com/files/StExBar64-1.8.3.msi'`  
  
`Install-ChocolateyPackage 'mono' 'exe' '/SILENT' 'http://ftp.novell.com/pub/mono/archive/2.10.2/windows-installer/5/mono-2.10.2-gtksharp-2.12.10-win32-5.exe'`  
  
[[Helper Reference|HelpersReference]]