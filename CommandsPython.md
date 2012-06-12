# Chocolatey Python (cpython)
##New as of v0.9.8.17.  
Installs packages from Python (easy_install).  
`chocolatey python packageName` or shortcut with 
`cpython packageName`. Also can call `cinst packageName -source python` 
  
##Parameters
###PackageName
Name of package to install.  
  
###Version (optional)
The version of the package to install.  
Defaults to the latest version available.  

##Examples
`chocolatey python Sphinx`  
  
`cpython Sphinx`  
  
`cpython Sphinx -version 1.1.2`  
  
`cinst Sphinx -source python`  
  
`cinst Sphinx -source python -version 1.1.2`  
  
[[Command Reference|CommandsReference]]