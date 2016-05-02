## Creating Chocolatey Packages - TL;DR version

Here's a TL;DR quick start version of the package creating tutorial. Follow these steps to create a simple package.

**Problem?** Read the detailed version: [[Creating Chocolatey Packages|CreatePackages]]

## Prerequisites

* You have Chocolatey installed.
* You've read [[What are Chocolatey Packages?|GettingStarted#what-are-chocolatey-packages]] first.
* You know how a package works
  * A package contains a `nuspec` file. This defines the package. ([Docs](http://docs.nuget.org/docs/reference/nuspec-reference)) ([Example](https://github.com/chocolatey/chocolateytemplates/blob/master/_templates/chocolatey/__NAME__.nuspec))
  * A package may contain embedded software.
  * A package may contain an installation script. This can be [[very simple|CreatePackagesQuickStart#examples]].

## Quick start guide

* **Generate new package**:
   * `choco new -h` will get you started seeing options available to you.
   * Once you figured out all of your options, you should move forward with generating your template.
* **Edit template** using common sense
   * `cd package-name`
   * Edit the `package-name.nuspec` configuration file.
   * Edit the `./tools/chocolateyInstall.ps1` install script.
     * Make sure you figure out the installer's silent mode. Use [Universal Silent Switch Finder](http://unattended.sourceforge.net/installers.php), which is available as a Choco package: `choco install ussf`
   * You __must__ save your files with _UTF-8_ character encoding without BOM. ([Details](https://github.com/chocolatey/choco/wiki/CreatePackages#character-encoding))
* **Build the package**
   * Still in package directory
   * `choco pack`
      * "Successfully created package-name.1.1.0.nupkg"
* **Test the package**
   * **Testing should probably be done on a Virtual Machine**
   * In your package directory, use:
      * `choco install package-name -s "$pwd" -f` - powershell
      * `choco install package-name -s '%cd%' -f` - everywhere else
   * Otherwise, use the full path:
      * `choco install package-name -source 'c:\path\to\Package\' -f`
* **Push the package** to the Chocolatey community feed repository:
   * Get a Chocolatey account:
      * [[https://chocolatey.org/account/Register]]
   * Copy the API key [[from your Chocolatey account|https://chocolatey.org/account]].
   * `choco apikey -k [API_KEY_HERE] -source https://chocolatey.org/`
   * `choco push package-name.1.1.0.nupkg -s https://chocolatey.org/` - packagename can be omitted

## Common Mistakes

* **NuSpec**
   * **id** is the package name and should meet the following criteria:
    * should contain no spaces and weird characters.
    * should be lowercase.
    * should separate spaces in the software name with `-` e.g. `classic-shell`. Yes, we realize there are a lot of older packages not following this convention.
   * **version** is a dot-separated identifier containing a maximum of 4 numbers. e.g. _1.0_ or _2.4.0.16_ - except for prerelease packages

## Environmental Variables

* `%ChocolateyInstall%` - Chocolatey installation directory
* `%ChocolateyInstall%\lib\package-name` - Package directory
* `%cd%` or `$pwd` - current directory

## Examples

Here are some simple examples.

### chocolateyInstall.ps1 for .exe installer

```powershell
$name = 'Package Name'
$installerType = 'exe'
$url  = 'http://path/to/download/installer.exe'
$silentArgs = '/VERYSILENT'

Install-ChocolateyPackage $name $installerType $silentArgs $url
```

**NOTE:** You have to figure out the command line switch to make the installer silent, e.g. **/VERYSILENT**. This changes from installer to installer.

### chocolateyInstall.ps1 for .msi installer

**NOTE:** Please maintain compatibility with Posh v2. Not every OS we support is on Posh v2 (nor comes OOB with Posh v3+). It's best to work with the widest compatibility of systems out there.

```powershell
$packageName = 'Package Name'
$installerType = 'msi'
$url = 'http://path/to/download/installer_x86.msi'
$url64 = 'http://path/to/download/installer_x64.msi'
$silentArgs = '/quiet'
$validExitCodes = @(0,3010)

Install-ChocolateyPackage $packageName $installerType $silentArgs $url $url64  -validExitCodes $validExitCodes
```

### Parsing Package Parameters
For a complete example of how you can use the PackageParameters argument of the ```choco install``` command, see this [[How-To|How-To-Parse-PackageParameters-Argument]].
## Tips

* If you cannot find the installer silent mode, you can try an old tool called [Universal Silent Switch Finder 1.5.0.0](http://www.softpedia.com/progDownload/Universal-Silent-Switch-Finder-Download-180984.html) - `choco install ussf`.
