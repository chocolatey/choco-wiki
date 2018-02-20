# How To Set Up Fully Offline Installation

<!-- TOC -->

- [References](#references)
- [Exercise 0: Prepare for Offline](#exercise-0-prepare-for-offline)
- [Exercise 1: Set Up Offline Package Repository](#exercise-1-set-up-offline-package-repository)
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
* Type `c:\` and hit enter.
* Type `mkdir choco-setup` and enter followed by `cd choco-setup` and enter.
* In `c:\choco-setup`, type `mkdir files` and press enter.
* In `c:\choco-setup`, type `mkdir packages` and press enter.
* Type `cd packages` and press enter
* Now run `Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))` (this will get Chocolatey installed and it is what you see at https://chocolatey.org/install). It also makes choco available in that current shell.
* C4B / MSP / TRIAL: Obtain the `chocolatey.license.xml` from the email sent from the Chocolatey team and save the license file to `c:\choco-setup\files` so we can use it here and on the offline machines.
* TRIAL: grab a copy of the two nupkgs from the email. If you don't have that email with the download links, request it from whoever provided you the trial license. Save those two packages to `c:\choco-setup\packages`.
* C4B / MSP / TRIAL: Run this command `New-Item $env:ChocolateyInstall\license -Type Directory -Force` - this creates the license directory.
* C4B / MSP / TRIAL: Copy the license file ("chocolatey.license.xml") into that folder that was just created. Run `Copy-Item c:\choco-setup\files\chocolatey.license.xml $env:ChocolateyInstall\license\chocolatey.license.xml -Force`.
* C4B / MSP / TRIAL: Verify the license is recognized - run choco. You should see something like "Chocolatey v0.10.8 Business". You may also see an error about installing something. Ignore that for now.
* C4B / MSP: Run `choco upgrade chocolatey.extension -y` (it will have what looks like an error, but that is fine, it is considered a warning and will clear up as soon as this package finishes installing)
* TRIAL: Run `choco upgrade chocolatey.extension --pre --source c:\choco-setup\packages` (this is where you saved the nupkgs earlier).
* Download packages (choose one):
  * C4B / MSP / TRIAL: - Now run the following: `choco download chocolatey chocolatey.server dotnet4.6.1 --internalize`. This is going to take quite awhile.
  * FOSS only - go download the following packages (internalize noted items with [[manual walkthrough|How-To-Recompile-Packages]]):
    * [Chocolatey](https://chocolatey.org/api/v2/package/chocolatey)
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

## Exercise 1: Set Up Offline Package Repository
Now we've finished the first Exercise and we have those files on our offline machine. We are going to set up the Chocolatey.Server (but you could set up something else here like Nexus, Artifactory Pro, or ProGet).

* Ensure the folders from the other drive are set in `c:\choco-setup` here on this machine.
* Let's get Chocolatey installed. Open up an administrative PowerShell.exe on this machine.
* Type `& c:\choco-setup\files\ChocolateyLocalInstall.ps1` and press enter. This should install Chocolatey if you have everything set up correctly from the first set of instructions.
* Run `choco source remove -n=chocolatey`. This removes the default Chocolatey source.
* Run `choco source add --name=setup --source=c:\choco-setup\packages`
* Follow the steps at https://chocolatey.org/docs/how-to-set-up-chocolatey-server#setup-normally
* Follow the steps at https://chocolatey.org/docs/how-to-set-up-chocolatey-server#additional-configuration
* Open a web browser and navigate to http://localhost. Read over the site and take notes.
* You may wish to install an SSL certificate.
* You may wish to set up authentication to the repository (the SSL certificate is highly recommended to not pass passwords in cleartext).

**NOTE:** You may wish to use another repository server, like Artifactory Pro, Nexus, ProGet, etc. They have the concept of a proxy repository. Unlike NuGet repositories, you likely ***DO NOT WANT*** a proxied NuGet/Chocolatey repository pointing to the community repository. They only cache packages, they don't internalize them. Please note that **[[cached is not the same concept as internalized|How-To-Recompile-Packages]]**.  The community repository is subject to distribution rights, which means many packages need to download things from the internet at ***runtime***. That's unreliable and a no go for many organizations. You can use Package Internalizer (as we are seeing above) or [[manually internalize packages||How-To-Recompile-Packages]] you want to use from the community repository. More on why

## Exercise 2: Packages in the Repository

* Now we need to simply place the packages we have in `c:\choco-setup\packages` to the packages folder of the website, which is typically at `c:\tools\Chocolatey.Server\App_Data\Packages`.
* Make sure we have Chocolatey here, as the Chocolatey install script that Chocolatey Server offers is dynamic and we want it to use a local package instead of the Chocolatey package at https://chocolatey.org/.

## Exercise 3: Installing Chocolatey From Offline Repository

One thing Chocolatey.Server v0.2.3+ contains is an install script much like you see at https://chocolatey.org/install and https://chocolatey.org/install.ps1.

* From a client, open a browser and navigate to the url of the package repository.
