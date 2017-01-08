## Requirements
* Windows 7+ / Windows Server 2003+
* PowerShell v2+
* .NET Framework 4+ (the installation will attempt to install .NET 4.0 if you do not have it installed)

That's it! All you need is choco.exe (that you get from the installation scripts) and you are good to go! No Visual Studio required.

## Installing Chocolatey
Chocolatey installs in seconds. Just run the following command from an ***administrative*** PowerShell v3+ prompt (Ensure [Get-ExecutionPolicy](https://go.microsoft.com/fwlink/?LinkID=135170) is not Restricted): <!--remove <button class="icon-clipboard copy-button" data-clipboard-text="iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex"></button> (copy command) remove-->

~~~powershell

iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex

~~~

We take security very seriously. <a href="https://chocolatey.org/security">Learn more</a>.

## More Install Options

<!--remove

<p><strong>Troubleshooting? Install from cmd.exe or PowerShell v2? Proxy? Need more options?</strong><br />

<a onclick="if ($(this).text() == 'Less Options') {$('#div-moreoptions').addClass('hide');$(this).text('More Options');} else {$('#div-moreoptions').removeClass('hide');$(this).text('Less Options');}">More Options</a>
</p>

<div id="div-moreoptions" class="hide">
remove-->

To install chocolatey now, open an <strong>administrative</strong> command prompt and paste the text from the box below that applies to the name of your shell and press enter.
If you need assistance opening an administrative prompt, see [open an elevated prompt in Windows 8+](http://www.howtogeek.com/194041/how-to-open-the-command-prompt-as-administrator-in-windows-8.1/) (or [Windows 7](http://www.howtogeek.com/howto/windows-vista/run-a-command-as-administrator-from-the-windows-vista-run-box/)).

**NOTE:** Please inspect [https://chocolatey.org/install.ps1](https://chocolatey.org/install.ps1) prior to running any of these scripts to ensure safety. We already know it's safe, but you should also be comfortable before running ***any*** script from the internet you are not familiar with. All of these scripts download a remote PowerShell script and execute it on your machine.

* [Install from cmd.exe](#install-from-cmdexe)
* [Install from PowerShell v2](#install-from-powershell-v2)
* [Install using PowerShell from cmd.exe](#install-using-powershell-from-cmdexe)
* [Install using NuGet Package Manager](#install-using-nuget-package-manager)
* [Install using NuGet.exe from PowerShell](#install-using-nugetexe-from-powershell)
* [Install downloaded NuGet package from PowerShell](#install-downloaded-nuget-package-from-powershell)
* [Install licensed edition](#install-licensed-edition)
* [Installing behind a proxy](#installing-behind-a-proxy)
* [Installing behind an explicit proxy](#installing-behind-an-explicit-proxy)
* [Installing to a different location](#installing-to-a-different-location)
* [Installing a particular version of Chocolatey](#installing-a-particular-version-of-chocolatey)
* [Use Windows built-in compression instead of downloading 7zip](#use-windows-built-in-compression-instead-of-downloading-7zip)
* [Installing with restricted TLS](#installing-with-restricted-tls)
* [Non-Administrative install](#non-administrative-install)

### Install from cmd.exe

* Cmd.exe - <button class="icon-clipboard copy-button" data-clipboard-text="@powershell -NoProfile -ExecutionPolicy Bypass -Command &quot;iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))&quot; && SET &quot;PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin&quot;"></button>

~~~sh

@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

~~~

### Install from PowerShell v2

* PowerShell.exe (Ensure [Get-ExecutionPolicy](https://go.microsoft.com/fwlink/?LinkID=135170) is not Restricted) - <button class="icon-clipboard copy-button" data-clipboard-text="iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))"></button>

~~~powershell

iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

~~~

### Install using PowerShell from cmd.exe

This is the best method if you want to repeat it or include it in source control. It requires no change to your existing PowerShell to allow for remote unsigned scripts.

Create a file named `installChocolatey.cmd` with the following:

~~~
@echo off

SET DIR=%~dp0%

::download install.ps1
%systemroot%\System32\WindowsPowerShell\v1.0\powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "((new-object net.webclient).DownloadFile('https://chocolatey.org/install.ps1','install.ps1'))"
::run installer
%systemroot%\System32\WindowsPowerShell\v1.0\powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "& '%DIR%install.ps1' %*"
~~~

You can also get to this file by going to [https://chocolatey.org/installchocolatey.cmd](https://chocolatey.org/installchocolatey.cmd).

If you prefer to have the install.ps1 file already, comment out the download line in the batch file and download the [`install.ps1`](https://chocolatey.org/install.ps1) from [chocolatey.org](https://chocolatey.org/install.ps1) and save it as `install.ps1` next to the `installChocolatey.cmd` file.

Run `installChocolatey.cmd` from an elevated `cmd.exe` command prompt and it will install the latest version of Chocolatey. You can not run this from `powershell.exe` without making changes to your execution policy.

**NOTE**: To create and save a `.cmd` file, please use a text editor and nothing fancy like Microsoft Word or OneNote.

### Install using NuGet Package Manager

When you have Visual Studio 2010+ and the NuGet extension installed (pre-installed on any newer versions of Visual Studio), you can simply type the following three commands and you will have Chocolatey installed on your machine.

 `Install-Package chocolatey`
 `Initialize-Chocolatey`
 `Uninstall-Package chocolatey`

### Install using NuGet.exe from PowerShell

You can also use NuGet command line to download Chocolatey:

 `nuget install chocolatey` or `nuget install chocolatey -pre`

Once you download it, open PowerShell (remote unsigned), navigate to the tools folder and run:

`& .\chocolateyInstall.ps1`

### Install downloaded NuGet package from PowerShell

You can also just download and unzip the Chocolatey package (`.nupkg` is a fancy zip file):

 1. Download the [Chocolatey package](https://chocolatey.org/api/v2/package/chocolatey/).
 1. Unzip it using any application that supports `zip` format.
 1. Open a PowerShell command shell and navigate into the unzipped package's tools folder.
 1. **NOTE**: Ensure PowerShell execution policy is set to at least bypass or remote signed (if you have issues, you may need to set it to Unrestricted).
 1. Call `& .\chocolateyInstall.ps1` to allow Chocolatey to install.
 1. **NOTE**: This will not set Chocolatey as an installed package, so it may be a good idea to also call `choco upgrade chocolatey -y` and let it reinstall the same version, but at least it will be available for upgrades then.

### Install licensed edition

Please see [[installation of licensed edition|Installation-Licensed]].

### Installing behind a proxy

Have a proxy? Try

* Cmd.exe - <button class="icon-clipboard copy-button" data-clipboard-text="@powershell -NoProfile -ExecutionPolicy Bypass -Command &quot;[System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))&quot; && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"></button>

~~~sh

@powershell -NoProfile -ExecutionPolicy Bypass -Command "[System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH="%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

~~~

* PowerShell.exe (Ensure [Get-ExecutionPolicy](https://go.microsoft.com/fwlink/?LinkID=135170) is at least RemoteSigned) - <button class="icon-clipboard copy-button" data-clipboard-text="[System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))"></button>

~~~powershell

[System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

~~~

### Installing behind an explicit proxy

See [[Installing Chocolatey Behind a Proxy Server|Proxy-Settings-for-Chocolatey#installing-chocolatey-behind-a-proxy-server]]

### Installing to a different location

1. Create a __machine__ level (__user__ level will also work) environment variable named ```ChocolateyInstall``` and set it to the folder you want Chocolatey to install to prior to installation (this environment variable must be set globally or available to PowerShell- it is not enough to simply make it available to your current command prompt session).
1. Don't use `"C:\Chocolatey"` unless necessary.
1. Create the folder manually.
1. If you have already installed (and want to change the location after the fact):
  * Follow the above steps.
  * Install Chocolatey again.
  * Copy/Move over the items from the old lib/bin directory.
  * Delete your old install directory.

**NOTE**: There is one really important consideration when installing Chocolatey to a non-default location - Chocolatey only locks down the permissions to Admins when installed to the default location.
If you are installing to another location, you will need to handle this yourself.
This is due to alternative locations could have a range of permissions that should not be changed.
See [[Why does Chocolatey install where it does|DefaultChocolateyInstallReasoning]] and https://github.com/chocolatey/choco/issues/398 for more details.

### Installing a particular version of Chocolatey

Set the following environment variable prior to install:

* `chocolateyVersion` - controls what version of Chocolatey is installed

In PowerShell, it looks like this:

~~~powershell
$env:chocolateyVersion = '0.9.9.12'
# install script
~~~

**NOTE:** This will only work with the installation methods that call https://chocolatey.org/install.ps1 as part of the install.

### Use Windows built-in compression instead of downloading 7zip

Set the following environment variable prior to install:

* `chocolateyUseWindowsCompression` - this will bypass the download and use of 7zip.

In PowerShell, it looks like this:

~~~powershell
$env:chocolateyUseWindowsCompression = 'true'
# install script
~~~

**NOTE:** This will only work with the installation methods that call https://chocolatey.org/install.ps1 as part of the install.

### Installing with restricted TLS

**NOTE:** If your server is restricted to TLS 1.1+, you need to add additional logic to be able to download and install Chocolatey (this is not necessary when running Chocolatey normally as it does this automatically). If this is for organizational use, you should consider hosting the Chocolatey package internally and installing from there. Otherwise, please see this section.

If you see an error that looks similar to the following:

~~~sh
Exception calling "DownloadString" with "1" argument(s): "The underlying connection was closed: An unexpected error
occurred on a receive."
At line:1 char:1
+ iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/in ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : WebException
~~~

It's possible that you are attempting to install from a server that needs to use TLS 1.1 or TLS 1.2 (has restricted the use of TLS 1.0 and SSL v3), you have some options.

#### Option 1
If you have the following:

* PowerShell v3+
* .NET Framework 4.5

You can just run the following instead of just the one-liner to get Chocolatey installed:

~~~powershell
$securityProtocolSettingsOriginal = [System.Net.ServicePointManager]::SecurityProtocol

try {
  # Set TLS 1.2 (3072), then TLS 1.1 (768), then TLS 1.0 (192), finally SSL 3.0 (48)
  # Use integers because the enumeration values for TLS 1.2 and TLS 1.1 won't
  # exist in .NET 4.0, even though they are addressable if .NET 4.5+ is
  # installed (.NET 4.5 is an in-place upgrade).
  [System.Net.ServicePointManager]::SecurityProtocol = 3072 -bor 768 -bor 192 -bor 48
} catch {
  Write-Warning 'Unable to set PowerShell to use TLS 1.2 and TLS 1.1 due to old .NET Framework installed. If you see underlying connection closed or trust errors, you may need to do one or more of the following: (1) upgrade to .NET Framework 4.5 and PowerShell v3, (2) specify internal Chocolatey package location (set $env:chocolateyDownloadUrl prior to install or host the package internally), (3) use the Download + PowerShell method of install. See https://chocolatey.org/install for all install options.'
}

iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

[System.Net.ServicePointManager]::SecurityProtocol = $securityProtocolSettingsOriginal
~~~

#### Option 2
You need to download and unzip the Chocolatey package, then call the PowerShell install script from there. See the [Download + PowerShell Method](#download--powershell-method) section below.

### Non-Administrative install

**NOTE**: This option should be a last resort and is considered to be an advanced scenario. Most things you do on Windows require administrative rights, especially surrounding software management, so you are going to be limited even in packages you attempt to install. If you run into issues with Chocolatey and you have set Chocolatey up this way, the first thing we are going to ask you to do is to see if it works when you have installed choco under normal circumstances. If you are using the [community package repository](https://chocolatey.org/packages), you should avoid this type of installation as over 75% of the packages you find there require administrative permission.

1. You must choose a different location than the default (see [Installing to a different location](#Installing-to-a-different-location) above). The default is a more secure location that only administrators can update.
1. Follow that with the command line / PowerShell methods of installation.

<!--remove
</div>
remove-->

## Upgrading Chocolatey

Once installed, Chocolatey can be upgraded in exactly the same way as any other package that has been installed using Chocolatey.  Simply use the command to upgrade to the latest stable release of Chocolatey:

~~~
choco upgrade chocolatey
~~~

## Uninstalling Chocolatey

See [[uninstall|Uninstallation]].

<!--remove
<p>&nbsp;</p>
remove-->

<script src="https://cdn.rawgit.com/zenorocha/clipboard.js/v1.5.10/dist/clipboard.min.js"></script>
<script>
  new Clipboard('.copy-button');
</script>

