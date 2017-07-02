# Package Reducer (Pro+)
> Reduce the size of your package installations automatically

If you have a significant number of Chocolatey packages you manage, you may notice that you also may have a pretty significant space usage under the Chocolatey lib directory. Package reducer automatically decreases the size of nupkg files to around 5KB and removes installers and zips automatically from your package install directories. This may allow you to save GBs of usage for a large amount of packages being managed!

<!-- TOC -->

- [Usage](#usage)
  - [Requirements](#requirements)
  - [Setup](#setup)
- [See It In Action](#see-it-in-action)
- [Options and Switches](#options-and-switches)
- [FAQ](#faq)
  - [How do I take advantage of this feature?](#how-do-i-take-advantage-of-this-feature)
  - [I'm a licensed customer, now what?](#im-a-licensed-customer-now-what)
  - [How does it work?](#how-does-it-work)
  - [Can I apply this already installed packages?](#can-i-apply-this-already-installed-packages)

<!-- /TOC -->

## Usage
When you normally create packages that embed or download resources, the impact on a system includes the following:

* A nupkg file size takes up some space - this is like a fancy zip file and may contain binaries
* Files extracted or downloaded to the package directory - this may include zips and installers
* The actual install location if using an installer
* MSI cached by Windows - Windows caches the complete MSI binaries

Typically with this in mind, the size of nupkg could account for an up to 4x impact on a system considering the above. So we typically recommend organizations set a comfortable threshold for package sizes (around 500MB) before splitting out binaries to downloaded resources.

Package Reducer nearly removes that need as it reduces the nupkg file to 5KB by removing all the embedded files. Then it takes it a step further and removes zip and installers from the package directory automatically (configurable by a feature switch, see [Setup](#setup)).

When you turn on Package Reducer, the first two items above no longer take up any significant space. This can reduce space usage in the order of GBs for some installations of Chocolatey.

With Package Reducer:

* nupkg file is reduced to 5KB or less, no matter the size on install
* zips / installers are automatically removed from the package directory if they are found

The following file extensions are removed automatically:

* 7z / zip / rar / gz / tar / sfx
* iso
* msi / msu / msp
* exe files if they are detected to be an installer

So the space usage impact changes to what you'd normally experience outside of Chocolatey:

* the actual install location (package directory, Program Files, etc)
* MSI cache for MSIs

### Requirements

* Chocolatey (`chocolatey` package) v0.10.7+.
* Chocolatey for Business (C4B) Edition
* Chocolatey Licensed Extension (`chocolatey.extension` package) v1.20.0+.

### Setup

To turn on Package Reducer, you need to run the following:

* `choco upgrade chocolatey.extension <options>`
* `choco feature enable -n reduceInstalledPackageSpaceUsage`
* If you want to limit to just nupkg files being reduced and not automatically removing zips and installers, run the following: `choco feature enable -n reduceOnlyNupkgSize`

## See It In Action

Coming soon!

## Options and Switches

Coming soon!

## FAQ

### How do I take advantage of this feature?
You must have a [licensed edition of Chocolatey](https://chocolatey.org/pricing) (Pro, MSP, or Business) and use the community package repository to install/upgrade packages. Pro is a personal, named license that costs about the price of a lunch outing per month and comes with several other features. Business editions are great for organizations that need to manage the total software management lifecycle. MSP editions are for managed service providers and contain the same features as Pro (minus VirusTotal integration).

### I'm a licensed customer, now what?
Once you have set up the feature(s), it will automatically reduce the size of the packaging on install and upgrade.

### How does it work?
It just works. When you install or upgrade a package, it will automatically reduce the size of the installation directory if you've set up Package Reducer properly.

### Can I apply this already installed packages?
Not yet, but we are looking into adding this functionality.
