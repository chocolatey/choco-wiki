# Getting Started

<!-- TOC -->

- [What is Chocolatey?](#what-is-chocolatey)
  - [Already familiar with other package managers?](#already-familiar-with-other-package-managers)
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

## What is Chocolatey?
What is Chocolatey?

Chocolatey is a software management solution unlike any you've ever experienced on Windows. Think of it like this - you create a software deployment package using a little PowerShell, then you can deploy it anywhere you have Windows with everything (like Puppet, SCCM, Altiris, or Connectwise Automate).

Write your deployment once for any software, then deploy it with any solution everywhere you have Windows.

You know those massively complicated, complex, and expensive software management solutions where you typically need to buy more machines and hire consultants to help you configure/maintain? Yeah, that's not us. We believe in simple solutions to complex problems. Software on Windows is already complex enough, we've designed our tools to be able to be simple to use, extremely powerful, flexible to fit nearly any situation, and for scale. And the best part is you can take advantage of our infrastructure and tools without any cost!


* **Deploy Anywhere You Have Windows**/**Cloud Ready** (except Nano, sorry little buddy!). Yes, that includes Server.Core and [Windows Docker Containers](https://github.com/Microsoft/vsts-agent-docker/blob/f870fbf259a803c6a6d902e1c01f631936069d66/windows/servercore/10.0.14393/standard/VS2017/Dockerfile). Windows 7+/Windows 2003+ (although, we have been scaling down on 2003. I mean, 2003 is a bit long in the tooth, but some customers are still using it). Requires PowerShell v2+ (not PowerShell 6 yet - you're doing amazing if you are already on this, but give us some time) and Microsoft .NET Framework 4.x. You can deploy on prem, to Azure, AWS, or any cloud provider you might be looking at.
* **Deploy with Everything.** Anything that can manage endpoints or do remote deployments can either direct Chocolatey through commands, batches, or scripts. Full configuration management solutions like Ansible, Chef, PowerShell DSC, Puppet or Salt typically have providers/modules that allow you to work within their languages to manage both Chocolatey installation/configuration and software.
* **All Software Is a First Class Citizen.** You know how for most things, they only manage/report on the things installed in Add/Remove Programs (Programs and Features)? We count it all, because Windows software is more than just installers, and they all have security findings. So deploy your installers, your scripts, zips, runtime binaries, and yes, internal software all with one simple solution. Then lean on the reporting and inventory to be aware of all aspects of software you are managing.
* **Packages are Independent and Portable.** When you deploy through multiple systems or want to migrate from one to another, you can take the work you've done with Chocolatey with you. How is that for some major time-savings?
* **Test Your Deployments.** When's the last time you tested that deployment script you wrote for SCCM? Write it as a Chocolatey package instead and then you can test your infrastructure to be more comfortable when you push that change out to your constituents.
* **Completely Offline and Secure.** Chocolatey has zero call home, requires no network access to use (although we recommend for ease of management, you at least have internal network access!). Chocolatey does come with a default source that is the community repository. Turn that off and set up your own sources (FOR FREE). See our [guide on organizatonal use|How-To-Setup-Offline-Installation]] to get started (it even has scripts!).
* **Create Your Own Deployment Packages** (FOR FREE) and, get this, **use them internally** (FOR FREE).
* **PowerShell Automation.** You know those little scripts you've been writing for software deployments for years? Welcome to a breath of fresh air, you are going to love Chocolatey!
* **Manage Dependencies With Ease.** You ever have a very complex, specific installation order? It becomes suddenly very easy when you are using a package manager like Chocolatey. Then you can concentrate on harder problems, like what flavor to add to your coffee today.
* **Open Source Licensing Is Ready for Business.** We have Apache v2 licensing, no special restrictions need apply.
* **Commercial Options Add Support + AMAZING Features** - Our top of the line [Chocolatey for Business (C4B) edition](https://chocolatey.org/compare#compare) includes over 20+ features that take Chocolatey to the next level and make it a full software management solution and not just a package manager. Go ahead, right click on an installer and create a fully unattended software deployment in 5 seconds! If you have any issues, our support team is ready and waiting to help you move forward.
* **Customers Help Define Our Work.** We determine and prioritize our work based on where customers want things to go. We are building a platform, and we are working with you to make it better.
* **Security Is Our Focus.** Take a stroll through our security page. With everything we design, we focus on scalability, simplicity, and security.
* **Bridge the Gap.** Have you started to look at moving from those legacy automation systems to something more modern like Ansible, Puppet, or Chef and didn't know where to start? Start shifting the software management over to Chocolatey and taking those existing infrastructure/end point management tools back to just the remote deployment. Then when you are ready, you shift over to those modern automation solutions without needing to rewrite everything! Those Chocolatey packages work in both places!

**"But some of the software I manage is extremely complex and difficult to install."** You mean like [Office](https://chocolatey.org/packages/Office365ProPlus), [SQL Server](https://github.com/DarwinJS/ChocolateyDesignPatterns/tree/af7896893de95bdd5c3c430a8cf7a1b6aa8f0983/mssqlserver-standard), or Oracle? How about MatLab? AutoCAD? All can easily be managed with Chocolatey. Stop torturing yourself, give Chocolatey a shot. Trust us, you'll never go back to those earlier methods. Come into modern software management and automation. It's available, and it's for Windows!!

**"I deal with software installers that don't install silently."** We have solutions for those badly behaved installers as well, we even have a team of folks that whip those badly behaved installers into shape for a low cost (provided you are using a commerical edition of Chocolatey).

All were saying is, it's time to step out of the dark ages and stop either doing things manually or stop killing yourself trying to work directly with those complex systems. Rub a little Chocolatey on them, stop powering through extra work at night, and go home to see your family in the evening because you look at modern software automation and a tool that uses a lot of common sense. Have confidence in your deployments and get the right information back to fix quickly when things go wrong. Chocolatey has been around for nearly ten years, thousands of companies and hundreds of thousands of people use it. Lean on a solution that works. It will change your life, it may get you a raise. You will get more done in the same amount of time, setting you apart from your peers.

### Already familiar with other package managers?

Chocolatey is like Dpkg/RPM, but built with Windows in mind (there are differences and limitations). It can also do configuration tasks and anything that you can do with PowerShell.

Chocolatey manages packages (strictly nupkg files) and those packages manage software (could be installers, could be runtime binaries, could be zips or scripts).

> Chocolatey is a software management tool that is also a package manager. It functions fantastically well when the runtime software is all included in the package and it doesn't make use of native installers. However to approach the Windows ecosystem a package manager also needs to know how to manage actual software installations, thus why Chocolatey does that as well. For publicly available packages, copyright keeps from having binaries embedded in packages, so Chocolatey is able to download from distribution points and checksum those binaries.

## Using Chocolatey
Now that you have Chocolatey on your machine ([[need to install?|Installation]]), you can run several commands.

Take a look at the [[command reference|CommandsReference]]. We are going to be using the [[install command|CommandsInstall]].

Let's install [Notepad++](http://notepad-plus-plus.org/).

1. Open a command line.
1. Type `choco install notepadplusplus` and press Enter.
1. If you have UAC turned on or are not an administrator, Chocolatey is going to request administrative permission at some point (at least once during the process). Otherwise it will not be able to finish what it is doing successfully. If you don't have UAC turned on, it will just continue on without stopping to bother you.
1. That's it. Pretty simple but powerful little concept!

<a name="overriding-default-install-directory"></a>
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

1. Chocolatey uses NuGet (NuGet.Core.dll) to retrieve the package from the source. This is typically a nupkg that is stored in a folder, share, or an OData location (HTTP/HTTPS). For more information on sources, please see [[Sources|CommandsSources]] and [[Source Repositories|How-To-Host-Feed]].
2. The package is installed into `$env:ChocolateyInstall\lib\<pkgId>`. The package install location is not configurable - the package must install here for tracking, upgrade, and uninstall purposes. The software that may be installed later during this process ***is*** configurable. See [Terminology](#terminology) to understand the difference between "package" and "software" as the terms relate to Chocolatey.
3. Choco determines if it is self-contained or has automation scripts - PowerShell scripts (*.ps1 files) and possibly other formats at a later date.
4. Choco takes a registry snapshot for later comparison.
5. If there are automation scripts, choco will run those. They can contain whatever you need to do, if they are PowerShell you have the full power of Posh (PowerShell), but you should try to ensure they are compatible with Posh v2+ (PowerShell v2 and beyond).
6. Choco compares the snapshot and determines uninstaller information and saves that to a .registry file.
7. Choco snapshots the folder based on all files that are currently in the package directory.
8. Choco looks for executable files in the package folder and generates shims into the `$env:ChocolateyInstall\bin` folder so those items are available on the path. Those could have been embedded into the package or brought down from somewhere (internet, ftp, file folder share, etc) and placed there. If there is a shim ignore file `<exeName>.exe.ignore`, then Chocolatey will not generate a shim in the bin folder.

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
