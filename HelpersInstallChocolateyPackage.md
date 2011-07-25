#Install-ChocolateyPackage
###NOTE: This command will assert UAC/Admin privileges on the machine.

`Install-ChocolateyPackage '__NAME__' 'EXE_OR_MSI' 'SILENT_ARGS' 'URL' '64BIT_URL_DELETE_IF_NO_64BIT'`  

##Parameters
###__NAME__
This is an arbitrary name.  
Example: `'7zip'`  
  
###EXE_OR_MSI (very important)
Pick only  one to leave here.  
Example: `'exe'`  
  
###SILENT_ARGS
Silent and other arguments to pass to the native installer.  
Example: `'/S'`  
If there are no silent arguments, pass this as `''`  
  
###URL (very important)
The Url to the native installer.  
Example: `'http://stexbar.googlecode.com/files/StExBar-1.8.3.msi'`  
  
###64BIT_URL_DELETE_IF_NO_64BIT (optional)
If there is a 64 bit installer available, put the link next to the other url. Chocolatey will automatically determine if the user is running a 64bit machine or not and adjust accordingly.  
Example: `'http://stexbar.googlecode.com/files/StExBar64-1.8.3.msi'`  
Defaults to the 32bit url.  

##Examples
`Install-ChocolateyPackage 'StExBar' 'msi' '/quiet' 'http://stexbar.googlecode.com/files/StExBar-1.8.3.msi' 'http://stexbar.googlecode.com/files/StExBar64-1.8.3.msi'`  
  
`Install-ChocolateyPackage 'mono' 'exe' '/SILENT' 'http://ftp.novell.com/pub/mono/archive/2.10.2/windows-installer/5/mono-2.10.2-gtksharp-2.12.10-win32-5.exe'`  
  
[[Helper Reference|HelpersReference]]