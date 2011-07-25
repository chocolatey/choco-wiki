#Install-ChocolateyPackage (Administrative command)

`Install-ChocolateyPackage '__NAME__' 'EXE_OR_MSI' 'SILENT_ARGS' 'URL' '64BIT_URL_DELETE_IF_NO_64BIT'`
##Parameters
###__NAME__
This is an arbitrary name.  
Example: `'7zip'`  
  
###EXE_OR_MSI
Pick only  one to leave here.  
Example: `'exe'`  

###64bit Url (optional)
If there is a 64 bit installer available, put the link next to the other url. Chocolatey will automatically determine if the user is running a 64bit machine or not and adjust accordingly.  
Defaults to the 32bit url.  