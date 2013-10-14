# Chocolatey Gem (choco gem)
##New as of v0.9.8.13.
Installs gems from Ruby Gems.
`chocolatey gem packageName` or shortcut with
`choco gem packageName`. Also can call `choco install packageName -source ruby`

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
