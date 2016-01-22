# How To Install or Upgrade a Package Without Running Scripts

So you've updated your applications manually or through some other means or you want to make Chocolatey aware of some previously installed applications without actually running the installation scripts.  

**NOTE**: This is considered an advanced scenario - make sure you read and understand everything here before trying it. You may mess something up if you don't understand the full implication of what you are doing.

## Installing a Package Without Running Install Script

The first thing to understand is that in most instances when you want to bring Chocolatey knowledgeable about an already installed application, if there is a version match out there (wherever your sources point), then this process is so much better. 

We normally recommend just calling the install as usual with no special switches and letting it handle the install - normally this turns into a no state change scenario for the computer (that is, the end state is that your application is still installed and Chocolatey now knows about it).

However if you are sold on skipping the automation scripts and tricking Chocolatey into believing that a script is installed.

If you wonder over to [[install switches|CommandsInstall#options-and-switches]] you will see 

~~~
-n, --skippowershell, --skip-powershell
  Skip Powershell - Do not run chocolateyInstall.ps1. Defaults to false.
~~~

This switch is going to be very important for you. Now we will go through some scenarios to help you make the best decision.

### Community Feed has a package with your version
Consider the following scenario:

* You have Firefox version 1.0.0 installed
* Your local Chocolatey doesn't have a package installed
* You see Chocolatey.org (the community feed) has a version of Firefox 1.0.0 (package id is firefox). It may or may not be the latest version.

Your command could be

`choco install firefox -n -y --version 1.0.0`

**Recommendation**: You should try to just install the package first and see what happens. In some cases the package may do additional things like update the path or do some helpful configuration for you (although this is not usually the case). If it fails or doesn't work, you can always fall back to skipping the install script as in the command above.

### Community feed has newer version than you
Consider the following scenario:

* You have Firefox version 1.0.0 installed.
* Your local Chocolatey doesn't have a package installed.
* You see Chocolatey.org (the community feed) has a newer version of Firefox 2.0.0 (package id is firefox) as the latest version.

**Recommendation**: You should run the regular install for Firefox and allow Chocolatey to upgrade the installed version. Check the package files first to be sure it is the same type of install (native installer versus zip archives). You can download the package and review the install script. You can also use `--noop` as part of your install arguments to see the script.

### Community Feed has a package older than your version
Consider the following scenario:

* You have Firefox version 2.0.0 installed.
* Your local Chocolatey doesn't have a package installed.
* You see Chocolatey.org (the community feed) has a latest version of Firefox 1.8.0 (package id is firefox). 
* There is no newer version than this available.

Your command could be:

`choco install firefox -n -y --version 1.8.0`

**Recommendation**: Ask the maintainer to update the package. Don't allow choco to know about an older version of a package. If the maintainer provides an update of 1.9.0 without also providing an update at or above your currently installed version, running upgrade later may put your installed software down to the version that is installed by the package. This is definitely not a situation you want to be in.


## Upgrade a Package Without Running Scripts
