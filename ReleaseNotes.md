#Release Notes
  
###[0.9.8.17](https://github.com/chocolatey/chocolatey/issues?labels=v0.9.8.17&sort=created&direction=desc&state=closed&page=1)  
 * Enhancement - Support for naive uninstall - https://github.com/chocolatey/chocolatey/issues/96  
 * Enhancement - Sources specified through config (or nuget.config) - https://github.com/chocolatey/chocolatey/pull/101    
 * Enhancement - Use CygWin as a package source - https://github.com/chocolatey/chocolatey/pull/93  
 * Enhancement - Use Python as a package source (uses easy_install) - https://github.com/chocolatey/chocolatey/issues/100  
 * Enhancement - Use Default Credentials before Get-Credentials when using proxy on web call - https://github.com/chocolatey/chocolatey/pull/83
 * Enhancement - Reduce the verbosity of running chocolatey - https://github.com/chocolatey/chocolatey/issues/84
 * Enhancement - Add a -debug switch - https://github.com/chocolatey/chocolatey/issues/85  
 * Enhancement - Improve pipelining of cver by returning an object - https://github.com/chocolatey/chocolatey/pull/94  
 * Fix - Executable batch links not created for "prerelease" versions - https://github.com/chocolatey/chocolatey/issues/88
 * Fix - Issue where latest version is not returned - https://github.com/chocolatey/chocolatey/pull/92  
 * Fix - Prerelease versions now broken out as separate versions - https://github.com/chocolatey/chocolatey/issues/90
 * Fix - Build path with spaces now works - https://github.com/chocolatey/chocolatey/pull/102  
 * More notes coming...
  
###0.9.8.16
 * Small fix to installer for upgrade issues from 0.9.8.15  
  
###[0.9.8.15](https://github.com/chocolatey/chocolatey/issues?labels=v0.9.8.15&sort=created&direction=desc&state=closed&page=1)  
 * **Breaking change** : Enhancement - Chocolatey's default folder is now C:\Chocolatey (and no longer C:\NuGet) - https://github.com/chocolatey/chocolatey/issues/58  
 * **Breaking change** : Enhancement - Use -force to reinstall existing packages - https://github.com/chocolatey/chocolatey/issues/45  
 * Enhancement - Install now supports **all** with a custom package source to install every package from a source! - https://github.com/chocolatey/chocolatey/issues/46  
 * Enhancement - Support Prerelease flag for Install - https://github.com/chocolatey/chocolatey/issues/71  
 * Enhancement - Support Prerelease flag for Update/Version - https://github.com/chocolatey/chocolatey/issues/72  
 * Enhancement - Support Prerelease flag in List - https://github.com/chocolatey/chocolatey/issues/74  
 * Fix - Parsing the wrong version when trying to update - https://github.com/chocolatey/chocolatey/issues/73  
  
###[0.9.8.14](https://github.com/chocolatey/chocolatey/issues?labels=v0.9.8.14&sort=created&direction=desc&state=closed&page=1)  
 * Enhancement - Pass ValidExitCodes to Install Helpers - https://github.com/chocolatey/chocolatey/issues/54  
 * Enhancement - Add 64-bit url to Install-ChocolateyZipPackage - https://github.com/chocolatey/chocolatey/issues/48  
 * Enhancement - Add 64-bit url to Install-ChocolateyPowershellCommand - https://github.com/chocolatey/chocolatey/issues/57  
 * Enhancement - Make the main helpers work with files not coming over HTTP - https://github.com/chocolatey/chocolatey/issues/51  
 * Enhancement - Upgrade NuGet.exe to 1.6.0 to take advantage of prerelease packaging - https://github.com/chocolatey/chocolatey/issues/64  
 * Fix - The packages.config feature has broken naming packages with '.config' - https://github.com/chocolatey/chocolatey/issues/56  
 * Fix - CList includes all versions without adding the switch - https://github.com/chocolatey/chocolatey/issues/60  
 * Fix - When NuGet.exe failes to run due to .NET Framework 4.0 not installed, chocolatey should report that. - https://github.com/chocolatey/chocolatey/issues/65  
  
