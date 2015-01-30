# Chocolatey Upgrade (choco upgrade / cup)
Upgrades an existing package to the latest version, if there is a newer
version available.
`choco upgrade packageName` or shortcut with `cup packageName`.

Upgrades a package or a list of packages.

## Usage

    choco upgrade pkg [pkg2 pkgN] [options/switches]

**NOTE**: `all` is a special package keyword that will allow you to upgrade
 all currently installed packages.

**NOTE**: If you do not have a package installed, upgrade will error.

**NOTE**: Options and switches apply to all items passed, so if you are installing multiple packages, and you use `--version=1.0.0`, it is going to look for and try to install version 1.0.0 of every package passed. So please split out multiple package calls when wanting to pass specific options.

## Examples

    choco upgrade chocolatey
    choco upgrade notepadplusplus googlechrome atom 7zip
    choco upgrade notepadplusplus googlechrome atom 7zip -dvfy
    choco upgrade git --params="/GitAndUnixToolsOnPath /NoAutoCrlf" -y
    choco upgrade nodejs.install --version 0.10.35


## Options and Switches

Includes [[default options/switches|CommandsReference#default-options-and-switches]]

```
-s, --source=VALUE
  Source - The source to find the package(s) to install. Special sources
  include: ruby, webpi, cygwin, windowsfeatures, and python. Defaults to
  default feeds.

--version=VALUE
  Version - A specific version to install. Defaults to unspecified.

--pre, --prerelease
  Prerelease - Include Prereleases? Defaults to false.

--x86, --forcex86
  ForceX86 - Force x86 (32bit) installation on 64 bit systems. Defaults
  to false.

--ia, --installargs, --installarguments, --install-arguments=VALUE
  InstallArguments - Install Arguments to pass to the native installer
  in the package. Defaults to unspecified.

-o, --override, --overrideargs, --overridearguments,
--override-arguments
  OverrideArguments - Should install arguments be used exclusively
  without appending to current package passed arguments? Defaults to
  false.

--notsilent, --not-silent
  NotSilent - Do not install this silently. Defaults to false.

--params, --parameters, --pkgparameters, --packageparameters,
--package-parameters=VALUE
  PackageParameters - Parameters to pass to the package. Defaults to
  unspecified.

-m, --sxs, --sidebyside, --side-by-side, --allowmultiple,
--allow-multiple, --allowmultipleversions, --allow-multiple-versions
  AllowMultipleVersions - Should multiple versions of a package be
  installed? Defaults to false.

-i, --ignoredependencies, --ignore-dependencies
  IgnoreDependencies - Ignore dependencies when upgrading package(s).
  Defaults to false.

-n, --skippowershell, --skip-powershell
  Skip Powershell - Do not run chocolateyInstall.ps1. Defaults to false.
```
