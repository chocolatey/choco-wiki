# Getting Started

<!-- TOC -->

- [Using Chocolatey](#using-chocolatey)
  - [Overriding default install directory or other advanced install concepts](#overriding-default-install-directory-or-other-advanced-install-concepts)
- [Terminology](#terminology)
- [What Are Chocolatey Packages?](#what-are-chocolatey-packages)
- [How does Chocolatey work?](#how-does-chocolatey-work)
  - [Installation](#installation)
  - [Upgrade](#upgrade)
  - [Uninstall](#uninstall)
- [Where are Chocolatey packages installed to?](#where-are-chocolatey-packages-installed-to)
- [How does Chocolatey work with Programs and Features? Existing installs?](#how-does-chocolatey-work-with-programs-and-features-existing-installs)
- [Where does Chocolatey install packages from?](#where-does-chocolatey-install-packages-from)

<!-- /TOC -->

## Using Chocolatey
Now that you have Chocolatey on your machine ([[need to install?|Installation]]), you can run several commands.

Take a look at the [[command reference|CommandsReference]]. We are going to be using the [[install command|CommandsInstall]].

Let's install [Notepad++](http://notepad-plus-plus.org/).

1. Open a command line.
1. Type `choco install notepadplusplus` and press Enter.
1. If you have UAC turned on or are not an administrator, Chocolatey is going to request administrative permission at some point (at least once during the process). Otherwise it will not be able to finish what it is doing successfully. If you don't have UAC turned on, it will just continue on without stopping to bother you.
1. That's it. Pretty simple but powerful little concept!

### Overriding default install directory or other advanced install concepts

1. Yes we support that through the use of install arguments - see [[Install Arguments|CommandsInstall#installarguments]]
1. If you wanted to pass native argument to the installer, like the install directory, you would need to know the silent argument passed to that particular installer and then you would specify it on the command line or in the packages.config.
1. If it was an MSI, then usually you could pass `-ia "INSTALLDIR=""D:\Program Files"""` (for cmd.exe, it's different for PowerShell). See [[how to pass options/switches|CommandsReference#how-to-pass-options--switches]] for specifics on passing quoted values through.
1. For example, Notepad++ uses the [NSIS](http://nsis.sourceforge.net/Main_Page) (NullSoft Scriptable Install System) installer. If we look at the silent options, we see that [/D](http://nsis.sourceforge.net/Docs/Chapter3.html#installerusagecommon) is how we influence the install directory. So we would pass `choco install notepadplusplus.install -ia "'/D=E:\SomeDirectory\somebody\npp'"` -note that we are looking at the specific package over the virtual (although you can do the same with notepadplusplus as well).

Is there a better way? Absolutely, see [[ubiquitous install directory switch|FeaturesInstallDirectoryOverride]]!

## Terminology

Software and Package are not terms used interchangeably in the Chocolatey community. It's important to understand the distinction between them and how they are related.

* **Chocolatey** - Windows package manager for software management, can also be considered a framework
* **Chocolatey.org** - Website that is one stop shop for Chocolatey information and contains a community maintained package repository. It is important to understand that Chocolatey and the community feed are not intertwined, they are not the same thing. See [[community feed disclaimer|CommunityPackagesDisclaimer]] to get a better understanding.
* **NuGet** - Framework and .NET package manager for software libraries. Chocolatey uses the NuGet packaging framework
* **Package** - See [What are Chocolatey Packages?](#what-are-chocolatey-packages). Packages can contain the software they represent and the final location of software may or may not be in the package.
* **Software** - Software refers to the actual runtime software that a package represents. This can be installed to the system through native installers, or come from zip/archive files or just dropping the runtime software right into the package.
* **Native Installer** - Actual installers that install software into Programs and Features, "natively" on a Windows machine. This is like MSI, InnoSetup (exe), NSIS (exe), InstallShield (exe/msi), etc. Windows has over 20 different installer formats.
* **Install Package** - packages that wrap native installers
* **Portable Package** - packages that use zip or just contain the runtime software. Usually these packages do not require administrative privileges to install or run. See [[|https://github.com/chocolatey/choco/wiki/ChocolateyFAQs#portable-application-something-that-doesnt-require-a-system-install-to-use]]
* **Extension Package** - packages that provide extensions to Chocolatey's PowerShell module through additional PowerShell modules.
* **Template Package** - packages that have packaging templates in them, used in package creation. See [[create your own package templates|How-To-Create-Custom-Package-Templates]].
* **Metapackage** - packages that only exist to take dependencies on other packages, usually as a way of providing one command to get a complete setup. Some metapackages exist to provide discoverability, such as "git" versus 'git.install." The git package just depends on git.install, so running the install for either package will result in the git software being installed on the machine.
* **Virtual package** - a concept that a package can "provide" some functionality and any package that meets that provides will be considered a dependency met. For example, if you need to take a dependency on a pdf reader, you wouldn't want to take a hard dependency on AdobeReader, but instead you would hope that adobereader provides pdf as well as other packages like SumatraPDF and FoxitReader. Then you could take a dependency on pdf and if any of those packages are installed, the dependency is met. Otherwise an algorithm would determine which one to install. THIS IS NOT IMPLEMENTED AT THIS TIME WITH CHOCOLATEY.

## What Are Chocolatey Packages?

Chocolatey packages are known as nupkg files, which is a compiled NuSpec or a fancy zip file that knows about package metadata (including dependencies and versioning). These packages are an enhanced NuGet package, they have additional metadata that is specific to Chocolatey. Chocolatey is also compatible with vanilla NuGet packages. A Chocolatey package can contain embedded software and/or automation scripts.

Chocolatey packages are not just something fancy on top of MSI/Exe installers. Chocolatey definitely supports that avenue and, with the addition of unzipping archives, it is the most widely used aspect of Chocolatey, especially when you see the packages on the community feed (https://chocolatey.org/packages aka dot org).  Chocolatey is about managing packages, and it works best when those packages contain all of the software instead of reaching out to external/internet resources for the software those packages represent. When you look at the community feed, you are seeing one representation of the way you can build packages, mostly driven by [[distribution rights|Legal#distributions]] that govern when packages can redistribute software or not. Those distribution rules do not govern private/internal packages, so the rules are a bit different. Packages internal to organizations are wide open to do quite a bit more. You can do software embedded packages where executables are automatically added to the path (shimmed) and/or PowerShell automation scripts to do pretty much anything, including running native installers that may be embedded or downloaded as part of the automation script (again, one of the most widely seen aspects on dot org).

Packages with everything embedded are much more deterministic and repeatable, things most businesses require. You just won't see that as often on the community feed due to the aforementioned [[distribution rights|Legal#distributions]].

The closer the underlying software a package represents is to the package (as in executables and files included in the package), the more Chocolatey behaves like a package manager.

## How does Chocolatey work?
How the heck does this all work?

### Installation

1. Chocolatey uses Nuget.Core to retrieve the package from the source.
2. Choco determines if it self-contained or has automation scripts - PowerShell scripts (*.ps1 files), and soon to be open to Scriptcs files in the `0.9.10.x` timeframe (I know, right?!).
3. Choco takes a registry snapshot for later comparison.
4. If there are automation scripts, choco will run those. They can contain whatever you need to do, if they are PowerShell you have the full power of Posh (PowerShell), but you should try to ensure they are compatible with Posh v2+.
5. Choco compares the snapshot and determines uninstaller information and saves that to a .registry file.
6. Choco snapshots the folder based on all files that are currently in the package directory.
7. Choco looks for executable files in the package folder and generates shims into the `$env:ChocolateyInstall\bin` folder so those items are available on the path. Those could have been embedded into the package or brought down from somewhere (internet, ftp, file folder share, etc) and placed there.

### Upgrade

1. Starting in 0.9.10, Chocolatey will look for and run a chocolateyBeforeModify.ps1 file in the existing package prior to upgrading or uninstalling a package. This is your opportunity to shut down services and/or processes. This is run from the existing package, not the new version of the package. If it fails, it just passes a warning and continues on.
2. Similar to install, except choco will make a backup of the package folder (and only the package folder) prior to attempting upgrade.
3. The files snapshot is used to determine what files can be removed from the package folder. If those files have not changed, they will be removed.
4. If the upgrade fails, choco will ask if you want to rollback the package folder to the previous version. If you choose to move roll back, it will put the backed up package directory back in place. This does not fix any folders you may have been using outside of the package directory, such as where the native installer may have installed a program to nor the location of `Get-ToolsLocation`/`Get-BinRoot` (e.g. `c:\tools`). You will need to handle those fixes on your own. Chocolatey also doesn't rerun any install scripts on rollback.

### Uninstall

1. Choco makes the determination that the package is actually installed.
2. Starting in 0.9.10, Chocolatey will look for and run a chocolateyBeforeModify.ps1 file in the existing package prior to upgrading or uninstalling a package. This is your opportunity to shut down services and/or processes. If it fails, it just passes a warning and continues on.
3. Choco will make a backup of the package folder.
4. The automation script is run if found. This should be used to clean up anything that is put there with the install script.
5. If auto uninstaller is turned on, choco will attempt to run the auto uninstaller if a silent uninstall can be determined. Otherwise it will prompt the user (unless -y) to ask if they want the uninstaller to continue. The auto uninstaller can automatically detect about 80% of the different native uninstallers and determine or use the silent uninstall arguments.
6. If everything is successful so far, the files snapshot is used to determine what files can be removed from the package folder. If those files have not changed, they will be removed.
7. If everything is deleted from the package folder, the folder is also removed.


When a package has an exe file, Chocolatey will create a link "shortcut" to the file (called a shim) so that you can run that tool anywhere on the machine. See [[shimming|FeaturesShim]] for more information
When a package has a chocolateyInstall.ps1, it will run the script. The instructions in the file can be anything. This is limited only by the .NET framework and PowerShell.
Most of the Chocolatey packages that take advantage of the PowerShell download an application installer and execute it silently.


## Where are Chocolatey packages installed to?

Chocolatey packages are installed to `ChocolateyInstall\lib`, but the software could go to various locations, depending on how the package maintainer created the package.

Some packages are installed under `ChocolateyInstall\lib`, others - especially packages that are based on Windows installers (.msi files) - install to the default path of the original installer (which is most likely within `Program Files`).

There are also packages for which you can set a custom installation path. These packages (like ruby) use the `$env:ChocolateyBinRoot` environment variable. If this variable does not exist, it will be created as `c:\tools` e.g. `C:\tools\ruby193`. To change this behaviour, you can set `$env:ChocolateyBinRoot` to an existing folder, e. g. `C:\somestuff`. Packages that use the environment variable, will then be installed in the given subfolder, f. ex. `C:\somestuff\ruby193`.

## How does Chocolatey work with Programs and Features? Existing installs?

Many packages use native software installers, so Chocolatey allows the installer itself to handle install/upgrade/uninstall scenarios. This means it can work directly with already installed software just by using `choco install` to make Chocolatey aware of existing software. You can also use a specially crafted install command (skip powershell) to allow choco to install a package without installing the already installed native software.

## Where does Chocolatey install packages from?
By default it installs packages from chocolatey.org (the community feed). But you can change this by adding default sources and/or using the  `--source` switch when running a command.

When you [[host internal packages|How-To-Host-Feed]], those packages can embed software and/or point to internal shares. You are not subject to software distribution rights like the packages on the community feed, so you can [[create packages|CreatePackages]] that are more reliable and secure.  See [[What are Chocolatey Packages|GettingStarted#what-are-chocolatey-packages]] for more details.
