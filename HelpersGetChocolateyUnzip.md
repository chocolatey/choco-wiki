# Get-ChocolateyUnzip
`Get-ChocolateyUnzip $fileFullPath $destination`

## Description
Unzips a .zip file and returns the location for further processing.

## Parameters
### $fileFullPath (important)
This is the full path to the .zip file
Example: `'c:\tools\poshgit.zip'`

### $destination (important)
This is the destination folder to unpack the contents of the zip file. If the destination doesn't exist, it will be created.
Example: `'C:\tools\poshgit'`

## Examples
`Get-ChocolateyUnzip 'c:\tools\poshgit.zip' 'C:\tools\poshgit'`

[[Helper Reference|HelpersReference]]