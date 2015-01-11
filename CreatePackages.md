#Creating Chocolatey Packages

## Rules to be observed before publishing packages 

There are a few rules that you have to follow before pushing packages to chocolatey.org:
* **Don't package illegal software.** Packages of software that is illegal in most countries in the world are prohibited to publish on Chocolatey.org. This applies in particular to software that violates the copyright, pirated software and activation cracks. Remember that this also affects software that is especially designed to accomplish software piracy.
* **Do not package software** into chocolatey packages **that you don't have the right to distribute.** Please see [Distribution Rights](https://github.com/chocolatey/chocolatey/wiki/Legal#wiki-distributions-aka-chocolatey-packages) for more information. Any package found not in compliance with this will be removed immediately.
* **Do not publish junk or malware** packages.
* **Only post publicly relevant packages.** A package creator should consider whether his package is also useful for others. If that is not the case, it shouldn’t be published on Chocolatey.org. Reasons for that can be if the package would require a very special configuration that is unacceptable for other users or that would lead to serious vulnerabilities.
* **Don't package software containing adware or spyware.** Packages of software that comes with bundled adware, spyware or any other unrelated software that installs even in silent mode are not allowed. But if you can figure out how to install the desired package without any adware or unrelated software, it is allowed to publish the package. That can be reached for example with additional command line switches or by adding specific values to the registry. Examples of packages which make use of this are [PDFCreator](https://github.com/stack72/MyChocolateyPackages/tree/master/PDFCreator) and [CCleaner](https://github.com/tonigellida/chocolateyautomaticpackages/tree/master/ccleaner).
* **Don't package software that is already packaged**. Use the search function in the [Chocolatey.org gallery](http://chocolatey.org/packages) and look if there is already a package for the desired software. If you would like to improve the already existing  package or if you have suggestions, just contact the package maintainer or open a pull request at the maintainer’s package repository.
* **Don't include other required software if there's a package of it.** If a package requires another software of which there is already a package, the already existing package should be used as dependency instead of including all needed software into one package.
* **Split dependencies into multiple packages.** Try to split up packages as much as possible. If for example a program comes with additional modules/installers that are optional, make different packages for them instead of including all the things into one package. This idea is already widely applied for Linux packages, because it leads to a more lightweight system and reduces potential issues and conflicts.
* **Use a simple intuitive lowercase name for the package**. See the [package naming guidelines](http://github.com/chocolatey/chocolatey/wiki/CreatePackages#naming-your-package) for details.
* **Packaging commercial or trial software?** Clearly state this in the package description.

Is your package unqualified for the Chocolatey feed, but you like to be able to install it through Chocolatey? Don't worry, you can always host your package for free on MyGet. See [Hosting Chocolatey Packages on MyGet](https://github.com/chocolatey/chocolatey/wiki/Hosting-Chocolatey-Packages-on-MyGet).

## Character encoding

* **Use the UTF-8 character encoding** for the `*.nuspec` and `*.ps1` files. If you don’t respect this rule, some characters are not displayed correctly in the [Gallery on Chocolatey.org](http://chocolatey.org/packages), because the Gallery assumes `UTF-8`.
* **Do not save your `*.nuspec` files with a Byte Order Mark (BOM)**. A `BOM` is neither required nor recommended for `UTF-8`, because it can lead to several issues.
* **PowerShell scripts need to be saved in UTF-8 with `BOM`**. PowerShell is ignoring the standards and needs a `BOM` in order to recognize scripts as `UTF-8`. Otherwise it processes non `ASCII` characters incorrectly.
* Don’t use the default Windows Editor. In addition to its lack of features, it can’t even save `UTF-8` files without `BOM`. Alternatives:
    * [Notepad++](http://chocolatey.org/packages/notepadplusplus)
    * [Geany](http://chocolatey.org/packages/geany)
* Use this **XML declaration**: `<?xml version="1.0" encoding="utf-8"?>`.

**Note:** There is a lot of confusion in the world of character encodings: For example, `ANSI` is an incorrect term for the internal Windows character encodings, e.&nbsp;g. `Windows-1252`. But you should not use this encoding family anyway.

## What version of the software should I package?
The main release of a product versions are usually sufficient. If there are also beta versions available and you would rather have that, then please create both the official release and the beta (and set the beta as a prerelease when pushing the item to chocolatey.org). Regular users of packages may want to use official releases only and not betas.
  
## Okay, how do I create the packages?
There are three main elements to a chocolatey package. Only the nuspec is required (#1 below).  
  
1. [Nuspec](https://github.com/chocolatey/chocolateytemplates/blob/master/_templates/chocolatey/__NAME__.nuspec) - [Nuspec Reference](http://docs.nuget.org/docs/reference/nuspec-reference)
1. [[chocolateyInstall.ps1|ChocolateyInstallPS1]] - check out the [[helper reference|HelpersReference]]
1. any application files to include (it is highly suggested that you are the author in this case or you have the right to [[distribute files|Legal]]). EXE files in the package/downloaded to package folder from chocolateyInstall.ps1 will get a link to the command line.
  
**Note:** Please maintain compatibility with Posh v2. Not every OS we support is on Posh v2 (nor comes OOB with Posh v3+). It's best to work with the widest compatibility of systems out there.

There is a video showing the creation of a package: [http://www.youtube.com/watch?v=Wt_unjS_SUo](http://www.youtube.com/watch?v=Wt_unjS_SUo)  
The video is a bit outdated in showing the contents of the chocolateyInstall.ps1. Have a look at what the [chocolateyInstall.ps1](https://github.com/ferventcoder/chocolatey-packages/blob/master/manual/windirstat/tools/chocolateyInstall.ps1) looks like now:
  
```powershell
﻿$packageName = 'windirstat'
$fileType = 'exe'
$url = 'http://prdownloads.sourceforge.net/windirstat/windirstat1_1_2_setup.exe'
$silentArgs = '/S'

Install-ChocolateyPackage $packageName $fileType "$silentArgs" "$url"
```

## Quick start guide

If you think you got what it takes and just want to know the basic steps to get a package out, there is a special [Quick Start Guide](https://github.com/chocolatey/chocolatey/wiki/CreatePackagesQuickStart) for you.

##Nuspec?##

For reference - [Nuspec Reference](http://docs.nuget.org/docs/reference/nuspec-reference)
  
The `Chocolatey` Windows package manager uses the same infrastructure as [NuGet](http://nuget.org/) , the `Visual Studio` package manager by `Microsoft`. Therefore packages are based on the same principals. One of those is a package description (specification) in `xml` format, known as the `Nuspec`.

The `Nuspec` contains basic information such as the version, license, maintainer, and package dependencies.

**Note:** If your package uses recently introduced functionality, you might want to include `chocolatey` as a dependency with the version being the lowest version that has the introduced functionality. Otherwise the installation could fail for users with an older version of `Chocolatey` installed.

You can indicate the `Chocolatey` dependency like any other dependency. E.g.:
```xml
    <dependencies>
        <dependency id="Chocolatey" version="0.9.8.21" />
    </dependencies>
```

Logically, the version is based on the lowest compatible version. But if you don't know and used a lot of sorcery in your package, depend on the version of `Chocolatey` that you succesfully tested your package on.

**See also:** [[http://docs.nuget.org/docs/reference/versioning]]

## Installation paths

As the package maintainer, you decide where the packaged application is installed or extracted to. Depending on your type of application (see *“What distinction does chocolatey make between an installable and a portable application?”* at the bottom of the [FAQ](https://github.com/chocolatey/chocolatey/wiki/ChocolateyFAQs)) there are a couple of suitable locations:

### 1. Path provided by the `Get-BinRoot` helper

The path returned by the helper `Get-BinRoot` can be used as the parent directory for the installation. `Get-BinRoot` will return the value of the  environment variable `%ChocolateyBinRoot%`. If the value does not contain a drive reference, the system drive will be prepended. If the environment variable is not set, the default path (~~`C:\Tools`~~ `C:\Chocolatey\bin`) will be returned. 

As an example, [MinGW](https://github.com/ferventcoder/chocolatey-packages/blob/master/manual/mingw/tools/chocolateyInstall.ps1) uses `%ChocolateyBinRoot%`. If the environment variable is not set, MinGW installs to ~~`C:\Tools\MinGW`~~ `C:\Chocolatey\bin\MinGW` by default. If `%ChocolateyBinRoot%` is set to "C:\Common\bin", MinGW installs to `C:\Common\bin\MinGW`.

`%ChocolateyBinRoot%` gives the chocolatey user a way of controlling where packages are installed. If you want to allow customizing the installation path, then this is currently the way to go.

### 2. The default installation path of your .msi/.exe setup file

The original creator probably had a reason for choosing a specific default installation path.  
If you think, the user should be able to customize this path and you, the package maintainer, know how to pass a custom path on to the installer, then you should use `%ChocolateyBinRoot%`.

### 3. The package directory in `%ChocolateyInstall%\lib\mypackage`

You can extract the application within the package directory itself (or even ship an extracted version with the package). This allows chocolatey to automatically find executables and put those on `%path%`.

### Make it clear in the package description

No matter how you decide, you are advised to state the default installation directory in your package description. This prevents confusion about where the application will end up being installed.

If you allow customizing the installation path, then append instructions on how to do that, too.
  
##Dependency Chaining
You can make packages that depend on other packages just by adding those dependencies to the nuspec. Take a look at [ferventcoder.chocolatey.utilities nuspec](https://github.com/ferventcoder/chocolatey-packages/blob/master/manual/ferventcoder.chocolatey.utilities/ferventcoder.chocolatey.utilities.nuspec).

##Avoid folders named “content”
Do not use a folder named “content” in your package. NuGet attaches a special meaning to this folder and will not allow you to have dependencies on packages that have content folders without also having a content folder.

##Naming your package
The __title__ of your package (`<title>` tag in the nuspec) should be the same as the name of the application. Follow the official spelling, use upper and lower case and don’t forget the spaces. Examples of correct package titles are: *Google&nbsp;Chrome*, *CCleaner*, *PuTTY* and *FileZilla*. The title will appear on the left side in the package list of the chocolatey gallery, followed by the version.

There are some guidelines in terms of the package __id__ (`<id>` tag in the nuspec):
* __Use only lowercase letters__, even if you used uppercase letters in the package title.
* __Don’t change the casing of the id of packages which are already submitted.__ Once a package is submitted (even prior moderation), the Gallery will always show the id with the casing of the first package version. In addition, changing the casing of the package id can have negative side effects on dependencies.
* If the original application name consists of compound words without spaces (CamelCase), just as *MKVToolNix*, *TightVNC* and *VirtualBox*, the package id’s are simply the same (but __lowercase__ of course): `mkvtoolnix`, `tightvnc`, and `virtualbox`.
* If the name of the application contains multiple words separated by spaces, such as *MusicBrainz&nbsp;Picard* or *Adobe Reader*, replace the spaces with the hyphen-minus character “-” (U+002D) or just omit them. __Don’t use dots.__ They should be used only if the original application name contains dots (e.&nbsp;g. *Paint.NET*). Hence the correct id’s of the previously mentioned applications can be `musicbrainz-picard` or `adobereader`. It is highly suggested to use the hyphen method when there are long package names, because that increases readability.
* For sub-packages, use the hyphen-minus character “-” (U+002D) as separator, not a dot. Sub-packages are intended for separate packages that include extensions, modules or additional features/files for other applications. Therefore `keepass-langfiles` is a proper package id, because it adds the language files for the main application which in this case is _KeePass_. Another example is `libreoffice-help` for the help pack for _LibreOffice_, the open source office suite.
* If you want to package an open source application, look on the repositories of some popular Linux distributions, such as [Debian](http://www.debian.org/distrib/packages#search_packages), [Ubuntu](http://packages.ubuntu.com/) and [Arch Linux](https://www.archlinux.org/packages/) if they already have a package of it. If that is the case, __use the same package id__. This will make it easier for Linux and Windows users, because then they don’t have to remember and use different package names.

These guidelines are already commonly applied on packages for all major Linux distributions, because they lead to a more consistent look of software repositories, easier to remember package id’s and less considerations about the naming for package creators.

Note that a lot of packages in the Chocolatey Gallery don’t follow these guidelines. The simple reason is that the affected packages were created before the introduction of these guidelines.

If you are going to offer a package that has both an installer and an archive (zip or executable only) version of the application, create three packages&nbsp;– see Rob’s guidance on this: http://devlicio.us/blogs/rob_reynolds/archive/2012/02/25/chocolatey-guidance-on-packaging-apps-with-both-an-install-and-executable-zip-option.aspx

##Package description and release notes

The `<description>` of the package should contain a short text or at least a few words about the software for which the package is made. Here are a few things that should be respected:

* The description should always be written in English, even if the packaged software does not provide an UI in English. You can also include the software’s description in its original language, but insert it after the English description.
* The description should not just contain a repetition of the package name.
* It should not just consist of a link where more information can be found. For that purpose there’s already `<projectUrl>`.
* The contents of `<description>` and also `<releaseNotes>` are parsed as Markdown, which means that it should not be indented, otherwise it would be interpreted as code by the Markdown parser, which results in monospaced, non-wrapping text on the package page. Also remember to separate paragraphs with an empty line. The same applies to `<releaseNotes>`. It should look like this:

``` XML
  …
  <package>
  …
    <description>
Paragraph 1

Paragraph 2

Paragraph 3
    </description>
    …
```

##Versioning Recommendations
Versioning can be both simple and complicated. The best recommendation is to use the same versioning that the installable/portable application uses. With chocolatey you get four version segments. If the application only uses 1, 2 or 3 version segments, follow suit.  
  
If the 4th segment is used, some folks like to drop the segment altogether and use that as only the package fix notation using one of the notations in the next section. There is no recommendations at this time.  
  
###Package Fix Version Notation
If you need to fix the package for some reason, you can use the fourth number for a package fix notation. There are two recommended methods of package fix version notation:  
  
 * **Date (Year/Month/Day)** - Some folks use year month day package fix notation (yyyyMMdd as in 20120627 seen as 1.2.0.20120627) 
 * Sequential - Some folks use sequential numbering (0, then 1, etc as in 0 for no fix, 1 for first fix and so on seen as 1.2.0.0 and 1.2.0.1).  
  
Date Package Fix Version Notation is recommended because one can ascertain what it is immediately upon seeing it.   
  
Package fix version notation is only acceptable in the fourth segment. Do not use any of the other segments for package fix notation. If an application only uses 1 or 2 version segments, add zeros into the other segments until you get to the 4th segment (i.e. 1.0.0.20120627).  

When the fourth segment is used, it is recommended to add two zeroes (00) to the end of the version. Then when you need to fix, you just increment that number. So if the package was ruby and the version was 2.0.0-p353, the package is 2.0.0.35300 (adding the two zeroes at the end). Then a fix would be 2.0.0.35301 and so on. 

##Internationalization and localization of packages
For chocolatey, internationalization and localization of packages is very important, because it has users from all over the world. Many applications support multiple languages, but they use several different methods to achieve that. Therefore, there is no standard how internationalization/localization has to be integrated into packages. However, here are a few examples of packages that use various techniques. You can use them as inspiration for new packages:
* The ideal situation is when an application determines the user’s system language and automatically installs with that language. Then you don’t have to take any action relating to localization, because the application already handles that. Examples of such applications are [VLC Media Player](https://chocolatey.org/packages/vlc) and [LibreOffice](https://chocolatey.org/packages/libreoffice).
* When an application provides different installers for different languages, you should determine the system language and download the appropriate installer. The package for [Mozilla Firefox](https://chocolatey.org/packages/Firefox) ([source code](https://github.com/chocolatey/chocolatey-coreteampackages/tree/master/automatic/Firefox)) uses this method.
* Sometimes an application installer or executable has already integrated all supported languages, but doesn’t automatically select the system language during a silent install. Often you can pass an additional install parameter to select the desired language. This is used for example in the [CCleaner](https://chocolatey.org/packages/ccleaner) package ([source code](https://github.com/tonigellida/chocolateyautomaticpackages/tree/master/ccleaner)).
* Some application use separate language files which must be downloaded separately and put somewhere in the application directory. It is best when you create a separate package for the language files. If your package id is `packageid`, then call it `packageid-langfiles`. The [language files package for KeePass](https://chocolatey.org/packages/keepass-langfiles) is an example how this can be achieved.

##Package icon guidelines
If there is an icon which is suitable for your package, you can specify it in the `<iconUrl>` tag in the nuspec. But there are a few things you should consider:
* **Avoid hotlinking icons from sites where you don’t have control over the file.** If you have a packages repository (recommended), use your own copy of the icon and put it there.
* For the **icon URLs it is recommended to use https://cdn.rawgit.com/ (production links).** Rawgit is a CDN service that permits you to serve files hosted in a repository on GitHub. Keep in mind that cdn.rawgit.com caches files permanently. Therefore it’s best to use a specific tag or commit URL, not a branch URL.
* **Avoid using GitHub raw links** (https://raw.githubusercontent.com/…). They are not intended to be used as CDN.
* **Use the software’s icon if one is available, not the logo.** This blog post explains the difference between logos and icons: http://blog.designcrowd.com/article/353/differences-between-logos-and-icons. If the software of your package doesn’t have an icon, but a logo with text and an image, you can extract the image with your favorite image editor and use that as package icon. An example is [MySQL’s dolphin](https://chocolatey.org/packages/mysql).
* **Use package icons with at least 128 pixels in width or height** if available. However, avoid very high resolutions, because this would only unnecessarily increase the page load time. If there are only icons with less than 128 pixels available, choose the icon with the highest resolution you can find without upscaling it. Don’t use low resolution favicons as package icons.
* Use icons with transparent background if available.
* The package list in the gallery shows the icons with a maximum of 48 pixels in width/height. Application logos with very detailed graphics that are barely visible at that dimension are not suitable as package icons.
* **PNG is the preferred format** for raster package icons. Avoid ICO, GIF and JPEG graphics.
* Good sources for package icons are the official desktop icons of the corresponding application you want to make a package of. The icons can be extracted from the app executables using tools like [BeCyIconGrabber](https://chocolatey.org/packages/becyicongrabber). Remember to take the icon with 128&nbsp;px or more and save it as PNG file.

## How do I exclude [executables from getting batch redirects](https://github.com/chocolatey/chocolatey/issues/106)?
If you have executables in the package or brought into the package folder during powershell run and you want to exclude them you need to create an empty file named exactly like (**case sensitive**) the executable with `.ignore` suffixed on the end in the same directory where the executable is or will be.  
  
Example: In the case of `Bob.exe` you would create a file named `Bob.exe.ignore` and that file would not get a redirect batch link. The chocolatey package has an example of that. To further expand, `bob.exe.ignore` would not work because it doesn't have the correct casing. 
  
## How do I set up batch redirects for [applications that have a GUI](https://github.com/chocolatey/chocolatey/issues/76)?
If you don't want to see a hanging window when you open an application from the command line that was set up with chocolatey, you want to create a file next to the executable that is named exactly the same (**case sensitive**) with `.gui` suffixed on the end.  
  
Example: In the case of `Bob.exe` you would create a file named `Bob.exe.gui` and that file would be set up as a GUI application so the window will call it and then move on without waiting for it to finish.  Again, `bob.exe.gui` would not work because it doesn't have the correct casing. 
  
##Build Your Package

Open a command line in the directory where the nuspec is and type [[cpack|CommandsPack]]. That's it.

##Testing Your Package

To test the package you just built, with the command line still open (and in the current working directory in the same folder as the newly created `*.nupkg` file) type:  

```cmd
 choco install packageName -source %cd%
```

This will install the package right out of your source. As you find things you may need to fix, you will want to delete the particular package folder out of the %ChocolateyInstall%\lib folder. 

If your changes to an existing package are not being loaded, check for a cached version of the package in %LocalAppData%\NuGet\Cache.

`%cd%` points to the current directory. You can specify multiple directories separated by a semicolon;

When your `nuspec` specifies dependencies that are not in your source, you should add their paths to the source directory. E.g. in the case of Chocolatey itself:
```xml
        <dependencies>
            <dependency id="Chocolatey" version="0.9.8.20" />
        </dependencies>
```
You'll need to append the API path like so:
`-source '"%cd%;http://chocolatey.org/api/v2/"'` (note the apostrophe then the double quotes here).
  
##Push Your Package

To push your package after you have built and tested it, you type `cpush packageName.nupkg` where *packageName.nupkg* is the name of the nupkg that was built with a version number as part of the package name.  You must have an api key for http://chocolatey.org/ set. You can do that with nuget.exe. You can install nuget.commandline so you can set this (notice it is on nuget.org and not on chocolatey.org - chocolatey installs packages from both sources!). 
Take a look at [[cpush|CommandsPush]]  
  
You can also log into chocolatey.org and upload your package from there.  

##Is there a SIMPLER way of creating packages?
I'm so glad you asked. Take a look at this repository and follow the instructions: https://github.com/chocolatey/chocolateytemplates
  
##Automatic package repositories? 
Yes - [[AutomaticPackages]]

##Becoming a primary maintainer of an existing package
See [[PackageMantainerHandover]]