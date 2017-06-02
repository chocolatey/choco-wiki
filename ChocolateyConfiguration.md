# Chocolatey Configuration

There are settings and features that can customize the way that Chocolatey works for you. The following is a list of config settings and features and their default values.


## Config Settings

Config settings are adjusted using `choco config set --name <nameFromBelow> --value <value>` and set back to default with `choco config unset --name <nameFromBelow>`. For more information see [[`choco config` command|CommandsConfig]] or run `choco config -?`.

General
* `cacheLocation` = **' '** - Cache location if not TEMP folder. Replaces `$env:TEMP` value.
* `containsLegacyPackageInstalls` = **'true'** - Install has packages installed prior to 0.9.9 series.
* `commandExecutionTimeoutSeconds` = **'2700'** - Default timeout for command execution. '0' for infinite (starting in 0.10.4).
* `webRequestTimeoutSeconds` = **'45'** - Default timeout for web requests. Available in 0.9.10+.

Proxy

* `proxy` = **' '** - Explicit proxy location. Available in 0.9.9.9+.
* `proxyUser` = **' '** - Optional proxy user. Available in 0.9.9.9+.
* `proxyPassword` = **' '**  - Optional proxy password. Encrypted. Available in 0.9.9.9+.
* `proxyBypassList` = **' '** - Optional proxy bypass list. Comma separated. Available in 0.10.4+.
* `proxyBypassOnLocal` = **'true '** - Bypass proxy for local connections. Available in 0.10.4+.

### Config Settings - Licensed Edition

General
* `maximumDownloadRateBitsPerSecond` = **' '** - The maximum download rate in bits per second. '0' or empty means no maximum. A number means that will be the maximum download rate in bps. Defaults to ''. Available in licensed editions v1.10+ only. See https://chocolatey.org/docs/features-download-throttle


Virus Scanning
* `virusCheckMinimumPositives` = **'5'** - Minimum numer of scan result positives before flagging a binary as a possible virus. Used when virusScannerType is VirusTotal. Available in 0.9.10+. Licensed editions only. See https://chocolatey.org/docs/features-virus-check
* `virusScannerType` = **'VirusTotal'** - Virus Scanner Type (Generic or VirusTotal). Defaults to VirusTotal for Pro. Available in 0.9.10+. Licensed editions only. See https://chocolatey.org/docs/features-virus-check
* `genericVirusScannerPath` = **' '** - The full path to the command line virus scanner executable. Used when virusScannerType is Generic. Available in 0.9.10+. Licensed editions only. See https://chocolatey.org/docs/features-virus-check
* `genericVirusScannerArgs` = **'[[File]]'** - The arguments to pass to the generic virus scanner. Use [[File]] for the file path placeholder. Used when virusScannerType is Generic. Available in 0.9.10+. Licensed editions only. See https://chocolatey.org/docs/features-virus-check
* `genericVirusScannerValidExitCodes` = **'0'** - The exit codes for the generic virus scanner when a file is not flagged. Separate with comma, defaults to 0. Used when virusScannerType is Generic. Available in 0.9.10+. Licensed editions only. See https://chocolatey.org/docs/features-virus-check

## Features

Features are adjusted using `choco feature enable|disable -n <nameFromBelow>`. For more information see [[`choco feature` command|CommandsFeature]] or run `choco feature -?`.

A checkbox means this feature is turned on by default.

General
* [x] `autoUninstaller` - Uninstall from programs and features without requiring an explicit uninstall script.
* [ ] `failOnAutoUninstaller` - Fail if automatic uninstaller fails.
* [x] `powershellHost` - Use Chocolatey's built-in PowerShell host. Available in 0.9.10+.
* [ ] `logEnvironmentValues` - Log Environment Values - will log values of environment before and after install (could disclose sensitive data). Available in 0.9.10+.
* [x] `ignoreInvalidOptionsSwitches` - Ignore Invalid Options/Switches - If a switch or option is passed that is not recognized, should choco fail? Available in 0.9.10+.
* [x] `usePackageExitCodes` - Use Package Exit Codes - Package scripts can provide exit codes. With this on, package exit codes will be what choco uses for exit when non-zero (this value can come from a dependency package). Chocolatey defines valid exit codes as 0, 1605, 1614, 1641, 3010. With this feature off, choco will exit with a 0 or a 1 (matching previous behavior). Available in 0.9.10+.
* [x] `showNonElevatedWarnings` - Show Non-Elevated Warnings - Display non-elevated warnings. Available in 0.10.4+.
* [x] `showDownloadProgress` - Show Download Progress - Show download progress percentages in the CLI. Available in 0.10.4+.
* [ ] `stopOnFirstPackageFailure` - Stop On First Package Failure - stop running install, upgrade or uninstall on first package failure instead of continuing with others. As this will affect upgrade all, it is normally recommended to leave this off. Available in 0.10.4+.
* [ ] `useRememberedArgumentsForUpgrades` - Use Remembered Arguments For Upgrades - when running upgrades, use arguments for upgrade that were used for installation ('remembered'). This is helpful when running upgrade for all packages. Available in 0.10.4+. This is considered in preview for 0.10.4 and will be flipped to on by default in a future release.

