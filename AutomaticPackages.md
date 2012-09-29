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
  
1. You need a scheduled job to run ketarin. You want it to run the actual ketarin.exe and not the chocolatey batch redirect.  
  
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

![Ketarin Job Main](images/chocopkgup/KetarinMain.png "Ketarin Job Main")  
![Ketarin Job Variables](images/chocopkgup/KetarinVariables.png "Ketarin Job Variables")  


  