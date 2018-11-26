# Chocolatey Central Management (CCM) (Business Editions Only)

## Usage

Chocolatey Central Management (CCM) works in conjunction with the [Chocolatey
Agent Service](https://chocolatey.org/docs/features-agent-service) to bring full
details of all Chocolatey controlled machines in your environment into one
location, which is then accessible by the CCM Website.  The Chocolatey Agent
Service will regularly report information about what is installed on each
machine, and whether any of that software is outdated, based on packages in
available sources.

## Requirements

* Chocolatey (`chocolatey` package) v0.10.12+
* Chocolatey for Business (C4B) Edition
* Chocolatey Licensed Extension (`chocolatey.extension` package) v2.0.0+
* Chocolatey Agent Service (`chocolatey-agent` package) v0.9.0+
* Chocolatey Central Management Database (`chocolatey-management-web` package) v0.1.0+
  * This deploys the CCM database schema to the specified SQL Server instance
* Chocolatey Central Management Service (`chocolatey-management-service` package) v0.1.0+
  * This installs the CCM Windows Service, which the Chocolatey Agent Windows Service will communicate with.
* Chocolatey Central Management Web (`chocolatey-management-web` package) v0.1.0+
  * This is the CCM front end website that is the main user interface of the application

## Setup

While it is envisioned that CCM will be installed across multiple servers, it is
certainly possible to run CCM on a single server.

Currently, the CCM packages do not provision the SQL Server Database Permissions that are required for the CCM components to function.  It is assumed that the necessary permissions have already been provided.  By default, two users will require read/write permissions to the CCM Database:

* ChocolateyLocalAdmin - which runs the CCM Windows Service
* IIS APPPOOL/ChocolateyCentralManagment - which runs the CCM IIS Application Pool

**NOTE:** If either of these users are changed during the installation of either of these components, the database permissions will need to be updated to reflect this.

Or, the required username/password for connecting to the CCM database are added as part of the connection string that is passed into the CCM packages during installation.

### Pre-Requisites

In order to install CCM, it is assumed that the following applications/software
are installed, either as a single server installation, or spread over multiple
servers.

1. SQL Server Instance, with administrator access for initial database provision
1. Internet Information Services

### Package Parameters

#### Chocolatey Central Management Database

This package creates the Chocolatey Central Management Database with the following defaults:

* Database Connection String:   **Server=&lt;LOCAL COMPUTER DNS NAME&gt;; Database=ChocolateyManagement; Trusted_Connection=True;**

##### Parameters

You can override the package defaults using the following parameters:

* `/ConnectionString`
  * The SQL Server database connection string to be used to connect to the Chocolatey Central Management database.
  * **NOTE:** Default Value: **Server=&lt;LOCAL COMPUTER DNS NAME&gt;; Database=ChocolateyManagement; Trusted_Connection=True;**
* `/Database`
  * Name of the SQL Server database to use. Note that if you do not also pass `/ConnectionString`, it will be generated using this parameter value and `/SqlServerInstance` (using defaults for missing parameters);
  * **NOTE:** Default Value: **ChocolateyManagement**
* `/SqlServerInstance`
  * Instance name of the SQL Server database to connect to. Note that if you do not also pass `/ConnectionString`, it will be generated using this parameter value and `/Database` (using defaults for missing parameters);
  * **NOTE:** Default Value: **&lt;LOCAL COMPUTER DNS NAME&gt;**

##### Example

Let's assume that you want to install the Chocolatey Central Management Database
onto a machine that will access a SQL Server instance called `SQLSERVERCCM`, on
a domain machine called `MACHINE1` which is part of the domain `ccmtest`, using a specific user name (ccmservice) and password
combination.  In this scenario, the installation command would look like the
following:

```
choco install chocolatey-management-database --package-parameters-sensitive="'/ConnectionString=""Server=MACHINE1\SQLSERVERCCM;Database=ChocolateyManagement;Integrated Security=SSPI;User ID=ccmtest\ccmservice;Password=Password01;""'"
```

**NOTE:** This command makes use of `package-parameters-sensitive` to ensure that
the sensitive information is not leaked out into log files.

**NOTE:** There is an assumption here that the username and password being used have the necessary permissions in order to create the CCM database in the destination SQL Server instance.

#### Chocolatey Central Management Service

This package creates the Chocolatey Central Management Service with the following defaults:

* Service Name:                         **chocolatey-management-service**
* Service Displayname                   **Chocolatey Management Service**
* Description:                          **Chocolatey Management Service is a background service for Chocolatey.**
* Service Startup:                      **Automatic**
* Service Username:                     **ChocolateyLocalAdmin**
* Database Connection String:           **Server=&lt;LOCAL COMPUTER DNS NAME&gt;; Database=ChocolateyManagement; Trusted_Connection=True;**
* Service Listening Port:               **24020**
* Self-Signed Certificate Domain Name:  **DNS name of the local computer**

##### Parameters

You can override the package defaults using the following parameters:

* `/Username`
  * Username to install the management service as;
  * **NOTE:** Default Value: **ChocolateyLocalAdmin**
* `/Password`
  * Password to use for the management service account;
  * **NOTE:** Automatically generated secure password
* `/EnterPassword`
  * This will prompt you to enter the password, during install, for the username (provided via the `/Username` parameter) the management service will run under;
  * **NOTE:** Default Value: Not provided
* `/ConnectionString`
  * The SQL Server database connection string to be used to connect to the Chocolatey Central Management database;
  * **NOTE:** Default Value: **Server=&lt;LOCAL COMPUTER DNS NAME&gt;; Database=ChocolateyManagement; Trusted_Connection=True;**
* `/Database`
  * Name of the SQL Server database to use. Note that if you do not also pass `/ConnectionString`, it will be generated using this parameter value and `/SqlServerInstance` (using defaults for missing parameters);
  * **NOTE:** Default Value: **ChocolateyManagement**
* `/SqlServerInstance`
  * Instance name of the SQL Server database to connect to. Note that if you do not also pass `/ConnectionString`, it will be generated using this parameter value and `/Database` (using defaults for missing parameters);
  * **NOTE:** Default Value: **&lt;LOCAL COMPUTER DNS NAME&gt;**
* `/PortNumber`
  * The port the Chocolatey Management Service will listen on. This will automatically create a rule to open the firewall on this port;
  * **NOTE:** Default Value **24020**
* `/CertificateDnsName`
  * The DNS name of the self-signed certificate that is generated if no existing certificate thumbprint is provided using the `/CertificateThumbprint` parameter is provided;
  * **NOTE:** Default Value: **&lt;LOCAL COMPUTER DNS NAME&gt;**
* `/CertificateThumbprint`
  * By default the Chocolatey Central Management service uses a self-signed SSL certificate to secure communication with the clients. Use this parameter to provide the thumbprint of a certificate to use instead. **Note that is you use this the certificate must already be in the LocalMachine\TrustedPeople Certificate Store on the Chocolatey Management Service computer**;
  * **NOTE:** Default Value: Not applicable as if not provided, a new Self Signed Certificate will be generated
* `/NoRestartService`
  * Explicit request not to restart the service
  * **NOTE:** Default Value: Not provided
* `/DoNotReinstallService`
  * Explicit request not to reinstall the service
  * **NOTE:** Default Value: Not provided

##### Example

Let's assume that you want to install the CCM Windows Service with a specific
connection string in order to connect to the CCM Database, as well as configure
the CCM Service to use a specific user name and password, as well as alter the
Port number that the CCM Service will be hosted on.  The necessary installation
command would look like the following:

```
choco install chocolatey-management-service --package-parameters-sensitive="'/PortNumber=24021 /Username=ccmtest\ccmservice /Password=Password01 /ConnectionString=""Server=MACHINE1\SQLSERVERCCM;Database=ChocolateyManagement;Integrated Security=SSPI;User ID=ccmtest\ccmservice;Password=Password01;""'"
```

**NOTE:** This command makes use of `package-parameters-sensitive` to ensure that
the sensitive information is not leaked out into log files.

#### Chocolatey Management Web

This package creates the Chocolatey Central Management Website and Application Pool with the following defaults:

* IIS Web Application Pool:                 **ChocolateyCentralManagement**
  * enable32BitAppOnWin64:                **True**
  * managedPipelineMode:                  **Integrated**
  * managedRuntimeVersion:                &lt;blank&gt;
  * startMode:                            **AlwaysRunning**
  * processModel.idleTimeout:             **0**
  * recycling.periodicRestart.schedule:   **Disabled**
  * recycling.periodicRestart.time:       **0**
* Website Name:                             **ChocolateyCentralManagement**
  * PortBinding:                          **80**
  * applicationDefaults.preloadEnabled:   **True**
* SQL Server Instance:                      **&lt;LOCAL COMPUTER DNS NAME&gt;**
* Connection String:                        **Server=&lt;LOCAL COMPUTER DNS NAME&gt;; Database=ChocolateyManagement; Trusted_Connection=True;**

##### Parameters

You can override the package defaults using the following parameters:

* `/ConnectionString`
  * The SQL Server database connection string to be used to connect to the Chocolatey Central Management database;
  * **NOTE:** Default Value: **Server=&lt;LOCAL COMPUTER DNS NAME&gt;; Database=ChocolateyManagement; Trusted_Connection=True;**
* `/Database`
  * Name of the SQL Server database to use. Note that if you do not also pass `/ConnectionString`, it will be generated using this parameter value and `/SqlServerInstance` (using defaults for missing parameters);
  * **NOTE:** Default Value: **ChocolateyManagement**
* `/SqlServerInstance`
  * Instance name of the SQL Server database to connect to. Note that if you do not also pass `/ConnectionString`, it will be generated using this parameter value and `/Database` (using defaults for missing parameters);
  * **NOTE:** Default Value: **&lt;LOCAL COMPUTER DNS NAME&gt;**
* `/Username`
  * The username that the IIS WebApplicationPool will run under. If this is not provided the pool will run under the default account. Note that if you provide this you must also provide either the `/Password` or `/EnterPassword` parameter;
  * **NOTE:** Default Value: **IIS APPPOOL\ChocolateyCentralManagement**
* `/Password`
  * The password for the username (provided via the `/Username` parameter) the IIS WebApplicationPool will run under;
  * **NOTE:** Automatically generated secure password
* `/EnterPassword`
  * This will prompt you to enter the password, during install, for the username (provided via the `/Username` parameter) the IIS WebApplicationPool will run under;
  * **NOTE:** Default Value: Not provided

##### Example

Let's assume that you want to install the CCM Web Site with a specific
connection string in order to connect to the CCM Database, as well as configure
the IIS Application Pool to use a specific user name and password.  The
necessary installation command would look like the following:

```
choco install chocolatey-management-web --package-parameters-sensitive="'/ConnectionString=""Server=MACHINE1\SQLSERVERCCM;Database=ChocolateyManagement;User ID=ccmtest\ccmservice;Password=Password01;"" /Username=ccmwebserver\ccmserviceuser /Password=Password01'"`
```

**NOTE:** This command makes use of `package-parameters-sensitive` to ensure that
the sensitive information is not leaked out into log files.

**NOTE:** Once installed, when you access the CCM Web Site you will be prompted to provide a username
and password to access the site.  By default, the username is `ccmadmin` and the password is `123qwe`.
After you input this, you will be prompted to change the password.

#### Chocolatey Configuration for Chocolatey Central Management

The following configuration values, with their default values, are added into the chocolatey.config file after installing Chocolatey Central Management and it's dependent packages.

##### centralManagementReportPackagesTimerIntervalInSeconds

This is the length of time, in seconds, that the Chocolatey Background Agent will wait between each attempt to report into Chocolatey Central Management.

**Default Value:** 1800

##### centralManagementServiceUrl

This is the URL that is used by the Chocolatey Background Agent to report into Chocolatey Central Management, and also by the Chocolatey Central Management Service to register the URL that it is listening for incoming reports on.

**Default Value:** _blank_

**NOTE:** If left blank, the Chocolatey Central Management Service will construct a URL based on the default Port number which is 24020, and the FQDN of the machine that the service is being executed on.

**NOTE:** Due to the fact that both the Chocolatey Background Agent and Chocolatey Central Management Service use this configuration value, if both of these services are located on the same machine, they Chocolatey Background Service on that machine has to report into the Chocolatey Central Management Service on that machine.  It can't report into another instance.

##### centralManagementReceiveTimeoutInSeconds

This is the length of time, in seconds, that a connection to Chocolatey Central Management can remain inactive, during which no application messages are received, before it is dropped.

**Default Value:** 30

##### centralManagementSendTimeoutInSeconds

This is the length of time, in seconds, that a write operation against Chocolatey Central Management has to complete before the transport raises an exception.

**Default Value:** 30

##### centralManagementCertificateValidationMode

This captures the options for determining the validity of the Chocolatey Central Management Service certificate obtained using SSL/TLS negotiation.

**Default Value:** PeerOrChainTrust

**Valid Values:** None, PeerTrust, ChainTrust, PeerOrChainTrust, Custom

#### Chocolatey Clients

Once CCM has been set up and configured, each machine that you want to report
into CCM will have to be enabled.  This can be done by doing the following:

```powershell
choco config set CentralManagementServiceUrl https://ccmsrvserver:24021/ChocolateyManagementService
choco feature enable -n useChocolateyCentralManagement
```

Here, the full URL, including the port number, to where the CCM service was
installed to is being set, and then the `useChocolateyCentralManagement` feature
is being enabled. In your environment you would replace `https://ccmsrvserver:24021` with the FQDN name of your server and the port you set.

**NOTE:** By default, this feature is disabled, and will need to be turned on.

**NOTE:** If not set, the Chocolatey Central Management Service will construct a URL based on the default Port number which is 24020, and the FQDN of the machine that the service is being executed on.

Additional configuration exists for CCM Service, which allows fine grained
control of how Chocolatey Agent will report into CCM.  For example:

```powershell
choco config set centralManagementReportPackagesTimerIntervalInSeconds 60
choco config set centralManagementReceiveTimeoutInSeconds 60
choco config set centralManagementSendTimeoutInSeconds 60
choco config set centralManagementCertificateValidationMode "PeerOrChainTrust"
```

## FAQ

### Will this become available for lower editions of Chocolatey?

The Chocolatey Central Management system will only be available in C4B (Chocolatey for Business).

### What's the minimum version of the Chocolatey packages I need to use CCM?

See [Requirements](#requirements).

## Common Errors and Resolutions

As common issues are encountered and resolved, we will list them here.
