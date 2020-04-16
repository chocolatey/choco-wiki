# Quick Deployment Environment SSL Setup

All services have been protected with Self-Signed SSL certificates and are placed in the appropriate stores.
Under the following situations you would want to run the script that follows:

* If you want to expose this to the internet so clients can connect from outside your network.
* If you change the hostname of this server.
* If you add the QDE to a domain.
* If you would like to use your own SSL/TLS certificates.

> :warning: **WARNING**: This script will seemingly prompt for input, and have other strange output.
> This is due to poor Java tooling and console output which cannot be suppressed.
> Just let things happen, as things are working as expected.

**NOTE**: Please run the below from an administrative PowerShell session.

Once complete, this script will generate new SSL certificates for all services and move them to the appropriate locations and configure the services to use them.

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; . C:\choco-setup\files\New-SslCertificates.ps1
```
