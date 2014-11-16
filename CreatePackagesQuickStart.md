## Creating Chocolatey Packages - TL;DR version

Here's a TL;DR quick start version of the package creating tutorial. Follow these steps to create a simple package.

**Problem?** Read the detailed version: [[Creating Chocolatey Packages|CreatePackages]]

## Prerequisites


* You have Chocolatey installed.
* You know how a package works
  * A package contains a `nuspec` file. This defines the package. ([Docs](http://docs.nuget.org/docs/reference/nuspec-reference)) ([Example](https://github.com/chocolatey/chocolateytemplates/blob/master/_templates/chocolatey/__NAME__.nuspec))
  * A package contains an installation script. This can be [very simple](https://github.com/chocolatey/chocolatey/wiki/CreatePackagesQuickStart#examples).

## Quick start guide


* **Generate package template** using [[WarmuP|http://chocolatey.org/packages/warmup]]:
   * `choco install warmup`
   * Get templates:
      * `choco install git`
      * close and reopen cmd prompt so git is in the PATH
      * `cd %ChocolateyInstall%`
      * `git clone https://github.com/chocolatey/chocolateytemplates.git`
      * `cd chocolateytemplates\_templates`
      * `warmup  addTemplateFolder chocolatey "%CD%\chocolatey"`
   * **Optional:** Add replacements. You can get more creative once you start making more complicated packages.
      * *warmup addTextReplacement \_\_CHOCO_PKG_OWNER_NAME\_\_ "Your Name"*
(could be same as other)
   * `warmup chocolatey PackageName`
* **Edit template** using common sense
   * `cd PackageName`
   * Edit the `PackageName.nuspec` configuration file.
   * Edit the `./tools/chocolateyInstall.ps1` install script.
     * Make sure you figure out the installer's silent mode. Use [Universal Silent Switch Finder](http://unattended.sourceforge.net/installers.php), which is available as a Choco package: `choco install ussf`
   * You __must__ save your files with _UTF-8_ character encoding without BOM. ([Details](https://github.com/chocolatey/chocolatey/wiki/CreatePackages#character-encoding))
* **Build the package**
   * Still in package directory
   * `cpack`
      * "successfully created PackageName.1.1.0.nupkg"
* **Test the package**
   * **Testing should probably be done on a Virtual Machine**
   * In your package directory, use: 
      * `choco install PackageName -source '%cd%'`
   * Otherwise, use the full path:
      * `choco install PackageName -source 'c:\path\to\Package\'`
* **Push the package** to the Chocolatey repository:
   * Get a Chocolatey account:
      * [[http://chocolatey.org/account/Register]]
   * Copy the API key [[from your Chocolatey account|http://chocolatey.org/account]].
   * `choco install nuget.commandline`
   * `nuget SetApiKey [API_KEY_HERE] -source `http://chocolatey.org/`
   * `cpush PackageName.1.1.0.nupkg`

## Common Mistakes


* **NuSpec**
   * **id** is the package name and should contain no spaces and weird characters.
   * **version** is a dot-separated identifier containing a maximum of 4 numbers. e.g. _1.0_ or _2.4.0.16_
   * **The path to your package** should contain no spaces, or enclose pathnames with single or double doublequotes, like so:
      * 'PackageName'
      * ""PackageName""
* **Using NuGet tools**
   * **Specify the source** if you accidentally use NuGet commands when following some guide instead of Chocolatey commands. E.g.:
      * `nuget push package.nupkg -source http://chocolatey.org/` instead of:
      * `cpush package.nupkg`

## Environmental Variables


* `%ChocolateyInstall%` - Chocolatey installation directory
* `%ChocolateyInstall%\lib\PackageName` - Package directory
* `%chocolatey_bin_root%` - Path of Binaries
* `%cd%` - current directory

## Examples

Here are some simple examples.

### chocolateyInstall.ps1 for .exe installer

```cmd
$name = 'Package Name'
$url  = 'http://path/to/download/installer.exe'

Install-ChocolateyPackage $name 'EXE' '/VERYSILENT' $url
```

**NOTE:** You have to figure out the command line switch to make the installer silent, e.g. **/VERYSILENT**. This changes from installer to installer.

### chocolateyInstall.ps1 for .msi installer

**NOTE:** Please maintain compatibility with Posh v2. Not every OS we support is on Posh v2 (nor comes OOB with Posh v3+). It's best to work with the widest compatibility of systems out there.

```cmd
$packageName = 'Package Name'
$installerType = 'msi' 
$url = 'http://path/to/download/installer_x86.msi'
$url64 = 'http://path/to/download/installer_x64.msi'
$silentArgs = '/quiet'
$validExitCodes = @(0,3010)

Install-ChocolateyPackage "$packageName" "$installerType" "$silentArgs" "$url" "$url64"  -validExitCodes $validExitCodes
```

## Tips

* If you cannot find the installer silent mode, you can try an old tool called [[Universal Silent Switch Finder 1.5.0.0|http://www.softpedia.com/progDownload/Universal-Silent-Switch-Finder-Download-180984.html]]