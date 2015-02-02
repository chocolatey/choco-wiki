# Start-ChocolateyProcessAsAdmin
### NOTE: This command will assert UAC/Admin privileges on the machine.

`Start-ChocolateyProcessAsAdmin $statements $exeToRun`

## Description
Runs a process as an administrator. If $exeToRun is not specified, it is run with PowerShell.

## Parameters
### $statements (important)
These are statements and/or arguments for an application.
Example: `'/i package /q'`

### $exeToRun (important)
This is the executable/application to run.
Example: `cmd.exe`
Defaults to `powershell`

### $validExitCodes (optional) - v0.9.8.14+
If there are other valid exit codes besides zero signifying a successful install, please pass `-validExitCodes` with the value, including 0 as long as it is still valid.
Example: `-validExitCodes @(0,44)`
Defaults to `@(0)`.

## Examples
`Start-ChocolateyProcessAsAdmin "$msiArgs" 'msiexec'`

`Start-ChocolateyProcessAsAdmin "$silentArgs" $file`

`Start-ChocolateyProcessAsAdmin "$silentArgs" $file -validExitCodes @(0,21)`

```powershell
$psFile = Join-Path "$(Split-Path -parent $MyInvocation.MyCommand.Definition)" 'someInstall.ps1'
Start-ChocolateyProcessAsAdmin "& `'$psFile`'"
```

[[Helper Reference|HelpersReference]]