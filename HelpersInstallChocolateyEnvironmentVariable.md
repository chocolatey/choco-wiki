## Install-ChocolateyEnvironmentVariable
## New as of v0.9.8.20.
### NOTE: This command will assert UAC/Admin privileges on the machine if $variableType== 'Machine'.

`Install-ChocolateyEnvironmentVariable "JAVA_HOME" "d:\oracle\jdk\bin"`

## Description
Install-ChocolateyEnvironmentVariable creates an environment variable
with the specified name and value. The variable is persistent and
will remain after reboots and across multiple PowerShell and command
line sessions. The variable can be scoped either to the user or to
the machine. If machine level scoping is specified, the command is
elevated to an administrative session.

## Parameters
### $variableName(important)
The name or key of the environment variable

## Parameters
### $variableValue(important)
A string value assigned to the above name

### $variableType(optional)
## New as of v0.9.8.20.
Pick only one : 'User' or 'Machine'
Example: `'User'` or `'Machine'`
Defaults to `'User'`

## Examples
`Install-ChocolateyEnvironmentVariable "JAVA_HOME" "d:\oracle\jdk\bin"`

Creates a User environment variable "JAVA_HOME" pointing to "d:\oracle\jdk\bin".

`Install-ChocolateyEnvironmentVariable "_NT_SYMBOL_PATH" "symsrv*symsrv.dll*f:\localsymbols*http://msdl.microsoft.com/download/symbols" "Machine"`

Creates a User environment variable "_NT_SYMBOL_PATH" pointing to "symsrv*symsrv.dll*f:\localsymbols*http://msdl.microsoft.com/download/symbols".
The command will be elevated to admin privileges.

[[Function Reference|HelpersReference]]