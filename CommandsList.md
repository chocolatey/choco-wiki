# Chocolatey List (choco list) / Search (choco search)
Lists packages available from a remote source.

`chocolatey list packageName` or shortcut with
`choco list packageName` or `choco search something`.

Search is a new alias as of 0.9.8.21+.

###NOTE: To see what git packages are available, type `choco search git`.

##Parameters
###Filter
A way to filter down results. Searches against name/description/tag.

###AllVersions flag (optional) - v0.9.8.13+
Whether to display more than one version of given results or not. Specified by either `-allversions` or `-all`.

Defaults to false.

###Prerelease flag (optional) - v0.9.8.15+
Whether to include prerelease packages in results. This is optional if you explicitly ask for a specific version that is a pre-release package. You can pass this as `-pre` or `-prerelease`.

Defaults to false.

###Verbose flag (optional)
Show more information about each package. You pass this as `-verbose`.

Defaults to false.

###Source (optional)
Source (directory, share or remote url feed) the package comes from.

Defaults to official chocolatey feed.

###LocalOnly flag (optional) - v0.9.8.21+
This searches against the local installed packages. You just pass `-lo` or `-localonly` to the list/search command.

Defaults to false.

#### -source webpi (v0.9.8.13+)
This retrieves a lising from what's available and installed based on WebPI. See [[WebPI|CommandsWebPI]].

#### -source windowsfeatures (v0.9.8.20+)
This retrieves a lising from what's available and installed for windows features. See [[Windows Features|CommandsWindowsFeatures]].

##Examples
`chocolatey list nunit` `chocolatey list nunit -all`

`chocolatey search nunit -source http://somelocalfeed.com/nuget`

`chocolatey list nunit -source http://somelocalfeed.com/nuget -all`

`choco list nunit -source http://somelocalfeed.com/nuget`

`choco list -source webpi`

`choco search -lo` `choco search -localonly`

`choco list -localonly` # lists all packages chocolatey has installed on your local system

![clist in action](images/clistExample.png "clist in action")

[[Command Reference|CommandsReference]]
