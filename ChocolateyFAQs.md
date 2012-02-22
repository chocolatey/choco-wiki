# Frequently Asked Questions

###What is chocolatey?  
Chocolatey is kind of like apt-get, but for Windows (with windows comes limitations). It is a machine level package manager that is built on top of nuget command line and the nuget infrastructure.  
[[More behind the name|History]]

"Okay, machine package manager, that's nice. What does that mean though?" It means you can simply install software with a few keystrokes and go get coffee while your co-workers are downloading and running an install manually (and I do mean something like an MSI).  
  
How about updates? Wouldn't it be nice to update nearly everything on your machine with a few simple keystrokes? We think so, too.  Chocolatey does that.  
  
###Why chocolatey?
First a [[story|ChocolateyStory]]  
[[Why chocolatey|Why]]  
  
###How is chocolatey different than NuGet and/or OpenWrap?
Chocolatey is a machine package manager. Where NuGet/OW are focused on developer library packages, chocolatey is focused on applications and tools, and not necessarily developer focused.
  
###Is there a video I can watch to show me chocolatey in action?
There is! This is a long video due to slow internet connections, but watch the first 1:30ish minutes and the last 1:30ish minutes and that will give you a general idea. [http://www.youtube.com/watch?v=N-hWOUL8roU](http://www.youtube.com/watch?v=N-hWOUL8roU)  
NOTE: This video shows dependency chaining, so you are seeing it install 11 applications/tools.  
  
###What can I install?
Check out http://chocolatey.org/packages 
and any package on another feed (nuget.org, rubygems.org, web pi tools, etc).  
  
###What kind of package types does chocolatey support?
  
* Binary Packages - Tools/Applications - This is 98% of the chocolatey packages - most are pointers to the real deal installers / zips.  
* Powershell Command Packages - Packages that have the suffix **.powershell** will install powershell scripts as commands for you to call from anywhere.
* Development Packages - Packages that have the suffix **.dev**. For instance [dropkick.dev](http://nuget.org/list/packages/dropkick.dev).
* Coming soon - Virtual Packages - Packages that are like a category, and you just want one package from that category. [Read more ...](https://github.com/ferventcoder/nugetpackages/issues/30)
  
###How is/will chocolatey be different than APT?  
  
 * Chocolatey does not support the idea of source packages, which are packages that must be built to be used. For someone interested in that, check out https://github.com/coapp.  
 * Library packages are not completely off the plate, but mostly. How would you link the library up to the application/tool?  
  
###What distinction does chocolatey make between an application and a tool?  
#### App/Application 
An application is something that comes with a native installer and ends up in the add/remove programs (in control panel of the system).  
Applications end up where the native installer wants them to end up (i.e. Program Files).  If you want to override that, please feel free to with the proper commands using InstallArgs (-ia) at the command line and possibly override - [[Install Command|CommandsInstall]]. Yes this does mean you will need to have intimate knowledge of the installer. Having chocolatey itself make the override directory is likely at some point, but it is wwwwaaaaayyyy out on the radar (like after [Rob](https://github.com/ferventcoder) is somehow paid to work on chocolatey full time ;) ).  
  
#### Tool - something that doesn't require a system install to use.
A tool is something that doesn't require a native installer to use. In other words, it is not "installed" on your system (where you can go to uninstall in the control panel).  
  
Tools end up in the %ChocolateyInstall%/lib (i.e. C:\Nuget\lib) folder yes, but they get a batch redirect to put them on the path of the machine. This behavior is very much to how chocolatey works and is not configurable (the directory). Where the tools end up is still going to be %ChocolateyInstall%/lib no matter where you move the directory, unless a package itself unpacks the tool elsewhere (as in the case of [git-tfs](http://chocolatey.org/packages/gittfs)).  
  