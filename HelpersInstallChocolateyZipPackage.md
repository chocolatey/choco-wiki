#Install-ChocolateyZipPackage

Has error handling built in. You do not need to surround this with try catch if it is the only thing in your [[chocolateyInstall.ps1|ChocolateyInstallPS1]].  

`Install-ChocolateyZipPackage '__NAME__' 'URL' "UNZIP_LOCATION"`  

##Parameters
###__NAME__
This is an arbitrary name.  
Example: `'7zip'`  
  
###URL (very important)
The Url to the zip file.  
Example: `'https://github.com/downloads/spraints/git-tfs/GitTfs-0.11.0.zip'`  
  
###UNZIP_LOCATION (very important)
Where to unzip contents of the downloaded zip file.  
Example: `'/S'`  
  
##Examples
`Install-ChocolateyZipPackage 'gittfs' 'https://github.com/downloads/spraints/git-tfs/GitTfs-0.11.0.zip' $gittfsPath`  
  
`Install-ChocolateyPackage 'mono' 'exe' '/SILENT' 'http://ftp.novell.com/pub/mono/archive/2.10.2/windows-installer/5/mono-2.10.2-gtksharp-2.12.10-win32-5.exe'`  
  
[[Helper Reference|HelpersReference]]