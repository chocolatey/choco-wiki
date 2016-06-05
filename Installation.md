## Installing Chocolatey
Chocolatey installs in seconds...

**NOTE:** Need to install a particular version of Chocolatey? Proxy? Install to a different location? Advanced Options? See [Before You Install](#before-you-install) below. [Alternative installation options](#alternative-installation-options)? [Non-Administrator installation](#non-administrative-install)?

To install chocolatey now, open an <strong>administrative</strong> command prompt and paste the text from the box below that applies to the name of your shell and press enter. If you need assistance opening an administrative prompt, see [open an elevated prompt in Windows 8+](http://www.howtogeek.com/194041/how-to-open-the-command-prompt-as-administrator-in-windows-8.1/) (or [Windows 7](http://www.howtogeek.com/howto/windows-vista/run-a-command-as-administrator-from-the-windows-vista-run-box/)).

**NOTE:** Please inspect [https://chocolatey.org/install.ps1](https://chocolatey.org/install.ps1) prior to running any of these scripts to ensure safety. We already know it's safe, but you should also be comfortable before running ***any*** script from the internet you are not familiar with. All of these scripts download a remote PowerShell script and execute it on your machine.

* Cmd.exe - <button class="icon-clipboard copy-button" data-clipboard-text="@powershell -NoProfile -ExecutionPolicy Bypass -Command &quot;iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))&quot; && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"></button>

~~~sh
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
~~~

* PowerShell.exe (Ensure [Get-ExecutionPolicy](https://go.microsoft.com/fwlink/?LinkID=135170) is at least RemoteSigned) - <button class="icon-clipboard copy-button" data-clipboard-text="iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))"></button>

~~~powershell
iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))
~~~

* PowerShell v3+ (Ensure [Get-ExecutionPolicy](https://go.microsoft.com/fwlink/?LinkID=135170) is at least RemoteSigned) - <button class="icon-clipboard copy-button" data-clipboard-text="iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex"></button>

~~~powershell
iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
~~~

The easiest option to remember is this one. You may not need `-UseBasicParsing`.

What are these scripts doing? `iwr` (`Invoke-WebRequest`)/`WebClient.DownloadString` downloads the install script and passes it to `iex` (`Invoke-Expression`) to execute the contents of the script. This runs the installation script for Chocolatey. 

## Requirements
* Windows 7+ / Windows Server 2003+
* PowerShell v2+
* .NET Framework 4+ (the installation will attempt to install .NET 4.0 if you do not have it installed)

That's it! All you need is choco.exe (that you get from the installation scripts) and you are good to go!. No Visual Studio required.


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

**Can I install with a proxy?** Yes

Chocolatey will detect and use a system set proxy. However some proxies will need to be set explicitly. To do so, you would do similar to [[Proxy settings for Chocolatey|Proxy-Settings-for-Chocolatey]]

Set the following environment variable(s) prior to install:

* `chocolateyProxyLocation` - explicit proxy location. This includes the port.
* `chocolateyProxyUser` / `chocolateyProxyPassword` - optional credentials
for explicit proxy

In PowerShell, it looks like this:

~~~powershell
$env:chocolateyProxyLocation = 'https://local/proxy/server'
#$env:chocolateyProxyUser = 'username'
#$env:chocolateyProxyPassword = 'password'
# install script
~~~

**NOTE:** This will only work with the installation methods that call https://chocolatey.org/install.ps1 as part of the install.

**Can I install a particular version of Chocolatey?** Yes

Set the following environment variable prior to install:

* `chocolateyVersion` - controls what version of Chocolatey is installed

In PowerShell, it looks like this:

~~~powershell
$env:chocolateyVersion = '0.9.9.12'
# install script
~~~

**NOTE:** This will only work with the installation methods that call https://chocolatey.org/install.ps1 as part of the install.

**Can I use Windows built-in compression instead of downloading 7zip?** Yes

Set the following environment variable prior to install:

* `chocolateyUseWindowsCompression` - this will bypass the download and use of 7zip.

In PowerShell, it looks like this:

~~~powershell
$env:chocolateyUseWindowsCompression = 'true'
# install script
~~~

**NOTE:** This will only work with the installation methods that call https://chocolatey.org/install.ps1 as part of the install.

## Non-Administrative Install

This option should be a last resort and is considered to be an advanced scenario. Most things you do on Windows require administrative rights, especially surrounding software management, so you are going to be limited even in packages you attempt to install. If you run into issues with Chocolatey and you have set Chocolatey up this way, the first thing we are going to ask you to do is to see if it works when you have installed choco under normal circumstances. If you are using the [community package repository](https://chocolatey.org/packages), you should avoid this type of installation as over 75% of the packages you find there require administrative permission to install. 

1. You must choose a different location than the default (see above). The default is a more secure location that only administrators can update.
1. Follow that with the command line / PowerShell methods of installation.

## Alternative Installation Options
There are a few ways to install Chocolatey. Chocolatey exists as a [NuGet package](http://chocolatey.org/packages/chocolatey), so virtually any way you can get a NuGet package, you have the opportunity to then install it.


### Command Line
This really is the easiest method because it requires no configuration of PowerShell prior to executing it. Open a command line, paste the following and press &lt;Enter&gt;:

```cmd
@powershell -NoProfile -ExecutionPolicy unrestricted -Command "(iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))) >$null 2>&1" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
```

### PowerShell
This is the second-most easy method. Open a PowerShell command line and paste in the following and press &lt;Enter&gt;:

```powershell
(iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1')))>$null 2>&1
```

**<font color="red">Note: You must have your execution policy set to unrestricted (or at least in bypass) for this to work (`Set-ExecutionPolicy Unrestricted`). There have been [reports](https://github.com/chocolatey/chocolatey/issues/70) that RemoteSigned is enough for the install to work.</font>**
It downloads and runs (https://chocolatey.org/install.ps1).

### Cmd/PowerShell w/Proxy Server
See [[Installing Chocolatey Behind a Proxy Server|Proxy-Settings-for-Chocolatey#installing-chocolatey-behind-a-proxy-server]]

### PowerShell Through Batch Method
This is the best method if you want to repeat it or include it in source control. It requires no change to your existing PowerShell to allow for remote unsigned scripts.

Create a file named `installChocolatey.cmd` with the following:

```
@echo off

SET DIR=%~dp0%

::download install.ps1
%systemroot%\System32\WindowsPowerShell\v1.0\powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "((new-object net.webclient).DownloadFile('https://chocolatey.org/install.ps1','install.ps1'))"
::run installer
%systemroot%\System32\WindowsPowerShell\v1.0\powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "& '%DIR%install.ps1' %*"
```

You can also get to this file by going to [https://chocolatey.org/installchocolatey.cmd](https://chocolatey.org/installchocolatey.cmd).

If you prefer to have the install.ps1 file already, comment out the download line in the batch file and download the [`install.ps1`](https://chocolatey.org/install.ps1) from [chocolatey.org](https://chocolatey.org/install.ps1) and save it as `install.ps1` next to the `installChocolatey.cmd` file.

Run `installChocolatey.cmd` and it will install the latest version of Chocolatey.

### NuGet Package Manager Method

When you have Visual Studio 2010+ and the NuGet extension installed (pre-installed on any newer versions of Visual Studio), you can simply type the following three commands and you will have Chocolatey installed on your machine.

 `Install-Package chocolatey`
 `Initialize-Chocolatey`
 `Uninstall-Package chocolatey`

### NuGet.exe + PowerShell Method

You can also use NuGet command line to download Chocolatey:

 `nuget install chocolatey` or `nuget install chocolatey -pre`

Once you download it, open PowerShell (remote unsigned), navigate to the tools folder and run:

`& .\chocolateyInstall.ps1`

### Download + PowerShell Method

You can also just download and unzip the Chocolatey package (`.nupkg` is a fancy zip file):

 1. Download the [Chocolatey package](https://chocolatey.org/api/v2/package/chocolatey/).
 1. Unzip it using any application that supports `zip` format.
 1. Open a PowerShell command shell and navigate into the unzipped package's tools folder.
 1. **NOTE**: Ensure PowerShell execution policy is set to at least bypass or remote signed (if you have issues, you may need to set it to Unrestricted).
 1. Call `& .\chocolateyInstall.ps1` to allow Chocolatey to install.


<script src="https://cdn.rawgit.com/zenorocha/clipboard.js/v1.5.10/dist/clipboard.min.js"></script>
<script>
  new Clipboard('.copy-button');
</script>
