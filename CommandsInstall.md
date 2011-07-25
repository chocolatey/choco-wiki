# Chocolatey Install (cinst)
Unconditionally installs a package, even if it already exists.  
`chocolatey install packageName` or shortcut with 
`cinst packageName` 
  
##Parameters
###PackageName
Name of package to install.  
  
###Version (optional)
The version of the package to install.  
Defaults to the latest version available.  
  
###Source (optional)
Source (directory, share or remote url feed) the package comes from.  
Defaults to official chocolatey feed.  
  
##Examples
`chocolatey install nunit`  
  
`chocolatey install nunit -version 2.5.7.10213`  
  
`chocolatey install nunit -version 2.5.7.10213 -source http://somelocalfeed.com/nuget`  
  
`cinst nunit -version 2.5.7.10213 -source http://somelocalfeed.com/nuget`  
  
  
[[Command Reference|CommandsReference]]