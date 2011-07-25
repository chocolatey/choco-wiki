# Chocolatey Update (cup)
Updates an existing package to the latest version, if there is a newer version available.  
`chocolatey update packageName` or shortcut with 
`cup packageName`  
###Update all packages to the latest version with `cup all`.  
  
##Parameters
###PackageName
Name of package to update. If left empty, assumes you meant for updating chocolatey itself to the latest version. If `all`, it compares against all installed packages.  
  
###Source (optional)
Source (directory, share or remote url feed) the package comes from.  
Defaults to official chocolatey feed.  
  
##Examples
`chocolatey update` - updates chocolatey to the latest version  
`chocolatey update nunit`  
`chocolatey update nunit -source http://somelocalfeed.com/nuget`  
`cup nunit -source http://somelocalfeed.com/nuget`  
  
[[Command Reference|CommandsReference]]