##Chocolatey - kind of like apt-get, but for Windows 
[![](http://img.shields.io/chocolatey/dt/chocolatey.svg)](https://chocolatey.org/packages/chocolatey) [![](http://img.shields.io/chocolatey/v/chocolatey.svg)](https://chocolatey.org/packages/chocolatey) [![](http://img.shields.io/gittip/Chocolatey.svg)](https://www.gittip.com/Chocolatey/) 

## Build Status

TeamCity  | AppVeyor | Travis
------------- | ------------- | -------------
[![TeamCity Build Status](http://img.shields.io/teamcity/codebetter/bt429.svg)](http://teamcity.codebetter.com/viewType.html?buildTypeId=bt429)  | [![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/jfxywa3xuwowt20w/branch/master?svg=true)](https://ci.appveyor.com/project/ferventcoder/choco/branch/master) | [![Travis Build Status](https://travis-ci.org/chocolatey/choco.svg?branch=master)](https://travis-ci.org/chocolatey/choco)

###You can just call me Chocolatey.  
![Chocolatey Logo](https://github.com/chocolatey/chocolatey/wiki/images/chocolateyicon.gif "Chocolatey")  
###Let's get Chocolatey!
"I'm a tools enabler, I'm a global silent application installer. I configure stuff. Some people want to call me apt-get for Windows, I just want to get #chocolatey!"  
  
Home: https://chocolatey.org (if this is down, try the backup at http://chocolatey.apphb.com )  
Forum: http://groups.google.com/group/chocolatey  
Twitter: https://twitter.com/chocolateynuget  
  
##Chocolatey?
Chocolatey is a global PowerShell execution engine using the NuGet packaging infrastructure. Think of it as the ultimate automation tool for Windows.  
  
Chocolatey is like apt-get, but built with Windows in mind (there are differences and limitations). For those unfamiliar with APT/Debian, think about Chocolatey as a global silent installer for applications and tools. It can also do configuration tasks and anything that you can do with PowerShell. The power you hold with a tool like Chocolatey is only limited by your imagination!  
  
You can develop your tools and applications with NuGet, and release them with Chocolatey! But Chocolatey is not just for .NET tools. It's for nearly any Windows application/tool!  

Want to [[learn more?|ChocolateyFAQs]]  

##Information

[[Frequently Asked Questions|ChocolateyFAQs]]  
[[Why Chocolatey|Why]]  
[[License Acceptance Terms|Legal]]  
[[Waiver of Responsibility|Legal]]  
[[Release Notes|ReleaseNotes]]

##Using Chocolatey
[[Installing Chocolatey|Installation]]  
[[Uninstalling Chocolatey|Uninstallation]]  
[[Getting Started|GettingStarted]]  
[[Command Reference|CommandsReference]]  
Use Chocolatey to set up [[source code development environments|DevelopmentEnvironmentSetup]]!  
What distinction does Chocolatey make between an [[installable and a portable application|https://github.com/chocolatey/chocolatey/wiki/ChocolateyFAQs#what-distinction-does-chocolatey-make-between-an-installable-and-a-portable-application]]?   
  
##Packages
[[Creating packages|CreatePackages]]  
Keep in Mind [Distribution Rights](https://github.com/chocolatey/chocolatey/wiki/Legal#wiki-distributions-aka-chocolatey-packages)  
[[Helper Reference|HelpersReference]]  
[[Outdated packages? Triage process|PackageTriageProcess]]

##How-To's
* [[How-To - Parse PackageParameters Argument|How-To-Parse-PackageParameters-Argument]]
* [[How-To - Mount an Iso in Chocolatey Package|How-To-Mount-An-Iso-In-Chocolatey-Package]]
* [[How-To - Deprecate a Chocolatey Package|How-To-Deprecate-A-Chocolatey-Package]]

##RoadMap
Where are [[we|RoadMap]]? Where are we [[going|RoadMap]]?
  
##Google Group
Chocolatey has a [Google Groups group](http://groups.google.com/group/chocolatey). Please join and ask questions and send feedback.
  
If you have issues with a package (or it is out of date), please use the contact maintainers link on the package page itself. If the maintainer doesn’t respond within a given time frame (say a day or two), feel free to bring it up with the group.  
  
##Requirements
 * Windows XP/Vista/7/8/2003/2008  
 * .NET Framework 4.0  
 * PowerShell 2.0
