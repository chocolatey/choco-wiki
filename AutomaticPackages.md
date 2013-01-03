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
  
 These are what you now will be passing from chocopkgup to this package. 

1. Install chocopkgup (which will install ketarin and nuget.commandline). cinst chocolateypackageupdater. 
1. Create a scheduled task (in windows). This is the command (edit the path to cmd.exe accordingly): C:\Windows\System32\cmd.exe /c c:\tools\chocolateypackageupdater\ketarinupdate.cmd 
Choose a schedule for the task. I run mine once a day but you can set it to run more often. Choose a time when the computer is not that busy. 
Save this Ketarin template somewhere: https://github.com/ferventcoder/chocolateyautomaticpackages/blob/master/_template/KetarinChocolateyTemplate.xml 
Open Ketarin. Choose File –> Settings. 
On the General Tab we are going to add the Version Column for all jobs. Click Add…, then put Version in Column name and {version} in Column value. 


###Tutorial
Stay tuned for this. Below may look like garbage; that's okay. I am collecting my thoughts prior to making it coherent. 
  
####Scheduled Tasks
  
C:\Windows\System32\cmd.exe /c c:\tools\chocolateypackageupdater\ketarinupdate.cmd
  
####Ketarin Global Settings Command
This is for before updating app
  
![Ketarin Settings](images/chocopkgup/KetarinSettings.png "Ketarin Settings")    
  
```cmd
chocopkgup /p {appname} /v {version} /u {preupdate-url} /u64 {url64} /pp {file} 
REM /disablepush
```
  
![Ketarin Settings Custom](images/chocopkgup/KetarinShowCustomField.png "Ketarin Custom Field Setup")  
  
![Ketarin Job Main](images/chocopkgup/KetarinMain.png "Ketarin Job Main")  
![Ketarin Job Variables](images/chocopkgup/KetarinVariables.png "Ketarin Job Variables")  


  