## Requirements
* Windows 7+ / Windows Server 2003+
* PowerShell v2+
* .NET Framework 4+ (the installation will attempt to install .NET 4.0 if you do not have it installed)

That's it! All you need is choco.exe (that you get from the installation scripts) and you are good to go! No Visual Studio required.

## Installing Chocolatey

Chocolatey installs in seconds. You are just a few steps from running choco right now!

1. First, ensure that you are using an ***[administrative shell](http://www.howtogeek.com/194041/how-to-open-the-command-prompt-as-administrator-in-windows-8.1/)*** - you can also install as a non-admin, check out [More Options](#more-install-options).
1. Copy the text specific to your command shell - [cmd.exe](#install-with-cmdexe) or [powershell.exe](#install-with-powershellexe).
1. Paste the copied text into your shell and press Enter.
1. Wait a few seconds for the command to complete.
1. If you don't see any errors, you are ready to use Chocolatey! Type `choco` or `choco -?` now.

**NOTE**: If you are behind a proxy, please see [More Options](#more-install-options).
**NOTE**: Need completely offline solution? See [More Options](#more-install-options).
**NOTE**: Installing the licensed edition? See [[install licensed edition|Installation-Licensed]].

#### Install with cmd.exe

Run the following command: <!--remove <button class="icon-clipboard copy-button" data-clipboard-text="@powershell -NoProfile -ExecutionPolicy Bypass -Command &quot;iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))&quot; && SET &quot;PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin&quot;"></button> (copy command text) remove-->

~~~sh

@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

~~~

#### Install with PowerShell.exe

With PowerShell, there is an additional step. You must ensure [Get-ExecutionPolicy](https://go.microsoft.com/fwlink/?LinkID=135170) is not Restricted. We suggest using `Bypass` to bypass the policy to get things installed or `AllSigned` for quite a bit more security.

* Run `Get-ExecutionPolicy`. If it returns `Restricted`, then run `Set-ExecutionPolicy AllSigned` or `Set-ExecutionPolicy Bypass`.
* Now run the following command: <!--remove <button class="icon-clipboard copy-button" data-clipboard-text="iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))"></button> (copy command text) remove-->

~~~powershell

# Don't forget to ensure ExecutionPolicy above
iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

~~~

#### Additional considerations

**NOTE:** Please inspect [https://chocolatey.org/install.ps1](https://chocolatey.org/install.ps1) prior to running any of these scripts to ensure safety. We already know it's safe, but you should also be comfortable before running ***any*** script from the internet you are not familiar with. All of these scripts download a remote PowerShell script and execute it on your machine.

We take security very seriously. <a href="https://chocolatey.org/security">Learn more</a>.

## More Install Options

<!--remove

<p><strong>Troubleshooting? Proxy? Need more options?</strong><br />

<a onclick="if ($(this).text() == 'Less Options') {$('#div-moreoptions').addClass('hide');$(this).text('More Options');} else {$('#div-moreoptions').removeClass('hide');$(this).text('Less Options');}">More Options</a>
</p>

<div id="div-moreoptions" class="hide">
remove-->

* [Install from PowerShell v3+](#install-from-powershell-v3)
* [Completely offline/internal install](#completely-offline-install)
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

### Install from PowerShell v3+

With PowerShell, there is an additional step or two. You must ensure [Get-ExecutionPolicy](https://go.microsoft.com/fwlink/?LinkID=135170) is not Restricted. We suggest using `Bypass` to bypass the policy to get things installed or `AllSigned` for quite a bit more security.

* Run `Get-ExecutionPolicy`. If it returns `Restricted`, then run `Set-ExecutionPolicy AllSigned` or `Set-ExecutionPolicy Bypass`.
* Now run the following command: <!--remove <button class="icon-clipboard copy-button" data-clipboard-text="iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex"></button> (copy command text) remove-->

~~~powershell

iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex

~~~


### Completely offline install

With completely offline use of Chocolatey, you want to ensure you remove the default community package source (`choco source list` followed by `choco source remove -n chocolatey`, or however you would do that with a configuration manager [like Puppet](https://forge.puppet.com/puppetlabs/chocolatey#sources-configuration)).

1. The first step with offline is to obtain a copy of the Chocolatey Nupkg (nupkg files are just fancy zip files). Go to https://chocolatey.org/packages/chocolatey and find a version you want.
1. Click on Download to download that version's nupkg file.

    ![download chocolatey.nupkg visual](images/DownloadChocolateyPackage.png)

1. You can put the chocolatey.nupkg on an internal package repository and then address that full path, similar to how you see in the Puppet provider - https://forge.puppet.com/puppetlabs/chocolatey#manage-chocolatey-installation
1. Then you would run a script similar to the below to address that local install. If it is on a repository somewhere, you will need to enhance the below script to get that file  (the Chocolatey Puppet provider install script shows that).

~~~powershell
# based on local file, see above instructions for how you can obtain package
# from internal repository and download it local
$localChocolateyPackageFilePath = 'c:\packages\chocolatey.0.10.0.nupkg'

$ChocoInstallPath = "$($env:SystemDrive)\ProgramData\Chocolatey\bin"
$env:ChocolateyInstall = "$($env:SystemDrive)\ProgramData\Chocolatey"
$env:Path += ";$ChocoInstallPath"
$DebugPreference = "Continue";
# if you really want to see debugging output related to the
# installation, uncomment the next line
#$env:ChocolateyEnvironmentDebug = 'true'

function Install-LocalChocolateyPackage {
param (
  [string]$chocolateyPackageFilePath = ''
)

  if ($chocolateyPackageFilePath -eq $null -or $chocolateyPackageFilePath -eq '') {
    throw "You must specify a local package to run the local install."
  }

  if (!(Test-Path($chocolateyPackageFilePath))) {
    throw "No file exists at $chocolateyPackageFilePath"
  }

  if ($env:TEMP -eq $null) {
    $env:TEMP = Join-Path $env:SystemDrive 'temp'
  }
  $chocTempDir = Join-Path $env:TEMP "chocolatey"
  $tempDir = Join-Path $chocTempDir "chocInstall"
  if (![System.IO.Directory]::Exists($tempDir)) {[System.IO.Directory]::CreateDirectory($tempDir)}
  $file = Join-Path $tempDir "chocolatey.zip"
  Copy-Item $chocolateyPackageFilePath $file -Force

  # unzip the package
  Write-Output "Extracting $file to $tempDir..."
  $shellApplication = new-object -com shell.application
  $zipPackage = $shellApplication.NameSpace($file)
  $destinationFolder = $shellApplication.NameSpace($tempDir)
  $destinationFolder.CopyHere($zipPackage.Items(),0x10)

  # Call chocolatey install
  Write-Output "Installing chocolatey on this machine"
  $toolsFolder = Join-Path $tempDir "tools"
  $chocInstallPS1 = Join-Path $toolsFolder "chocolateyInstall.ps1"

  & $chocInstallPS1

  Write-Output 'Ensuring chocolatey commands are on the path'
  $chocInstallVariableName = "ChocolateyInstall"
  $chocoPath = [Environment]::GetEnvironmentVariable($chocInstallVariableName)
  if ($chocoPath -eq $null -or $chocoPath -eq '') {
    $chocoPath = 'C:\ProgramData\Chocolatey'
  }

  $chocoExePath = Join-Path $chocoPath 'bin'

  if ($($env:Path).ToLower().Contains($($chocoExePath).ToLower()) -eq $false) {
    $env:Path = [Environment]::GetEnvironmentVariable('Path',[System.EnvironmentVariableTarget]::Machine);
  }
}

# Idempotence - do not install Chocolatey if it is already installed
if (!(Test-Path $ChocoInstallPath)) {
  # Install Chocolatey
  Install-LocalChocolateyPackage $localChocolateyPackageFilePath
}
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
 1. Ensure the downloaded nupkg is not blocked.
 1. Unzip it using any application that supports `zip` format.
 1. Open a PowerShell command shell and navigate into the unzipped package's tools folder.
 1. **NOTE**: Ensure PowerShell execution policy is set to at least bypass or remote signed (if you have issues, you may need to set it to Unrestricted).
 1. Call `& .\chocolateyInstall.ps1` to allow Chocolatey to install.
 1. **NOTE**: This will not set Chocolatey as an installed package, so it may be a good idea to also call `choco upgrade chocolatey -y` and let it reinstall the same version, but at least it will be available for upgrades then.

### Install licensed edition

Please see [[installation of licensed edition|Installation-Licensed]].

### Installing behind a proxy

Have a proxy? Try

* Cmd.exe: <!--remove <button class="icon-clipboard copy-button" data-clipboard-text="@powershell -NoProfile -ExecutionPolicy Bypass -Command &quot;[System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))&quot; && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"></button> (copy command text) remove-->

~~~sh

@powershell -NoProfile -ExecutionPolicy Bypass -Command "[System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH="%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

~~~

* PowerShell.exe (Ensure [Get-ExecutionPolicy](https://go.microsoft.com/fwlink/?LinkID=135170) is at least RemoteSigned): <!--remove <button class="icon-clipboard copy-button" data-clipboard-text="[System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))"></button> (copy command text) remove-->

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

**NOTE**: This option should be a last resort and is considered to be a more advanced scenario - most things you do on Windows require administrative rights, especially surrounding software management, so you are going to be limited even in packages you attempt to install. If you are using the [community package repository](https://chocolatey.org/packages), there are over 200 packages you can install from the community repository without administrative permission - see https://chocolatey.org/packages?q=id%3Aportable.

1. You must choose a different location than the default (see [Installing to a different location](#Installing-to-a-different-location) above). The default is a more secure location that only administrators can update.
1. Follow that with the command line / PowerShell methods of installation.
1. Here is an example of this.

NonAdmin.ps1:

~~~powershell
# Set directory for installation - Chocolatey does not lock
# down the directory if not the default
$InstallDir='C:\ProgramData\chocoportable'
$env:ChocolateyInstall="$InstallDir"

# If your PowerShell Execution policy is restrictive, you may
# not be able to get around that. Try setting your session to
# Bypass.
Set-ExecutionPolicy Bypass

# All install options - offline, proxy, etc at
# https://chocolatey.org/install
iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
# PowerShell 3+?
#iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex

choco install puppet-agent.portable -y
choco install ruby.portable -y
choco install git.commandline -y

# pick an editor
#choco install visualstudiocode.portable -y # not yet available
choco install notepadplusplus.commandline -y
#choco install nano -y
#choco install vim-tux.portable

# What else can I install without admin rights?
# https://chocolatey.org/packages?q=id%3Aportable
~~~

If you prefer or need cmd.exe example, please see https://gist.github.com/ferventcoder/78fa6b6f4d6e2b12c89680cbc0daec78


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

