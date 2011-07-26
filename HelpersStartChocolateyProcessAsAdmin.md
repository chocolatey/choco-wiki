#Start-ChocolateyProcessAsAdmin
###NOTE: This command will assert UAC/Admin privileges on the machine.  
  
`Start-ChocolateyProcessAsAdmin $statements $exeToRun`  
  
##Description
Runs a process as an administrator. If $exeToRun is not specified, it is run with powershell.  
  
##Parameters
###$statements (important)
These are statements and/or arguments for an application.  
Example: `'/i package /q'`  
  
###$exeToRun (important)
This is the executable/application to run.  
Example: `cmd.exe`  
Defaults to `powershell`  
  
##Examples
`Start-ChocolateyProcessAsAdmin "$msiArgs" 'msiexec'`  
  
`Start-ChocolateyProcessAsAdmin "$silentArgs" $file`  
  
  
[[Helper Reference|HelpersReference]]  