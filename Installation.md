# Installing Chocolatey
There are a few ways to install chocolatey.  


## PowerShell Through Batch Method

This is probably the easiest method.

Download the two items from [chocolateyInstall](https://github.com/ferventcoder/chocolatey/tree/master/chocolateyInstall).

Run `installChocolatey.cmd` and it will install and update to the latest version of chocolatey.

## PowerShell

This is maybe the second easiest method. Open a powershell command line and run the following:

```powershell
# variables
$url = "http://packages.nuget.org/v1/Package/Download/Chocolatey/0.9.8.3"
$chocTempDir = Join-Path $env:TEMP "chocolatey"
$tempDir = Join-Path $chocTempDir "chocInstall"
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
$toolsFolder = Join-Path $tempDir "tools"
$chocInstallPS1 = Join-Path $toolsFolder "chocolateyInstall.ps1"

& $chocInstallPS1


# update chocolatey to the latest version
Write-Host "Updating chocolatey to the latest version"
cup chocolatey

```

## NuGet Package Manager Method
  
Chocolatey exists as a [nuget package](http://nuget.org/list/packages/chocolatey).  
  
 `Install-Package chocolatey`  
 `Initialize-Chocolatey`  
 `Uninstall-Package chocolatey`  

## NuGet.exe Method

Use nuget command line to download chocolatey:  
  
 `nuget install chocolatey`  
  
Once you download it, open powershell (remote unsigned) and run 

 `Initialize-Chocolatey`
