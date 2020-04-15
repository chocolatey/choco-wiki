# Quick Deployment Environment SSL Setup

All services have been protected with Self-Signed SSL certificates and are placed in the appropriate stores. If you change the hostname of this server, add it to a domain, or otherwise would like to use your own SSL certificates, perform the following:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force ; . C:\choco-setup\files\New-SslCertifcates.ps1
```

> :warning: **WARNING**: This script will seemingly prompt for input, and have other strange output. This is due to poor Java tooling and console output which cannot be suppressed. Just let things happen, as things are working as expected.
Once complete, this script will generate new SSL certificates for all services and move them to the appropriate locations and configure the services to use them.
