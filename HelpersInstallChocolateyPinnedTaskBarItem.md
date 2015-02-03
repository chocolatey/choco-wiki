## Install-ChocolateyPinnedTaskBarItem
## New as of v0.9.8.20.

`Install-ChocolateyPinnedTaskBarItem "${env:ProgramFiles(x86)}\Microsoft Visual Studio 11.0\Common7\IDE\devenv.exe"`

## Description
Creates an item in the task bar linking to the provided path.

## Parameters
### $targetFilePath
The path to the application that should be launched when clicking on the task bar icon.

## Examples
`Install-ChocolateyPinnedTaskBarItem "${env:ProgramFiles(x86)}\Microsoft Visual Studio 11.0\Common7\IDE\devenv.exe"`

This will create a Visual Studio task bar icon.

[[Function Reference|HelpersReference]]