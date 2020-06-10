# Central Mangement Windows Service(s) Setup

<!-- TOC depthFrom:2 depthTo:5 -->

- [Summary](#summary)
- [Step 1: Complete Prerequisites](#step-1-complete-prerequisites)
  - [Script for some prerequisites](#script-for-some-prerequisites)
- [Step 2: Install Central Management Service Package](#step-2-install-central-management-service-package)
  - [FQDN Usage](#fqdn-usage)
  - [Package Parameters](#package-parameters)
  - [Chocolatey Configuration](#chocolatey-configuration)
  - [Chocolatey Managed Password](#chocolatey-managed-password)
  - [Chocolatey Central Management Service Windows Account Considerations](#chocolatey-central-management-service-windows-account-considerations)
  - [Scenarios](#scenarios)
    - [SQL Server Windows Authentication](#sql-server-windows-authentication)
      - [Use Active Directory Domain Account](#use-active-directory-domain-account)
      - [Use Local Windows Account to Local SQL Server](#use-local-windows-account-to-local-sql-server)
      - [Use Local Windows Account to Remote SQL Server](#use-local-windows-account-to-remote-sql-server)
    - [SQL Server Account Authentication](#sql-server-account-authentication)
      - [Use SQL Server Authentication Locally](#use-sql-server-authentication-locally)
      - [Use SQL Server Account to Remote SQL Server](#use-sql-server-account-to-remote-sql-server)
- [Step 3: Verify Installation](#step-3-verify-installation)
- [FAQ](#faq)
- [Common Errors and Resolutions](#common-errors-and-resolutions)

<!-- /TOC -->

## Summary

This is the service that the agents (chocolatey-agent) communicates with. You could install one or more of these depending on the size of your environment (not multiple on one machine though). The FQDN and certificate used determine what the URL will be for the agents to check into Central Management.

____
## Step 1: Complete Prerequisites

* > :warning: The [[database|CentralManagementSetupDatabase]] must be setup and available, along with [[logins and access|CentralManagementSetupDatabase#step-2-set-up-sql-server-logins-and-access]].
* Windows Server 2012+
* PowerShell 4+
* .NET Framework 4.6.1+

### Script for some prerequisites

```powershell
choco install dotnet4.6.1 -y
```

___
## Step 2: Install Central Management Service Package

By default the service will install as a local administrative user `ChocolateyLocalAdmin` (and manage the password as well). However you can specify your own user with package parameters (such as using a domain account). You will need to specify credentials to the database as we'll see in scenarios below.

> :warning: **WARNING**
>
> Timezones are super important here and time synchronization is really important when generating SSL Certificates. You want to make sure you have this correct and good. Otherwise there is a potential edge case you could generate an SSL Certificate that is not yet valid. As the service package could generate an SSL certificate if you don't pass an existing thumbprint, its best to ensure that time synchronization is not an issue with the machine you are installing this on.

### FQDN Usage

When installing the CCM Service, the default is to use the Fully Qualified Domain Name (FQDN) of the machine that it is being installed on.  As a result, there is an expectation that the certificate (either the self signed certificate that is created during installation, or the existing certificate which is configured with the [CertifcateThumbprint](#package-parameters-1) parameter) that is used to secure the transport layer of this service, also uses the same FQDN.

If this is not the case, it will be necessary to provide the information to the package about the actual name for the machine that is being used.  When using a self signed certificate, this can be specified using the `CertifcateDnsName`, and when using an existing certificate, no additional parameters are required.  In both cases, it will be necessary to also set the `centralManagementServiceUrl` [configuration parameter](#centralmanagementserviceurl).  This can be done using the following command:

~~~powershell
choco config set CentralManagementServiceUrl https://<accessible_name_of_machine>:24020/ChocolateyManagementService
~~~

### Package Parameters
Note items with "`:`" mean a value should be provided, items without are simply switches.

* `/Username:` - Username to run the service under. Defaults to `ChocolateyLocalAdmin`.
* `/Password:` - Password for the user. Default is the [Chocolatey Managed Password](#chocolatey-managed-password).
* `/EnterPassword` - Receive the password at runtime as a secure string. Requires input at runtime whe installing/upgrading the package.
* `/NoRestartService` - Do not shut down and restart the service. You will need to restart later to take advantage of new service information.
* `/DoNotReinstallService` - Do not re-install the service.
* `/PortNumber:` - The port the Chocolatey Management Service will listen on. This will automatically create a rule to open the firewall on the port specified. Defaults to `24020`.
* `/CertificateDnsName:` - The DNS name of the self-signed certificate that is generated if no existing certificate thumbprint is provided using the `/CertificateThumbprint` parameter (below). Defaults to `<LOCAL COMPUTER FQDN NAME>`.
* `/CertificateThumbprint:` - Provide the thumbprint of an existing certificate (already installed in `LocalMachine\TrustedPeople` certificate store) to use for secure communication with clients. Defaults to a new self-signed SSL certificate on first installation / reuses existing on upgrades.
* `/ConnectionString:` - The SQL Server database connection string to be used to connect to the Chocolatey Central Management database. Defaults to default or explicit values for `/SqlServiceInstance` and `/Database`, along with Integrated Security (`Server=<LOCAL COMPUTER FQDN NAME>; Database=ChocolateyManagement; Trusted_Connection=True;`). The account should have `db_datareader`/`db_datawriter` access to the database ([data reader / data writer](https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/database-level-roles#fixed-database-roles)).
* `/SqlServerInstance:` - Instance name of the SQL Server database to connect to. Alternative to passing full connection string with `/ConnectionString`. Uses `/Database` (below) to build a connection string. Defaults to `<LOCAL COMPUTER FQDN NAME>`.
* `/Database:` - Name of the SQL Server database to use. Alternative to passing full connection string with `/ConnectionString`. Uses `/SqlServerInstance` (above) to build a connection string. Defaults to `ChocolateyManagement`.

**NOTE:** If the Chocolatey Agent is installed on the same machine that has the CCM Service installed, it can only report into that CCM Service as they will share a

### Chocolatey Configuration

* `centralManagementServiceUrl` = **' '** (empty) - The URL that should be used to communicate with Chocolatey Central Management. It should look something like https://servicemachineFQDN:24020/ChocolateyManagementService. See https://chocolatey.org/docs/central-management-setup-service#fqdn-usage. Defaults to '' (empty). NOTE: Chocolatey Agent and CCM Service share this value on a machine that contains both. If blank, the CCM Service will construct a URL based on defaults of the machine, but is required to be set for Agents.


### Chocolatey Managed Password

When Chocolatey manages the password for a local administrator, it creates a very complex password:

* It is 32 characters long.
* It uses uppercase, lowercase, numbers, and symbols to meet very stringent complexity requirements.
* The password is different for every machine.
* Due to the way that it is generated, it is completely unguessable.
* No one at Chocolatey Software could even tell you what the password is for a particular machine without local access.

### Chocolatey Central Management Service Windows Account Considerations

* Windows Account (required, defaults to `ChocolateyLocalAdmin`)
  * The Chocolatey Central Management Service requires ***an*** administrative account, whether that is a domain account or a local account - it just needs to be a local admin (a member of the Administrators group).
  * The Chocolatey Central Management Service doesn't specifically require the `ChocolateyLocalAdmin` account, any Windows account can be used. The `ChocolateyLocalAdmin` is used as the default if one is not specified.
  * Upon use of an account during installation, it will make that account a member of the Administrators account.
  * The account used will also be granted LogonAsService and LogonAsBatch privileges.
* Managed Password (optional, default)
  * When the `ChocolateyLocalAdmin` account is used, it generates a managed password that is different on every machine, 32 characters long, meets complexity requirements, and basically very strong.
  * To determine the managed password, it would take access to the box and someone from Chocolatey Software who has access to the algorithm used to generate the password (more information in the FAQs below).
* Rotating/Updating Passwords
  * If a different account with a rotating password is used, the service will need to be updated with the new credentials and restarted soon after changing that password.
  * The managed password is not currently updated/rotated, but it is something we are looking at how best to implement.

### Scenarios
#### SQL Server Windows Authentication
##### Use Active Directory Domain Account
Scenario 1: Active Directory - you have set up the [[database|CentralManagementSetupDatabase]] to use Windows Authentication (or Mixed Mode Authentication).

```powershell
choco install chocolatey-management-service -y --package-parameters="'/ConnectionString:""Server=<RemoteSqlHost>;Database=ChocolateyManagement;Trusted_Connection=True;"" /Username:<DomainAccount>'" --package-parammeters-sensitive="'/Password:<domain account password>'"
```

> :memo: **NOTE**: Note the connection string doesn't include credentials. That's because Windows Authentication for SQL Server uses the context of what is running it and why the service itself needs the right user/password.

##### Use Local Windows Account to Local SQL Server
Scenario 2: Monolithic - you have set up the [[database|CentralManagementSetupDatabase]] to use Windows Authentication (or Mixed Mode Authentication). You wish to use a local Windows account to connect to the local database.

* Specify User:

```powershell
choco install chocolatey-management-service -y --package-parameters="'/ConnectionString:""Server=<Localhost\SQLEXPRESS>;Database=ChocolateyManagement;Trusted_Connection=True;"" /Username:<LocalWindowsAccount>'" --package-parammeters-sensitive="'/Password:<Local account password>'"
```

* ChocolateyLocalAdmin User:

```powershell
choco install chocolatey-management-service -y --package-parameters="'/ConnectionString:""Server=<Localhost\SQLEXPRESS>;Database=ChocolateyManagement;Trusted_Connection=True;""'"
```

> :memo: **NOTE**: Note the connection string doesn't include credentials. That's because Windows Authentication for SQL Server uses the context of what is running it and why the service itself needs the right user/password.


##### Use Local Windows Account to Remote SQL Server
Scenario 3: you have set up the [[database|CentralManagementSetupDatabase]] to use Windows Authentication (or Mixed Mode Authentication). You wish to use a local Windows account to connect to a remote database (on another computer).

> :warning: **WARNING**
>
> STOP right here.
> This is an invalid scenario and will not work. Please look at one of the other options. If you don't have LDAP, you will want to look at [SQL Server Account Authentication](#sql-server-account-authentication) below.

It's worth noting here that `ChocolateyLocalAdmin` on two boxes is NOT the same account, so there is no way for Windows to recognize the account from a different box.


#### SQL Server Account Authentication
##### Use SQL Server Authentication Locally
Scenario 4: Monolithic - you are installing the management service on the same machine as a SQL Server Express instance. You likely have a smaller environment where you have up to 1,000 machines. You have set up the [[database|CentralManagementSetupDatabase]] to use Mixed Mode Authentication.

```powershell
choco install chocolatey-management-service -y --package-parameters-sensitive="'/ConnectionString:""Server=Localhost;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```

* SQL Server Express:

```powershell
choco install chocolatey-management-service -y --package-parameters-sensitive="'/ConnectionString:""Server=Localhost\SQLEXPRESS;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```

##### Use SQL Server Account to Remote SQL Server
Scenario 5: Split - you are installing the management service(s) on a server, and targeting an existing SQL Server instance in your organization. You have set up the [[database|CentralManagementSetupDatabase]] to use Mixed Mode Authentication.

```powershell
choco install chocolatey-management-service -y --package-parammeters-sensitive="'/ConnectionString:""Server=<RemoteSqlHost>;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```

___
## Step 3: Verify Installation

The `chocolatey-management-service` is responsible for making a number of changes to your system.  A successful installation of this package can be verified by:

* Open the services snap-in (services.msc) and check for the presence of the `Chocolatey Management Service` which should be in the started state
* Open the Service log file located at `$env:ChocolateyInstall\logs\ccm-service.log` and verify that there are no recently reported errors. If you are on a version of CCM prior to 0.2.0, the log will be located at `$env:ChocolateyInstall\lib\chocolatey-management-service\tools\service\logs\chocolatey.service.host.log`.

___
## FAQ

___
## Common Errors and Resolutions

[[Central Management Setup|CentralManagementSetup]] | [[Chocolatey Central Management|CentralManagement]]
