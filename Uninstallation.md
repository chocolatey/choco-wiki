## Uninstalling Chocolatey

Should you decide you don't like Chocolatey, you can uninstall it simply by removing the folder (and the environment variable(s) that it creates).  Since it is not actually installed on your system, you don't have to worry that it cluttered up your registry (the applications that you installed with Chocolatey or manually, now that's a different story).

### Folder
* C:\Chocolatey - for Chocolatey version < ```0.9.8.27```
* C:\ProgramData\chocolatey > ```0.9.8.27```

**NOTE:** If you upgraded from ```0.9.8.26``` to ```0.9.8.27``` it is likely that ```Chocolatey``` is installed at C:\Chocolatey, which was the default prior to ```0.9.8.27```.  If you did a fresh install of Chocolatey at version ```0.9.8.27``` then the installation folder will be ```C:\ProgramData\Chocolatey```

### Environment Variables
* ChocolateyInstall
* ChocolateyBinRoot
* ChocolateyToolsLocation
* PATH (will need updated to remove)


## Script

There are no warranties on this script whatsoever, but here is something you can try:

**WARNING!!** This will remove Chocolatey and all packages, software, and configurations in the Chocolatey Installation folder from your machine. Everything will be GONE. This is very destructive. DO NOT RUN this script unless you completely understand what the intention of this script is and are good with it. If you mess something up, we cannot help you fix it. 

***WARNING:*** Seriously, this script may destroy your machine and require a rebuild. Think twice before running this.

If you also intend to delete the Chocolatey directory, remove the `-WhatIf`:

~~~powershell
Remove-Item -Recurse -Force "$env:ChocolateyInstall" -WhatIf
[System.Text.RegularExpressions.Regex]::Replace( ` 
[Microsoft.Win32.Registry]::CurrentUser.OpenSubKey('Environment').GetValue('PATH', '',  `
[Microsoft.Win32.RegistryValueOptions]::DoNotExpandEnvironmentNames).ToString(),  `
[System.Text.RegularExpressions.Regex]::Escape("$env:ChocolateyInstall\bin") + '(?>;)?', '', `
[System.Text.RegularExpressions.RegexOptions]::IgnoreCase) | `
%{[System.Environment]::SetEnvironmentVariable('PATH', $_, 'User')}
[System.Text.RegularExpressions.Regex]::Replace( `
[Microsoft.Win32.Registry]::LocalMachine.OpenSubKey('SYSTEM\CurrentControlSet\Control\Session Manager\Environment\').GetValue('PATH', '', `
[Microsoft.Win32.RegistryValueOptions]::DoNotExpandEnvironmentNames).ToString(),  `
[System.Text.RegularExpressions.Regex]::Escape("$env:ChocolateyInstall\bin") + '(?>;)?', '', `
[System.Text.RegularExpressions.RegexOptions]::IgnoreCase) | `
%{[System.Environment]::SetEnvironmentVariable('PATH', $_, 'Machine')}
~~~

If you also intend to delete the tools directory that was managed by Chocolatey, remove both of the `-WhatIf` switches:

~~~powershell
if ($env:ChocolateyBinRoot -ne '' -and $env:ChocolateyBinRoot -ne $null) { Remove-Item -Recurse -Force "$env:ChocolateyBinRoot" -WhatIf }
if ($env:ChocolateyToolsRoot -ne '' -and $env:ChocolateyToolsRoot -ne $null) { Remove-Item -Recurse -Force "$env:ChocolateyToolsRoot" -WhatIf }
[System.Environment]::SetEnvironmentVariable("ChocolateyBinRoot", $null, 'User')
[System.Environment]::SetEnvironmentVariable("ChocolateyToolsLocation", $null, 'User')
~~~