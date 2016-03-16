## Chocolatey - kind of like apt-get, but for Windows
[![](http://img.shields.io/chocolatey/dt/chocolatey.svg)](https://chocolatey.org/packages/chocolatey) [![](http://img.shields.io/chocolatey/v/chocolatey.svg)](https://chocolatey.org/packages/chocolatey) [![](http://img.shields.io/gittip/Chocolatey.svg)](https://www.gittip.com/Chocolatey/) [![Project Stats](https://www.openhub.net/p/chocolatey/widgets/project_thin_badge.gif)](https://www.openhub.net/p/chocolatey)

##### Notes:
 * Edit me at https://github.com/chocolatey/choco-wiki
 * ~~If you are on Chocolatey 0.9.8.x and below, you want to go to https://github.com/chocolatey/chocolatey/wiki~~ (this is no longer available)

## Build Status

TeamCity  | AppVeyor | Travis
------------- | ------------- | -------------
[![TeamCity Build Status](http://img.shields.io/teamcity/codebetter/bt429.svg)](http://teamcity.codebetter.com/viewType.html?buildTypeId=bt429) | [![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/jfxywa3xuwowt20w/branch/master?svg=true)](https://ci.appveyor.com/project/ferventcoder/choco/branch/master) | [![Travis Build Status](https://travis-ci.org/chocolatey/choco.svg?branch=master)](https://travis-ci.org/chocolatey/choco)

### You can just call me Chocolatey.

![Chocolatey Logo](https://github.com/chocolatey/choco/wiki/images/chocolateyicon.gif "Chocolatey")
### Let's get Chocolatey!
"I'm a tools enabler, I'm a global silent application installer. I configure stuff. Some people want to call me apt-get for Windows, I just want to get #chocolatey!"

Home: https://chocolatey.org (Backup: http://chocolatey.apphb.com)
Forum: http://groups.google.com/group/chocolatey
Twitter: https://twitter.com/chocolateynuget

## Chocolatey?
Chocolatey is a global PowerShell execution engine using the NuGet packaging infrastructure. Think of it as the ultimate automation tool for Windows.

Chocolatey is designed to work with all aspects of Windows software (wrapping installers, zips, scripts and executables) with a single unified interface that allows managing all aspects with packaging and versioning.

Chocolatey is not just something fancy on top of MSI/Exe installers. It definitely supports that avenue and, with the addition of unzipping archives, it is the most widely used aspect of Chocolatey, especially when you see the packages on the community feed (https://chocolatey.org/packages aka dot org).  Chocolatey is about managing packages, and it works best when those packages contain all of the software instead of reaching out to external/internet resources for the software those packages represent. When you look at the community feed, you are seeing one representation of the way you can build packages, mostly driven by distribution rules that govern when packages can redistribute software or not. Those distribution rules do not govern private/internal packages, so the rules are a bit different. Packages internal to organizations are wide open to do quite a bit more. You can do software embedded packages where executables are automatically added to the path (shimmed) and/or PowerShell automation scripts to do pretty much anything, including running native installers that may be embedded or downloaded as part of the automation script (again, one of the most widely seen aspects on dot org).

Chocolatey is like apt-get, but built with Windows in mind (there are differences and limitations). For those unfamiliar with APT/Debian, think about Chocolatey as a global silent installer for applications and tools. It can also do configuration tasks and anything that you can do with PowerShell. The power you hold with a tool like Chocolatey is only limited by your imagination!

What's the difference between NuGet and Chocolatey? Developers can use NuGet to get 3rd party libraries that they use to build the .NET tools and applications that they release with Chocolatey! But Chocolatey is not just for .NET tools. It's for nearly any Windows application/tool!

Want to [[learn more?|ChocolateyFAQs]]

## Information

* [[Frequently Asked Questions|ChocolateyFAQs]]
* [[Why Chocolatey|Why]]
* [[License Acceptance Terms|Legal#package-license-acceptance-terms]]
* [[Waiver of Responsibility|Legal#waiver-of-responsibility]]
* [[Release Notes|ReleaseNotes]]

## Using Chocolatey

* [[Installing Chocolatey|Installation]]
* [[Uninstalling Chocolatey|Uninstallation]]
* [[Getting Started|GettingStarted]]
* [[Command Reference|CommandsReference]]
* Use Chocolatey to set up [[source code development environments|DevelopmentEnvironmentSetup]]!
* What distinction does Chocolatey make between an [[installable and a portable application|ChocolateyFAQs#what-distinction-does-chocolatey-make-between-an-installable-and-a-portable-application]]?

## Packages
* [[Creating packages|CreatePackages]]
* Keep in Mind [[Distribution Rights|Legal#distributions-aka-chocolatey-packages]]
* [[Package Function and Variable Reference|HelpersReference]]
* [[Outdated packages? Triage process|PackageTriageProcess]]

## How-To's
* [[Parse PackageParameters Argument|How-To-Parse-PackageParameters-Argument]]
* [[Mount an Iso in Chocolatey Package|How-To-Mount-An-Iso-In-Chocolatey-Package]]
* [[Deprecate a Chocolatey Package|How-To-Deprecate-A-Chocolatey-Package]]
* [[Host Your Own Feed|How-To-Host-Feed]]

## RoadMap
Where are [[we|RoadMap]]? Where are we [[going|RoadMap]]?

## Google Group
Chocolatey has a [Google Groups group](http://groups.google.com/group/chocolatey). Please join and ask questions and send feedback.

If you have issues with a package (or it is out of date), please use the contact maintainers link on the package page itself. If the maintainer doesn’t respond within a given time frame (say a day or two), please refer to the [[Package Triage Process|PackageTriageProcess]].

## Chat Room

Come join in the conversation about Chocolatey in our Gitter Chat Room

[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/chocolatey/choco?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Or, you can find us in IRC on Freenode at #chocolatey.

## The Chocolatey Team
You can find all of the members of the Chocolatey Team [here](https://github.com/orgs/chocolatey/people).

##Requirements
 * Windows 7+/Windows 2003+
 * .NET Framework 4.0
 * PowerShell 2.0
