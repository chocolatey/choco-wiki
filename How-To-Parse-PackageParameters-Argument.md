When installing a Chocolatey Package, it is possible to use a number of arguments to control how the package is installed.  Each one of these arguments is detailed [here](https://github.com/chocolatey/choco/wiki/CommandsInstall).

This _How-To_ focuses on how a package creator can make use of the PackageParameters argument within their package, and how they can parse the string which is passed through into their package from the installation command.

## Code Sample

```powershell

  $arguments = @{}

  # Let's assume that the input string is something like this, and we will use a Regular Expression to parse the values
  # /Port:81 /Edition:LicenseKey /AdditionalTools

  # Now we can use the $env:chocolateyPackageParameters inside the Chocolatey package
  $packageParameters = $env:chocolateyPackageParameters

  # Default the values
  $port = "81"
  $edition = "LicenseKey"
  $additionalTools = $false
  $installationPath = "c:\temp"

  # Now parse the packageParameters using good old regular expression
  if ($packageParameters) {
      $match_pattern = "\/(?<option>([a-zA-Z]+)):(?<value>([`"'])?([a-zA-Z0-9- _\\:\.]+)([`"'])?)|\/(?<option>([a-zA-Z]+))"
      $option_name = 'option'
      $value_name = 'value'

      if ($packageParameters -match $match_pattern ){
          $results = $packageParameters | Select-String $match_pattern -AllMatches
          $results.matches | % {
            $arguments.Add(
                $_.Groups[$option_name].Value.Trim(),
                $_.Groups[$value_name].Value.Trim())
        }
      }
      else
      {
          Throw "Package Parameters were found but were invalid (REGEX Failure)"
      }

      if ($arguments.ContainsKey("Port")) {
          Write-Host "Port Argument Found"
          $port = $arguments["Port"]
      }

      if ($arguments.ContainsKey("Edition")) {
          Write-Host "Edition Argument Found"
          $edition = $arguments["Edition"]
      }

      if ($arguments.ContainsKey("AdditionalTools")) {
          Write-Host "You want Additional Tools installed"
          $additionalTools = $true
      }

      if ($arguments.ContainsKey("InstallationPath")) {
          Write-Host "You want to use a custom Installation Path"
          $installationPath = $arguments["InstallationPath"]
      }
  } else {
      Write-Debug "No Package Parameters Passed in"
  }

  $silentArgs = "/S /Port:" + $port + " /Edition:" + $edition + " /InstallationPath:" + $installationPath
  if ($additionalTools) { $silentArgs += " /Additionaltools" }

  Write-Debug "This would be the Chocolatey Silent Arguments: $silentArgs"
```

## What does this mean?

The Code Sample assumes that there will be no PackageParameters passed into it, as a result, we need to define a number of default values for each of the variables contained within the script.  In this case, the ```port```, the ```edition```, the ```additionalTools``` and the ```installationPath```.

Once that is done, assuming that the PackageParameters contains "something", use a Regular Expression to parse each of the values into a dictionary.  Here, we are assuming that the package parameters will come through in a pre-defined format, such as ```/Port:82 /Edition:LicenseKey1 /AdditionalTools /InstallationPath:'C:\temp\folder with spaces'```.  Now, this format can be anything you want it to be.  What is shown here is just **one** way of doing it.  If you need to deviate from this sample structure, it is likely that you will need to update the regular expression to account for this.

Having collected all the arguments into the dictionary, we can then inspect the values of each parameter that we are interested in.  If it exists in the dictionary, replace the corresponding default value, otherwise, continue to use the default value.

## Add Package Parameter Information to the Description
Be sure to let folks know about the package parameters (**Note:** this will be a holding review item by moderators).

Here's an example:

```xml
    <description>
Git (for Windows) - Git is a powerful distributed Source Code Management tool. If you just want to use Git to do your version control in Windows, you will need to download Git for Windows, run the installer, and you are ready to start.

Note: Git for Windows is a project run by volunteers, so if you want it to improve, volunteer!

### Package Specifics
The package uses default install options minus cheetah integration and desktop icons. Cheetah prevents a good upgrade scenario, so it has been removed.

#### Package Parameters
The following package parameters can be set:

 * `/GitOnlyOnPath` - this puts gitinstall\cmd on path. This is also done by default if no package parameters are set.
 * `/GitAndUnixToolsOnPath` - this puts gitinstall\bin on path. This setting will override `/GitOnlyOnPath`.
 * `/NoAutoCrlf` - this setting only affects new installs, it will not override an existing `.gitconfig`. This will ensure 'Checkout as is, commit as is'

These parameters can be passed to the installer with the use of `-params`.
For example: `-params '"/GitAndUnixToolsOnPath /NoAutoCrlf"'`.
    </description>
```

## Installing the Package
Now, in this example, if we were to call:

```choco install <packageName>```

The output would be:

```
This would be the Chocolatey Silent Arguments: /S /Port:81 /Edition:LicenseKey /InstallationPath:c:\temp
```

i.e. it is using the default values which we made at the top of the file

However, if we instead used:

```
choco install <packageName> -packageParameters '"/Port:82 /Edition:LicenseKey1 /InstallationPath:""C:\temp\folder with space"" /AdditionalTools"'
```
Keep in mind how to pass pkg args: https://github.com/chocolatey/choco/wiki/CommandsReference#how-to-pass-options--switches


The output would be:

```
This would be the Chocolatey Silent Arguments: /S /Port:82 /Edition:LicenseKey1 /InstallationPath:"C:\temp\folder with space" /Additionaltools
```