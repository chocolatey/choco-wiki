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
  
##Screenshots
Installing mSysGit silently:  
![msysgit](images/msysgit.png "msysgit")  
![msysgit install](images/msysgit2.png "msysgit install")  
  
NHProf:  
![nhprof](images/chocolateynhprofiler.png "nhprof")  
  
Statlight:  
![statlight](images/statlight.png "statlight")  
  
Git-Tfs:  
![git tfs](images/git-tfs.png "git tfs chocolatey")  
![git tfs helper run](images/git-tfs2.png "git tfs chocolatey helper")  
  
[[Command Reference|CommandsReference]]