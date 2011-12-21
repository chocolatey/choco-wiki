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
  
### -source Ruby (v0.9.8.13+)  
This specifies the source is Ruby Gems and that we are installing a gem. If you do not have ruby installed prior to running this command, the command will install that first.  
  
### -source webpi (v0.9.8.13+)
This specifies the source is Web PI and that we are installing a WebPI product, such as IISExpress. If you do not have the Web PI command line installed, it will install that first and then the product requested.  
  
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