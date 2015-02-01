#Install-ChocolateyDesktopLink
`Install-ChocolateyDesktopLink $targetFilePath`  
  
This is deprecated for `Install-ChocolateyShortcut`. (need docs on it though)

##Description
This adds a shortcut on the desktop to the specified file path.  
  
##Parameters
###$targetFilePath (important)
This is the location to the application/executable file that you want to add a shortcut to on the desktop.  
Example: `'C:\tools\NHibernateProfiler\nhprof.exe'`  
  
##Examples
`Install-ChocolateyDesktopLink 'C:\tools\NHibernateProfiler\nhprof.exe'`  
  
[[Helper Reference|HelpersReference]]  