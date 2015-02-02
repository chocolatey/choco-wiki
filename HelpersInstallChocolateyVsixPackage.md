## Install-ChocolateyVsixPackage
## New as of v0.9.8.20.

`Install-ChocolateyVsixPackage $packageName $vsixUrl $vsVersion -checksum $checksum -checksumType $checksumType`

## Description
Downloads and installs a VSIX package for Visual Studio. You do not need to surround this with try catch if it is the only thing in your [[chocolateyInstall.ps1|ChocolateyInstallPS1]].

VSIX packages are Extensions for the Visual Studio IDE. The Visual Studio Gallery at http://visualstudiogallery.msdn.microsoft.com/ is the public extension feed and hosts thousands of extensions. You can locate a VSIX Url by finding the download link of Visual Studio extensions on the Visual Studio Gallery.

## Parameters
### $packageName
This is an arbitrary name.
Example: `'7zip'`

### $vsixUrl
The URL of the package to be installed.
Example: `http://visualstudiogallery.msdn.microsoft.com/ea3a37c9-1c76-4628-803e-b10a109e7943/file/73131/1/AutoWrockTestable.vsix`

### $vsVersion
The Major version number of Visual Studio where the package should be installed. This is optional. If not specified, the most recent Visual Studio installation will be targeted.

### $checksum (optional but will be required later) - v0.9.8.24+
This allows the file being downloaded to be validated. Can be an MD5 or SHA1 hash.
Example: `-checksum 'C67962F064924F3C7B95D69F88E745C0'`
Defaults to ``.

### $checksumType (optional) - v0.9.8.24+
This allows the file being downloaded to be validated. Can be an MD5 or SHA1 hash.
Example: `-checksumType 'sha1'`
Defaults to `md5`.

## Examples
    Install-ChocolateyVsixPackage "MyPackage" http://visualstudiogallery.msdn.microsoft.com/ea3a37c9-1c76-4628-803e-b10a109e7943/file/73131/1/AutoWrockTestable.vsix

This downloads the AutoWrockTestable VSIX from the Visual Studio Gallery and installs it to the latest version of VS.

    Install-ChocolateyVsixPackage "MyPackage" http://visualstudiogallery.msdn.microsoft.com/ea3a37c9-1c76-4628-803e-b10a109e7943/file/73131/1/AutoWrockTestable.vsix 11

This downloads the AutoWrockTestable VSIX from the Visual Studio Gallery and installs it to Visual Studio 2012 (v11.0).

[[Helper Reference|HelpersReference]]