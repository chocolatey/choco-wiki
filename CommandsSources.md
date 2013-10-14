# Chocolatey Sources (choco sources)
##New as of v0.9.8.20.
Allows you to manage sources with several subcommands.


###NOTE: You can also `clist -source windowsfeatures` to get an idea of what features are available and what is already enabled.

##Parameters
### Operation to perform

#### list
#### add
#### remove
#### enable
This is performed against a source. SourceName is required. Must be disabled to enable.
#### disable
This is performed on a source. SourceName is required.

###SourceName (optional, depending on operation)
Name of the source to work with.

###SourceLocation (optional, depending on operation)


##Examples
`chocolatey WindowsFeatures TelnetClient`

`choco winfeatures IIS-WebServerRole`

`cinst Microsoft-Hyper-V-All -source windowsfeatures`

[[Command Reference|CommandsReference]]
