# Chocolatey Install Missing (choco installmissing)
# DEPRECATED, will be removed in v1.
**NOTE:** This functionality is no longer necessary as choco install will behave the same way when the local version / remote version are the same.  If you feel this should stay in chocolatey, please voice your concerns on the mailing list. Thanks.

Installs a package if it doesn't already exist.
`chocolatey installmissing packageName` or shortcut with
`cinstm packageName`

##Parameters
###PackageName
Name of package to install if not already in machine repository.

###Version (optional)
The version of the package to install.

Defaults to the latest version available.

###Source (optional)
Source (directory, share or remote url feed) the package comes from.

Defaults to official chocolatey feed.

##Examples
`chocolatey installmissing nunit`

`chocolatey installmissing nunit -version 2.5.7.10213`

`chocolatey installmissing nunit -version 2.5.7.10213 -source http://somelocalfeed.com/nuget`

`choco installmissing nunit -version 2.5.7.10213 -source http://somelocalfeed.com/nuget`


[[Command Reference|CommandsReference]]
