# Chocolatey Version (cver)
Compares an installed package to the version available from a remote feed.  
`chocolatey version packageName` or shortcut with 
`cver packageName`  
  
##Parameters
###PackageName
Name of package to check.  
If left empty, assumes you meant check chocolatey.  
If `all`, then compares all installed packages.  
  
###Source (optional)
Source (directory, share or remote url feed) the package comes from.  
Defaults to official chocolatey feed.  
  
###Prerelease (optional) - v0.9.8.15+
Whether to include prerelease packages in results.  
This is optional if you explicitly ask for a specific version that is a pre-release package.  
You can pass this as `-pre` or `-prerelease`.  
Defaults to false.  

###LocalOnly (optional) - v0.9.8.20+
Whether to only look at local versions.  
When this flag is provided, simply lists installed versions.  
You can pass this as `-lo` or `-localonly`  
Defaults to false.
  
##Examples
`chocolatey version` - looks to see if there is an update available for chocolatey  
  
`chocolatey version nunit`  
  
`chocolatey version nunit -source http://somelocalfeed.com/nuget`  
  
`cver nunit -source http://somelocalfeed.com/nuget`  
  
![cver in action](images/cverExample.png "cver in action")  
  
[[Command Reference|CommandsReference]]