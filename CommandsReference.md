#Command Reference

This is a listing of all of the different things you can pass to choco.

**NOTE:** When you need to quote things, such as when using spaces, please use single quote marks (`'`). In cmd.exe, you can also use double double quotes (i.e. `""yo""`). This is due to the hand off to PowerShell - seems to strip off the outer set of quotes. TODO: TEST THIS, MAY NOT BE RELEVANT NOW.

## Commands

 * [[list|CommandsList]] - lists remote or local packages
 * [[search|CommandsList]] - searches remote or local packages
 * [[install|CommandsInstall]] - installs packages from various sources
 * installmissing - **REMOVED**
 * update - **REMOVED** - RESERVED for future use (You are looking for upgrade, these are not the droids you are looking for)
 * [[upgrade|CommandsUpgrade]] - upgrades packages from various sources
 * version - **REMOVED** - use `choco upgrade pkgname --noop` instead
 * [[uninstall|CommandsUninstall]] - uninstalls a package
 * help - **REMOVED** - use `-h` on any command.

## Commands (intermediate to advanced)
 * [[source|CommandsSources]] - view and configure default sources
 * [[apikey|CommandsApiKey]] - retrieves or saves an apikey for a particular source
 * [[pin|CommandsPin]] - suppress upgrades to a package

## Package Creation Commands
 * [[new|CommandsNew]] - generates files necessary for a chocolatey package
 * [[pack|CommandsPack]] - packages up a nuspec to a compiled nupkg
 * [[push|CommandsPush]] - pushes a compiled nupkg

## Default Options and Switches

```
-?, --help, -h
  Prints out the help menu.

-d, --debug
  Debug - Run in Debug Mode.

-v, --verbose
  Verbose - See verbose messaging.

--acceptlicense, --accept-license
  AcceptLicense - Accept license dialogs automatically.

-y, --yes, --confirm
  Confirm all prompts - Chooses default answer instead of prompting.
  Implies --accept-license

-f, --force
  Force - force the behavior

--noop, --whatif, --what-if
  NoOp - Don't actually do anything.

-r, --limitoutput, --limit-output
  LimitOuptut - Limit the output to essential information

--execution-timeout=VALUE
  CommandExecutionTimeoutSeconds - Override the default execution
  timeout in the configuration of 2700 seconds.

-c, --cache, --cachelocation, --cache-location=VALUE
  CacheLocation - Location for download cache, defaults to %TEMP% or
  value in chocolatey.config file.

--allowunofficial, --allow-unofficial, --allowunofficialbuild,
--allow-unofficial-build
  AllowUnofficialBuild - When not using the official build you must set
  this flag for choco to continue.
```

### How To Pass Options / Switches

You can pass options and switches in the following ways:

 * `-` - except for normal switches (not 1 character) when using option name blending, see below.
 * `--` - except for one character switches
 * `+`
 * `/`
 * One character switches can be blended. e.g. `-d` (debug), `-f` (force), `-v` (verbose), and `-y` (confirm yes) can be blended as -dfvy, but you must use `--` for all other normal options/switches e.g. `--version` instead of `-version`. If you don't do this, you may have unintended results.
 * You can also include or not include an equals sign `=` between options and values. And quote the values.
