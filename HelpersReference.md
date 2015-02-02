# Chocolatey Package Functions aka Helpers Reference
## Main Functions
These functions call other functions and have error handling built in. When using just them, you don't need to put error handling in your [[chocolateyInstall.ps1 file|ChocolateyInstallPS1]]. These functions call down to the other functions and encapsulate everything nicely so that it is possible to have one line chocolateyInstall.ps1 files.

* [[Install-ChocolateyPackage|HelpersInstallChocolateyPackage]]
* [[Install-ChocolateyZipPackage|HelpersInstallChocolateyZipPackage]]
* [[Install-ChocolateyPowershellCommand|HelpersInstallChocolateyPowershellCommand]]
* [[Install-ChocolateyVsixPackage|HelpersInstallChocolateyVsixPackage]]

## Error / Success Functions

* [[Write-ChocolateySuccess|HelpersWriteChocolateySuccess]]  - DEPRECATED
* [[Write-ChocolateyFailure|HelpersWriteChocolateyFailure]]  - DEPRECATED

If do anything besides use the main helpers, it is strongly suggested you format your chocolateyInstall.ps1 as follows:

```powershell
try {

  #Your code here...

} catch {
  throw $_
}
```

## More Functions
These helpers require you to wrap a try catch around your chocolateyInstall.ps1 file. See the example script above.

### Administrative Access Functions
When creating packages that need to run one of the following commands below, one should add the tag `admin` to the nuspec.

* [[Install-ChocolateyPackage|HelpersInstallChocolateyPackage]]
* [[Start-ChocolateyProcessAsAdmin|HelpersStartChocolateyProcessAsAdmin]]
* [[Install-ChocolateyInstallPackage|HelpersInstallChocolateyInstallPackage]]
* [[Install-ChocolateyPath|HelpersInstallChocolateyPath]] - when specifying machine path
* [[Install-ChocolateyEnvironmentVariable|HelpersInstallChocolateyEnvironmentVariable]] - when specifying machine path v0.9.8.20+
* [[Install-ChocolateyExplorerMenuItem|HelpersInstallChocolateyExplorerMenuItem]] - v0.9.8.20+
* [[Install-ChocolateyFileAssociation|HelpersInstallChocolateyFileAssociation]] - v0.9.8.20+
* [[Update-SessionEnvironment|HelpersUpdateSessionEnvironment]] - v0.9.8.20+

### Non-Administrator Safe Functions
Some folks expressed a desire to have Chocolatey not run as administrator to reach continuous integration and developers that are not administrators on their machines.

These are the functions from above as one list.

* [[Install-ChocolateyZipPackage|HelpersInstallChocolateyZipPackage]]
* [[Install-ChocolateyPowershellCommand|HelpersInstallChocolateyPowershellCommand]]
* [[Write-ChocolateySuccess|HelpersWriteChocolateySuccess]]
* [[Write-ChocolateyFailure|HelpersWriteChocolateyFailure]]
* [[Get-ChocolateyWebFile|HelpersGetChocolateyWebFile]]
* [[Get-ChocolateyUnzip|HelpersGetChocolateyUnzip]]
* [[Install-ChocolateyPath|HelpersInstallChocolateyPath]] - when specifying user path
* [[Install-ChocolateyEnvironmentVariable|HelpersInstallChocolateyEnvironmentVariable]] - when specifying user path
* [[Install-ChocolateyDesktopLink|HelpersInstallChocolateyDesktopLink]] - deprecated, see [[Install-ChocolateyShortcut|HelpersInstallChocolateyShortcut]]
* [[Install-ChocolateyPinnedTaskBarItem|HelpersInstallChocolateyPinnedTaskBarItem]]
* [[Install-ChocolateyShortcut|HelpersInstallChocolateyShortcut]] - v0.9.9+

## Overview

### Functions in alphabetical order
_(needs updating)_

* __Get-BinRoot__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-BinRoot.ps1)\]
Gets the path to where binaries should be installed. Either by environmental variable `ChocolateyBinRoot` or by default. E.g. `c:\tools`

* __Get-CheckSumValid__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-CheckSumValid.ps1)\]

* __Get-ChocolateyUnzip__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-ChocolateyUnzip.ps1)\]
Unzips a .zip file and returns the location for further processing.
```powershell
$scriptPath = (Split-Path -parent $MyInvocation.MyCommand.Definition)
Get-ChocolateyUnzip "c:\someFile.zip" $scriptPath somedirinzip\somedirinzip
```

* __Get-ChocolateyWebFile__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-ChocolateyWebFile.ps1)\]

Downloads a file from the internets.
```powershell
Get-ChocolateyWebFile '__NAME__' 'C:\somepath\somename.exe' 'URL' '64BIT_URL_DELETE_IF_NO_64BIT'
```

* __Get-FtpFile__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-FtpFile.ps1)\]

* __Get-ProcessorBits__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-ProcessorBits.ps1)\]
Get the system architecture address width; return the system architecture address width (`32` or `64`). Optionally return `true` or `false` by specifying a width to test against.
```powershell
$architecture = Get-ProcessorBits; # 64
$is32bit = Get-ProcessorBits 32; # false
```

* __Get-UACEnabled__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-UACEnabled.ps1)\]

* __Get-VirusCheckValid__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-VirusCheckValid.ps1)\]
:warning: not implemented!

* __Get-WebFile__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-WebFile.ps1)\]

* __Get-WebHeaders__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Get-WebHeaders.ps1)\]

