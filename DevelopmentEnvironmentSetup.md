#Set Up A Development Environment using chocolatey

This does the following:  
  
* downloads and installs chocolatey  
* installs nuget.commandline if it is not installed
* installs ruby if it is not installed
* updates gems and installs some required gems for rake
* restores nuget packages from the configs
* builds the source with rake 
  
  
Original [on github](https://gist.github.com/1107920).   

###setup.ps1:  
  
```powershell
### install chocolatey ###
$url = "http://packages.nuget.org/v1/Package/Download/Chocolatey/0.9.8.4"
$tempDir = "$env:TEMP\chocolatey\chocInstall"
if (![System.IO.Directory]::Exists($tempDir)) {[System.IO.Directory]::CreateDirectory($tempDir)}
$file = Join-Path $tempDir "chocolatey.zip"

# download the package
Write-Host "Downloading $url to $file"
$downloader = new-object System.Net.WebClient
$downloader.DownloadFile($url, $file)

# unzip the package
Write-Host "Extracting $file to $destination..."
$shellApplication = new-object -com shell.application 
$zipPackage = $shellApplication.NameSpace($file) 
$destinationFolder = $shellApplication.NameSpace($tempDir) 
$destinationFolder.CopyHere($zipPackage.Items(),0x10)

# call chocolatey install
Write-Host "Installing chocolatey on this machine"
$chocInstallPS1 = "$tempDir\tools\chocolateyInstall.ps1"
& $chocInstallPS1
### finish installing chocolatey ###

# update chocolatey to the latest version
Write-Host "Updating chocolatey to the latest version"
cup chocolatey

# install nuget and ruby if they are missing
cinstm nuget.commandline
cinstm ruby

#perform ruby updates and get gems
gem update --system
gem install rake
gem install bundler

#restore the nuget packages
$nugetConfigs = Get-ChildItem '.\' -Recurse | ?{$_.name -match "packages\.config"} | select
foreach ($nugetConfig in $nugetConfigs) {
  Write-Host "restoring packages from $($nugetConfig.FullName)"
  nuget install $($nugetConfig.FullName) /OutputDirectory packages
}

rake
```

###setup.cmd: 
  
```
@echo off
SET DIR=%~dp0%
%windir%\System32\WindowsPowerShell\v1.0\powershell.exe -NoProfile -ExecutionPolicy unrestricted -Command "& '%DIR%setup.ps1' %*"
pause
```
  