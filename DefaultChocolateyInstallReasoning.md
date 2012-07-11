## Default Chocolatey Installation
  
Lots of people tend to ask why the system root for an installation of chocolatey and why not Program Files or the User Path (C:\Users\username or C:\Documents and Settings\username).  
  
That's a great question. Let's get into use cases for chocolatey first.  
  
### Low Privilege Install and Use
Chocolatey should be able to be installed on a machine where the user does not have administrative rights.  A user with non-administrative access should be able to install (through one of the methods, possibly not the default) chocolatey and use it to install tools (non-administrative chocolatey packages, see [[tools versus applications at the bottom of FAQs|ChocolateyFAQs#AppVsTool]]) on their system.  
  
This use case is the hardest to accommodate when it comes to Windows and how it expects you to put new software on it.  
  
This immediately removes the ability to install to Program Files (although I'm not a fan of that location in the first place for things that are not actually "installed applications" on a system) since it requires something greater than Users group privileges to write to.

### Why not Program Files? Is it because you don't like spaces?
Two really big reasons:  
  
* Chocolatey is not an application that is installed through the registry on the machine
* Program Files is not writable with low user privileges
  
To me if something goes to the program files it also goes to the registry as an installed application, one that can be uninstalled from programs list in the control panel. To me having things in there that are not controlled through the system install registry is not clean so I wouldn't see chocolatey there.  
  
Program Files also requires higher privileges than the Users group to be able to write to the folder (at least on Windows 7, I didn't check all the others). Understanding low privilege install, this makes sense.    
  
### Why not User Path (C:\Users\someuser / C:\Documents and Settings\someuser)?
This actually seems the most reasonable place to install it since the environment variables are user variables. The reasoning against this location is:  
  
* It's a machine repository, not a user repository
* I use chocolatey to install applications to the system (not the user). 
* User2 would not be able to uninstall an application (with chocolatey) that User1 installed with chocolatey since they couldn't reach the directory.
* Some of the pathing could get very long (maybe beyond the limits of some older operating systems DOS) for the batch redirects.

### Why the System Root (C:\ on most computers)?
This is really terse but the number of folders one has to navigate to get to something useful is a pain we don't really know we have. If you think about the amount of folders you need to navigate just to get to some configuration file in program files (which is already 3 levels deep). Not that I always use folder navigation starting with "Computer."  
  
Some folks don't like anyone cluttering up system root, but really much of program files and other directories are already pretty cluttered.  
  
Chocolatey IMHO is a core component tool, which gives it the privilege to go into the root system folder. You may not agree with me and that's fine, that's why we provide the ChocolateyInstall variable so you can have it installed wherever you want.  
    
### Where would you install it?
Since Program Files and User Path are out, what's left? C:\Windows?  I wouldn't install it there. So logically all that is left is to either create a Tools directory (or something else at root) and put it under that or install it to the root itself. But creating the tools directory (whatever the name) doesn't really subvert the fact that I am still adding to the system root folder.    
  
But really, where would/do you install chocolatey?  
  