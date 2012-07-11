## Default Chocolatey Installation
  
Lots of people tend to ask why the system root for an installation of chocolatey and why not Program Files or the User Path (C:\Users\username or C:\Documents and Settings\username).  
  
That's a great question. Let's get into use cases for chocolatey first.  
  
### Low Privilege Install and Use
Chocolatey should be able to be installed on a machine where the user does not have administrative rights.  A user with non-administrative access should be able to install (through one of the methods, possibly not the default) chocolatey and use it to install tools (non-administrative chocolatey packages, see [[tools versus applications at the bottom of FAQs|ChocolateyFAQs#AppVsTool]]) on their system.  
  
This use case is the hardest to accommodate when it comes to Windows and how it expects you to put new software on it.  
  
This immediately removes the ability to install to Program Files (although I'm not a fan of that location in the first place for things that are not actually "installed applications" on a system) since it requires something greater than Users group privileges to write to.

### Why not Program Files? Is it because you don't like spaces?
To me if something goes to the program files it also goes to the registry as an installed application, one that can be uninstalled from programs list in the control panel. To me having things in there that are not controlled through the system install registry is not clean so I wouldn't see chocolatey there.  
  
Program Files also requires higher privileges than the Users group to be able to write to the folder (at least on Windows 7, I didn't check all the others). Understanding low privilege install, this makes sense.    