###[0.9.8.13](https://github.com/chocolatey/chocolatey/issues?labels=0.9.8.13&sort=created&direction=desc&state=closed&page=1)  
 * New Command! Enhancement - Integration with Ruby Gems (`cgem packageName` or `cinst packageName -source ruby`) - https://github.com/chocolatey/chocolatey/issues/29  
 * New Command! Enhancement - Integration with Web PI (`cwebpi packageName` or `cinst packageName -source webpi`) - https://github.com/chocolatey/chocolatey/issues/28  
 * Enhancement - Call chocolatey install with packages.config file (thanks AnthonyMastrean!) - https://github.com/chocolatey/chocolatey/issues/31 and https://github.com/chocolatey/chocolatey/pull/43 and https://github.com/chocolatey/chocolatey/issues/50  
 * New Command! Enhancement - Chocolatey Push (`chocolatey push packageName.nupkg` or `cpush packageName.nupkg`) - https://github.com/chocolatey/chocolatey/issues/36  
 * New Command! Enhancement - Chocolatey Pack (`chocolatey pack [packageName.nuspec]` or `cpack [packageName.nuspec]`) - https://github.com/chocolatey/chocolatey/issues/35  
 * Enhancement - @datachomp feature - Override Installer Arguments `chocolatey install packageName -installArgs "args to override" -override` or `cinst packageName -ia "args to override" -o`) - https://github.com/chocolatey/chocolatey/issues/40  
 * Enhancement - @datachomp feature - Append Installer Arguments (`chocolatey install packageName -installArgs "args to append"` or `cinst packageName -ia "args to append"`) - https://github.com/chocolatey/chocolatey/issues/39  
 * Enhancement - Run installer in not silent mode (`chocolatey install packageName -notSilent` or `cinst packageName -notSilent`) - https://github.com/chocolatey/chocolatey/issues/42  
 * Enhancement - List available Web PI packages (`clist -source webpi`) - https://github.com/chocolatey/chocolatey/issues/37  
 * Enhancement - List command should allow the All or AllVersions switch - https://github.com/chocolatey/chocolatey/issues/38  
 * Enhancement - Any install will create the ChocolateyInstall environment variable so that installers can take advantage of it - https://github.com/chocolatey/chocolatey/issues/30  
 * Fixing an issue on proxy display message (Thanks jasonmueller!) - https://github.com/chocolatey/chocolatey/pull/44  
 * Fixing the source path to allow for spaces (where chocolatey is installed) - https://github.com/chocolatey/chocolatey/issues/33  
 * Fixing the culture to InvariantCulture to eliminate the turkish "I" issue - https://github.com/chocolatey/chocolatey/issues/22  
  
###0.9.8.12
 * Enhancement - Reducing the number of window pop ups - https://github.com/chocolatey/chocolatey/issues/25  
 * Fixed an issue with write-host and write-error overrides that happens in the next version of powershell - https://github.com/chocolatey/chocolatey/pull/24  
 * Fixing an issue that happens when powershell is not on the path - https://github.com/chocolatey/chocolatey/issues/23  
 * Fixing the replacement of capital ".EXE" in addition to lowercase ".exe" when creating batch redirects - https://github.com/chocolatey/chocolatey/issues/26  
  
###0.9.8.11
 * Fixing an update issue if the package only exists on chocolatey.org - https://github.com/chocolatey/chocolatey/issues/16  
 * Fixing an issue with install missing if the package never existed - https://github.com/chocolatey/chocolatey/issues/13  
        
###0.9.8.10
 * New Helper! Install-ChocolateyPowershellCommand - install a powershell script as a command - https://github.com/chocolatey/chocolatey/issues/11  
      
###0.9.8.9
 * Reinstalls an existing package if -version is passed (first surfaced in 0.9.8.7 w/NuGet 1.5) - https://github.com/chocolatey/chocolatey/issues/9  
    
###0.9.8.8
 * Fixing version comparison - https://github.com/chocolatey/chocolatey/issues/4  
 * Fixed package selector to not select like named packages (i.e. ruby.devkit when getting information about ruby) - https://github.com/chocolatey/chocolatey/issues/3  
    
