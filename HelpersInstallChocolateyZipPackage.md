#Install-ChocolateyZipPackage

Has error handling built in. You do not need to surround this with try catch if it is the only thing in your [[chocolateyInstall.ps1|ChocolateyInstallPS1]].  

`Install-ChocolateyZipPackage '__NAME__' 'URL' "UNZIP_LOCATION"`  

##Parameters
###__NAME__
This is an arbitrary name.  
Example: `'7zip'`  
  
###URL (very important)
The Url to the native installer.  
Example: `'http://stexbar.googlecode.com/files/StExBar-1.8.3.msi'`  
  
###UNZIP_LOCATION
Silent and other arguments to pass to the native installer.  
Example: `'/S'`  
If there are no silent arguments, pass this as `''`  
  
##Examples
`Install-ChocolateyPackage 'StExBar' 'msi' '/quiet' 'http://stexbar.googlecode.com/files/StExBar-1.8.3.msi' 'http://stexbar.googlecode.com/files/StExBar64-1.8.3.msi'`  
  
`Install-ChocolateyPackage 'mono' 'exe' '/SILENT' 'http://ftp.novell.com/pub/mono/archive/2.10.2/windows-installer/5/mono-2.10.2-gtksharp-2.12.10-win32-5.exe'`  
  
[[Helper Reference|HelpersReference]]