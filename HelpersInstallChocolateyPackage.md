## Install-ChocolateyPackage

### Synopsis
Installs a package based on a remote file download. Use Install-ChocolateyInstallPackage when local or embedded file.

### Syntax
~~~powershell
Install-ChocolateyPackage [[-packageName] <String>] [[-fileType] <String>] [[-silentArgs] <String>] [[-url] <String>] 
[[-url64bit] <String>] [[-validExitCodes] <Object>] [[-checksum] <String>] [[-checksumType] <String>] [[-checksum64] 
<String>] [[-checksumType64] <String>] [[-options] <Hashtable>] [<CommonParameters>]
~~~

### Parameters

#### packageName

 * **Required** - false
 * Pipeline Input? false

The name of the package we want to download - this is arbitrary, call it whatever you want.
It's recommended you call it the same as your nuget package id.

#### fileType

 * **Required** - false
 * Pipeline Input? false
 * **Aliases** - installerType, installType
 * **Default Value** - `exe`

This is the extension of the file. This should be 'exe', 'msi', or 'msu'.

#### silentArgs

 * **Required** - false
 * Pipeline Input? false

OPTIONAL - These are the parameters to pass to the native installer.
Try any of these to get the silent installer - /s /S /q /Q /quiet /silent /SILENT /VERYSILENT
With msi it is always /quiet. Please pass it in still but it will be overridden by chocolatey to /quiet.
If you don't pass anything it will invoke the installer with out any arguments. That means a nonsilent installer.

Please include the notSilent tag in your chocolatey nuget package if you are not setting up a silent package.

#### url

 * **Required** - false
 * Pipeline Input? false

This is the url to download the file from.

#### url64bit

 * **Required** - false
 * Pipeline Input? false
 * **Aliases** - url64

OPTIONAL - If there is a 64 bit installer available, put the link next to the other url. Chocolatey will automatically 
determine if the user is running a 64bit machine or not and adjust accordingly. Please note that the 32 bit url will be used 
in the absence of this. This link should only be used for 64 bit native software. If the original Url contains both (which is 
quite rare), set this to '$url' Otherwise remove this parameter.

#### validExitCodes

 * **Required** - false
 * Pipeline Input? false
 * **Default Value** - `@(0)`

#### checksum

 * **Required** - false
 * Pipeline Input? false

OPTIONAL (Right now), highly recommended - This allows a checksum to be validated for files that are not local

#### checksumType

 * **Required** - false
 * Pipeline Input? false

OPTIONAL (Right now) - 'md5', 'sha1', 'sha256' or 'sha512' - defaults to 'md5'

#### checksum64

 * **Required** - false
 * Pipeline Input? false

OPTIONAL (Right now) - This allows a checksum to be validated for files that are not local

#### checksumType64

 * **Required** - false
 * Pipeline Input? false

OPTIONAL (Right now) - 'md5', 'sha1', 'sha256' or 'sha512' - defaults to ChecksumType

#### options

 * **Required** - false
 * Pipeline Input? false
 * **Default Value** - `@{Headers=@{}}`

OPTIONAL - Specify custom headers

### Outputs
 - None

### Note
This method has error handling built into it.
    This command will assert UAC/Admin privileges on the machine.

### Examples
**EXAMPLE 1**

~~~powershell
$packageName= 'bob'
$toolsDir   = "$(Split-Path -parent $MyInvocation.MyCommand.Definition)"
$url        = 'https://somewhere.com/file.msi'
$url64      = 'https://somewhere.com/file-x64.msi'

$packageArgs = @{
  packageName   = $packageName
  fileType      = 'msi'
  url           = $url
  url64bit      = $url64
  silentArgs    = "/qn /norestart"
  validExitCodes= @(0, 3010, 1641)
  softwareName  = 'Bob*'
  checksum      = '12345'
  checksumType  = 'sha1'
  checksum64    = '123356'
  checksumType64= 'sha256'
}

Install-ChocolateyPackage @packageArgs
~~~

**EXAMPLE 2**

~~~powershell
Install-ChocolateyPackage 'bob' 'exe' '/S' 'https://somewhere/bob.exe'

~~~

**EXAMPLE 3**

~~~powershell
Install-ChocolateyPackage 'bob' 'exe' '/S' 'https://somewhere/bob.exe' 'https://somewhere/bob-x64.exe'

~~~

**EXAMPLE 4**

~~~powershell
$options =
@{
  Headers = @{
    Accept = 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8';
    'Accept-Charset' = 'ISO-8859-1,utf-8;q=0.7,*;q=0.3';
    'Accept-Language' = 'en-GB,en-US;q=0.8,en;q=0.6';
    Cookie = 'requiredinfo=info';
    Referer = 'https://somelocation.com/';
  }
}

