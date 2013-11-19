# Chocolatey Gem (choco gem)
##New as of v0.9.8.13.
Installs gems from Ruby Gems.
`choco gem packageName` or call `choco install packageName -source ruby`

####Deprecation Notice: The shortcut `cgem` was deprecated in 0.9.8.21 for the ubiquitous `choco command`.

##Parameters
###PackageName
Name of gem to install.

###Version (optional)
The version of the gem to install.

Defaults to the latest version available.

##Examples
`chocolatey gem gemcutter`

`choco gem gemcutter` `choco gem gemcutter -version 0.7.1`

`cinst gemcutter -source ruby`

`choco install gemcutter -source ruby -version 0.7.1`

[[Command Reference|CommandsReference]]
