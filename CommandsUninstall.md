## Chocolatey Uninstall (cuninst)
##New as of v0.9.8.17.  
**NOTE: This is early support for uninstall - it will continue to get better over time.** 
Uninstalls a package.  
`chocolatey uninstall packageName` or shortcut with 
`cuninst packageName`  
  
## Known Limitations
* There are no functions defined in the chocolatey powershell module that would help with uninstall
* There is no automatic removal of MSIs
* Uninstall only removes the most current version of a package in the machine repository (instead of giving you options to remove a certain one or all of them) 

##Parameters
###PackageName
Name of package to uninstall.  
  
###Version (optional)
The version of the package to uninstall.  
Defaults to the latest version installed.  
  
##Examples
`chocolatey uninstall nunit`  
  
`chocolatey uninstall nunit -version 2.5.7.10213`  
  
[[Command Reference|CommandsReference]]