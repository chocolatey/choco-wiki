## Chocolatey Uninstall (choco uninstall)
***NOTE***: 100% compatible with previous version with options and switches. Add `-y` for previous behavior with no prompt.

Uninstalls a package or a list of packages.  Some
 may prefer to use `cuninst` as a shortcut for `choco uninstall`.

## Usage

    choco uninstall pkg [pkg2 pkgN] [options/switches]

**NOTE**: `all` is a special package keyword that will allow you to
 uninstall all packages.

## Examples

    choco uninstall git
    choco uninstall notepadplusplus googlechrome atom 7zip
    choco uninstall notepadplusplus googlechrome atom 7zip -dv
    choco uninstall ruby --version 1.8.7.37402
    choco uninstall nodejs.install --all-versions

## Options and Switches

**NOTE**: Options and switches apply to all items passed, so if you are uninstalling multiple packages, and you use `--version=1.0.0`, it is going to look for and try to uninstall version 1.0.0 of every package passed. So please split out multiple package calls when wanting to pass specific options.

Includes [[default options/switches|CommandsReference#default-options-and-switches]]

```
--version=VALUE
  Version - A specific version to uninstall. Defaults to unspecified.

-a, --allversions, --all-versions
  AllVersions - Uninstall all versions? Defaults to false.

--ua, --uninstallargs, --uninstallarguments, --uninstall-arguments=VALUE
  UninstallArguments - Uninstall Arguments to pass to the native
  installer in the package. Defaults to unspecified.

-o, --override, --overrideargs, --overridearguments, --override-arguments
  OverrideArguments - Should uninstall arguments be used exclusively
  without appending to current package passed arguments? Defaults to
  false.

--notsilent, --not-silent
  NotSilent - Do not uninstall this silently. Defaults to false.

--params, --parameters, --pkgparameters, --packageparameters, --package-parameters=VALUE
  PackageParameters - Parameters to pass to the package. Defaults to
  unspecified.

-x, --forcedependencies, --force-dependencies
  ForceDependencies - Force dependencies to be uninstalled when
  uninstalling package(s). Defaults to false.

-n, --skippowershell, --skip-powershell
  Skip PowerShell - Do not run chocolateyUninstall.ps1. Defaults to false.
```

## Known Limitations
* There are no functions defined in the Chocolatey PowerShell module that would help with uninstall - yet
* There is no automatic removal of MSIs (well there is, but auto uninstaller is not turned on by default).