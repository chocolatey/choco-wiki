#Get-ChocolateyWebFile
`Get-ChocolateyWebFile $packageName $fileFullPath $url $url64bit`  
  
##Description
Downloads a file from the internets.  
  
##Parameters
###$packageName
This is an arbitrary name.  
Example: `'7zip'`  
  
###$fileFullPath (important)
The full path and name of the file to save. This should include the name of the file and extension.  
Example: `'c:\tools\nodejs\node.exe'`  
  
###$url (very important)
The Url to the file.  
Example: `'http://nodejs.org/dist/v0.5.2/node.exe'`  
  
###$url64bit (optional)
If there is a 64 bit file available, put the link next to the other url. Chocolatey will automatically determine if the user is running a 64bit machine or not and adjust accordingly.  
Example: `'http://nodejs.org/dist/v0.5.2/nodex64.exe'`  
Defaults to the 32bit url.  
  
##Examples
```powershell
$scriptPath = $(Split-Path -parent $MyInvocation.MyCommand.Definition)
$nodePath = Join-Path $scriptPath 'node.exe'
Get-ChocolateyWebFile 'nodejs' "$nodePath" 'http://nodejs.org/dist/v0.5.2/node.exe'
```  
  
[[Helper Reference|HelpersReference]]  