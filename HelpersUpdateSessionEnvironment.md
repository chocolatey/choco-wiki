# Update-SessionEnvironment

Updates the environment variables of the current powershell session with
any environment variable changes that may have occured during a chocolatey
package install.

## Syntax

~~~powershell
Update-SessionEnvironment
~~~

## Description

When chocolatey installs a package, the package author may add or change
certain environment variables that will affect how the application runs
or how it is accessed. Often, these changes are not visible to the current
powershell session. This means the user needs to open a new powershell
session before these settings take effect which can render the installed
application nonfunctional until that time.

Use the Update-SessionEnvironment command to refresh the current
powershell session with all environment settings possibly performed by
chocolatey package installs.


## Aliases

None

## Inputs

None

## Outputs

None

## Parameters
 




[[Function Reference|HelpersReference]]

***NOTE:*** This documentation has been automatically generated from `Import-Module "$env:ChocolateyInstall\helpers\chocolateyInstaller.psm1" -Force; Get-Help Update-SessionEnvironment -Full`.
