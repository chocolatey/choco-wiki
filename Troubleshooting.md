# Troubleshooting

There are some well-known things you may run into when you are using Chocolatey. We've tried to get some of the high level things you may run into into one document.

**NOTE**: This is a work in progress. It doesn't cover all of the troubleshooting steps that are known but it is attempting to cover quite a few.

<!-- TOC insertAnchor:true -->

- [General](#general)
- [Chocolatey Installation](#chocolatey-installation)
  - [The underlying connection was closed](#the-underlying-connection-was-closed)
  - [I'm getting a 403 unauthorized issue attempting to install Chocolatey](#im-getting-a-403-unauthorized-issue-attempting-to-install-chocolatey)
  - [I am having trouble with PowerShell to install Chocolatey](#i-am-having-trouble-with-powershell-to-install-chocolatey)
- [Licensed Installation](#licensed-installation)
- [Creating Packages](#creating-packages)
  - [Install-ChocolateyPath doesn't seem to work.](#install-chocolateypath-doesnt-seem-to-work)
  - [ERROR: Cannot bind parameter because parameter 'fileType' is specified more than once.](#error-cannot-bind-parameter-because-parameter-filetype-is-specified-more-than-once)
  - [ERROR: This package does not support 64 bit architecture.](#error-this-package-does-not-support-64-bit-architecture)
  - ["ERROR: This package does not support 64 bit architecture." when trying to install from a local or included binary.](#error-this-package-does-not-support-64-bit-architecture-when-trying-to-install-from-a-local-or-included-binary)
- [Runtime](#runtime)
  - [I can't get the PowerShell tab completion working.](#i-cant-get-the-powershell-tab-completion-working)
  - [Why does choco in{tab} not work for me?](#why-does-choco-intab-not-work-for-me)
  - [Microsoft.Powershell_profile.ps1 cannot be loaded. The file is not digitally signed.](#microsoftpowershell_profileps1-cannot-be-loaded-the-file-is-not-digitally-signed)
  - [I'm getting a 403 unauthorized issue when attempting to use the community package repository.](#im-getting-a-403-unauthorized-issue-when-attempting-to-use-the-community-package-repository)
  - [I'm seeing Chocolatey / *application* / *tool* using 32 bit to run instead of x64. What is going on?](#im-seeing-chocolatey--application--tool-using-32-bit-to-run-instead-of-x64-what-is-going-on)
  - [A package is broken for me](#a-package-is-broken-for-me)
  - [The package install failed with 1603](#the-package-install-failed-with-1603)
  - [Already referencing a newer version of 'packagename'](#already-referencing-a-newer-version-of-packagename)
  - [Not recognized as the name of a cmdlet, function, script file, or operable program](#not-recognized-as-the-name-of-a-cmdlet-function-script-file-or-operable-program)
  - [My PATH is not getting updated](#my-path-is-not-getting-updated)
  - [RefreshEnv has no effect](#refreshenv-has-no-effect)

<!-- /TOC -->

<a id="markdown-general" name="general"></a>
## General

If you are unable to find answers to your questions here, please see https://chocolatey.org/support (FOSS and Licensed) and https://chocolatey.org/bugs to learn more about how you can report issues and get things fixed if they are broken.

Also consider the [[frequently asked questions|ChocolateyFAQs]].

<a id="markdown-chocolatey-installation" name="chocolatey-installation"></a>
## Chocolatey Installation

<a name="the-underlying-connection-was-closed"></a>
<a id="markdown-the-underlying-connection-was-closed" name="the-underlying-connection-was-closed"></a>
### The underlying connection was closed
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

It's possible that you are attempting to install from a server that needs to use TLS 1.1 or TLS 1.2.

Please see [[Installing with Restricted TLS|Installation#installing-with-restricted-tls]]

<a id="im-getting-a-403-unauthorized-issue-when-attempting-to-install-chocolatey"></a>
<a id="markdown-im-getting-a-403-unauthorized-issue-attempting-to-install-chocolatey" name="im-getting-a-403-unauthorized-issue-attempting-to-install-chocolatey"></a>
### I'm getting a 403 unauthorized issue attempting to install Chocolatey

Please see [I'm getting a 403 unauthorized issue when attempting to use the community package repository.](#im-getting-a-403-unauthorized-issue-when-attempting-to-use-the-community-package-repository)

<a id="markdown-i-am-having-trouble-with-powershell-to-install-chocolatey" name="i-am-having-trouble-with-powershell-to-install-chocolatey"></a>
### I am having trouble with PowerShell to install Chocolatey

See the More Options section of [[installation|Installation#more-install-options]].

<a id="markdown-licensed-installation" name="licensed-installation"></a>
## Licensed Installation

See [[licensed installation|Installation-Licensed]]. If you are having issues, please see https://chocolatey.org/support for details on how to get support.

<a id="markdown-creating-packages" name="creating-packages"></a>
## Creating Packages

<a id="markdown-install-chocolateypath-doesnt-seem-to-work" name="install-chocolateypath-doesnt-seem-to-work"></a>
### Install-ChocolateyPath doesn't seem to work.
I added `Install-ChocolateyPath $binPath`, but after installing when I try to run the installed application I get "not recognized as the name of a cmdlet, function, script file, or operable program." Please see [My PATH is not getting updated](#my-path-is-not-getting-updated).

<a id="markdown-error-cannot-bind-parameter-because-parameter-filetype-is-specified-more-than-once" name="error-cannot-bind-parameter-because-parameter-filetype-is-specified-more-than-once"></a>
### ERROR: Cannot bind parameter because parameter 'fileType' is specified more than once.

This error is seen sometimes on versions of Chocolatey older than 0.10.6. The problem is likely you have the following in your packaging:

~~~powershell
$toolsPath      = $(Split-Path -parent $MyInvocation.MyCommand.Definition)

$packageArgs = @{
  packageName   = 'test'
  fileType      = 'MSI'
  file          = "$toolsPath\somefile.msi"
  softwareName  = 'test'
  silentArgs    = '/qn /norestart'
  validExitCodes= @(0)
}

Install-ChocolateyPackage @packageArgs
#Install-ChocolateyInstallPackage @packageArgs # this is what you meant to use in this case.
~~~

`Install-ChocolateyPackage` didn't have both a `File` parameter and a `FileType` parameter. PowerShell has a "feature" where it does partial matching of parameters. When you splat the parameters in, it tries to apply both `File` and `FileType` to `FileType` and throws the above error.

Typically, when you are installing locally, you likely want to use `Install-ChocolateyInstallPackage` anyway.

Reference: https://groups.google.com/d/msgid/chocolatey/40736df7-7f3f-4be7-929d-1606be0e3a62%40googlegroups.com

<a id="markdown-error-this-package-does-not-support-64-bit-architecture" name="error-this-package-does-not-support-64-bit-architecture"></a>
### ERROR: This package does not support 64 bit architecture.
This message is from https://github.com/chocolatey/choco/issues/527 - it is when the url value chosen is empty. This is common when you are creating a package and you forget to use splatting, instead passing the variable in as the first positional parameter to a function.

This means you have set up your arguments for a function and then called something like `Install-ChocolateyPackage $packageArgs` instead of `Install-ChocolateyPackage @packageArgs`. Note `@` is for splatting, taking the values in the hash variable and using the key/values to pass those each as parameters to a function, where `$` just passes the entire hash as the first parameter of the function.

~~~powershell
# this is a hash array
$packageArgs = @{
  packageName   = 'test'
  fileType      = 'exe'
  url           = 'https://location'
  url64bit      = 'https://location64'
  checksum      = 'checksum'
  checksum64    = 'checksum64'
  checksumType  = 'sha256'
  silentArgs    = "/qn /norestart /l*v `"$($env:TEMP)\$($packageName).$($env:chocolateyPackageVersion).MsiInstall.log`""
  validExitCodes= @(0, 3010, 1641)
}

Install-ChocolateyPackage $packageArgs # this is incorrect and will pass the entire hash as the first positional parameter
#Install-ChocolateyPackage @packageArgs # is what you are looking for

# Splatting takes the above hash and calls Install-ChocolateyPackage like this:
# Install-ChocolateyPackage -PackageName 'test' -FileType 'exe' -Url 'https://location' `
#           -Url64bit 'https://location64' -Checksum 'checksum' -Checksum64 'checksum64' `
#           -ChecksumType 'sha256' `
#           -SilentArgs "/qn /norestart /l*v `"$($env:TEMP)\$($packageName).$($env:chocolateyPackageVersion).MsiInstall.log`"" `
#           -ValidExitCodes @(0, 3010, 1641)
~~~

**NOTE**: It is helpful to always use `choco new` when creating packages, it has this correct and you never run into this error.

References:
* https://github.com/majkinetor/au/issues/70
* https://groups.google.com/d/msgid/chocolatey/5c544e16-e1b2-4249-bad6-4591017df81b%40googlegroups.com
* https://github.com/chocolatey/chocolatey-coreteampackages/issues/439 (separate issue)

<a id="markdown-error-this-package-does-not-support-64-bit-architecture-when-trying-to-install-from-a-local-or-included-binary" name="error-this-package-does-not-support-64-bit-architecture-when-trying-to-install-from-a-local-or-included-binary"></a>
### "ERROR: This package does not support 64 bit architecture." when trying to install from a local or included binary.

This is similar to the above, the error is the same. In most cases it stems from setting up your package parameters for `Install-ChocolateyInstallPackage` but calling `Install-ChocolateyPackage` instead. Learn the differences at the [[PowerShell function reference|HelpersReference]].

Reference: https://groups.google.com/d/msgid/chocolatey/d11d8eb2-74b3-4c2c-b0bb-d1a1ed3df389%40googlegroups.com

<a id="markdown-runtime" name="runtime"></a>
## Runtime

<a id="markdown-i-cant-get-the-powershell-tab-completion-working" name="i-cant-get-the-powershell-tab-completion-working"></a>
### I can't get the PowerShell tab completion working.
See next question.

<a id="markdown-why-does-choco-intab-not-work-for-me" name="why-does-choco-intab-not-work-for-me"></a>
### Why does choco in{tab} not work for me?

This means the import failed during install/upgrade. Chocolatey does supply a warning when this happens in the install/upgrade log. Take a look there.

The warning may look like: `"Not setting tab completion: Profile file does not exist at 'C:\Users\garyc\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1'."`

Once you've looked at your log to determine what it said, here are some followup steps:
- If this is the same shell that the upgrade occurred in, the message states you need to update your profile - run `. $profile`. Try that first, then try restarting your shell and see if it takes hold.
- If it still doesn't work, it means there was a failure setting the profile with the module.
- This could be due to PowerShell Execution Policy settings. Run `Get-ExecutionPolicy` - if it is set to `Restricted` you need to adjust that to something like `RemoteSigned`. See about execution policies (link)
- If that is fine, then we need to look at your "$profile". Run `type $profile`. Examine the output. You should have something like this in the file:
    ~~~ powershell
    # Chocolatey profile
    $ChocolateyProfile = "$env:ChocolateyInstall\helpers\chocolateyProfile.psm1"
    if (Test-Path($ChocolateyProfile)) {
      Import-Module "$ChocolateyProfile"
    }
    ~~~
- If you don't see that, let's add it. Run `Write-Host $profile` - note the location and open it up in an editor (anything but plain old notepad.exe for the love of..., well your favorite editor).
  - Now let's add that text above to the profile. Save and close the file. Now type `. $profile` to update your current Shell. Give `choco in<tab>` a shot again. If it still doesn't work we'll need to examine something a bit more deeply about your environment. Please submit an issue so we can investigate.

<a id="markdown-microsoftpowershell_profileps1-cannot-be-loaded-the-file-is-not-digitally-signed" name="microsoftpowershell_profileps1-cannot-be-loaded-the-file-is-not-digitally-signed"></a>
### Microsoft.Powershell_profile.ps1 cannot be loaded. The file is not digitally signed.
If you are seeing this, your PowerShell execution policy is either `AllSigned` or `Restricted`. You could be seeing this due to the addition of the Chocolatey profile (above question) for tab completion. You need to authenticode sign the PowerShell profile file, rename it, or remove it.

Reference: https://groups.google.com/d/msgid/chocolatey/58ef0ece-5e2a-4c2c-82a8-10e1711bdd3f%40googlegroups.com

<a id="markdown-im-getting-a-403-unauthorized-issue-when-attempting-to-use-the-community-package-repository" name="im-getting-a-403-unauthorized-issue-when-attempting-to-use-the-community-package-repository"></a>
### I'm getting a 403 unauthorized issue when attempting to use the community package repository.

It could be one of a few things:

* You have a proxy that you need to configure
* It is being blocked in your organization
* We broke something (this is the least likely reason)
* CloudFlare has blocked your IP due to reasons
* The Chocolatey Community Team may have blocked access due to abuse (1M+ package installs over 30 days) see [[excessive use for details|CommunityPackagesDisclaimer#excessive-use]]

You can use a tool like [Fiddler](http://www.telerik.com/fiddler) (choco install for this would not be helpful in your case) to help determine what is going on.

If you determine it is CloudFlare blocking your IP, we may be able to get you whitelisted for Chocolatey - see https://chocolatey.org/contact (send message to "Website" in drop down). If you have been completely blocked, go to https://gitter.im/chocolatey/choco and contact us there.

<a id="markdown-im-seeing-chocolatey--application--tool-using-32-bit-to-run-instead-of-x64-what-is-going-on" name="im-seeing-chocolatey--application--tool-using-32-bit-to-run-instead-of-x64-what-is-going-on"></a>
### I'm seeing Chocolatey / *application* / *tool* using 32 bit to run instead of x64. What is going on?
The shims are generated as "Any CPU" programs, which depend on the `Enable64Bit` registry value to be set to `1`, which it is by default. A way to fix it is to issue the following command at the location where the prompt shows below:

    C:\Windows\Microsoft.NET\Framework64\v2.0.50727> Ldr64 set64

[Any CPU 32-bit mode on 64 bit machine](http://stackoverflow.com/a/14857294)

<a id="markdown-a-package-is-broken-for-me" name="a-package-is-broken-for-me"></a>
### A package is broken for me
Depening on where you are installing this package from, we suggest you first look at your log files for more detailed output on the logs (based on the failure instructions).

<a id="markdown-the-package-install-failed-with-1603" name="the-package-install-failed-with-1603"></a>
### The package install failed with 1603
This is a generic MSI error code - you probably want to ensure you capture the log output from the MSI - if the package doesn't have it in the script, add it with `--install-arguments '"/l*v c:\msi_install.log"'` and then search the log file that is created for `Return Value 3`. This typically surrounds the actual error. Typically it can be anything from

* The installer doesn't allow reinstalling the same version
* There is a pending reboot
* Some prerequisite has not been met
* The installer doesn't allow installing an older version
* etc - it's a generic error like we said

<a id="markdown-already-referencing-a-newer-version-of-packagename" name="already-referencing-a-newer-version-of-packagename"></a>
###  Already referencing a newer version of 'packagename'
So you are attempting to upgrade and you get this strange message. But you know you have a more up to date package than Chocolatey thinks you do, at least in this instance.

This cryptic error typically means there is a stray nupkg somewhere in the structure. There is a tiny bug somewhere and rarely a nupkg will stick around when it should have been removed. Once we can determine where this happens we can fix it, until then we have a way to fix the issue manually.

* Open PowerShell and run the following script:
* `gci -Path "$env:ChocolateyInstall\lib" -Recurse -Filter "*.nupkg" | %{ $_.FullName }`
* Look for any nupkg files that should not be there. Typically this is probably going to be a nupkg with a version number in the name in a folder that doesn't have a version number in the name of the folder.
* Delete the offending nupkg from the folder.
* Then everything should be back to normal again.

<a id="markdown-not-recognized-as-the-name-of-a-cmdlet-function-script-file-or-operable-program" name="not-recognized-as-the-name-of-a-cmdlet-function-script-file-or-operable-program"></a>
### Not recognized as the name of a cmdlet, function, script file, or operable program
* With Chocolatey (choco) itself? Close and reopen the shell as the install didn't ensure the PATH was updated in the current shell.
* With something you installed? See [My PATH is not getting updated](#my-path-is-not-getting-updated).

<a id="markdown-my-path-is-not-getting-updated" name="my-path-is-not-getting-updated"></a>
### My PATH is not getting updated
First let's understand the scopes of the PATH environment variable. There is Machine, Current User (User), and Process environment variables. Process is a special scope that applies to the command shell (cmd.exe/powershell.exe). Process gathers Machine and User scopes when it first loads up **AND *ONLY* when it first loads up**. Yes, you read that right. This is a limitation of Windows, shells were never given the ability to see changes to environment variables and act accordingly. You traditionally need to install something, then close and reopen your shell to see it updated. That is a pretty clunky experience.

To understand this, open Powershell, then install something that updates the PATH, and run the following in that already open PowerShell command shell:

* `Write-Host "$((Get-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment\' -Name 'PATH').Path); $((Get-ItemProperty -Path 'HKCU:\Environment' -Name 'PATH').Path)"` - This is Machine/User PATH pulled directly from the registry.
* `$env:PATH` - This is Process PATH

Contrast the two, notice there is a difference (there may be a lot of data to sift through).

When you update the Machine/User environment variables, you would also need to update the Process environment variables as it doesn't not see those changes. Fortunately, with Chocolatey, we have a tool called `refreshenv` that does this for you so you don't need to close and reopen the shell. If you run `refreshenv` and it doesn't have any effect, see [RefreshEnv has no effect](#refreshenv-has-no-effect).

<a id="markdown-refreshenv-has-no-effect" name="refreshenv-has-no-effect"></a>
### RefreshEnv has no effect
If you are in cmd.exe, it should just work. In PowerShell, you need to install the Chocolatey PowerShell profile first for the command to work.

Take note of whether it says "refreshing environment variables for ***cmd.exe***" or "refreshing environment variables for ***powershell.exe***". If you are in PowerShell and you see "***cmd.exe***" when you run `refreshenv`, then you need to do some additional work to get things set up. see [Why does choco in{tab} not work for me?](#why-does-choco-intab-not-work-for-me).
