## Installing Chocolatey
Chocolatey installs in seconds...

There are a few ways to install Chocolatey. Chocolatey exists as a [nuget package](http://nuget.org/list/packages/chocolatey) so virtually any way you can get a nuget package, you have the opportunity to then install it.

If you have Visual Studio 2010 and the NuGet extension installed, perhaps the quickest method is to use NuGet Package Manager. Three commands in succession and you are done. See below.

## Why does Chocolatey install where it does by default?
Great question - [[Why does Chocolatey install where it does|DefaultChocolateyInstallReasoning]]

## Before You Install
**Can I install Chocolatey to another location?** Yes

1. Create a __machine__ level (__user__ level will also work) environment variable named ```ChocolateyInstall``` and set it to the folder you want Chocolatey to install to prior to installation (this environment variable must be set globally or available to PowerShell- it is not enough to simply make it available to your current command prompt session).  Keep in mind the [[restrictions|DefaultChocolateyInstallReasoning]] though!
1. Create the folder manually.
1. If you have already installed (and want to change the location after the fact):
  * Follow the above steps.
  * Install Chocolatey again.
  * Copy/Move over the items from the old lib/bin directory.
  * Delete your old install directory.

## Non-Administrative Install

1. You must choose a different location than the default. The default is a more secure location that only administrators can write to.
1. Follow that with the command line / PowerShell methods of installation.

## Command Line
This really is the easiest method because it requires no configuration of PowerShell prior to executing it. Open a command line, paste the following and press &lt;Enter&gt;:

```cmd
@powershell -NoProfile -ExecutionPolicy unrestricted -Command "(iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))) >$null 2>&1" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
```

## PowerShell
This is the second-most easy method. Open a PowerShell command line and paste in the following and press &lt;Enter&gt;:

```powershell
(iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1')))>$null 2>&1
```

**<font color="red">Note: You must have your execution policy set to unrestricted (or at least in bypass) for this to work (`Set-ExecutionPolicy Unrestricted`). There have been [reports](https://github.com/chocolatey/chocolatey/issues/70) that RemoteSigned is enough for the install to work.</font>**
It downloads and runs (https://chocolatey.org/install.ps1).

## PowerShell Through Batch Method
`NOTE: This method will give you a deprecated warning.`
This is the best method if you want to repeat it or include it in source control. It requires no change to your existing PowerShell to allow for remote unsigned scripts.

Download the [`install.ps1`](https://chocolatey.org/install.ps1) from [chocolatey.org](https://chocolatey.org/install.ps1) and save it as `install.ps1.

Create a file named `installChocolatey.cmd` next to that file with the following:

```
@echo off

SET DIR=%~dp0%

%systemroot%\System32\WindowsPowerShell\v1.0\powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "& '%DIR%install.ps1' %*"
```

Run `installChocolatey.cmd` and it will install and update to the latest version of Chocolatey.

## NuGet Package Manager Method

When you have Visual Studio 2010 and the NuGet extension installed, you can simply type the following three commands and you will have Chocolatey installed on your machine.

 `Install-Package chocolatey`
 `Initialize-Chocolatey`
 `Uninstall-Package chocolatey`

## NuGet.exe + PowerShell Method

You can also use nuget command line to download Chocolatey:

 `nuget install chocolatey` or `nuget install chocolatey -pre`

Once you download it, open PowerShell (remote unsigned), navigate to the tools folder and run:

`& .\chocolateyInstall.ps1`

## Download + PowerShell Method

You can also just download and unzip the Chocolatey package (`.nupkg` is a fancy zip file):

 1. Download the [Chocolatey package](https://chocolatey.org/api/v2/package/chocolatey/).
 1. Unzip it using any application that supports `zip` format.
 1. Open a PowerShell command shell and navigate into the unzipped package's tools folder.
 1. **NOTE**: Ensure PowerShell execution policy is set to at least bypass or remote signed (if you have issues, you may need to set it to Unrestricted).
 1. Call `& .\chocolateyInstall.ps1` to allow Chocolatey to install.