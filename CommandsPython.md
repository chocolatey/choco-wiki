# Chocolatey Python (choco python)
##New as of v0.9.8.17.  
Installs packages from Python (easy_install).  
`choco python packageName` or call `cinst packageName -source python` 
  
####Deprecation Notice: The shortcut `cpython` was deprecated in 0.9.8.21 for the ubiquitous `choco command`.

##Parameters
###PackageName
Name of package to install.  
  
###Version (optional)
The version of the package to install.  
Defaults to the latest version available.  

##Examples
`choco python Sphinx`  
  
`choco python Sphinx`  
  
`choco python Sphinx -version 1.1.2`  
  
`cinst Sphinx -source python`  
  
`cinst Sphinx -source python -version 1.1.2`  
  
[[Command Reference|CommandsReference]]