## Update-SessionEnvironment
## New as of v0.9.8.20.
### NOTE: This command will assert UAC/Admin privileges on the machine.

`Update-SessionEnvironment`

## Description
When Chocolatey installs a package, the package author may ad or change
certain environment variables that will affect how the application runs
or how it is accessed. Often, these changes are not visible to the current
PowerShell session. This means the user needs to open a new PowerShell
session before these settings take effect which can render the installed
application nonfunctional until that time.

Use the Update-SessionEnvironment command to refresh the current
PowerShell session with all environment settings possibly performed by
Chocolatey package installs.

[[Helper Reference|HelpersReference]]