# Chocolatey WebPI (cwebpi)
##New as of v0.9.8.13.  
Installs products from Web Platform Installer (WebPI).  
`chocolatey webpi packageName` or shortcut with 
`cwebpi packageName`. Also can call `cinst packageName -source webpi` 
  
###NOTE: You can also `clist -source webpi` to get an idea of what is available and what you have installed. You must install webpi first prior to attempting the list command `cinst webpicommandline`.  
  
##Parameters
###PackageName
Name of package to install.  
  
##Examples
`chocolatey webpi IISExpress`  
  
`cwebpi IISExpress`  
  
`cinst IISExpress -source webpi`  
  
[[Command Reference|CommandsReference]]