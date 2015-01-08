# Frequently Asked Questions

###What is chocolatey?  
Chocolatey is kind of like apt-get, but for Windows (with windows comes limitations). It is a machine level package manager that is built on top of nuget command line and the nuget infrastructure.  
[[More behind the name|History]]

"Okay, machine package manager, that's nice. What does that mean though?" It means you can simply install software with a few keystrokes and go get coffee while your co-workers are downloading and running an install manually (and I do mean something like an MSI).  
  
How about updates? Wouldn't it be nice to update nearly everything on your machine with a few simple keystrokes? We think so, too.  Chocolatey does that.  
  
###Why chocolatey?
First a [[story|ChocolateyStory]]  
[[Why chocolatey|Why]]  
  
###How is chocolatey different than Ninite?
Great question, see [[Chocolatey vs Ninite|ChocolateyVsNinite]].  
  
###How is chocolatey different than NuGet and/or OpenWrap?
Chocolatey is a machine package manager. Where NuGet/OW are focused on developer library packages, chocolatey is focused on applications and tools, and not necessarily developer focused.
  
###How is/will chocolatey be different than apt?  
  
 * Chocolatey does not support the idea of source packages, which are packages that must be built to be used. For someone interested in that, check out https://github.com/coapp.  
 * Library packages are not completely off the plate, but mostly. How would you link the library up to the application/tool?  
  
