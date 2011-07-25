#Release Notes
  
###0.9.8.4
 * Fixed a small issue with the Install-ChocolateyDesktopLink
  
###0.9.8.3
 * NuGet updated to v1.4
 * New chocolatey command! InstallMissing allows you to install a package only if it is not already installed. Shortcut is 'cinstm'.
 * Much of the error handling is improved. There are two new Helpers to call (ChocolateySuccess and Write-ChocolateyFailure).
 * New Helper! Install-ChocolateyPath - give it a path for out of band items that are not imported to path with chocolatey 
 * New Helper! Start-ChocolateyProcessAsAdmin - this allows you to run processes as administrator
 * New Helper! Install-ChocolateyDesktopLink - put shortcuts on the desktop
  
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

