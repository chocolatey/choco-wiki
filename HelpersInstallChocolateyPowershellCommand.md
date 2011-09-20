#Install-ChocolateyPowershellCommand
`Install-ChocolateyPowershellCommand $packageName $psFileFullPath $url`  
  
##Description
This will download a file from a url and unzip it on your machine. Has error handling built in. You do not need to surround this with try catch if it is the only thing in your [[chocolateyInstall.ps1|ChocolateyInstallPS1]].  

##Parameters
###$packageName
This is an arbitrary name.  
Example: `'7zip'`  
  
###$psFileFullPath (important)
The full path and name of the file to save. This should include the name of the file and extension.  
Example: `'c:\tools\nodejs\node.exe'`  
  
###$url (optional)
The Url to the file.  
Example: `'http://nodejs.org/dist/v0.5.2/node.exe'`  
  
  
##Examples
`Install-ChocolateyZipPackage 'gittfs' 'https://github.com/downloads/spraints/git-tfs/GitTfs-0.11.0.zip' $gittfsPath`  
  
`Install-ChocolateyZipPackage 'sysinternals' 'http://download.sysinternals.com/Files/SysinternalsSuite.zip' "$(Split-Path -parent $MyInvocation.MyCommand.Definition)"`  
  
[[Helper Reference|HelpersReference]]