#Set Up A Development Environment Using Chocolatey

How many of you out there are rake fans? Getting developers to look at your source code can sometimes be an issue. Wouldn't it be nice if it was simple for them to get all set up? What about Ruby DevKit? It would be nice, right?  


  
## Set up From the Source
This does the following:  
  
* downloads and installs chocolatey  
* installs nuget.commandline if it is not installed
* installs ruby.devkit if it is not installed
* installs ruby if it is not installed
* updates gems and installs some required gems for rake
* restores nuget packages from the configs
* builds the source with rake 
  
### setup.ps1:  
  
```powershell
function Install-NeededFor {
param([string]$packageName = '')
  if ($packageName -eq '') {return $false}
  
  $yes = '6'
  $no = '7'
  $msgBoxTimeout='-1'
  
  $answer = $msgBoxTimeout
  try {
    $timeout = 10
    $question = "Do you need to install $($packageName)? Defaults to 'Yes' after $timeout seconds"
    $msgBox = New-Object -ComObject WScript.Shell
    $answer = $msgBox.Popup($question, $timeout, "Install $packageName", 0x4)
  }
  catch {
  }

  if ($answer -eq $yes -or $answer -eq $msgBoxTimeout) {
    write-host 'returning true'
    return $true
  }
  return $false
}

#install chocolatey
if (Install-NeededFor 'chocolatey') {
  iex ((new-object net.webclient).DownloadString('http://bit.ly/psChocInstall'))
}

# install nuget, ruby.devkit, and ruby if they are missing
cinstm nuget.commandline

if (Install-NeededFor 'ruby / ruby devkit') {
  cinstm ruby.devkit
  #cinstm ruby #devkit install will automatically install ruby
}

#perform ruby updates and get gems
gem update --system
gem install rake
gem install bundler

#restore the nuget packages
$nugetConfigs = Get-ChildItem '.\\' -Recurse | ?{$_.name -match "packages\\.config"} | select
foreach ($nugetConfig in $nugetConfigs) {
  Write-Host "restoring packages from $($nugetConfig.FullName)"
  nuget install $($nugetConfig.FullName) /OutputDirectory packages
}

rake

```
  
### setup.cmd:  
  
```  
@echo off
SET DIR=%~dp0%
@PowerShell -NoProfile -ExecutionPolicy unrestricted -Command "& '%DIR%setup.ps1' %*"
pause

```
  
The original is [on github](https://gist.github.com/1107920).   

## Getting the Source
Create a package for your project and call it projectname*.dev*.  It should take a nuspec dependency on whatever source control you use. So in the case of git, a dependency on msysgit.  
Now, in [[chocolateyInstall.ps1|ChocolateyInstallPS1]], you just need something like the following: 

```powershell
try {

  $dirSelected = Read-Host "Please tell me the directory where you want to clone dropkick. Press enter to use .\\dropkick"
  
  if ($dirSelected -eq '') {$dirSelected = '.\\dropkick'}
  
  git clone git://github.com/chucknorris/dropkick.git $dirSelected
  
  Start-Sleep 6
} catch {
@"
Error Occurred: $($_.Exception.Message)
"@ | Write-Host -ForegroundColor White -BackgroundColor DarkRed
	Start-Sleep 8
	throw 
}
```
  
The above is from [DropkicK.Dev](https://github.com/ferventcoder/nugetpackages/blob/master/dropkick.dev/tools/chocolateyInstall.ps1)  