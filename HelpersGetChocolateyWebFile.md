# Get-ChocolateyWebFile
`Get-ChocolateyWebFile $packageName $fileFullPath $url $url64bit -checksum $checksum -checksumType $checksumType -checksum64 $checksum64 -checksumType64 $checksumType64`

## Description
Downloads a file from the internets.

## Parameters
### $packageName
This is an arbitrary name.
Example: `'7zip'`

### $fileFullPath (important)
The full path and name of the file to save. This should include the name of the file and extension.
Example: `'c:\tools\nodejs\node.exe'`

### $url (important)
The Url to the file.
Example: `'http://nodejs.org/dist/v0.5.2/node.exe'`

### $url64bit (optional)
If there is a 64 bit file available, put the link next to the other url. Chocolatey will automatically determine if the user is running a 64bit machine or not and adjust accordingly.
Example: `'http://nodejs.org/dist/v0.5.2/nodex64.exe'`
Defaults to the 32bit url.

### $checksum (optional but will be required later)
This allows the file being downloaded to be validated. Can be an MD5 or SHA1 hash.
Example: `-checksum 'C67962F064924F3C7B95D69F88E745C0'`
Defaults to ``.

### $checksumType (optional)
This allows the file being downloaded to be validated. Can be an MD5 or SHA1 hash.
Example: `-checksumType 'sha1'`
Defaults to `md5`.

### $checksum64 (optional)
This allows the x64 file being downloaded to be validated. Can be an MD5 or SHA1 hash.
Example: `-checksum64 'C67962F064924F3C7B95D69F88E745C0'`
Defaults to ``.

### $checksumType64 (optional)
This allows the file being downloaded to be validated. Can be an MD5 or SHA1 hash.
Example: `-checksumType64 'sha1'`
Defaults to checksumType's value.

## Examples
```powershell
$scriptPath = $(Split-Path -parent $MyInvocation.MyCommand.Definition)
$nodePath = Join-Path $scriptPath 'node.exe'
Get-ChocolateyWebFile 'nodejs' "$nodePath" 'http://nodejs.org/dist/v0.5.2/node.exe'
```

[[Helper Reference|HelpersReference]]