#Install-ChocolateyDesktopLink
`Install-ChocolateyDesktopLink $targetFilePath`  
  
**NOTE:** This is deprecated in favour of [[Install-ChocolateyShortcut|HelpersInstallChocolateyShortcut]].

##Description
This adds a shortcut on the desktop to the specified file path.  
  
##Parameters
###$targetFilePath (important)
This is the location to the application/executable file that you want to add a shortcut to on the desktop.  
Example: `'C:\tools\NHibernateProfiler\nhprof.exe'`  
  
##Examples
`Install-ChocolateyDesktopLink 'C:\tools\NHibernateProfiler\nhprof.exe'`  
  
[[Helper Reference|HelpersReference]]  