###0.9.8.7
 * Added proxy support based on https://github.com/chocolatey/chocolatey/issues/1  
 * Updated to work with NuGet 1.5 - https://github.com/chocolatey/chocolatey/issues/2  
  
###0.9.8.6
 * Fixed a bug introduced in 0.9.8.5 - Start-ChocolateyProcessAsAdmin erroring out when setting machine path as a result of trying to log the message.  
  
###0.9.8.5
 * Improving Run-ChocolateyProcessAsAdmin to allow for running entire functions as administrator by importing helpers to that command if using PowerShell.
 * Fixed bug in installer when User Environment Path is null.  
 * Updating some of the notes.
   
###0.9.8.4
 * Fixed a small issue with the Install-ChocolateyDesktopLink
  
###0.9.8.3
 * NuGet updated to v1.4
 * New chocolatey command! InstallMissing allows you to install a package only if it is not already installed. Shortcut is 'cinstm'.
 * Much of the error handling is improved. There are two new Helpers to call (ChocolateySuccess and Write-ChocolateyFailure).
 * New Helper! Install-ChocolateyPath - give it a path for out of band items that are not imported to path with chocolatey 
 * New Helper! Start-ChocolateyProcessAsAdmin - this allows you to run processes as administrator
 * New Helper! Install-ChocolateyDesktopLink - put shortcuts on the desktop
 * chocolatey no longer needs administrative rights to install itself.
 * chocolatey no longer runs the entire powershell script as an administrator. With the addition of the Start-ChocolateyProcessAsAdmin, this is how you will get to administrative tasks outside of the helpers.
  
###0.9.8.2
 * You now have the option of a custom installation folder. Thanks Jason Jarrett!
  
###0.9.8.1
 * General fix to bad character in file. Fixed selection for update as well.  
 
###0.9.8
 * Shortcuts have been added: 'cup' for 'chocolatey update', 'cver' for 'chocolatey version', and 'clist' for 'chocolatey list'.
 * Update only runs if newer version detected.
 * Calling update with no arguments will update chocolatey.
 * Calling update with all will update your entire chocolatey repository.
 * A dependency will not reinstall once it has been installed. To have it reinstall, you can install it directly (or delete it from the repository and run the core package).  
  
###0.9.7.3 
 * Fixing Install-ChocolateyZipPackage so that it works again.
  
###0.9.7.2 
 * Fixing an underlying issue with not having silent arguments for exe files.  
 
###0.9.7.1 
 * Fixing an introduced bug where the downloader didn't get the file name passed to it.  
  
###0.9.7
 * New helper added Install-ChocolateyInstallPackage - this was previously part of the download & install and has been broken out.
 * The powershell module is automatically loaded, so packages no longer need to import the module. This means one line chocolateyInstall.ps1 files!
 * Error handling is improved.
 * Silent installer override for msi has been removed to allow for additional arguments that need to be passed.
 * New chocolatey command! Version allows you to see if a package you have installed is the most up to date. Leave out package and it will check for chocolatey itself.
  
###0.9.6.4
 * remove powershell execution timeout
  
###0.9.6.3
 * New Helper added Install-ChocolateyZipPackage - this wraps the two upper commands into one smaller command and addresses the file name bug.
  
###0.9.6.2
 * Addressed a small bug in getting back the file name from the helper.
  
###0.9.6.1
 * Adding in ability to find a dependency when the version doesn't exist.
  
###0.9.6
 * Can execute powershell and chocolatey without having to change execution rights to powershell system wide.
 * New Helper added - Get-ChocolateyWebFile - downloads a file from a url and gives you back the location of the file once complete.
 * New Helper added - Get-ChocolateyZipContents - unzips a file to a directory of your choosing.
  
###0.9.5 
 * Helper for native installer added (Install-ChocolateyPackage). Reduces the amount of powershell necessary to download and install a native package to two lines from over 25.
 * Helper outputs progress during download.
 * Dependency runner is complete
  
###0.9.4 
 * List command has a filter.
 * Package license acceptance terms notated
  
###0.9.3 
 * You can now pass -source and -version to install command.
  
###0.9.2
 * List command added.  
  
###0.9.1 
 * Shortcut for 'chocolatey install' - 'cinst' now available.