# Chocolatey WebPI (choco webpi)
##New as of v0.9.8.13.  
Installs products from Web Platform Installer (WebPI).  
`choco webpi packageName` or call `cinst packageName -source webpi` 
  
###NOTE: You can also `clist -source webpi` to get an idea of what is available and what you have installed. You must install webpi first prior to attempting the list command `cinst webpicommandline`.  

####Deprecation Notice: The shortcut `cwebpi` was deprecated in 0.9.8.21 for the ubiquitous `choco command`.

##Parameters
###PackageName
Name of package to install.  
  
##Examples
`chocolatey webpi IISExpress`  
  
`cwebpi IISExpress`  
  
`cinst IISExpress -source webpi`  
  
[[Command Reference|CommandsReference]]