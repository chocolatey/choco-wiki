## The Problem
From time to time, a previously approved Chocolatey Package needs to be deprecated.  This could be for a number of reasons:

* It was created in error
* It has been superseded by another package
* It's package id has been changed to something that better fits with the [package naming guidelines](https://github.com/chocolatey/chocolatey/wiki/CreatePackages#naming-your-package)

All versions of this package could simply be unlisted from chocolatey.org, meaning that they could no longer be installed, however, this solution is not ideal.  Any user who previously installed this package, and added it as part of an installation script, would get an error the next time that they tried to install it, and this is far from ideal.

When a package needs to be deprecated, it needs to be handled in such a way that existing users will continue to be able to use the old package id, but take advantage of the replacement package, if there is one.

## The Solution
When deprecating a Chocolatey Package, the following steps should be followed:

* Begin creating a new version of the Chocolatey Package (using the [Package Fix Version Notation](https://github.com/chocolatey/chocolatey/wiki/CreatePackages#package-fix-version-notation) if required
* Update the Title of the package to include [Deprecated] at the start of the ID
* Update the package description with an explanation as to why the package is being deprecated
* Remove all files from the Chocolatey Package
* In the case where the package is being replaced by another package, add a dependency on the new package id

By following this process, any existing users who try to install the old package id, will automatically get the new package, as it will be installed as a dependency