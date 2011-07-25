# Chocolatey List (clist)
Lists packages available from a remote source.  
`chocolatey list packageName` or shortcut with 
`cinst packageName` 
  
###NOTE: Some chocolatey packages are tagged with chocolatey. To see what chocolatey packages are available, type `clist chocolatey`. This is not a comprenhensitve listing, but it's enough to give you an idea.  
  
##Parameters
###Filter
A way to filter down results. Searches against name/description/tag.  
  
###Source (optional)
Source (directory, share or remote url feed) the package comes from.  
Defaults to official chocolatey feed.  
  
##Examples
`chocolatey list nunit`  
  
`chocolatey list nunit -source http://somelocalfeed.com/nuget`  
  
`clist nunit -source http://somelocalfeed.com/nuget`  
  
![clist in action](images/clistExample.png "clist in action")  
  
[[Command Reference|CommandsReference]]