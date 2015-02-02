## Chocolatey vs Ninite

A lot of folks out there are always wanting to point out that there is Ninite when someone mentions Chocolatey. That is fine, Ninite works but it only has like 90+ things you can install. They are limited by what Ninite can rebundle and redistribute. Both are solid solutions in their own right, but you need to understand the needs and what the two solutions provide to really make a choice on them.

## Package Management Approach

### Ninite
* Main purpose: Ninite is an installer that keeps off the crapware.
* Keeps everything centralized.
* Has a tight integration with products as the Ninite staff is the only one who updates packages.
* Guarantees success with installs since they control every aspect of the packages.
* Does not take contributions.
* All GUI based unless you pay for Pro version.
* Update apps simply by running the installer again.
* Only use case is for folks who have access to install applications on their machines.

### Chocolatey
* Main purpose: Chocolatey is a global PowerShell execution engine that knows about a packaging format.
* Decentralized with a central "official" community package repository.
* Multiple sources including private sources.
* Packages are created by the community and reviewed by moderators.
* Allows for community contributions.
* Allows for pay for apps to be included as packages.
* CLI focused.
* Easily scriptable which allows for adding setup scripts to things like source control.
* Update apps simply by running `cup packagename` or `cup all`.
* Integration with other package managers (ruby, python, WebPI, cygwin, more coming)
* Able to be used without needing administrative permissions (almost all tool packages are non-admin)

#### Packaging solution needs (that brought about chocolatey in the first place)
* Good CLI that is simple to use
* Main repository that takes packages contributions from the community (and is being maintained)
* Ability to use additional/multiple sources
* Meta packages that can chain dependencies
* Virtual packages
* Packages should be easy to create / maintain
* Packages should be concise and be able to be created without worrying about distribution rights
* Unattended installs
* Installation of multiple packages with one command
* Script setup of environments

## Chocolatey and  Ninite : Compare and Contrast

### Interfaces:
* Ninite - choose apps from a website, download installer just for those apps. Pay for the pro version and use the command line.
* Chocolatey - open a command line. Install app with `cinst appname`. Lather rinse repeat.

### Packages:
* Ninite - closed, only items available are what Ninite staff choose to make available.
* Chocolatey - community packages on a central server

### Package sources:
* Ninite - one at Ninite.com
* Chocolatey - central one at chocolatey.org, create and use public/private sources (folder, network share, odata feed like nuget.org, [chocolatey.org] and/or myget.org)
* Chocolatey (revisited) - cinst bash -source cygwin | cinst gemcutter -source ruby | cinst sphynx -source python | cinst IISExpress -source webpi

### Creating packages:
* Ninite - no
* Chocolatey - yes and stupid simple. Take a look at [github.com] and keep in mind that it is one line call to a PowerShell function to install many apps. Consider windirstat is:
`Install-ChocolateyPackage 'windirstat' 'exe' '/S' 'windirstat.info/wds_current_setup.exe'`

### Available packages:
* Ninite - Handled by Ninite staff, so there's less chance of anything being broken.
* Chocolatey - Handled by the community, reviewed by moderators. Possibility of breakages.

### Package updates:
* Both suffer from keeping packages up to date.

### Package dependencies:
* Ninite - not really
* Chocolatey - Yes! Install Git Extensions, it makes sure Git is also installed.

### Versioning/upgrades:
* Ninite - sort of, you just rerun the installer every once in awhile
* Chocolatey - Yes. Consider 'cup all'.

## Conclusion
Ninite is a solid solution if you don't mind not being able to script it and only install the applications that it has listed on the Ninite page. You are, however, guaranteed that you have everything you need to install sheerly by having the Ninite Installer. In that way it is better than Chocolatey, which most packages require access to the internet to download installers that do not have distribution rights with them (Ninite does not incur this extra point of failure).

Chocolatey has 600% more packages than Ninite and a community that is driving to continually make it better. If you need to get to older versions of packages, many of the packages on Chocolatey.org allow for this. What Chocolatey lacks in the guarantee that Ninite provides, it makes up for in features and options. Chocolatey can provide packages for non-free products, have multiple sources and folks can script the installations. Chocolatey is more than just an installer and with that does not require administrative privileges to use.

Both solutions suffer from the issue of having the most up-to-date packages available, it's just that Chocolatey is more transparent about it.

Whether you use Chocolatey or Ninite, consider that the two answer the same question differently and that is okay. They can live in harmony with each other and at some point Chocolatey may offer Ninite as package source.