Security
* [ ] `allowGlobalConfirmation` - Prompt for confirmation in scripts or bypass.
* [ ] `useFipsCompliantChecksums` - Use FIPS Compliant Checksums - Ensure checksumming done by choco uses FIPS compliant algorithms. Not recommended unless required by FIPS Mode. Enabling on an existing installation could have unintended consequences related to upgrades/uninstalls. Available in 0.9.10+.

Security - Downloaded Files
* [x] `checksumFiles` - Checksum files when pulled in from internet (based on package).
* [ ] `allowEmptyChecksums` - Allow packages to have empty/missing checksums for downloaded resources from non-secure locations (HTTP, FTP). Enabling is not recommended if using sources that download resources from the internet. Available in 0.10.0+.
* [x] `allowEmptyChecksumsSecure` - Allow packages to have empty/missing checksums for downloaded resources from secure locations (HTTPS). Available in 0.10.0+.

Other
* [ ] `failOnStandardError` - Fail if install provider writes to stderr. Available in 0.9.10+.
* [ ] `failOnInvalidOrMissingLicense` - Fail On Invalid Or Missing License - allows knowing when a license is expired or not applied to a machine. Available in 0.9.10+.
* [ ] `scriptsCheckLastExitCode` - Scripts Check $LastExitCode (external commands) - Leave this off unless you absolutely need it while you fix your package scripts  to use `throw 'error message'` or `Set-PowerShellExitCode #` instead of `exit #`. This behavior started in 0.9.10 and produced hard to find bugs. If the last external process exits successfully but with an exit code of not zero, this could cause hard to detect package failures. Available in 0.10.3+. Will be removed in 0.11.0.


### Features - Licensed Edition

General
* [x] `virusCheck` - Virus Check - perform virus checking on downloaded files. Available in 0.9.10+. Licensed editions only. See https://chocolatey.org/docs/features-virus-check
* [x] `downloadCache` - Download Cache - use the private download cache if available for a package. Available in 0.9.10+. Licensed editions only. See https://chocolatey.org/docs/features-private-cdn
* [ ] `internalizeAppendUseOriginalLocation` - Package Internalizer - When `Install-ChocolateyPackage` is internalized, append the `-UseOriginalLocation` parameter to the function. Business editions only (licensed version 1.7.0+). Requires at least Chocolatey v0.10.1 for `Install-ChocolateyPackage` to recognize the switch appropriately. See https://chocolatey.org/docs/features-automatically-recompile-packages
* [ ] `allowPreviewFeatures` - Allow Preview Features - Turns on Preview Features. Some features become available for preview before they are released for testing purposes. Please note these should not be used for production systems as they could mess up a system.  Licensed editions only (version 1.9.0+).

Package Synchronizer
* [x] `allowSynchronization` - Synchronization - Keep Chocolatey packages in sync with changes in Programs and Features. Available in 0.9.10+. Licensed editions only. See https://chocolatey.org/docs/features-synchronize
* [ ] `showAllPackagesInProgramsAndFeatures` - Package Synchronizer - Packages In Programs And Features Synchronization - Show all packages in Programs and Features, not just packages that use a native installer. Business editions only (version 1.10.0+).

Self-Service / Background Mode
* [ ] `useBackgroundService` - Use Background Service - For some commands like install and upgrade, use a background service instead of running the command directly. Business editions only (licensed version 1.8.4+). Uninstall requires Chocolatey v0.10.4. Requires the package chocolatey-agent (choco install chocolatey-agent). See https://chocolatey.org/docs/features-agent-service
* [x] `useBackgroundServiceWithSelfServiceSourcesOnly` - Use Background Service With Self-Service Sources Only - When using Self-Service, opt-in only sources configured to be used with self-service. This allows for other sources only an admin can use. Business editions only (version 1.9.7+). Requires Chocolatey 0.10.4+ for enabling sources with self-service only.
