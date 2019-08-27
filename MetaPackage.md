# Metapackages in Chocolatey

Chocolatey has the concept of a "meta" package. These types of packages don't typically include any sort of logic that would leverage installation, upgrade, and uninstallation scripts. Alternatively, they provide a means to bundle _existing_ packages as dependencies, giving you a single package to install, which in turn will install all of those subsequent dependent packages.

Controlling what packages you wish to include is handled entirely in the dependencies section of a nuspec file. In the following sections, we will work through the creation of a meta package.

## Creating your metapackage

Open up an administrative Powershell window and issue the following commands:

```powershell

#configure a location for your metapackage
mkdir C:\packages

#enter the newly created directory
Set-Location C:\packages

#Create a new package
choco new -n metapackage

```

This sequence of commands will create a new C:\packages folder, and scaffold out a package named "metapackage" inside of that directory

Let's clean up this newly scaffolded package. Issue the following command:

```powershell

Remove-Item C:\packages\metapackage\tools -Recurse -Force

```

This will remove the tools folder and all the files inside of it, since we don't need them. 
Next, open up the `metapackage.nuspec` file in your favorite editor. I'll be using VSCode moving forward.

```powershell

code C:\packages\metapackage\metapackage.nuspec

```

To make things a little easier, just replace the code inside of this file with the following:

```xml

<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2015/06/nuspec.xsd">
  <metadata>
    <id>metapackage</id>
    <version>0.1.0</version>
    <!-- <packageSourceUrl>Where is this Chocolatey package located (think GitHub)? packageSourceUrl is highly recommended for the community feed</packageSourceUrl>-->
    <!-- owners is a poor name for maintainers of the package. It sticks around by this name for compatibility reasons. It basically means you. -->
    <!--<owners>__REPLACE_YOUR_NAME__</owners>-->
    <title>metapackage (Install)</title>
    <authors>Your Name Here</authors>
    <tags>metapackage SPACE_SEPARATED</tags>
    <summary>Installs included packages onto a system</summary>
    <description>
        #Chocolatey MetaPackage Installer

        Installs the following applications:

        - Google Chrome
        - Foxit Reader
        - VLC Media Player
    </description>
    <!-- Specifying dependencies and version ranges? https://docs.nuget.org/create/versioning#specifying-version-ranges-in-.nuspec-files -->
    <dependencies>
      <dependency id="googlechrome"  />
      <dependency id="foxitreader"  />
      <dependency id="vlc"  />
    </dependencies>
  </metadata>
</package>

```

In our example package, we are going to be installing Google Chrome, Foxit Reader, and VLC at their latest versions available from our internal repository.
In your environment, you may use any packages you have available in your internal repositories. You may also specify strict version requirements for each package. The versioning follows the NuGet standard, which you can reference [here](https://docs.microsoft.com/en-us/nuget/concepts/package-versioning#version-ranges-and-wildcards).

## Packaging your metapackage

Once you have all of the packages you require for your metapackage to be complete, we can pack it to create the package nupkg. Issue the following command:

```powershell
#Enter the package files directory
Set-Location C:\packages\metapackage

#Pack the package
choco pack metapackage.nuspec

```

## Pushing your package

Next, let's push this up to our internal repo

```powershell

#Enter the package files directory
Set-Location C:\packages\metapackage

#Push the package
choco push metapackage.0.1.0.nupkg -s $YourRepositoryUrl

```

## Testing your package

Now that we have our nupkg generated, we can test it:

```powershell

choco install metapackage -y

```

You should see that all 3 of our packages are installed on the system.

## Uninstalling metapackages

Thought must be taken when you wish to remove a metapackage. By default issuing a `choco uninstall metapackage` will _not_ remove all of the dependencies installed by the package. In order to uninstall the metapackage, and all of its dependencies, use the `-x` or `--remove-depenedencies` command line argument.

We'll uninstall the metapackage and all of its dependencies next.

```powershell

choco uninstall metapackage -x

```

## Wrapping up

If you've followed along with this document, you should have successfully created a metapackage and worked through the installation and uninstallation of that package. If you are stuck, our awesome community is here to help. Join the conversation on [Gitter](https://gitter.im/chocolatey/choco). Or, if you are a business customer, reach out through our commercial support channels.

Chocolatey metapackages....they're sweet!