###Is there a video I can watch to show me chocolatey in action?
There is! This is a long video due to slow internet connections, but watch the first 1:30ish minutes and the last 1:30ish minutes and that will give you a general idea. [http://www.youtube.com/watch?v=N-hWOUL8roU](http://www.youtube.com/watch?v=N-hWOUL8roU)  
NOTE: This video shows dependency chaining, so you are seeing it install 11 applications/tools.  
  
###What is the feed URL?
https://chocolatey.org/api/v2/  
  
###What can I install?
Check out http://chocolatey.org/packages 
and any package on another feed (nuget.org, rubygems.org, web pi tools, etc).  
  
###What if I install X and I already have X installed?
With most packages when you already have something installed, the chocolatey process will just perform the install again silently. Most times this means that it does nothing and in the end you have what you already had.
  
###Can I override the installation directory? 
Yes you can, see https://github.com/chocolatey/chocolatey/wiki/GettingStarted#overriding-default-install-directory-or-other-advanced-install-concepts

###What does chocolatey do? Are you redistributing software?
Chocolatey does the same thing that you would do based on the package instructions. This usually means going out and downloading an installer from the official distribution point and then silently installing it on your machine. With most packages this means chocolatey is not redistributing software because they are going to the same distribution point that you yourself would go get the software if you were performing this process manually.
  
###When I install a portable app like autohotkey_l.portable, how is it on my path? Without littering my path?
When you install portable apps that have executables in the package, chocolatey automatically creates a batch file redirect file and puts that in a folder that is on the path. That allows you to run a portable application by asking for it on the command line.  
  
When you take an application with a native installer, say like WinDirStat, it is only on your path if the native installer has put it there or the instructions in the chocolatey package itself has requested for it to be on the path. In this case, this is the “littering” the path concept.
  
###What is with the "chocolatey gods" in the installs?
We like humor. You should try some.  
  
###No executables in the packages? Is this an error?
You see 
  
```
-------------------------
   Executable Links (*.exe)
  -------------------------
There are no executables (that are not ignored) in the package.
-------------------------
=====================================================
Chocolatey has finished installing 'windirstat' - check log for errors.
=====================================================
```

Is this an error? It's not. It will be removed from future versions.

###Where does chocolatey install by default?
As of version 0.9.8.24, binaries, libraries and Chocolatey components install in ```C:\ProgramData\chocolatey``` (environment variable %ProgramData%) by default. This reduces the attack surface on a local installation of Chocolatey and limits who can make changes to the directory.

**NOTE:** Historically, Chocolatey installed to ```C:\Chocolatey``` and currently, performing an update of Chocolatey doesn't change the installation location.  For more information about why Chocolatey used ```C:\Chocolatey``` as the default location, look here - [[Default Install Reasoning|DefaultChocolateyInstallReasoning]]

###What kind of package types does chocolatey support?
* Binary Packages – Installable/portable applications – This is 98 % of the chocolatey packages – most are pointers to the real deal installers/zips.  
* Powershell Command Packages – Packages that have the suffix **.powershell** will install powershell scripts as commands for you to call from anywhere.
* Development Packages – Packages that have the suffix **.dev**. For instance [dropkick.dev](http://nuget.org/list/packages/dropkick.dev).
* Coming soon – Virtual Packages – Packages that are like a category, and you just want one package from that category. [Read more …](https://github.com/ferventcoder/nugetpackages/issues/30)
  
<a name="AppVsTool" />
###What distinction does chocolatey make between an installable and a portable application?
####Installable application
An installable application is something that comes with a native installer and ends up in the add/remove programs (in control panel of the system).
Installable applications end up where the native installer wants them to end up (i.&nbsp;e. Program Files). If you want to override that, please feel free to with the proper commands using InstallArgs (-ia) at the command line and possibly override – Install Command. Yes this does mean you will need to have intimate knowledge of the installer. Having chocolatey itself make the override directory is likely at some point, but it is wwwwaaaaayyyy out on the radar (like after Rob is somehow paid to work on chocolatey full time ;) ).

#### Portable application – something that doesn’t require a system install to use
A portable application is something that doesn’t require a native installer to use. In other words, it is not “installed” on your system (where you can go to uninstall in the control panel).

Portable applications end up in the %ChocolateyInstall%/lib (i.&nbsp;e. C:\Nuget\lib) folder yes, but they get a batch redirect to put them on the path of the machine. This behavior is very much to how chocolatey works and is not configurable (the directory). Where the portable apps end up is still going to be %ChocolateyInstall%/lib no matter where you move the directory, unless a package itself unpacks the portable app elsewhere (as in the case of [git-tfs](http://chocolatey.org/packages/gittfs)). 

###What is the difference between packages named *.install (i.&nbsp;e. git.install), *.portable (i.&nbsp;e. git.portable) and * (i.&nbsp;e. git)?

Hey, good question! You are paying attention! Chocolatey has the concept of virtual packages (coming) and meta packages. Virtual packages are packages that represent other packages when used as a dependency. Metapackages are packages that only exist to provide a grouping of dependencies.

A package with no suffix that is surrounded by packages with suffixes is to provide a virtual package. So in the case of git, git.install, and git.commandline – git is that virtual package (currently it is really just a metapackage until the virtual packages feature is complete). That means that other packages could depend on it and you could have either git.install or git.portable installed and you would meet the dependency of having git installed. That keeps chocolatey from trying to install something that already meets the dependency requirement for a package.

Talking specifically about the *.install package suffix – those are for the packages that have a native installer that they have bundled or they download and run. Note that the suffix *.app has been used previously to mean the same as *.install. But the *.app suffix is now deprecated and should not be used for new packages.

The *.portable packages are the packages that will usually result in an executable on your path somewhere but do not get installed onto the system (Add/Remove Programs). Previously the suffixes *.tool and *.commandline have been used to refer to the same type of packages. Note that now *.tool and *.commandline are deprecated and should not be used for new packages.

Want more information? See http://devlicio.us/blogs/rob_reynolds/archive/2012/02/25/chocolatey-guidance-on-packaging-apps-with-both-an-install-and-executable-zip-option.aspx

###I just took over as the primary maintainer of a package. What do I need to do?
See [[PackageMantainerHandover]]

###I'm seeing chocolatey / *application* / *tool* using 32 bit to run instead of x64. What is going on?
The shims are generated as "Any CPU" programs, which depend on the `Enable64Bit` registry value to be set to `1`, which it is by default. A way to fix it is to issue the following command at the location where the prompt shows below:

    C:\Windows\Microsoft.NET\Framework64\v2.0.50727> Ldr64 set64

[Any CPU 32-bit mode on 64 bit machine](http://stackoverflow.com/a/14857294)