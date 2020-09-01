# Quick Deployment Client Setup

> :memo: **NOTE**
>
> This document is for **Version 2.0** of the Quick Deployment Environment.
> If you're using an older version of QDE, please refer to the [[this document||QuickDeploymentClientSetup_v1]]

<!-- TOC depthFrom:2 -->

- [Summary](#summary)
- [DNS](#dns)
- [Client Installation](#client-installation)

<!-- /TOC -->

---

## Summary

Once you have QDE set up in your environment, you'll need to get clients talking to it.
To do that, you'll need to do the following on the clients:

1. Setup DNS to allow access to QDE.
1. Install the QDE SSL/TLS certificate so clients can access HTTPS components.
1. Install Chocolatey components.

---

## DNS

In most environments, once you've added the QDE server, clients should be able to access it.
In some situations, you may need to add the host name with the IP address to your HOSTS file to reach your environment.

---

## Client Installation

On your client machines, you will need to run the following script in an administrative context:

> :memo: **NOTE**
>
> `chocoserver` is the default hostname for QDE.
> If you've adjusted the QDE server hostname or added custom HOSTS entries, use the appropriate hostname instead.

```powershell
$downloader = New-Object -TypeName System.Net.WebClient
Invoke-Expression ($downloader.DownloadString('https://chocoserver:4431/Import-ChocoServerCertificate.ps1'))
Invoke-Expression ($downloader.DownloadString('https://chocoserver:8443/repository/choco-install/ClientSetup.ps1'))
```

> :warning: **WARNING**
>
> If your clients are air-gapped, you will need to ensure that they can at least access the QDE server itself in order to import the QDE Certificate.

This takes care of the following actions:

* Imports the SSL Certificate from the Quick Deploy Environment.
* Calls Client setup script from the QDE environment (see below for what it does).

The `ClientSetup.ps1` script will:

1. Install Chocolatey
1. License Chocolatey
1. Install the licensed extension (without the PackageBuilder/Internalizer shims)
1. Install the agent service
1. Configure ChocolateyInternal source
1. Configure Self-Service mode
1. Configure Central Management check-in

---

> :memo: **NOTE**
> For internet enabled client setup please refer to [[Quick deployment Environment Internet Setup||QuickDeploymentEnvironmentInternetSetup]]

[[Quick Deployment Environment||QuickDeploymentEnvironment]]
