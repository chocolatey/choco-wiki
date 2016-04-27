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

No warranties on this script, but here is something you can try:

**NOTE:** Warning! This will remove Chocolatey from your machine. Only run this if you intend for that to happen.

~~~powershell
Remove-Item -Recurse -Force "$env:ChocolateyInstall" -WhatIf
[System.Text.RegularExpressions.Regex]::Replace( ` [Microsoft.Win32.Registry]::CurrentUser.OpenSubKey('Environment').GetValue('PATH', '',  `
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
[System.Environment]::SetEnvironmentVariable("ChocolateyInstall",$null, 'Machine')
[System.Environment]::SetEnvironmentVariable("ChocolateyInstall",$null, 'User')
[System.Environment]::SetEnvironmentVariable("ChocolateyBinRoot",$null, 'User')
[System.Environment]::SetEnvironmentVariable("ChocolateyToolsLocation", $null, 'User')
~~~