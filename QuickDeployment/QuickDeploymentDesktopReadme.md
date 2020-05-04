# Thanks for trying Chocolatey For Business!

This system has been pre-configured as a fully functioning C4B environment.

> :warning: **WARNING**
>
> Please follow these steps in ***exact*** order. These steps build on each other and need to be completed in order.

> :memo: **NOTE**: This is likely more up to date than the ReadMe you will find on the desktop (not including redacted items like credentials). If there are conflicts between the desktop readme and what you see here, use this document.

<!-- TOC depthFrom:2 -->

- [Summary](#summary)
- [Step 0: Complete Prerequisites](#step-0-complete-prerequisites)
- [Step 1: Create a License Package](#step-1-create-a-license-package)
- [Step 2: Regenerate SSL Certificates](#step-2-regenerate-ssl-certificates)
- [Step 3: Enable Central Management](#step-3-enable-central-management)
- [Step 4: Review Server Information](#step-4-review-server-information)
  - [Nexus Repository](#nexus-repository)
  - [Jenkins](#jenkins)
  - [Chocolatey Central Management](#chocolatey-central-management)
  - [Firewall ports](#firewall-ports)
  - [Browser considerations](#browser-considerations)
- [Step 5: Change the API Key (Optional, Recommended)](#step-5-change-the-api-key-optional-recommended)
    - [Choco Apikey Command](#choco-apikey-command)
- [Step 6: Install and Configure Chocolatey On Clients](#step-6-install-and-configure-chocolatey-on-clients)
- [Step 7: Turn On Package Internalization](#step-7-turn-on-package-internalization)
- [Step 8: License the QDE VM](#step-8-license-the-qde-vm)

<!-- /TOC -->

## Summary

To finish setting up QDE (Quick Deployment Environment), you'll need to work through this document and run the different commands you find here.
Please note that nearly _all_ the commands need to be run from an administrative context.

If you run into any issues as you set up your QDE and clients, please reach out to support at [REDACTED] and folks can set up a session to work with you on this.

Additional information can be found in our [Online Documentation](https://chocolatey.org/docs/quick-deployment-environment).

* [[QDE Setup|QuickDeploymentSetup]]
* [[QDE Desktop ReadMe File|QuickDeploymentDesktopReadme]] (online version of the desktop readme, online is most up to date)
* [[QDE SSL/TLS Setup|QuickDeploymentSslSetup]]
* [[QDE Firewall Changes|QuickDeploymentFirewallChanges]]
* [[QDE Client Setup|QuickDeploymentClientSetup]] (setting up your client machines)

___
## Step 0: Complete Prerequisites

There are some steps you will have taken before you come to this readme. Please make sure you have taken those steps ahead of time. Please see the [Online Documentation](https://chocolatey.org/docs/quick-deployment-environment) for the most up to date information.

* [[QDE Setup|QuickDeploymentSetup]]

___
## Step 1: Create a License Package

To leverage all of the features of C4B, copy the license file you received via email to `C:\ProgramData\chocolatey\license`.
Make sure the name of the file is exactly `chocolatey.license.xml`.

In an administrative Powershell session, execute the following:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; . 'C:\choco-setup\files\CreateLicensePackage.ps1'
```

This will create the licensed package at `C:\choco-setup\packages` and push it up to your Nexus repository for use.

___
## Step 2: Regenerate SSL Certificates

Under almost all circumstances for security purposes, you are going to want to complete this step. We've made it easy for you with a script. Once complete, the script will generate new SSL certificates for all services and move them to the appropriate locations and configure the services to use them. Please see [[SSL/TLS Setup|QuickDeploymentSslSetup]] for more details.

> :memo: **NOTE**: Please run the below from an administrative PowerShell session.

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; . C:\choco-setup\files\New-SslCertificates.ps1
```

> :warning: **WARNING**
>
> This script will seemingly prompt for input, and have other strange output.
> This is due to poor Java tooling and console output which cannot be suppressed.
> Just let things happen, as things are working as expected.

Once complete, this script will generate new SSL certificates for all services and move them to the appropriate locations and configure the services to use them.

___
## Step 3: Enable Central Management

> :memo: **NOTE**
>
> This step should _only_ be completed once the license package has been created in the step above.
> All licensed features are already installed, but will not be functional without a valid license file.

Run the following to turn on the Central Management services in an administrative PowerShell session:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; . 'C:\choco-setup\files\EnableCCM.ps1'
```

___
## Step 4: Review Server Information

### Nexus Repository

* Url: [https://chocoserver:8443](https://chocoserver:8443)
* Username: admin
* Password: [REDACTED] (credentials included on desktop readme)
* API Key: [REDACTED] (credentials included on desktop readme)

When you first log in to Nexus, you will immediately be asked you change your password.
You will then be asked if you'd like to enable Anonymous Access to the repositories.
We typically recommend doing this, unless security requirements in your organization stipulate that RBAC controls be in place.

> :warning: **WARNING**
>
> If you plan to allow clients to connect in from outside your network (over the internet), please contact support for the right options.
> There will be more work you'll need to do to limit access to specific repositories.

Sources configured in Chocolatey can only read data from their remote endpoints, and cannot delete items.
If you need to limit the packages people have access to, control this with separate Hosted and Group repositories.
Consult the Nexus documentation or reach out to Chocolatey Support for more information.

### Jenkins

* Url: [http://chocoserver:8080](http://chocoserver:8080)
* Username: admin
* Password: [REDACTED] (credentials included on desktop readme)

For using Jenkins, please refer to our documentation here: [https://chocolatey.org/docs/how-to-setup-internal-package-repository](https://chocolatey.org/docs/how-to-setup-internal-package-repository).
At most, you will need to login to Jenkins, change the password (`By going to People on the Sidebar > Click on admin > Click Configure on the Sidebar, scroll down to change password section`), and enable the pre-configured jobs to run on the schedule of your choosing.
Our documentation can assist with ensuring this is done correctly.

### Chocolatey Central Management

* Url: [https://chocoserver](https://chocoserver)
* Username: ccmadmin
* Default Password: 123qwe (You will be prompted to change this on first login)

> :memo: **NOTE**
>
> You will see 2 packages (aspnetcore-runtimepackagestore and dotnetcore-windowshosting) listed as outdated at version 2.2.7.
> These packages have been pinned to that version, as they are required at that level for the current version of CCM to function correctly.

### Firewall ports

To allow access to all services firewall ports have been opened on QDE as follows:

* 8443: Nexus WebUI
* 8081: Jenkins WebUI
* 443: Central Management WebUI
* 24020: Central Management Service communications for Agent check-in

### Browser considerations

We recommend you use Google Chrome to interact with all Web interfaces for the different services installed. You will find Google Chrome pre-installed in the environment.

___
## Step 5: Change the API Key (Optional, Recommended)

You may wish to change the API key before you start using things.
To do so, log in to Nexus using the information above, or your new credentials if you have already gone through the first run wizard.
Once logged in perform the following steps:

1. Click on your username in the upper right-hand side of the homepage.
2. Select "NuGet API Key" from the left-hand navigation window.
3. Select "Reset API key"
4. Enter your password
5. Take note of the new API key

If you change your API key, you will need to change the key in the Jenkins jobs that are pre-configured for you.
See the next section for information on how to connect to Jenkins.

#### Choco Apikey Command

To help make pushing packages easier, the `choco apikey` command is available.
This will store your API key for a specific source as part of Chocolatey's configuration.
This will be encrypted. To setup, do the following:

> :memo: **NOTE**: Please run the below from an administrative PowerShell session.

```powershell
choco apikey add --key="'$YourApiKey'" --source="'https://chocoserver:8443/repository/ChocolateyInternal/'"
```

___
## Step 6: Install and Configure Chocolatey On Clients


This script, like all of the others here would need to be run in an administrative PowerShell context. However, this one is run from your client machines and not the QDE.

```powershell
Set-ExecutionPolicy RemoteSigned -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/Import-QuickDeployCertificate.ps1')); Set-ExecutionPolicy RemoteSigned -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocoserver:8443/repository/choco-install/ClientSetup.ps1'))
```

What does this do?

* Sets the execution policy for this script run to remote signed scripts. This is only in the scope of this process and not permanent.
* Imports the SSL Certificate from the Quick Deploy Environment. **NOTE**: This is a signed script that is used to import a certificate. Due to how it works and security considerations, there are very few options allowed.
* Switches execution policy to bypass for the internal script. This is only in the scope of this process and not permanent.
* Calls Client setup script from the QDE environment (see below for what it does).

> :warning: **WARNING**
>
> If your clients are airgapped or you have changed the hostname, you will need to find a different means to import the QDE Certificate.
> Please reach out to support for options.

> :warning: **WARNING**
>
> If the QDE hostname has been changed, the above script most likely will fail.
>
> You won't be able to use the above script, and you will need to host your own script somewhere that is trusted so that the QDE certificates can be trusted. Please see [[SSL/TLS Setup|QuickDeploymentSslSetup]] for options.
>
> Please contact support if you need help here.


The ClientSetup.ps1 script will :

* Install Chocolatey
* License Chocolatey
* Install the licensed extension (without the PackageBuilder/Internalizer shims)
* Install the licensed agent
* Configure ChocolateyInternal source
* Configure Self-Service mode
* Configure Central Management check-in

___
## Step 7: Turn On Package Internalization

Chocolatey For Business includes the Package Internalizer feature, which takes a package from the Community Repository and rewrites the package to include all the binaries necessary to complete the installation of the application. You'll find in the C:\choco-setup\files directory a script named `Invoke-ChocolateyInternalizer.ps1` to help you with the process of internalizing additional packages into your environment.

The script accepts an array of Packages, your Local Nexus repository URL, the remote URL to look to for internalization, and your Nexus repository API key.

Example Usage:

```powershell
. C:\choco-setup\files\Invoke-ChocolateyInternalizer.ps1 -Packages adobereader,vlc,vscode -RepositoryUrl https://chocoserver:8443/repository/ChocolateyTest/ -RemoteRepo https://chocolatey.org/api/v2 -LocalRepoApiKey [REDACTED_API_KEY]
```

> :memo: **NOTE**: Please run the above from an administrative PowerShell session.

___
## Step 8: License the QDE VM


This VM is running an **UNACTIVATED** Server 2019 Standard Operating System. If you plan to use this virtual machine long-term, you _will_ need to apply a license to the VM. If you use a KMS server in your environment, and have it configured on clients via Group Policy, you likely have nothing to do here, but verify.

If you rely on Retail or MAK licensing, you will need to apply the license using the following, replacing the x's with your actual product key:

> :memo: **NOTE**: Please run the below from an administrative PowerShell session.

```powershell
slmgr.vbs /ipk xxxxx-xxxxx-xxxxx-xxxxx
```


[[Quick Deployment Environment|QuickDeploymentEnvironment]]
