## The Problem
Almost all Chocolatey Packages download a file from the internet, and then directly work with this file.  This can be an EXE or an MSI, whose location is passed directly into the ```Install-ChocolateyPackage``` Helper method, or a zip file passed into the ```Install-ChocolateyZipPackage``` Helper method.

However, there are times when using a file directly is not an option.  From time to time, it is necessary to use an ISO file within your Chocolatey Package.  In later versions of the Windows Operating System, PowerShell provides the ability to mount this ISO file directly using the ```Mount-DiskImage``` cmdlet.  However, in earlier versions of Windows, this is not possible.

In order to maintain backwards compatibility with older Operating Systems, when using an ISO file, you should use a single method that works everywhere.

## The Solution
ImDisk Virtual Disk Driver (imdisk) is a software package that allows an ISO file to be mounted, but more importantly, it works for Windows NT/2000/XP/Vista/7/8/8.1 or Windows Server 2003/2008/2012 (so basically, everything!).  That means that you can use one, common, method, for mounting ISO files when required within your Chocolatey Packages.

## Code Sample

```powershell

# First, download the ISO file from the internet (NOTE: you could equally use a locally stored ISO)
Get-ChocolateyWebFile 'WindowsSDK2008' "$env:temp\winsdk2008.iso" 'http://download.microsoft.com/download/f/e/6/fe6eb291-e187-4b06-ad78-bb45d066c30f/6.0.6001.18000.367-KRMSDK_EN.iso'

# Next, mount the ISO file, ready for using it's contents (NOTE: the last parameter here is the drive letter that will be assigned to the mounted ISO)
imdisk -a -f "$env:temp\winsdk2008.iso" -m "w:"

# Run commands against the mounted ISO, in this case, run the setup.exe
Install-ChocolateyInstallPackage 'WindowsSDK2008' 'exe' '/q' 'w:\Setup.exe'

# Unmount the ISO file when finished
imdisk -d -m w:

```

**NOTE:** Code sample was taken from this [package](https://chocolatey.org/packages/WindowsSDK2008/6.0.6001), thanks to [dave42](https://chocolatey.org/profiles/dave42) for sharing