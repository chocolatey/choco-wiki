# Chocolatey WindowsFeatures (choco windowsfeatures)
##New as of v0.9.8.20.
Installs Windows Features via the Deployment Image Servicing and Management tool on the local machine.
`choco WindowsFeatures featureName` or call `choco install featureName -source windowsfeatures`

###NOTE: You can also `clist -source windowsfeatures` to get an idea of what features are available and what is already enabled.

####Deprecation Notice: The shortcut `cwindowsfeatures` was deprecated in 0.9.8.21 for the ubiquitous `choco command`.

##Parameters
###featureName
Name of feature to install.

##Examples
`chocolatey WindowsFeatures TelnetClient`

`choco windowsfeatures IIS-WebServerRole`

`cinst Microsoft-Hyper-V-All -source windowsfeatures`

[[Command Reference|CommandsReference]]
