# Chocolatey Sources (choco sources)
##New as of v0.9.8.20.
Allows you to manage sources with several subcommands.

####Deprecation Notice: The shortcut `csources` was deprecated in 0.9.8.21 for the ubiquitous `choco sources`.

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
`choco sources list`

`choco sources add -name bob -source c:\localpackages`

`choco sources disable bob`

`choco sources remove bob`

[[Command Reference|CommandsReference]]