* __Install-ChocolateyDesktopLink__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyDesktopLink.ps1)\]
# This adds a shortcut on the desktop to the specified file path.
```powershell
Install-ChocolateyDesktopLink -TargetFilePath "C:\tools\NHibernatProfiler\nhprof.exe"
# This will create a new Desktop Shortcut pointing at the NHibernate Profiler exe.
```

* __Install-ChocolateyEnvironmentVariable__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyEnvironmentVariable.ps1)\]
Creates a persistent environment variable
```powershell
Install-ChocolateyEnvironmentVariable "JAVA_HOME" "d:\oracle\jdk\bin"
# Creates a User environment variable "JAVA_HOME" pointing to "d:\oracle\jdk\bin".
```

* __Install-ChocolateyExplorerMenuItem__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyExplorerMenuItem.ps1)\]
Creates a windows explorer context menu item that can be associated with a command
```powershell
C:\PS>$sublimeDir = (Get-ChildItem $env:systemdrive\chocolatey\lib\sublimetext* | select $_.last)
C:\PS>$sublimeExe = "$sublimeDir\tools\sublime_text.exe"
C:\PS>Install-ChocolateyExplorerMenuItem "sublime" "Open with Sublime Text 2" $sublimeExe
# This will create a context menu item in Windows Explorer when any file is right clicked. The menu item will appear with the text "Open with Sublime Text 2" and will invoke sublime text 2 when selected.
```

* __Install-ChocolateyFileAssociation__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyFileAssociation.ps1)\]
Creates an association between a file extension and a executable
```powershell
C:\PS>$sublimeDir = (Get-ChildItem $env:systemdrive\chocolatey\lib\sublimetext* | select $_.last)
C:\PS>$sublimeExe = "$sublimeDir\tools\sublime_text.exe"
C:\PS>Install-ChocolateyFileAssociation ".txt" $sublimeExe
# This will create an association between Sublime Text 2 and all .txt files. Any .txt file opened will by default open with Sublime Text 2.
```

* __Install-ChocolateyInstallPackage__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyInstallPackage.ps1)\]
Installs a package
```powershell
Install-ChocolateyInstallPackage '__NAME__' 'EXE_OR_MSI' 'SILENT_ARGS' 'FilePath'
```

* __Install-ChocolateyPackage__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyPackage.ps1)\]
Installs a package
```powershell
Install-ChocolateyPackage '__NAME__' 'EXE_OR_MSI' 'SILENT_ARGS' 'URL' '64BIT_URL_DELETE_IF_NO_64BIT'
```

* __Install-ChocolateyPath__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyPath.ps1)\]

* __Install-ChocolateyPinnedTaskBarItem__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyPinnedTaskBarItem.ps1)\]
Creates an item in the task bar linking to the provided path.
```powershell
Install-ChocolateyPinnedTaskBarItem "${env:ProgramFiles(x86)}\Microsoft Visual Studio 11.0\Common7\IDE\devenv.exe"
# This will create a Visual Studio task bar icon.
```

* __Install-ChocolateyPowershellCommand__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyPowershellCommand.ps1)\]

* __Install-ChocolateyShortcut__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyShortcut.ps1)\]
This adds a shortcut, at the specified location, with the option to specify
a number of additional properties for the shortcut, such as Working Directory,
Arguments, Icon Location, and Description.
```powershell
Install-ChocolateyShortcut -shortcutFilePath "C:\test.lnk" -targetPath "C:\test.exe"
# This will create a new shortcut at the location of "C:\test.lnk" and link to the file located at "C:\text.exe"
```

* __Install-ChocolateyVsixPackage__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyVsixPackage.ps1)\]
Downloads and installs a VSIX package for Visual Studio
```powershell
Install-ChocolateyVsixPackage "MyPackage" http://visualstudiogallery.msdn.microsoft.com/ea3a37c9-1c76-4628-803e-b10a109e7943/file/73131/1/AutoWrockTestable.vsix
# This downloads the AutoWrockTestable VSIX from the Visual Studio Gallery and installs it to the latest version of VS.
```

* __Install-ChocolateyZipPackage__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Install-ChocolateyZipPackage.ps1)\]
Downloads and unzips a package
```powershell
Install-ChocolateyZipPackage '__NAME__' 'URL' "$(Split-Path -parent $MyInvocation.MyCommand.Definition)"
```

* __Start-ChocolateyProcessAsAdmin__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Start-ChocolateyProcessAsAdmin.ps1)\]

* __Uninstall-ChocolateyPackage__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Uninstall-ChocolateyPackage.ps1)\]
Uninstalls a package
```powershell
Uninstall-ChocolateyPackage '__NAME__' 'EXE_OR_MSI' 'SILENT_ARGS' 'FilePath'
```

* __Uninstall-ChocolateyZipPackage__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/UnInstall-ChocolateyZipPackage.ps1)\]
UnInstalls a previous installed zip package
```powershell
Uninstall-ChocolateyZipPackage '__NAME__' 'filename.zip'
```

* __Update-SessionEnvironment__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Update-SessionEnvironment.ps1)\]
Updates the environment variables of the current PowerShell session with
any environment variable changes that may have occured during a Chocolatey
package install.

* __Write-ChocolateyFailure__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Write-ChocolateyFailure.ps1)\]



* __Write-FileUpdateLog__ \[[src](https://github.com/chocolatey/chocolatey/blob/master/src/helpers/functions/Write-FileUpdateLog.ps1)\]


[[Home]]