Install-ChocolateyPackage 'package' 'exe' '/S' 'https://somelocation.com/thefile.exe' -options $options
~~~

### Links

 - [[Get-ChocolateyWebFile|HelpersGetChocolateyWebFile]]
 - [[Install-ChocolateyInstallPackage|HelpersInstallChocolateyInstallPackage]]
 - [[Install-ChocolateyZipPackage|HelpersInstallChocolateyZipPackage]]

---

# Install-ChocolateyPackage

This will download a native installer from a url and install it on your machine. Has error handling built in. You do not need to surround this with try catch if it is the only thing in your [[chocolateyInstall.ps1|ChocolateyInstallPS1]].

**NOTE:** This command will assert UAC/Admin privileges on the machine.

## Usage

```powershell
Install-ChocolateyPackage $packageName $installerType $silentArgs $url $url64bit `
 -validExitCodes $validExitCodes -checksum $checksum -checksumType $checksumType `
 -checksum64 $checksum64 -checksumType64 $checksumType64
```

## Examples

```powershell
Install-ChocolateyPackage 'StExBar' 'msi' '/quiet' ` 
 'http://stexbar.googlecode.com/files/StExBar-1.8.3.msi' `
 'http://stexbar.googlecode.com/files/StExBar64-1.8.3.msi'

Install-ChocolateyPackage 'mono' 'exe' '/SILENT' `
 'http://ftp.novell.com/pub/mono/archive/2.10.2/windows-installer/5/mono-2.10.2-gtksharp-2.12.10-win32-5.exe'

Install-ChocolateyPackage 'mono' 'exe' '/SILENT' ` 
 'http://somehwere/something.exe' -validExitCodes @(0,21)

Install-ChocolateyPackage 'ruby.devkit' 'exe' '/SILENT' `
 'http://cdn.rubyinstaller.org/archives/devkits/DevKit-mingw64-32-4.7.2-20130224-1151-sfx.exe' `
 'http://cdn.rubyinstaller.org/archives/devkits/DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe' `
 -checksum '9383f12958aafc425923e322460a84de' -checksumType = 'md5' `
 -checksum64 'ce99d873c1acc8bffc639bd4e764b849'
```

## Parameters

* `-packageName`

    This is an arbitrary name.

    Example: `'7zip'`

* `-installerType`

    Pick only  one to leave here.

    Example: `'exe'` or `'msi'` or `'msu'`

* `-silentArgs`

    Silent and other arguments to pass to the native installer.

    Example: `'/S'`

    If there are no silent arguments, pass this as `''`

* `-url`

    The Url to the native installer.

    Example: `'http://stexbar.googlecode.com/files/StExBar-1.8.3.msi'`

* `-url64bit` _(optional)_

    If there is a 64 bit installer available, put the link next to the other url. Chocolatey will automatically determine if the user is running a 64bit machine or not and adjust accordingly.

    Example: `'http://stexbar.googlecode.com/files/StExBar64-1.8.3.msi'`

    Defaults to the 32bit url.

* `-validExitCodes` _(optional)_

    If there are other valid exit codes besides zero signifying a successful install, please pass `-validExitCodes` with the value, including 0 as long as it is still valid.

    Example: `-validExitCodes @(0,44)`

    Defaults to `@(0)`.

* `-checksum` _(optional)_

    This allows the file being downloaded to be validated. Can be an MD5 or SHA1 hash.

    Example: `-checksum 'C67962F064924F3C7B95D69F88E745C0'`

    Defaults to ``.

* `-checksumType` _(optional)_

    This allows the file being downloaded to be validated. Can be an MD5 or SHA1 hash.

    Example: `-checksumType 'sha1'`

    Defaults to `md5`.

* `-checksum64` _(optional)_

    This allows the x64 file being downloaded to be validated. Can be an MD5 or SHA1 hash.

    Example: `-checksum64 'C67962F064924F3C7B95D69F88E745C0'`

    Defaults to ``.

* `-checksumType64` _(optional)_

    This allows the file being downloaded to be validated. Can be an MD5 or SHA1 hash.

    Example: `-checksumType64 'sha1'`

    Defaults to checksumType's value.

## See Also

* [[Install-ChocolateyZipPackage|HelpersInstallChocolateyZipPackage]] for installing a zip package.
* To add executables to the path see [[Get-ChocolateyBins|HelpersGetChocolateyBins]]

[[Function Reference|HelpersReference]]