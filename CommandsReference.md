# Command Reference

This is a listing of all of the different things you can pass to choco.

## Commands

 * [[list / search |CommandsList]] - searches and lists remote or local packages
 * [[install|CommandsInstall]] - installs packages from various sources
 * installmissing - **REMOVED**
 * update - **DEPRECATED** - RESERVED for future use (You are looking for upgrade, these are not the droids you are looking for)
 * [[upgrade|CommandsUpgrade]] - upgrades packages from various sources
 * version - **DEPRECATED** - use `choco upgrade pkgname --noop` instead
 * [[uninstall|CommandsUninstall]] - uninstalls a package
 * help - **REMOVED** - use `-h` on any command.

## Commands (intermediate to advanced)
 * [[source / sources|CommandsSources]] - view and configure default sources
 * [[apikey|CommandsApiKey]] - retrieves or saves an apikey for a particular source
 * [[pin|CommandsPin]] - suppress upgrades to a package

## Package Creation Commands
 * [[new|CommandsNew]] - generates files necessary for a Chocolatey package
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

## How To Pass Options / Switches

You can pass options and switches in the following ways:

 * `-` - except for normal switches (not 1 character) when using option 
   bundling, see below.
 * `--` - except for one character switches
 * `+`
 * `/`
 * **Option Bundling / Bundled Options**: One character switches can be
   bundled. e.g. `-d` (debug), `-f` (force), `-v` (verbose), and `-y` 
   (confirm yes) can be bundled as `-dfvy`.
 * **Use Equals**: You can also include or not include an equals sign 
   `=` between options and values. And quote the values.
 * Options and switches apply to all items passed, so if you are 
   installing multiple packages, and you use `--version=1.0.0`, choco 
   is going to look for and try to install version 1.0.0 of every 
   package passed. So please split out multiple package calls when 
   wanting to pass specific options.
 * When you need to quote things, such as when using spaces, please use
   single quote marks (`'`). In cmd.exe, you can also use double double
   quotes (i.e. `""""yo""""`). This is due to the hand off to PowerShell - 
   it seems to strip off the outer set of quotes. TODO: TEST THIS, MAY 
   NOT BE RELEVANT NOW.


