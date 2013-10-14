# Chocolatey WindowsFeatures (choco windowsfeatures)
##New as of v0.9.8.20.
Installs Windows Features via the Deployment Image Servicing and Management tool on the local machine.
`choco WindowsFeatures featureName` or shortcut with
`cwindowsfeatures featureName`. Also can call `choco install featureName -source windowsfeatures`

###NOTE: You can also `clist -source windowsfeatures` to get an idea of what features are available and what is already enabled.

##Parameters
###featureName
Name of feature to install.

##Examples
`chocolatey WindowsFeatures TelnetClient`

`choco windowsfeatures IIS-WebServerRole`

`cinst Microsoft-Hyper-V-All -source windowsfeatures`

[[Command Reference|CommandsReference]]
