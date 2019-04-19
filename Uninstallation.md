# Uninstalling Chocolatey

Should you decide you don't like Chocolatey, you can uninstall it simply by removing the folder (and the environment variable(s) that it creates).  Since it is not actually installed in Programs and Features, you don't have to worry that it cluttered up your registry (however that's a different story for the applications that you installed with Chocolatey or manually).

## Folder
Most of Chocolatey is contained in `C:\ProgramData\chocolatey` or whatever `$env:ChocolateyInstall` evaluates to. You can simply delete that folder.

**NOTE** You might first back up the sub-folders `lib` and `bin` just in case you find undesirable results in removing Chocolatey. Bear in mind not every Chocolatey package is an installer package, there may be some non-installed applications contained in these subfolders that could potentially go missing. Having a backup will allow you to test that aspect out.

## Environment Variables
There are some environment variables that need to be adjusted or removed.

* ChocolateyInstall
* ChocolateyToolsLocation
* PATH (will need updated to remove)

## Script
There are no warranties on this script whatsoever, but here is something you can try:

**WARNING!!** This will remove Chocolatey and all packages, software, and configurations in the Chocolatey Installation folder from your machine. Everything will be GONE. This is very destructive. DO NOT RUN this script unless you completely understand what the intention of this script is and are good with it. If you mess something up, we cannot help you fix it.

***WARNING:*** Seriously, this script may destroy your machine and require a rebuild. It may have varied results on different machines in the same environment. Think twice before running this.

If you also intend to delete the Chocolatey directory, remove the `-WhatIf`:

~~~powershell
$userPath = [Microsoft.Win32.Registry]::CurrentUser.OpenSubKey('Environment').GetValue('PATH', '', [Microsoft.Win32.RegistryValueOptions]::DoNotExpandEnvironmentNames).ToString()
$machinePath = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey('SYSTEM\CurrentControlSet\Control\Session Manager\Environment\').GetValue('PATH', '', [Microsoft.Win32.RegistryValueOptions]::DoNotExpandEnvironmentNames).ToString()

@"
User PATH:
$userPath

Machine PATH:
$machinePath
@" | Out-File "C:\PATH_backups_ChocolateyUninstall.txt" -Encoding UTF8 -Force

# WARNING: The next two scripts updating PATH could cause issues after
# reboot where nothing is found if something goes wrong. In that case,
# look at the backed up files for PATH.
[System.Text.RegularExpressions.Regex]::Replace($userPath, [System.Text.RegularExpressions.Regex]::Escape("$env:ChocolateyInstall\bin") + '(?>;)?', '', [System.Text.RegularExpressions.RegexOptions]::IgnoreCase) | %{[System.Environment]::SetEnvironmentVariable('PATH', $_.Replace(";;",";"), 'User')}
[System.Text.RegularExpressions.Regex]::Replace($machinePath, [System.Text.RegularExpressions.Regex]::Escape("$env:ChocolateyInstall\bin") + '(?>;)?', '', [System.Text.RegularExpressions.RegexOptions]::IgnoreCase) | %{[System.Environment]::SetEnvironmentVariable('PATH', $_.Replace(";;",";"), 'Machine')}

Remove-Item -Recurse -Force "$env:ChocolateyInstall" -WhatIf
~~~

If you also intend to delete the tools directory that was managed by Chocolatey, remove both of the `-WhatIf` switches:

~~~powershell
if ($env:ChocolateyBinRoot -ne '' -and $env:ChocolateyBinRoot -ne $null) { Remove-Item -Recurse -Force "$env:ChocolateyBinRoot" -WhatIf }
if ($env:ChocolateyToolsRoot -ne '' -and $env:ChocolateyToolsRoot -ne $null) { Remove-Item -Recurse -Force "$env:ChocolateyToolsRoot" -WhatIf }
[System.Environment]::SetEnvironmentVariable("ChocolateyBinRoot", $null, 'User')
[System.Environment]::SetEnvironmentVariable("ChocolateyToolsLocation", $null, 'User')
[System.Environment]::SetEnvironmentVariable("ChocolateyToolsLocation", $null, 'Machine')
[System.Environment]::SetEnvironmentVariable("ChocolateyInstall", $null, 'User')
[System.Environment]::SetEnvironmentVariable("ChocolateyInstall", $null, 'Machine')
~~~
