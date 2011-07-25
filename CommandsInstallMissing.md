# Chocolatey Install Missing (cinstm)
Installs a package if it doesn't already exist.  
`chocolatey installmissing packageName` or shortcut with 
`cinstm packageName` 
  
##Parameters
###PackageName
Name of package to install if not already in machine repository.  
  
###Version (optional)
The version of the package to install.  
Defaults to the latest version available.  
  
###Source (optional)
Source (directory, share or remote url feed) the package comes from.  
Defaults to official chocolatey feed.  
  
##Examples
`chocolatey installmissing nunit`  
`chocolatey installmissing nunit -version 2.5.7.10213`  
`chocolatey installmissing nunit -version 2.5.7.10213 -source http://somelocalfeed.com/nuget`  
`cinstm nunit -version 2.5.7.10213 -source http://somelocalfeed.com/nuget`  
  
[[Command Reference|CommandsReference]]