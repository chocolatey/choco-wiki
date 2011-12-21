# Chocolatey Gem (cgem)
##New as of v0.9.8.13.  
Installs gems from Ruby Gems.  
`chocolatey gem packageName` or shortcut with 
`cgem packageName`. Also can call `cinst packageName -source ruby` 
  
##Parameters
###PackageName
Name of gem to install.  
  
###Version (optional)
The version of the gem to install.  
Defaults to the latest version available.  

##Examples
`chocolatey gem gemcutter`  
  
`cgem gemcutter`  
  
`cgem gemcutter -version 0.7.1`  
  
`cinst gemcutter -source ruby`  
  
`cinst gemcutter -source ruby -version 0.7.1`  
  
[[Command Reference|CommandsReference]]