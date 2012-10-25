# Chocolatey WindowsFeatures (cwindowsfeatures)
##New as of v0.9.8.20.  
Installs Windows Features via the Deployment Image Servicing and Management tool on the local machine.  
`chocolatey WindowsFeatures featureName` or shortcut with 
`cwindowsfeatures featureName`. Also can call `cinst featureName -source windowsfeatures` 
  
###NOTE: You can also `clist -source windowsfeatures` to get an idea of what features are available and what is already enabled. 
  
##Parameters
###featureName
Name of feature to install.  
  
##Examples
`chocolatey WindowsFeatures TelnetClient`  
  
`cwindowsfeatures IIS-WebServerRole`  
  
`cinst Microsoft-Hyper-V-All -source windowsfeatures`  
  
[[Command Reference|CommandsReference]]