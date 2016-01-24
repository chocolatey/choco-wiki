# Package Questions/Updates/Fixes For Community Feed - Triage Process

The Community Feed (https://chocolatey.org/packages) has a collection of packages provided and maintained by the community. You may have come across a package and found you have questions about it or you have found it outdated or broken.

**NOTE**: If you are a **software vendor** wanting to maintain your own software's packages, please use the contact site admins link from the package page on the website.

## Table of Contents

* [Questions about the package or software](#questions-about-a-package-or-software)
* [The package is broken](#package-is-broken)
* [The package is outdated](#package-is-outdated)
* [The Package Triage Process](#the-triage-process) - requesting updates/fixes to packages

### Questions About a Package or Software

Use the Contact Maintainers link on the package page of the community feed or the Disqus comment thread (at the bottom of the package page) to reach out to the maintainers. Neither require an account on Chocolatey.org. 

![contact maintainers link](https://cloud.githubusercontent.com/assets/63502/12519569/406aa70a-c105-11e5-9db2-6191ce5bc100.png)

### Package Missing?
If you are looking for packages to be added to the community feed (aka https://chocolatey.org/packages) - you are part of the community and we welcome your packages. It's super simple to create and maintain packages, see [[Creating packages|CreatePackages]].

### Package is Broken

![red ball failing testing verification](https://cloud.githubusercontent.com/assets/63502/12519534/1728a086-c105-11e5-9af7-96b188114ae7.png)

If the ball is red, the maintainer has already been informed. See the next section on how you can ask for updates

### Package is Outdated

Be sure of four things:

1. The newer version is a stable release - not an alpha or 
1. The newer version that is cross platform is available for Windows. Sometimes the release of the Windows version may lag behind availability on other platforms.
1. It has been at least a day since the new release. Sometimes the package automatically updates - check to see if the package is an automatically updating package in the description of the package. Typically if it has been more than 48 hours since the updated version is available, it's a good time to let the maintainers know they should update the package.
1. The package doesn't already have a newer version in moderation. The maintainer(s) may already be actively working towards getting a newer version up on the community feed.
![versions awaiting moderation](https://cloud.githubusercontent.com/assets/63502/12520149/7bb85aac-c108-11e5-983c-eef01448b00a.png)

## The Triage Process
So a package needs fixed or updated. You have reached out to the maintainer or disqus already and are curious what the next steps are. We will elaborate on all of the steps here, including contacting the maintainers.

1. The first step is to contact the maintainers. 
1. If you are able to provide a fix, that's even better. Look for the maintainer's package source. Please look for package source url information on the package page.  
![Package source link](https://cloud.githubusercontent.com/assets/63502/12520704/c124c60e-c10b-11e5-9de9-1127ce0c602e.png)
1. If you are still looking for a way to provide a fix, but don't find a package source link on the package itself, you can try to find it in one of the maintainer's profiles (this is an older method of finding your way to a maintainer's packages source).
![maintainers are links](https://cloud.githubusercontent.com/assets/63502/12520819/537dbaa6-c10c-11e5-8907-4b822d6f6161.png)
![maintainer profile with github repository link](https://cloud.githubusercontent.com/assets/63502/12520758/f755bf4e-c10b-11e5-831e-4da3a91c42c9.png)
1. If the maintainer(s) do[es] not respond within a few days, it's possible they haven't received your message or don't watch the repository. It's also possible they are on holiday. Use the contact site admins link to contact the site curators, who will attempt to triage the situation.   
![Contact admins link](https://cloud.githubusercontent.com/assets/63502/12520681/a8949c9a-c10b-11e5-84f5-ce4063249cab.png)
1. If you don't hear anything back from the curators within 2 days (usually same day response), then you send a message to the Chocolatey google group (http://groups.google.com/group/chocolatey) and we work to contact the maintainers.
1. If the maintainer(s) are unresponsive after a period of time (determined by the admins), the package is considered abandoned. The curators will TOS the package and new folks can be added as maintainers. The moderators will ask if you would like to become a maintainer of the package. If you choose to become a new maintainer, see the next section.

**NOTE**: We don't have non-maintainer uploads in the community feed and we haven't needed them yet.

### New Maintainers

Sometimes this results in new maintainers. You can be a maintainer updating the project at a central location that multiple maintainers share. However, if you became maintainer because the package was deemed abandoned, you will want to follow the instructions for a [Package Maintainer Handover/Switch](PackageMantainerHandover).
