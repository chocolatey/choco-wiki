# Frequently Asked Questions

### Can't find your answer here?
Feel free to reach out to us on [Gitter](https://gitter.im/chocolatey/choco) or by the [email distribution list / forum](https://groups.google.com/group/chocolatey).

### What is Chocolatey?
Chocolatey is kind of like apt-get, but for Windows (with Windows comes limitations). It is a machine level package manager that is built on top of nuget command line and the nuget infrastructure.
[[More behind the name|History]]

"Okay, machine package manager, that's nice. What does that mean though?" It means you can simply install software with a few keystrokes and go get coffee while your co-workers are downloading and running an install manually (and I do mean something like an MSI).

How about updates? Wouldn't it be nice to update nearly everything on your machine with a few simple keystrokes? We think so, too. Chocolatey does that. `choco upgrade all -y`

### What is the purpose of Chocolatey?

Great question! See [[The purpose of Chocolatey|Why#what-is-the-purpose-of-chocolatey]]

### How does Chocolatey work?

See [[What is Chocolatey?|Why#what-is-chocolatey]]

### Why Chocolatey?
First a [[story|ChocolateyStory]]. Then [[Why Chocolatey?|Why]]

### What can a Chocolatey Package consist of?
See [[What are Chocolatey Packages?|GettingStarted#what-are-chocolatey-packages]]

### Can I use Chocolatey with existing installs?

Fantastic question, see [[Can I use Chocolatey with existing software?|Why#can-i-use-chocolatey-with-existing-software]]

### I would like to use Chocolatey in my organization, is the licensing friendly?

Yes, it is. Chocolatey carries a FOSS Apache 2.0 license, which is extremely business friendly. You can use Chocolatey and most of it's infrastructure completely free. Chocolatey will have a business offering with features specifically targeted to businesses in the first half of 2016. See the [Kickstarter](https://www.kickstarter.com/projects/ferventcoder/chocolatey-the-alternative-windows-store-like-yum/description) for details.

### Should my organization depend on (use) the community feed (https://chocolatey.org/packages)?

For production-level scenarios, I couldn't justify giving up that level of control and trust to the internet in an organization. It's recommended that you copy and modify existing packages and/or create your own internal packages and host them internally. That way you can completely guarantee that an install/upgrade/uninstall will always work every time. See [[Security|Security#chocolateyorg-the-community-feed]] for more details.

If you are just setting up or updating developer workstations and can tolerate things breaking every once in awhile because internet/uncertainty, it's fine to use the community feed.

### How is Chocolatey different than OneGet/PowerShell Package Management?
OneGet is a package manager ***aggregator***, which means [it is not really a package manager at all](http://blogs.msdn.com/b/garretts/archive/2015/05/05/10-things-about-oneget-that-are-completely-different-than-you-think.aspx). Chocolatey will have a provider that plugs right into OneGet. At the current time there is a CTP available, but it is based on 2 year old Chocolatey technology (we've had security fixes since then, plus a world of features), so we can't really recommend it. But if you must use it, make sure your PowerShell execution policy is set correctly and you are in an administrative console. See http://www.hanselman.com/blog/AptGetForWindowsOneGetAndChocolateyOnWindows10.aspx for more details.

### How is Chocolatey different than Ninite?
Great question, see [[Chocolatey vs Ninite|ChocolateyVsNinite]].

### How is Chocolatey different than NuGet and/or OpenWrap?
Chocolatey is a machine package manager. Where NuGet/OW are focused on developer library packages, Chocolatey is focused on applications and tools, and not necessarily developer focused.

A typical way of stating the difference is "Developers use NuGet to get 3rd party libraries that they use to build the .NET tools and applications that they release with Chocolatey!"

### How is/will Chocolatey be different than apt?

 * Chocolatey does not support the idea of source packages, which are packages that must be built to be used. For someone interested in that, check out https://github.com/coapp.
 * Library packages are not completely off the plate, but mostly. How would you link the library up to the application/tool?

### Is there a video I can watch to show me Chocolatey in action?
There is! This is a long video due to slow internet connections, but watch the first 1:30ish minutes and the last 1:30ish minutes and that will give you a general idea. [http://www.youtube.com/watch?v=N-hWOUL8roU](http://www.youtube.com/watch?v=N-hWOUL8roU)

**NOTE:** This video shows dependency chaining, so you are seeing it install 11 applications/tools.

### What is the default feed URL (community feed url)?
https://chocolatey.org/api/v2/

### What can I install?
Check out http://chocolatey.org/packages
and any package on another feed (nuget.org, rubygems.org, web pi tools, etc).

### What if I install X and I already have X installed?
With most packages when you already have something installed, the Chocolatey process will just perform the install again silently. Most times this means that it does nothing and in the end you have what you already had.

### Can I override the installation directory?
Yes you can, see [[Overriding install directory|GettingStarted#overriding-default-install-directory-or-other-advanced-install-concepts]].

### What is moderation?
Related to the community package repository only (aka the default feed aka https://chocolatey.org/packages), we have a concept called moderation, where submitted packages are held until they are considered safe and of minimal quality for regular consumption.

Moderation involves checking a package version for quality (validation) and correctness, whether it installs and uninstalls correctly (verification). We have two automated services that validate and verify packages. The validator checks the quality of a package. If no requirements are flagged as failing review, it will be passed on to the verifier, which checks that the package actually works as intended (it may help to think of the validator as unit testing and the verifier as integration testing). If both of these automated reviews pass the package version is submitted to a moderator for final review and approval.

Things to note:
* We have trusted packages, and those packages skip human review/moderation.
* A maintainer can not moderate his/her own pkgs.
* You can see if a package has been verified by the green circle next to it's name on the package page. If it is green or red, it will also be a clickable link. To see all packages verified, see https://gist.github.com/choco-bot
* Besides trusted packages, a package version is never approved without a moderator clicking approve.

### How does the moderation review process work?
See [[Review Process|Moderation#package-review-process]].

### What is a trusted package?
Related to the community package repository only (aka the default feed aka https://chocolatey.org/packages).

A package that is considered trusted comes from the original software creator or is maintained by someone in the community who has a track record for high quality and safe packages.

Two ways your packages can become trusted:
* You write the underlying software that the package installs. For instance the ReSharper package that comes directly from JetBrains.
* You put in a lot of good packages and your packages will eventually become trusted.

For a package to switch to trusted, a moderator must manually make the change. It is not an automated process.

**Note:** Once everything is ready, all packages will go under automated verification and validation and be held for fixes if they don't pass, even trusted packages.

**Note:** Another note, we've been setting trust per package. That is planned to change at some point for the most part as the trust level has always been about the maintainer and not always the package itself.

### How do I install a package version under moderation?
Related to the community package repository only (aka the default feed aka https://chocolatey.org/packages).

You can install a version of a package version that's still under moderation - however know that if the maintainer needs to fix the package version during the review process, you will never get those fixes locally since they are updating the ***SAME*** version. Package versions are not immutable until they are approved. The caveat for "never" is that if you know it changed (likely you won't and there is no notification, *what you have installed technically never existed*), you could force the reinstall of that same version of the package.

Another thing to consider: if the package version or the package as a whole is rejected (usually for renaming the package to something better to better meet naming standards), you are likely to get weird warnings later and won't see updates at all. Remember, you have installed something that technically never existed, so any thing you see related to choco not knowing about it is to be expected and not a bug.

To actually install, see the next question.

### How do I install an unlisted package / package version?
You need to specify name AND version to any package to install the unlisted/unapproved version. This goes for any NuGet compatible feed that understands unlisted packages.

### How do I install a rejected package?
Related to the community package repository only (aka the default feed aka https://chocolatey.org/packages), we have a concept of packages that have been rejected. You cannot install a rejected package. It could do bad things to your system so we don't allow install from the community repository.

### How do I self-reject a package?
If you are a maintainer of a package and you would like to self-reject an older version of a package that is failing verification or validation, we support that. If however you just want to reject a working package because it is older, we don't support that. Rejected != Obsolete. It's really about when the underlying software has the same download url for every release so the older versions do not apply. If you are using checksums to verify the download (and you should be), then your older versions should start failing.

![image](https://cloud.githubusercontent.com/assets/63502/12429736/8c67689c-beb1-11e5-83e9-02fe1db91272.png)


* Failing verification -
![image](https://cloud.githubusercontent.com/assets/63502/12429697/509d2b94-beb1-11e5-9e1d-73a156117672.png)
* Failing validation - a message from the validator telling you it failed validation.

### What is the validator?
The [validator](https://github.com/chocolatey/package-validator) is a service that checks the quality of a package based on requirements, guidelines and suggestions for creating packages for Chocolatey’s community feed. Many of the validation items will automatically roll back into choco and will be displayed when packaging a package. We like to think of the validator as unit testing. It is validating that everything is as it should be and meets the minimum requirements for a package on the community feed.

What does the validator check? https://github.com/chocolatey/package-validator/wiki

### What is the verifier?
The [verifier](https://github.com/chocolatey/package-verifier) is a service that checks the correctness (that the package actually works), that it installs and uninstalls correctly, has the right dependencies to ensure it is installed properly and can be installed silently. The verifier runs against both submitted packages and existing packages (currently checking once a month that a package can still install and sending notice when it fails). We like to think of the verifier as integration testing. It’s testing all the parts and ensuring everything is good. On the site, you can see the current status of a package based on a little colored ball next to the title. If the ball is green or red, the ball is a link to the results (only on the package page, not in the list screen).

![Passing verification](https://cloud.githubusercontent.com/assets/63502/11872220/bf58f590-a499-11e5-84bb-6fcf6d320227.png)

* Green means good. The ball is a link to the results
* Orange if still pending verification (has not yet run).
* Red means it failed verification for some reason. The ball is a link to the results.
* Grey means unknown or excluded from verification (if excluded, a reason will be listed on the package page).

### What does Chocolatey do? Are you redistributing software?
Packages on Chocolatey.org are subject to software distribution rights, so in that case the following applies:

Chocolatey does the same thing that you would do based on the package instructions. This usually means going out and downloading an installer from the official distribution point and then silently installing it on your machine. With most packages this means Chocolatey is not redistributing software because they are going to the same distribution point that you yourself would go get the software if you were performing this process manually.

When you host internal packages, those packages can embed software and/or point to internal shares. You are not subject to software distribution rights, thus you can create packages that are more reliable and secure. See [[What are Chocolatey Packages|GettingStarted#what-are-chocolatey-packages]] for more details.

### When I install a portable app like [autohotkey.portable](https://chocolatey.org/packages/autohotkey.portable), how is it on my path? Without littering my path?
When you install portable apps that have executables in the package, Chocolatey automatically creates a "shim" file and puts that in a folder that is on the path. That allows you to run a portable application by asking for it on the command line.

When you take an application with a native installer, say like WinDirStat, it is only on your path if the native installer has put it there or the instructions in the Chocolatey package itself has requested for it to be on the path. In this case, this is the “littering” the path concept.

### Where does Chocolatey install by default?
As of version 0.9.8.24, binaries, libraries and Chocolatey components install in ```C:\ProgramData\chocolatey``` (environment variable %ProgramData%) by default. This reduces the attack surface on a local installation of Chocolatey and limits who can make changes to the directory.

**NOTE:** Historically, Chocolatey installed to ```C:\Chocolatey``` and currently, performing an update of Chocolatey doesn't change the installation location, except for when the install path is `C:\chocolatey`. It will upgrade that path and all variables automatically.  For more information about why Chocolatey used ```C:\Chocolatey``` as the default location, look here - [[Default Install Reasoning|DefaultChocolateyInstallReasoning]]

### What kind of package types does Chocolatey support?
* Binary Packages – Installable/portable applications – This is 98% of the Chocolatey packages – most are pointers to the real deal native installers and/or zipped software.
* PowerShell Command Packages – Packages that have the suffix **.powershell** will install PowerShell scripts as commands for you to call from anywhere.
* Development Packages – Packages that have the suffix **.dev**. For instance [dropkick.dev](http://nuget.org/list/packages/dropkick.dev).
* Coming soon – Virtual Packages – Packages that are like a category, and you just want one package from that category. [Read more …](https://github.com/chocolatey/chocolatey/issues/7)

<a name="AppVsTool" />
### What distinction does Chocolatey make between an installable and a portable application?
#### Installable application
An installable application is something that comes with a native installer and ends up in the add/remove programs (in control panel of the system).
Installable applications end up where the native installer wants them to end up (i.&nbsp;e. Program Files). If you want to override that, please feel free to with the proper commands using InstallArgs (-ia) at the command line and possibly override – Install Command. Yes this does mean you will need to have intimate knowledge of the installer. Having Chocolatey itself make the override directory is likely at some point, but it is wwwwaaaaayyyy out on the radar (like after Rob is somehow paid to work on Chocolatey full time ;) ).

#### Portable application – something that doesn't require a system install to use
A portable application is something that doesn't require a native installer to use. In other words, it is not “installed” on your system (where you can go to uninstall in the control panel). It also requires no administrative access for the package install.

Portable applications end up in the %ChocolateyInstall%/lib (i.&nbsp;e. C:\ProgramData\Chocolatey\lib) folder yes, but they get a "shim" to put them on the path of the machine. This behavior is very much to how Chocolatey works and is not configurable (the directory). Where the portable apps end up is still going to be %ChocolateyInstall%/lib no matter where you move the directory, unless a package itself unpacks the portable app elsewhere (as in the case of [git-tfs](http://chocolatey.org/packages/gittfs)).

### Why doesn't a package install software to Program Files?
Most packages that use native installers (MSI, InnoSetup, etc) will install to Program Files, but there are packages that do not. There are two really important reasons why:

* Program Files is synonymous with software that has uninstall registry keys - or put another way, applications that have native installers that you can find for uninstall in the Control Panel under Programs and Features.
* Writing to Program Files requires administrative permissions and the package you are installing is likely a portable package (even if not explicitly named so, it may have a zip that it extracts). There is also a way for non-administrators to use Chocolatey and these types of packages need to work for them as well.

It really depends on the underlying software the package "installs". If the underlying software is a native installer, then it has a machine install (meaning it gets an uninstall registry key and shows up in Programs and Features) and Program Files is the appropriate place for it.

Chocolatey has a different avenue for portable packages that allows both admins and non-admins to be able to use these packages, after all they are just downloading and unzipping an archive. These packages either go into a Tools Root location or just get shims (executables are put on the path) and continue to stay in the Chocolatey lib directory. If we restricted a non-admin for an avenue that would work for them manually, it wouldn't make choco useful for them at all. Since we support non-admin usage of Chocolatey, packages that can support the portable model should support it.

Also consider that if the package **is** using `$env:ChocolateyBinRoot` (which will later be named `$env:ChocolateyToolsRoot`) you an set the root under Program Files and then you get the better of both worlds.

### What is the difference between packages named *.install (i.&nbsp;e. [autohotkey.install](https://chocolatey.org/packages/autohotkey.install)), *.portable (i.&nbsp;e. [autohotkey.portable](https://chocolatey.org/packages/autohotkey.portable)) and * (i.&nbsp;e. [autohotkey](https://chocolatey.org/packages/autohotkey))?

Hey, good question! You are paying attention! Chocolatey has the concept of virtual packages (coming) and meta packages. Virtual packages are packages that represent other packages when used as a dependency. Metapackages are packages that only exist to provide a grouping of dependencies.

A package with no suffix that is surrounded by packages with suffixes is to provide a virtual package. So in the case of git, git.install, and git.commandline (deprecated for .portable) – git is that virtual package (currently it is really just a metapackage until the virtual packages feature is complete). That means that other packages could depend on it and you could have either git.install or git.portable installed and you would meet the dependency of having git installed. That keeps Chocolatey from trying to install something that already meets the dependency requirement for a package.

Talking specifically about the *.install package suffix – those are for the packages that have a native installer that they have bundled or they download and run.

**NOTE:** the suffix *.app has been used previously to mean the same as *.install. But the *.app suffix is now deprecated and should not be used for new packages.

The *.portable packages are the packages that will usually result in an executable on your path somewhere but do not get installed onto the system (Add/Remove Programs). Previously the suffixes *.tool and *.commandline have been used to refer to the same type of packages.

**NOTE:** now *.tool and *.commandline are deprecated and should not be used for new packages.

Want more information? See http://ferventcoder.com/archive/2012/02/25/chocolatey---guidance-on-packaging-apps-with-both-an-install.aspx

### I just took over as the primary maintainer of a package. What do I need to do?
See [[PackageMantainerHandover]]

### I'm seeing Chocolatey / *application* / *tool* using 32 bit to run instead of x64. What is going on?
The shims are generated as "Any CPU" programs, which depend on the `Enable64Bit` registry value to be set to `1`, which it is by default. A way to fix it is to issue the following command at the location where the prompt shows below:

    C:\Windows\Microsoft.NET\Framework64\v2.0.50727> Ldr64 set64

[Any CPU 32-bit mode on 64 bit machine](http://stackoverflow.com/a/14857294)

### Is there a PowerShell Module for Chocolatey?
Not yet, but when there is it will be provided as a binary DLL.

Chocolatey itself is now a binary with 0.9.9+. This provides the ability to run it on Linux/OSX, which is why it is not by default a PowerShell module (\*nix still doesn't really have PowerShell support). The intention is to have something that has a larger portability - this is why it is not something you can ***only*** use in PowerShell, but rather something you can ***also*** use in PowerShell.

**It was NEVER a PowerShell module, it just used PowerShell as a programming language and was meant to be a CLI app.** I bolded this so it might get read twice. ;)

Chocolatey up until 0.9.9 was provided completely written in PowerShell, with the approach above. I don't know of any other app that has ever tried that approach, which made the original chocolatey client a rarity indeed.

### Why do I have to confirm packages now? Is there a way to remove this?
tl;dr - Yes, completely possible. Use `-y` or turn on `allowGlobalConfirmation`.

Also check out the help menus now - `choco -h`, `choco install -h`

Longer answer, we've moved a little closer towards other package managers for security reasons, where by default we stop and confirm if you are okay with the state change. We always communicate changes in the [release notes][[ReleaseNotes]] / [Changelog](https://github.com/chocolatey/choco/blob/master/CHANGELOG.md), which also end up in the [nuspec file](https://chocolatey.org/packages/chocolatey#releasenotes), so we highly recommend folks scan at least one of those to see anything tagged breaking changes. Always scan from your current version up to the one you are upgrading to so that you catch all changes.

The one that is the most important right now is the `x.y.z` release (in this case 0.9.9), once we reach v1 we will be fully SemVer compliant and breaking changes will constitute a major version bump (we're still SemVer in a less than v1), so you can scan breaking changes and major new features in an `x` release, new compatible features in a `.y` release, and `.z` releases will only contain compatible fixes for the current release.

0.9.9 introduced a new compiled client that was/is a total rewrite. 0.9.10 will have complete feature parity with the older client - see [FeatureParity](https://github.com/chocolatey/choco/labels/FeatureParity). Why the rewrite? For a more maintainable, faster client that can run on mono now, so you are not completely tied to Windows. We've started adding support for other install providers (like [Scriptcs](https://github.com/chocolatey/choco/issues/247)).

The [relevant bits of the release notes](https://github.com/chocolatey/choco/wiki/ReleaseNotes#099-march-3-2015) for the FAQ:

 - [Security] Prompt for confirmation: For security reasons, we now stop for confirmation before changing the state of the system on most commands. You can pass `-y` to confirm any prompts or set a value in the config that will globally confirm and behave like older versions of Chocolatey (`allowGlobalConfirmation`, see `choco feature -h` for how to enable).

Some folks may find the change quite annoying. The perspective of some folks isn't the perspective of everyone and we have some very security-conscious folks that want a verification of what they requested is what they end up with. So this prompt is extremely important for them. We are moving to a more secure by default approach so this change was important to get us there. Security related changes are some of the only things you will see that affect Chocolatey in such a way.

We spent many months stressing over this change because of the breaking part and decided it wasn't going to get easier to change later. We've added the ability for you to set Chocolatey back to the way it was before with `allowGlobalConfirmation` and if the prompts annoy you, you should probably look at making this change.
