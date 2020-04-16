# Quick Deployment Client Setup

<!-- TOC -->

- [Summary](#summary)
- [DNS](#dns)
- [Client Installation](#client-installation)

<!-- /TOC -->

## Summary
Once you have QDE set up in your environment, you'll need to get clients talking to it. To do that, we'll need to do the following:

* Setting up DNS on the client to access QDE.
* Install the QDE SSL/TLS certificate so clients can access HTTPS components
* Install Chocolatey and friends

## DNS
Typically in your environment, onces you've added QDE, it should be able to start talking to the QDE. In some situations, you may need to add the host name with the IP address to reach your environment.

## Client Installation

On your client machines, you will be running the following script in an administrative context:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/Import-QuickDeployCertificate.ps1')); iex ((New-Object System.Net.WebClient).DownloadString('https://chocoserver:8443/repository/choco-install/ClientSetup.ps1'))
```

What does this do?
* Sets the execution policy for this script run to bypass. It does not affect permanent settings.
* Imports the SSL Certificate from the Quick Deploy Environment.
* Calls Client setup script from the QDE environment.

> :warning: **WARNING**: If you have changed the host name, this will not work for you. Please reach out to support for options.

The ClientSetup.ps1 script will:

- Install Chocolatey
- License Chocolatey
- Install the licensed extension (without the PackageBuilder/Internalizer shims)
- Install the licensed agent
- Configure ChocolateyInternal source
- Configure Self-Service mode
- Configure Central Management check-in
