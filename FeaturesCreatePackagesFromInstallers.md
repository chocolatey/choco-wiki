# Package Builder - Create Packages Automatically From Installers (Business Editions Only)

Creating packages is a pretty quick process as compared to manually installing software on multiple machines. There is still some time involved to create a package. Even the best packagers take between 5-10 minutes to create a package. Chocolatey's Package Builder creates high quality packages in 5-10 seconds when pointed to native installers and zips! You can even point package builder to both a 32-bit and 64-bit url and seconds later you have a fully functioning package using all local/embedded resources!

Chocolatey for Business is able to inspect an installer and determine silent arguments and complete packaging components for you, saving you hours of time in packaging and maintaining software!

## Usage

When calling `choco new`, just add `--file=value` to point to a native installer and Chocolatey for Business will automatically determine the silent arguments, create the packaging and wrap that all around the installer.

![Create Packages from Installers - if you are on https://chocolatey.org/docs/features-create-packages-from-installers, see commented html below for detailed description of image](images/features/features_packages_from_installers.png)

<!--
Text in the image above:

Chocolatey for Business automatically creates all packaging from an installer!

Create Packages for Installers In Seconds

- What used to take minutes or hours, is now done in seconds.
- Allows customization
- Automatically create packages for your entire organization's file share in under 15 minutes, saving you weeks of work.

This image shows running `choco new --file .\installers\1Password-4.6.0.598.exe`.

-->

## See It In Action

![auto package creation/synchronize](images/gifs/choco_business_features.gif)

## Options and Switches

When running `choco new` in the Business editions, pass the following:

~~~sh
     --file, --url=VALUE
     Inspect a file (native installer, zip, patch/upgrade file, or remote url
       to download first) to to completely create a package with proper silent
       arguments! Can be 32-bit or 64-bit architecture.  Available in Business
       editions only (licensed version 1.4.0+, url/zip starting in 1.6.0). See
       https://chocolatey.org/docs/features-create-packages-from-installers

     --file64, --url64=VALUE
     Optional - used when specifying both a 32-bit and a 64-bit file. Can be
       an installer or a zip, or remote url to download. Available in Business
       editions only (licensed version 1.6.0+).

     --keepremote, --originallocation, --original-location, --useoriginallocation, --use-original-location, --useoriginalfileslocation, --use-original-files-location
     Use Original Files Location - when using file or url, use the original
       location in packaging. Available in Business editions only (licensed
       version 1.6.0+).

     --checksum, --downloadchecksum, --download-checksum=VALUE
     Download Checksum - checksum to verify File/Url with. Defaults to empty.
       Available in Business editions only (licensed version 1.7.0+).

     --checksum64, --checksumx64, --downloadchecksumx64, --download-checksum-x64=VALUE
     Download Checksum 64-bit - checksum to verify File64/Url64 with.
       Defaults to empty. Available in Business editions only (licensed version
       1.7.0+).

     --checksumtype, --checksum-type, --downloadchecksumtype, --download-checksum-type=VALUE
     Download Checksum Type - checksum type for File/Url (and optional
       separate 64-bit files when specifying both). Used in conjunction with
       Download Checksum and Download Checksum 64-bit. Available values are
       'md5', 'sha1', 'sha256' or 'sha512'. Defaults to 'sha256'.  Available in
       Business editions only (licensed version 1.7.0+).

     --pauseonerror, --pause-on-error
     Pause on Error - Pause when there is an error with creating the package.
       Available in Business editions only (licensed version 1.7.0+).

     --buildpackage, --build-package
     Build Package - Attempt to compile the package after creating it.
       Available in Business editions only (licensed version 1.7.0+).
~~~

## FAQ

### How do I take advantage of Package Builder?
You must have a [Business edition of Chocolatey](https://chocolatey.org/compare). Business editions are great for organizations that need to manage the total software management lifecycle.

### I'm a licensed customer, now what?
When you run `choco new`, you can add `--file` and point Chocolatey at the installer file and let Chocolatey figure out everything for creating a package and the silent arguments and wrap that around the installer.

### How does it work?
It inspects the installer file using a series of rules that helps determine the installer type. From there it builds a package with the information it is able to extract from the installer.

### What types of extensions are supported?

Package Builder supports `.exe`, `.msi`, `.msu`, `.msp`, `.7z`, and `.zip`.

### Will it catch all types of installers?
It is able to detect 12 types of installers currently.

### Does it always find the silent uninstall arguments?
In over 90% of the cases it will, but not always. Even with this in mind, it still removes 90% of the packaging work. See the next question on how to handle cases where it can not.

### This was unable to detect custom installer arguments.
Unfortunately, some installers out there are just a pain to work with. In the case of custom installers, you may be able to find the silent arguments for install and uninstall by searching online. If you can not find anything, consider unpacking the installer and putting the software binaries directly in the package.

### Does it create auto unattend files?
Unfortunately, it is not able to do this. See the [[automatic internalize and recompile packages feature|FeaturesAutomaticallyRecompilePackages]] to take advantage of thousands of existing packages without a need for internet access.

### Does it work with zip archive?
Yes, but somewhat naively. It will generate the packaging to unpack the archive for both 7z and zip files.

### Does this work with keeping the installer on a UNC share or elsewhere yet?
Yes, as of Licensed version v1.6.0+. Use `--use-original-location`.
