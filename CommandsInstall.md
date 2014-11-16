## Chocolatey Install (cinst / choco install)
Installs a package or a list of packages in a packages.config.
`choco install packageName` or shortcut with
`cinst packageName` or `cinst packages.config`

##Multiple installs  - v0.9.8.21+
`choco install pkg1 pkg2 pkgN`


###Workaround for earlier versions of chocolatey:

 * PowerShell - `'pkg1','pkg2','pkgN' | %{ cinst $_ }`
 * Cmd Shell - `FOR %%G IN (pkg1, pkg2, pkgN) DO (cinst %%G)`


##Parameters
###PackageName
Name of package to install. May be repeated.

#### All (special PackageName keyword) - v0.9.8.15+
This allows you to keep your packages in a feed somewhere and maintain that feed over maintaining a file.

When this is specified you must also pass in a source that is not MS official or Chocolatey official as the hit would likely be way too big.

Example `choco install all -source http://myget.org/somefeed`

###Packages.config - v0.9.8.13+
Alternative to PackageName. This is a list of packages in an xml manifest for chocolatey to install.  This is like the packages.config that NuGet uses except it also adds the source element. This can also be the path to the `packages.config` file if it is not in the current working directory.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="apackage" />
  <package id="anotherPackage" version="1.1" />
  <package id="chocolateytestpackage" version="0.1" source="somelocation" />
</packages>
```

###Prerelease flag (optional) - v0.9.8.15+
Whether to include prerelease packages in results.
This is optional if you explicitly ask for a specific version that is a pre-release package. You can pass this as `-pre` or `-prerelease`. If you use this when installing multiple packages, all specified packages will install a prerelease if there is one available.

Defaults to false.

###Version (optional)
The version of the package to install. Do not use this when installing multiple packages.

Defaults to the latest version available.

###Source (optional)
Source (directory, share or remote url feed) the package comes from. You can specify multiple sources by separating with a comma and single quotes surrounding source. If you use source for multiple packages, watch out for interesting results.

Example `-source 'http://chocolatey.org/api/v2/;http://someother/feed/'`

Defaults to official chocolatey feed.

#### -source ruby (v0.9.8.13+)
This specifies the source is Ruby Gems and that we are installing a gem. If you do not have ruby installed prior to running this command, the command will install that first.

#### -source webpi (v0.9.8.13+)
This specifies the source is Web PI and that we are installing a WebPI product, such as IISExpress. If you do not have the Web PI command line installed, it will install that first and then the product requested.

#### -source cygwin (v0.9.8.17+)
This specifies the source is Cygwin and that we are installing a cygwin package, such as bash. If you do not have Cygwin installed, it will install that first and then the product requested.

#### -source python (v0.9.8.17+)
This specifies the source is Python and that we are installing a python package, such as Sphinx. If you do not have easy_install and Python installed, it will install that first and then the product requested.

### PackageParameters - v0.9.8.22+
Parameters that you want to pass to the package (if the package accepts these). You can pass this as `-params` `-parameters` or `-packageparameters`.

**NOTE:** There are some subtle things to watch out for here when trying to pass a delimited string of variables into the Chocolatey Package.  For example, Powershell strips off double quotes, so if you need to pass double quotes for values, you should `'value1=''some value'' '` using two single quotation marks instead of a `"`. Chocolatey will convert this back to double quotes (e.g. `value1="some value"` for the above).

For package creators: You would pick this up as `$env:chocolateyPackageParameters` and expect it to be a string that you need to parse.

A complete example of how you **could** do this is shown here:

```powershell

$arguments = @{};

# Let's assume that the input string is something like this, and we will use a Regular Expression to parse the values
# /Port:81 /Edition:LicenseKey

# Now, we can use the $env:chocolateyPackageParameters inside the Chocolatey package
$packageParameters = $env:chocolateyPackageParameters;

# Default the values
$port = "81";
$edition = "LicenseKey";

# Now, let’s parse the packageParameters using good old regular expression
if($packageParameters) {
    $MATCH_PATTERN = "/([a-zA-Z]+):([`"'])?([a-zA-Z0-9- _]+)([`"'])?"
    $PARAMATER_NAME_INDEX = 1
    $VALUE_INDEX = 3
    
    if($packageParameters -match $MATCH_PATTERN ){
        $results = $packageParameters | Select-String $MATCH_PATTERN -AllMatches 
        $results.matches | % { 
          $arguments.Add(
              $_.Groups[$PARAMATER_NAME_INDEX].Value.Trim(),
              $_.Groups[$VALUE_INDEX].Value.Trim()) 
      }
    }     
    
    if($arguments.ContainsKey("Port")) {
        Write-Host "Port Argument Found";
        $port = $arguments["Port"];
    }  
    
    if($arguments.ContainsKey("Edition")) {
        Write-Host "Edition Argument Found";
        $edition = $arguments["Edition"];
    }
} else {
    Write-Host "No Package Parameters Passed in";
}

