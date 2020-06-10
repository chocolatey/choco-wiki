# Central Mangement Windows Service(s) Setup

<!-- TOC depthFrom:2 -->

- [Step 1: Install Central Management Service Package](#step-1-install-central-management-service-package)
  - [Package Parameters](#package-parameters)
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
- [FAQ](#faq)
- [Common Errors and Resolutions](#common-errors-and-resolutions)

<!-- /TOC -->

> :warning: **WARNING**: Ensure you have completed installing the [[database package|CentralManagementSetupDatabase]] and have set up sql server [[logins and access|CentralManagementSetupDatabase#step-2-set-up-sql-server-logins-and-access]].

> :memo: **NOTE**: You could be installing several of these if your environment is large enough.

> :warning: **WARNING**
>
> Timezones are super important here and time synchronization is really important when generating SSL Certificates. You want to make sure you have this correct and good. Otherwise there is a potential edge case you could generate an SSL Certificate that is not yet valid. As the service package could generate an SSL certificate if you don't pass an existing thumbprint, its best to ensure that time synchronization is not an issue with the machine you are installing this on.

> :note: **NOTE**
The installation of this package will assume that a new SSL certificate will need to created and used. This will be generated using the fully-qualified domain name of the system on which you are installing this package. It will also be assumed that port 24020 will be used to facilitate communication between an agent and the management service. The following parameters can be optionally provided to override this default behavior:

## Step 1: Install Central Management Service Package

By default the service will install as a local administrative user `ChocolateyLocalAdmin` (and manage the password as well). However you can specify your own user with package parameters (such as using a domain account). You will need to specify credentials to the database as we'll see in scenarios below.

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

Once you have completed the steps necessary to allow Sql access for the user in Step 2, we can install the management service.

## Scenarios
### SQL Server Windows Authentication
#### Use Active Directory Domain Account
Scenario 1: Active Directory - you have set up the [[database|CentralManagementSetupDatabase]] to use Windows Authentication (or Mixed Mode Authentication).

```powershell
choco install chocolatey-management-service -y --package-parameters="'/ConnectionString:""Server=<RemoteSqlHost>;Database=ChocolateyManagement;Trusted_Connection=True;"" /Username:<DomainAccount>'" --package-parammeters-sensitive="'/Password:<domain account password>'"
```

> :memo: **NOTE**: Note the connection string doesn't include credentials. That's because Windows Authentication for SQL Server uses the context of what is running it and why the service itself needs the right user/password.

#### Use Local Windows Account to Local SQL Server
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


#### Use Local Windows Account to Remote SQL Server
Scenario 3: you have set up the [[database|CentralManagementSetupDatabase]] to use Windows Authentication (or Mixed Mode Authentication). You wish to use a local Windows account to connect to a remote database (on another computer).

> :warning: **WARNING**
>
> STOP right here.
> This is an invalid scenario and will not work. Please look at one of the other options. If you don't have LDAP, you will want to look at [SQL Server Account Authentication](#sql-server-account-authentication) below.

It's worth noting here that `ChocolateyLocalAdmin` on two boxes is NOT the same account, so there is no way for Windows to recognize the account from a different box.


### SQL Server Account Authentication
#### Use SQL Server Authentication Locally
Scenario 4: Monolithic - you are installing the management service on the same machine as a SQL Server Express instance. You likely have a smaller environment where you have up to 1,000 machines. You have set up the [[database|CentralManagementSetupDatabase]] to use Mixed Mode Authentication.

```powershell
choco install chocolatey-management-service -y --package-parameters-sensitive="'/ConnectionString:""Server=Localhost;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```

* SQL Server Express:

```powershell
choco install chocolatey-management-service -y --package-parameters-sensitive="'/ConnectionString:""Server=Localhost\SQLEXPRESS;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```

#### Use SQL Server Account to Remote SQL Server
Scenario 5: Split - you are installing the management service(s) on a server, and targeting an existing SQL Server instance in your organization. You have set up the [[database|CentralManagementSetupDatabase]] to use Mixed Mode Authentication.

```powershell
choco install chocolatey-management-service -y --package-parammeters-sensitive="'/ConnectionString:""Server=<RemoteSqlHost>;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```


## FAQ

## Common Errors and Resolutions

[[Central Management Setup|CentralManagementSetup]] | [[Chocolatey Central Management|CentralManagement]]
