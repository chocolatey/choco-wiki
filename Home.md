# Chocolatey - Software Management for Windows

![Chocolatey Logo](https://cdn.rawgit.com/chocolatey/choco/14a627932c78c8baaba6bef5f749ebfa1957d28d/docs/logo/chocolateyicon.gif "Chocolatey")

<!--remove <div id="RightNav"> remove-->
<!-- TOC -->

- [What is Chocolatey?](#what-is-chocolatey)
- [Requirements](#requirements)
- [Information](#information)
- [Using Chocolatey](#using-chocolatey)
- [Packages](#packages)
- [Walkthroughs / How To Links](#walkthroughs-how-to-links)
- [Road Map](#road-map)
  - [Important Reference Links](#important-reference-links)
- [See It In Action](#see-it-in-action)
- [Who Are We?](#who-are-we)

<!-- /TOC -->
<!--remove </div> remove-->

## What is Chocolatey?
Chocolatey is a software management solution unlike anything else you've ever experienced on Windows. It focuses on simplicity, security, and scalability. You write a software deployment in PowerShell once for any software (not just installers), then you can deploy it everywhere you have Windows with any solution that can manage systems (configuration management, endpoint management, etc) and track and manage updates of that software over time. Manage software on-premise, in the "Cloud", or in [Docker containers](https://github.com/Microsoft/vsts-agent-docker/blob/f870fbf259a803c6a6d902e1c01f631936069d66/windows/servercore/10.0.14393/standard/VS2017/Dockerfile) with Chocolatey.

Whew, that was a mouthful! For a bit more detail into what all of that means and more, see [[What is Chocolatey?|GettingStarted#what-is-chocolatey]]

## Requirements
* Windows 7+/Windows 2003+ (Server Core also, but not Windows Nano Server)
* Windows PowerShell v2+ (not PowerShell Core aka PowerShell 6 yet)
* .NET Framework 4.x+
* See [[Requirements|GettingStarted#requirements]]

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

## Walkthroughs / How To Links
* [[Parse PackageParameters Argument|How-To-Parse-PackageParameters-Argument]]
* [[Mount an Iso in Chocolatey Package|How-To-Mount-An-Iso-In-Chocolatey-Package]]
* [[Deprecate a Chocolatey Package|How-To-Deprecate-A-Chocolatey-Package]]
* [[Host Your Own Feed|How-To-Host-Feed]]
* [[Deploy Chocolatey in an Organization|How-To-Setup-Offline-Installation]]

## Road Map
Where are [[we|RoadMap]]? Where are we [[going|RoadMap]]?

### Important Reference Links

 * [Chocolatey Website and Community Package Repository](https://chocolatey.org)
 * [Mailing List](http://groups.google.com/group/chocolatey) / [Release Announcements Only Mailing List](https://groups.google.com/group/chocolatey-announce) / [Build Status Mailing List](http://groups.google.com/group/chocolatey-build-status)
 * [Twitter](https://twitter.com/chocolateynuget) / [Facebook](https://www.facebook.com/ChocolateySoftware) / [Github](https://github.com/chocolatey)
 * [Blog](https://chocolatey.org/blog) / [Newsletter](https://chocolatey.us8.list-manage1.com/subscribe?u=86a6d80146a0da7f2223712e4&id=73b018498d)
 * [Documentation](https://chocolatey.org/docs) / [Support](https://chocolatey.org/support) / [![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/chocolatey/choco?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

## See It In Action
![Chocolatey Install](https://raw.githubusercontent.com/wiki/chocolatey/choco/images/gifs/choco_install.gif)

## Who Are We?
You can find all of the members of the Chocolatey Team [here](https://github.com/orgs/chocolatey/people).
