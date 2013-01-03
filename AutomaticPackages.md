##Automatic Packages
Automatic packages are what set chocolatey apart from other package managers. 
http://chocolatey.org/packages/ChocolateyPackageUpdater  
  
##Chocolatey Package Updater aka chocopkgup  
The tool that accomplishes this process is known as chocopkgup (Chocolatey Package Updater). It is a free tool (unless you want to use it for uploads to not chocolatey.org).  
  
###Licensing
Check the license at http://realdimensions.net/licenses/chocolateypackageupdater/license.txt to be sure that it applies to you.  
  
Basically it boils down to this: if you want to use chocopkgup privately, you will need to pay for it. As long as you are publishing to chocolatey.org, the tool is completely free! The license does expire every once in awhile, but if you are keeping up on your chocolatey updates locally, you won't even notice (cup all, remember?).  
  
###Credits
This tool makes use of Ketarin and Nuget.exe. Both are awesome tools that help chocopkgup accomplish it's tasks.  
  
###Setup
More of this will become automated over time.  
  
1. Ensure you are using a source control repository and file system for keeping packages. A good example is here. 
1. Make sure you have installed the chocolatey package templates. If you’ve installed the chocolatey templates (ReadMe has instructions), then all you need to do is take a look at the chocolateyauto and chocolateyauto3. You will note this looks almost exactly like the regular chocolatey template, except this has some specially named token values. 
```powershell 
#Items that could be replaced based on what you call chocopkgup.exe with
#{{PackageName}} - Package Name (should be same as nuspec file and folder) |/p
#{{PackageVersion}} - The updated version | /v
#{{DownloadUrl}} - The url for the native file | /u
#{{PackageFilePath}} - Downloaded file if including it in package | /pp
#{{PackageGuid}} - This will be used later | /pg
#{{DownloadUrlx64}} - The 64bit url for the native file | /u64
```
1. These are the tokens that chocopkgup will replace when it generates an instance of a package. 
1. Install chocopkgup (which will install ketarin and nuget.commandline). `cinst chocolateypackageupdater`. 
1. Check the config in C:\tools\ChocolateyPackageUpdater\chocopkgup.exe.config. The PackagesFolder key should point to where your repository is located.
1. Create a scheduled task (in windows). This is the command (edit the path to cmd.exe accordingly): `C:\Windows\System32\cmd.exe /c c:\tools\chocolateypackageupdater\ketarinupdate.cmd` 
1. Choose a schedule for the task. I run mine once a day but you can set it to run more often. Choose a time when the computer is not that busy. 
1. Save the following Ketarin template somewhere: https://github.com/ferventcoder/chocolateyautomaticpackages/blob/master/_template/KetarinChocolateyTemplate.xml 
1. Open Ketarin. Choose File –> Settings. 
1. On the **General Tab** we are going to add the Version Column for all jobs. Click Add…, then put Version in Column name and {version} in Column value.  
![Ketarin Settings Custom](images/chocopkgup/KetarinShowCustomField.png "Ketarin Custom Field Setup")  
1. Click **[OK]**. This should add it to the list of Custom Columns.
1. Click on the **Commands Tab** and set **Edit command for event** to “Before updating an application”.  
![Ketarin Settings](images/chocopkgup/KetarinSettings.png "Ketarin Settings")    
1. Add the following text: 
```cmd
chocopkgup /p {appname} /v {version} /u {preupdate-url} /u64 {url64} /pp {file} 
REM /disablepush
```
1. Check the bottom of this section to be sure it set to **Command**.  
![Ketarin Settings Command](images/chocopkgup/KetarinCustomCommand.png "Ketarin Settings Command")
1. Click Okay. 
1. Note the commented out /disablepush. This is so you can create a few packages and test that everything is working well before actually pushing those packages up to chocolatey. You may want to add that switch to the main command above it. 

This gets Ketarin all set up with a global command for all packages we create. If you want to use this outside of chocolatey, all you need to do is remove the global setting for Before updating an application and instead apply it to every job that pertains to chocolatey update.

###Create an Automatic Package
Preferably you are taking an existing package that you have tested and converting it to an automatic package.  
  
1. Open Ketarin. Choose File –> Import…  
1. Choose the template you just saved earlier (KetarinChocolateyTemplate.xml). 
1. Answer the questions. This will create a new job for Ketarin to check. 
1. One important thing to keep in mind is that **the name of the job needs to match the name of the package folder exactly.**
1. Right click on that new job and select Edit. Take a look at the following:  
![Ketarin Job Main](images/chocopkgup/KetarinMain.png "Ketarin Job Main")
1. Set the URL appropriately. I would shy away from FileHippo for now, the URL has been known to change and if you upload that as the download url in a chocolatey packages, it won’t work very well. 
1. Click on Variables on the right of URL  
![Ketarin Job Variables](images/chocopkgup/KetarinSetVariables.png "Ketarin Job Variables")  
1. On the left side you should see a variable for version and one for url64. Click on version.
1. Choose the appropriate method for you. Here I’ve chosen Content from URL (start/end).
1. Enter the URL for versioning information.  
![Ketarin Job Variables](images/chocopkgup/KetarinVariables.png "Ketarin Job Variables") 
1. In the contents itself, highlight enough good information before a version to be able to select it uniquely during updates (but not so much it doesn’t work every time as the page changes). Click on **Use selection as start**.
1. Now observe that it didn’t jump back too far. 
1. Do the same with the ending part, keeping in mind that this side doesn’t need to be too much because it is found AFTER the start. Once selected click on **Use selection as end**.
1. It should look somewhat similar to have is presented in the picture above.
1. If you have a 64bit url you want to get, do the same for the url64 variable.
1. When all of this is good, click **OK**.
1. Click **OK** again. 

  

  

  
 


  