$silentArgs = "/S /Port=" + $port + " /Edition=" + $edition;

Write-Host "This would be the Chocolatey Silent Arguments: $silentArgs"
```

Now, in this example, if we were to call:

```choco install <packageName>```

The output would be:

```
This would be the Chocolatey Silent Arguments: /S /Port=81 /Edition=LicenseKey
```

i.e. it is using the default values which we made at the top of the file

However, if we instead used:

```
choco install <packageName> -packageParameters "/Port:82 /Edition=LicenseKey1"
```

The output would be:

```
This would be the Chocolatey Silent Arguments: /S /Port=82 /Edition=LicenseKey1
```

This parameters defaults to ''.

###InstallArguments (optional) - v0.9.8.13+
Install arguments that you want to pass to the native installer (if you have some custom ones that you know). By default this appends to the items already passed, unless you also pass `-overrideArguments`. You can pass this as `-ia` `-installArgs` or `-installArguments`.

**NOTE:** You should pass this as `'/value1 /value2'`. Powershell strips off double quotes so if you need to pass double quotes for values, you should `'/value1=''some value'' '` using two single quotation marks instead of a `"`. Chocolatey will convert this back to double quotes (e.g. `/value1="some value"` for the above).

For example, one may want to override the default installation directory of a piece of software. See https://github.com/chocolatey/chocolatey/wiki/GettingStarted#overriding-default-install-directory-or-other-advanced-install-concepts.

Defaults to ''.

###OverrideArguments flag (optional) - v0.9.8.13+
If you want to override the original install arguments (for the native installer) in the package and use your own. Use with InstallArguments.
You can pass this as `-o` `-override` `-overrideArgs` or `-overrideArguments`.

Defaults to false.

###NotSilent flag (optional) - v0.9.8.13+
If you want to use the native installer to step through the installer, use `-notSilent` to have chocolatey download the package and installer and bring it up for you.

Defaults to false.

###IgnoreDependencies flag (optional) - 0.9.8.21+
If you want to install something but ignore all of the dependencies, use `-ignoreDependencies` to force chocolatey to only install the package and not any of its dependencies.

Defaults to false.

###Forcex86 flag (optional) - v0.9.8.22+
If you want to install the 32 bit version of a package, you can pass `-x86` and chocolatey will ignore the 64 bit url and only use the 32 bit url. This only applies on an x64 system when you are installing packages that also have x64 versions. You can pass this as `-x86` or `-forcex86`.

Defaults to false.

##Examples
`choco install nunit`

`choco install nunit -version 2.5.7.10213`

`choco install nunit -version 2.5.7.10213 -source http://somelocalfeed.com/nuget`

`cinst nunit -version 2.5.7.10213 -source http://somelocalfeed.com/nuget`

`cinst nunit -source \\someserver\someshare`

`cinst nunit -source c:\somefolder`

`cinst nunit -source 'http://chocolatey.org/api/v2/;c:\somefolder'`

`cinst nodejs.install -installArgs '/qb'`

`cinst nodejs.install -installArgs '/qb /bob=''a value''' -override` -note that the `/bob=''a value''` will be converted to `/bob="a value"`.

`cinst nodejs.install -notSilent`

`cinst packages.config`

`choco install git ruby python`

`choco install python -x86`

##Screenshots
Installing mSysGit silently:

![msysgit](images/msysgit.png "msysgit")
![msysgit install](images/msysgit2.png "msysgit install")

NHProf:

![nhprof](images/chocolateynhprofiler.png "nhprof")

Statlight:

![statlight](images/statlight.png "statlight")

Git-Tfs:

![git tfs](images/git-tfs.png "git tfs chocolatey")
![git tfs helper run](images/git-tfs2.png "git tfs chocolatey helper")

[[Command Reference|CommandsReference]]