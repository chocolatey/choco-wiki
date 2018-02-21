# How To Set Up Fully Offline Installation

<!-- TOC -->

- [References](#references)
- [Exercise 0: Prepare for Offline](#exercise-0-prepare-for-offline)
- [Exercise 1: Set Up Chocolatey Installation From a Local Script](#exercise-1-set-up-chocolatey-installation-from-a-local-script)
- [Exercise 2: Add the Offline Package Repository](#exercise-2-add-the-offline-package-repository)
  - [Exercise 2A: Set up Chocolatey.Server](#exercise-2a-set-up-chocolateyserver)
  - [Exercise 2B: Set up a Different Repository](#exercise-2b-set-up-a-different-repository)
- [Exercise 2: Packages in the Repository](#exercise-2-packages-in-the-repository)
- [Exercise 3: Installing Chocolatey From Offline Repository](#exercise-3-installing-chocolatey-from-offline-repository)

<!-- /TOC -->


## References

* [Offline Chocolatey Install](https://chocolatey.org/install#completely-offline-install)
* [[Licensed Install|Installation-Licensed]]
* [[Host Your Own Package Server|How-To-Host-Feed]]
* [[Set up Chocolatey Server|How-To-Set-Up-Chocolatey-Server]]

## Exercise 0: Prepare for Offline

The first thing we need to do is prepare. **NOTE:** This uses Chocolatey.Server, but you can use any of the known options in [[hosting your own package repository|How-To-Host-Feed]].

We need a machine with internet access. To set up Chocolatey for offline installation (air-gapped network if you will), first we need a way to obtain everything.

So from the machine with internet access:

* Open PowerShell.exe as an administrative shell. You can type "Windows Key + X + A" (Windows 8+ - when that comes up if it is cmd.exe, simply type `powershell` to get into it).
* Type `cd /` and hit enter.
* Type `New-Item -Path "$env:SystemDrive\choco-setup" -ItemType Directory -Force` and enter followed by `cd choco-setup` and enter.
* In `c:\choco-setup`, type `New-Item -Path "$env:SystemDrive\choco-setup\files" -ItemType Directory -Force` and press enter.
* In `c:\choco-setup`, type `New-Item -Path "$env:SystemDrive\choco-setup\packages" -ItemType Directory -Force` and press enter.
* Type `cd packages` and press enter
* Now run `Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))` (this will get Chocolatey installed and it is what you see at https://chocolatey.org/install). It also makes choco available in that current shell.
* C4B / MSP / TRIAL: Obtain the `chocolatey.license.xml` from the email sent from the Chocolatey team and save the license file to `c:\choco-setup\files` so we can use it here and on the offline machines.
* TRIAL: grab a copy of the two nupkgs from the email. If you don't have that email with the download links, request it from whoever provided you the trial license. Save those two packages to `c:\choco-setup\packages`.
* C4B / MSP / TRIAL: Run this command `New-Item $env:ChocolateyInstall\license -Type Directory -Force` - this creates the license directory.
* C4B / MSP / TRIAL: Copy the license file ("chocolatey.license.xml") into that folder that was just created. Run `Copy-Item "$env:SystemDrive\choco-setup\files\chocolatey.license.xml" $env:ChocolateyInstall\license\chocolatey.license.xml -Force`.
* C4B / MSP / TRIAL: Verify the license is recognized - run choco. You should see something like "Chocolatey v0.10.8 Business". You may also see an error about installing something. Ignore that for now.
* C4B / MSP: Run `choco upgrade chocolatey.extension -y` (it will have what looks like an error, but that is fine, it is considered a warning and will clear up as soon as this package finishes installing)
* TRIAL: Run `choco upgrade chocolatey.extension --pre --source c:\choco-setup\packages` (this is where you saved the nupkgs earlier).
* Download packages (choose one):
  * C4B / MSP / TRIAL: - Now run the following: `choco download chocolatey chocolatey.server dotnet4.6.1 chocolateygui --internalize`. This is going to take quite awhile.
  * FOSS only - go download the following packages:
    * [Chocolatey](https://chocolatey.org/api/v2/package/chocolatey)
    * [Chocolatey GUI](https://chocolatey.org/api/v2/package/chocolateygui)
  * FOSS only - download Chocolatey.Server package and dependencies  (internalize noted items with [[manual walkthrough|How-To-Recompile-Packages]]):
    * [Chocolatey.Server](https://chocolatey.org/api/v2/package/chocolatey.server)
    * [dotnet4.6](https://chocolatey.org/api/v2/package/DotNet4.6) - internalize manually
    * [dotnet4.6.1](https://chocolatey.org/api/v2/package/DotNet4.6.1) - internalize manually
    * [KB2919355](https://chocolatey.org/api/v2/package/KB2919355) - internalize manually
    * [KB2919442](https://chocolatey.org/api/v2/package/KB2919442) - internalize manually
* C4B / MSP - Run the following additionally: `choco download chocolatey.extension chocolatey-agent --internalize`. TRIAL - you should already have placed these nupkgs in the folder earlier.
* Now we should have several packages in `c:\choco-setup\packages`. If not, type `start .` and go copy the files here to that location.
* Obtain the PowerShell script from https://chocolatey.org/install#completely-offline-install and copy it to `c:\choco-setup\files` as "ChocolateyLocalInstall.ps1". We will need this to install Chocolatey on the airgapped box.
* Open `c:\choco-setup\files\ChocolateyLocalInstall.ps1` in an editor like Notepad++ or Visual Studio Code (do not use Notepad.exe!!).
* Change this line `$localChocolateyPackageFilePath = 'c:\packages\chocolatey.0.10.0.nupkg'` to `$localChocolateyPackageFilePath = 'c:\choco-setup\packages\chocolatey.0.10.8.nupkg'` (adjust for the actual path to the Chocolatey package).

Get those files over to that air-gapped network (USB key and sneakernet if you need to).

Here is a handy script you can use for MSP/C4B (FOSS, adjust appropriately):

~~~powershell
# Ensure we can run everything
Set-ExecutionPolicy Bypass -Scope Process -Force;

# Setting up directories for values
New-Item -Path "$env:SystemDrive\choco-setup" -ItemType Directory -Force
New-Item -Path "$env:SystemDrive\choco-setup\files" -ItemType Directory -Force
New-Item -Path "$env:SystemDrive\choco-setup\packages" -ItemType Directory -Force

# Installing Chocolatey
iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

# Add license to setup and to local install
New-Item $env:ChocolateyInstall\license -Type Directory -Force
Write-Host "Please add chocolatey.license.xml to '$env:SystemDrive\choco-setup\files'."
Write-Host 'Once you do so, press any key to continue...';
$null = $Host.UI.RawUI.ReadKey('NoEcho,IncludeKeyDown');
Copy-Item $env:SystemDrive\choco-setup\files\chocolatey.license.xml $env:ChocolateyInstall\license\chocolatey.license.xml -Force

# Install Chocolatey Licensed Extension
#  BEST PRACTICE: We always call choco upgrade and -y with scripts. We also spell out most options.
choco upgrade chocolatey.extension -y --pre
# TRIAL: Place nupkgs into the "$env:SystemDrive\choco-setup\packages" directory (add a script here to do so)
# TRIAL: Run this instead: choco upgrade chocolatey.extension --pre --source c:\choco-setup\packages

# Download and internalize packages.
choco download chocolatey chocolatey.server dotnet4.6.1 chocolateygui --internalize --output-directory="'$env:SystemDrive\choco-setup\packages'" --source="'https://chocolatey.org/api/v2/'"
# TRIAL: skip this next step
choco download chocolatey.extension chocolatey-agent --internalize --output-directory="'$env:SystemDrive\choco-setup\packages'" --source="'https://licensedpackages.chocolatey.org/api/v2/'" # will fail on trial

# Download local install script
$installScript = iwr -UseBasicParsing -Uri https://gist.githubusercontent.com/ferventcoder/d0aa1703a7d302fce79e7a4cc13797c0/raw/b1f7bad2441fa6c371b48b8475ef91cecb4d6370/ChocolateyLocalInstall.ps1 -UseDefaultCredentials
$installScript | Out-File -FilePath "$env:SystemDrive\choco-setup\files\ChocolateyLocalInstall.ps1" -Encoding UTF8 -Force
Write-Warning "Check and adjust script at '$env:SystemDrive\choco-setup\files\ChocolateyLocalInstall.ps1' to ensure it points to the right version of Chocolatey in the choco-setup\packages folder."
~~~

## Exercise 1: Set Up Chocolatey Installation From a Local Script
Now that we've finished the first exercise and have those files over on our offline machine, we need to get Chocolatey set up on that machine. This could be a Chocolatey Server, it could be something else.

* Ensure the folders from the other drive are set in `c:\choco-setup` here on this machine.
* Open PowerShell.exe as an administrative shell. You can type "Windows Key + X + A" (Windows 8+ - when that comes up if it is cmd.exe, simply type `powershell` to get into it).
* Type `& $env:SystemDrive\choco-setup\files\ChocolateyLocalInstall.ps1` and press enter. This should install Chocolatey if you have everything set up correctly from the first set of instructions.
* Run `choco source remove --name="'chocolatey'"`. This removes the default Chocolatey source.
* Run `choco source add --name="'setup'" --source="'$env:SystemDrive\choco-setup\packages'"`
* C4B / MSP / TRIAL: Run this command `New-Item $env:ChocolateyInstall\license -Type Directory -Force` - this creates the license directory.
* C4B / MSP / TRIAL: Copy the license file ("chocolatey.license.xml") into that folder that was just created. Run `Copy-Item "$env:SystemDrive\choco-setup\files\chocolatey.license.xml" $env:ChocolateyInstall\license\chocolatey.license.xml -Force`.
* C4B / MSP / TRIAL: Run `choco source disable --name="'chocolatey.licensed'"` (it will have what looks like an error, but is a warning that clears up after the next command)
* C4B / MSP / TRIAL: Run `choco upgrade chocolatey.extension -y --pre` (it will have what looks like an error, but that is fine, it is considered a warning and will clear up as soon as this package finishes installing)
* C4B / MSP / TRIAL: Are we installing the [optional Chocolatey Agent Service as well](https://chocolatey.org/docs/features-agent-service#setup)? If so, run `choco upgrade chocolatey-agent -y --pre`


~~~powershell
# Ensure we can run everything
Set-ExecutionPolicy Bypass -Scope Process -Force;

# Installing Chocolatey
& $env:SystemDrive\choco-setup\files\ChocolateyLocalInstall.ps1

# Sources - Remove community repository and add a local folder source
choco source remove --name="'chocolatey'"
choco source add --name="'local'" --source="'$env:SystemDrive\choco-setup\packages'"

# Add license to setup and to local install
New-Item $env:ChocolateyInstall\license -Type Directory -Force
Copy-Item $env:SystemDrive\choco-setup\files\chocolatey.license.xml $env:ChocolateyInstall\license\chocolatey.license.xml -Force

# Sources - Disable licensed source
choco source disable --name="'chocolatey.licensed'"
Write-Host "You can ignore the red text in the output above, as it is more of a warning until we have chocolatey.extension installed"

# Install Chocolatey Licensed Extension
choco upgrade chocolatey.extension -y --pre

# Are we installing the Chocolatey Agent Service?
# https://chocolatey.org/docs/features-agent-service#setup
# choco upgrade chocolatey-agent -y --pre
~~~

## Exercise 2: Add the Offline Package Repository
This is a continuation of the previous exercise - if we are setting up an internal repository on that offline machine, this is how we'll do it. We are going to set up the Chocolatey.Server (but you could set up something else here like Nexus, Artifactory Pro, or ProGet). Pick one of the following paths:

* [Set up Chocolatey.Server](#exercise-2a-set-up-chocolateyserver)
* [Set up a Different Repository](#exercise-2b-set-up-a-different-repository)

### Exercise 2A: Set up Chocolatey.Server
Since we put the items on this machine in the previous exercise, we can just pick up where we left off.

* Follow the steps at https://chocolatey.org/docs/how-to-set-up-chocolatey-server#setup-normally
* Follow the steps at https://chocolatey.org/docs/how-to-set-up-chocolatey-server#additional-configuration
* Open a web browser and navigate to http://localhost. Read over the site and take notes.
* Change the API key in the web.config file following the instructions at http://localhost.
* You may wish to install an SSL certificate.
* You may wish to set up authentication to the repository (the SSL certificate is highly recommended to not pass passwords in cleartext).

~~~powershell
# Ensure we can run everything
Set-ExecutionPolicy Bypass -Scope Process -Force;

# Install Chocolatey.Server
choco upgrade chocolatey.server -y --pre

Write-Warning "Follow the steps at https://chocolatey.org/docs/how-to-set-up-chocolatey-server#setup-normally and  https://chocolatey.org/docs/how-to-set-up-chocolatey-server#additional-configuration for now."
# more may be added later
~~~

### Exercise 2B: Set up a Different Repository
If you are setting up something different than Chocolatey.Server, you may wish to read over [[How To Set Up an Internal Repository|How-To-Host-Feed]]. This will give you options and links to repositories like Artifactory Pro, Nexus, and ProGet.

**NOTE:** Many repositories have a concept of a proxy repository. Unlike NuGet repositories, you likely ***DO NOT WANT*** a proxied NuGet/Chocolatey repository pointing to the community repository. They only cache packages - ***cached* is not the same concept as *internalized***. To reuse packages from the community repository in a reliable way, you need to [[internalize them|How-To-Recompile-Packages]]. The community repository is subject to distribution rights, which means many packages need to download things from the internet at ***runtime***. That's unreliable and a no go for many organizations. You can use Package Internalizer (as we are seeing above) or [[manually internalize packages|How-To-Recompile-Packages]] you want to use from the community repository. More on [[why (community packages repository notes)|CommunityPackagesDisclaimer]].

## Exercise 2: Packages in the Repository

* Now we need to get the packages we have in `c:\choco-setup\packages` to the package repository. With Chocolatey.Server, we can cheat a little and simply copy the nupkg files to `$env:ChocolateyToolsLocation\Chocolatey.Server\App_Data\Packages`.

**NOTE:** We'll put Chocolatey here, and use this location of Chocolatey for all further client installations. If we are using Chocolatey.Server, we'll have an install.ps1 that it serves that is dynamic and will use a local chocolatey.nupkg if we have one in the repository (it will use the Chocolatey package at https://chocolatey.org/ otherwise, which we won't want).


Here is a script for Chocolatey.Server:

~~~powershell
# Ensure we can run everything
Set-ExecutionPolicy Bypass -Scope Process -Force;

# Copy the packages to the Chocolatey.Server repo folder
Copy-Item "$env:SystemDrive\choco-setup\packages\*" -Destination "$env:ChocolateyToolsLocation\Chocolatey.Server\App_Data\Packages\" -Force -Recurse
~~~

For other things, just loop over the nupkg files and call `choco push`

## Exercise 3: Installing Chocolatey From Offline Repository

One thing Chocolatey.Server v0.2.3+ contains is an install script much like you see at https://chocolatey.org/install and https://chocolatey.org/install.ps1.

* From a client, open a browser and navigate to the url of the package repository.


**WORK IN PROGRESS, CHECK BACK LATER**
---

