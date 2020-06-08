# Chocolatey Central Mangement Setup

> :warning: **WARNING**: This is a Work in Progress. Please check back later
>
> Also see [[Chocolatey Central Management in Features|FeaturesChocolateyCentralManagement]].

<!-- TOC depthFrom:2 -->

- [Summary](#summary)
- [Step 0: Complete Prerequisites](#step-0-complete-prerequisites)
    - [Step 0.1: Internalize needed packages](#step-01-internalize-needed-packages)
    - [Step 0.2: Install Or Prepare SQL Server](#step-02-install-or-prepare-sql-server)
- [Step 1: Install Central Management Database Package](#step-1-install-central-management-database-package)
- [Step 2: Set up SQL Server Logins And Users](#step-2-set-up-sql-server-logins-and-users)
- [Step 3: Install Central Mangement Service Package](#step-3-install-central-mangement-service-package)
    - [Dependency Installation](#dependency-installation)
- [Step 4: Install Central Management Web Package](#step-4-install-central-management-web-package)

<!-- /TOC -->

## Summary

> :warning: **WARNING**: This is a Work in Progress. Please check back later
>
> Also see [[Chocolatey Central Management in Features|FeaturesChocolateyCentralManagement]].

Installing CCM takes a little more pre-thought than simply running the package installs.


> :memo: **NOTE**
>
> Please read through all of this prior to running installation as you could run into issues that require support to help you correct later.


> :warning: **WARNING**
>
> Unless otherwise noted, please follow these steps in ***exact*** order. These steps build on each other and need to be completed in order.

> :memo: **NOTE**
>
> If this seems like a lot to set up, you have the ability to get access to the [[Quick Deployment Environment (QDE)|QuickDeploymentEnvironment]]. It comes preloaded with Central Management and other Chocolatey recommended infrastructure. Please see [[Quick Deployment Environment (QDE)|QuickDeploymentEnvironment]].

___
## Step 0: Complete Prerequisites

### Step 0.1: Internalize needed packages

The complete installation of CCM requires several packages that are available from the community repository. Let's get them internalized. We will internalize them to a C:\packages directory. It is highly recommended that you push the packages to an internal repository before continuing with other steps in this guide. Change the values in the last line of this script to match what you need in your environment.

```powershell
if(!(Test-Path C:\packages)){
  $null = New-Item C:\packages -ItemType Directory
}

@('sql-server-express','sql-server-management-studio') | Foreach-Object {
  choco download $_ --internalize --ouput-directory C:\packages
}

@('aspnetcore-runtimepackagestore','dotnetcore-windowshosting') | Foreach-Object {
  choco download $_ --version 2.2.7 --internalize --output-directory C:\packages
}

Get-ChildItem C:\packages -Recurse -Filter *.nupkg | Foreach-Object { choco push $_.Fullname --source="'https://yourRepoUrl'" --api-key="'YourApiKeyHere'"}
```

### Step 0.2: Install Or Prepare SQL Server

> :memo: **NOTE**: While we'd like to support different database engines at some point in the distant future, currently only SQL Server is supported.

CCM will not install or take a dependency on a database engine install as there are different editions that could be installed and multiple packages out there. At this time, it is expected that you have this ready. This is required before you can continue to other steps.

* You need a SQL Server set up somewhere. Editions really don't matter until you have a large number of computers checking in.
* SQL Server should support mixed mode for logins (unless you are going to use AD authentication). 98% of the time you are going to want mixed mode authentication for SQL Server unless you hit options.
* You need to create the user access to the database (logins at the server level/users at the db level).

> :warning: **WARNING**
>
> SQL Server Mixed Mode Authentication is what you will want for ease of installation. If you decide you need to go Windows Authentication (aka integrated security), you ***will*** need to ensure the following addtional items:
> * **You *must* have active directory** - Full stop. Local machine accounts can not authenticate to remote machines (nor SQL Server instances on remote machines).
> * **SQL Server machine security** - ensure the domain accounts being used for the service and web are not local administrators (members of the `BUILTIN\Administrators`) group on the machine that contains the SQL Server instance, or they will have `sysadmin` privileges by default to the SQL Server instance (until removed).
> * **Central Management Service installation** - You'll need to use an Active Directory (LDAP) account. See the install options for how to pass that through.
>    * ***!!Security!!*** - As part of installation, an account will be made a member of the `BUILTIN\Administrators` group on the machine where the service is installed. Ensure that is ***not*** the same machine where SQL Server is installed or that account will immediately be a member of the `sysadmin` role by default in SQL Server (until removed).
> * **Central Management Web installation** - After you finish the installation, you need to go back and change the user the account is running in IIS / Application Pools to an Active Directory account. Then reset the website (the out of band exe as well). **NOTE**: There are no install options for this like there are the service at this time, so you will be doing this by hand or scripting it separately.
>
> :memo: Incorrect credentials to the database is 90% of support tickets related to Central Management.
>
>Unless you are an expert in in hooking things up to SQL Server, its probably best to stick with SQL Server Mixed Mode Authentication.
> See https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/authentication-in-sql-server

The quickest option to get going with the database is to use the Community Repository, or an internalized version of a sql server package from the community repository For SQL Server Express 2019, run the following:

```powershell
choco install sql-server-express -y
```

You will also want to have the management tools installed, which can be installed with:

```powershell
choco install sql-server-management-studio -y
```

___

## Step 1: Install Central Management Database Package

> :warning: **WARNING**: CCM packages do ***NOT*** install SQL Server. You must take care of that in the prerequisites. Do not even start on central management installs until you have a SQL Server instance up and ready. I repeat, SQL Server engine must be already installed.

The CCM database package will add or update a database to an existing SQL Server instance.

When you run this package installation, you will want to do so as integrated security, or with Windows Authentication. When you run the other two package installations, you will want to do so providing a connection string.

Scenario 1: You are installing the database package on the same machine as a SQL Server Express installation:

```powershell
choco install chocolatey-management-database -y --package-paramaeters-sensitive="'/ConnectionString=""Server=Localhost\SQLEXRESS;Database=ChocolateyManagement;Trusted_Connection=true;""'"
```

Scenario 2: You are installing the database package on a single server, but connecting to an existing SQL Server in your environment:

```powershell
choco install chocolatey-management-database -y --package-parameters-sensitive="'/ConnectionString=""Server=DatabaseServer;Database=ChocolateyManagement;Trusted_Connection=true;""'"
```

___

## Step 2: Set up SQL Server Logins And Users

Once we have the database, we can create logins and map those logins to users in the database.

> :warning: **WARNING**: CCM packages do ***NOT*** configure SQL Server access either.

The difference between a login and a user when it comes to SQL Server accounts has long confused folks. Simply put:

- Login (Authentication) - A login is at instance level (the credentials) - [https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/create-a-login](https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/create-a-login)
- User (Authorization) - A user is that login being mapped to a database and given roles/privileges (an instance can contain multiple databases) - [https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/create-a-database-user](https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/create-a-database-user)

Notes:

- Grant `db_reader` and `db_writer` to the accounts you create for the web and service.
- You can share the same login for the two accounts, unless your internal best practices dictate using different passwords.


```powershell
function Add-DatabaseUserAndRoles {
  param(
   [parameter(Mandatory=$true)][string] $UserName,
   [parameter(Mandatory=$true)][string] $DatabaseName,
   [parameter(Mandatory=$false)][string] $DatabaseServer = 'localhost\SQLEXPRESS',
   [parameter(Mandatory=$false)] $DatabaseRoles = @('db_datareader'),
   [parameter(Mandatory=$false)][string] $DatabaseServerPermissionsOptions = 'Trusted_Connection=true;',
   [parameter(Mandatory=$false)][switch] $CreateSqlUser,
   [parameter(Mandatory=$false)][string] $SqlUserPassword
  )


  $LoginOptions = 'FROM WINDOWS'
  if ($CreateSqlUser) {
    $LoginOptions = "WITH PASSWORD '$SqlUserPassword'"
  }

  $addUserSQLCommand = @"
USE [master]
IF EXISTS(SELECT * FROM msdb.sys.syslogins WHERE UPPER([name]) = UPPER('$UserName'))
  BEGIN
    DROP LOGIN [$UserName]
  END
CREATE LOGIN [$UserName] $LoginOptions WITH DEFAULT_DATABASE=[$DatabaseName]

USE [$DatabaseName]
IF EXISTS(SELECT * FROM sys.sysusers WHERE UPPER([name]) = UPPER('$UserName'))
  BEGIN
    DROP USER [$UserName]
  END

CREATE USER [$UserName] FOR LOGIN [$UserName]

"@

  foreach ($DatabaseRole in $DatabaseRoles) {
    $addUserSQLCommand += @"

ALTER ROLE [$DatabaseRole] ADD MEMBER [$UserName]
"@
  }

  Write-Output "Adding $UserName to $DatabaseName with the following permissions: $($DatabaseRoles -Join ', ')"
  Write-Debug "running the following: \n $addUserSQLCommand"


  $Connection = New-Object System.Data.SQLClient.SQLConnection
  $Connection.ConnectionString = "server='$DatabaseServer';database='master';$DatabaseServerPermissionsOptions"
  $Connection.Open()
  $Command = New-Object System.Data.SQLClient.SQLCommand
  $Command.CommandText = $addUserSQLCommand
  $Command.Connection = $Connection
  $Command.ExecuteNonQuery()
  $Connection.Close()

}

Add-DatabaseUserAndRoles -Username 'ChocoUser' -SqlUserPassword 'Ch0c0R0cks' -CreateSqlUser
```

___

## Step 3: Install Central Mangement Service Package

> :warning: **WARNING**: Ensure you have completed installing the database package and have set up sql server logins and accounts.

> :memo: **NOTE**: You could be installing several of these if your environment is large enough.

> :warning: **WARNING**
>
> Timezones are super important here and time synchronization is really important when generating SSL Certificates. You want to make sure you have this correct and good. Otherwise there is a potential edge case you could generate an SSL Certificate that is not yet valid. As the service package could generate an SSL certificate if you don't pass an existing thumbprint, its best to ensure that time synchronization is not an issue with the machine you are installing this on.

> :note: **NOTE**
The installation of this package will assume that a new SSL certificate will need to created and used. This will be generated using the fully-qualified domain name of the system on which you are installing this package. It will also be assumed that port 24020 will be used to facilitate communication between an agent and the management service. The following parameters can be optionally provided to override this default behavior:

### Package Parameters

- `/Username`
  - Username to install the management service as;
NOTE: Default Value: ChocolateyLocalAdmin
- `/Password`
  - Password to use for the management service account;
  - NOTE: Automatically generated secure password
- `/EnterPassword`
  - This will prompt you to enter the password, during install, for the username (provided via the /Username parameter) the management service will run under;
  - NOTE: Default Value: Not provided
- `/ConnectionString`
  - The SQL Server database connection string to be used to connect to the CCM database;
  - NOTE: Default Value: Server=<LOCAL COMPUTER FQDN NAME>; Database=ChocolateyManagement; Trusted_Connection=True;
- `/Database`
  - Name of the SQL Server database to use. Note that if you do not also pass /ConnectionString, it will be generated using this parameter value and /SqlServerInstance (using defaults for missing parameters);
  - NOTE: Default Value: ChocolateyManagement
- `/SqlServerInstance`
  -Instance name of the SQL Server database to connect to. Note that if you do not also pass /ConnectionString, it will be generated using this parameter value and /Database (using defaults for missing parameters);
  - NOTE: Default Value: <LOCAL COMPUTER FQDN NAME>
- `/PortNumber`
  - The port the Chocolatey Management Service will listen on. This will automatically create a rule to open the firewall on this port;
  - NOTE: Default Value 24020
- `/CertificateDnsName`
  - The DNS name of the self-signed certificate that is generated if no existing certificate thumbprint is provided using the /CertificateThumbprint parameter is provided;
  - NOTE: Default Value: <LOCAL COMPUTER FQDN NAME>
- `/CertificateThumbprint`
  - By default the CCM Service uses a self-signed SSL certificate to secure communication with the clients. Use this parameter to provide the thumbprint of a certificate to use instead. Note that if you use this the certificate must already be in the LocalMachine\TrustedPeople Certificate Store on the Chocolatey Management Service computer;
  - NOTE: Default Value: Not applicable as if not provided, a new Self Signed Certificate will be generated
- `/NoRestartService`
  - Explicit request not to restart the service
  - NOTE: Default Value: Not provided
- `/DoNotReinstallService`
  - Explicit request not to reinstall the service
  - NOTE: Default Value: Not provided



Once you have completed the steps necessary to allow Sql access for the user in Step 2, we can install the management service.

Scenario 1: You are installing the management service on the same machine as a SQL Server Express instance

```powershell
choco install chocolatey-management-service -y --package-parameters-sensitive="'/ConnectionString=""Server=Localhost\SQLEXPRESS;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```

Scenario 2: You are installing the management service on a server, and targeting an existing SQL Server instance in your organization

```powershell
choco install chocolatey-management-service -y --package-parammeters-sensitive="'/ConnectionString=""Server=RemoteSqlHost;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```

___

## Step 4: Install Central Management Web Package

> :warning: **WARNING**: Ensure you have completed installing the database package first on whatever machine the database is on.

> :memo: **NOTE**: At this time we don't recommend opening internet access to CCM web. However, if you choose to, you will want to set up SSL/TLS certificates to ensure communication is encrypted over the internet.

### Dependency Installation

The CCM Service and Web packages are built on ASP .Net Core, and as such we need to ensure that it is installed on the server for proper function. Note, that the codebase is currently locked to version `2.2.7` of these packages, and it is critical that you install these right, otherwise you will encounter errors.

```powershell
choco install IIS-WebServer -s windowsfeatures -y
choco install IIS-ApplicationInit -s windowsfeatures -y
choco install aspnetcore-runtimepackagestore --version 2.2.7 -y
choco install dotnetcore-windowshosting --version 2.2.7 -y
```

> :note: **NOTE** In 90% of cases, accepting the defaults this package provides is sufficient, however if you have specific use cases warranting you change anything will the installation the following parameters can be provided:

- `/ConnectionString`
  - The SQL Server database connection string to be used to connect to the CCM Database;
  - NOTE: Default Value: Server=<LOCAL COMPUTER FQDN NAME>; Database=ChocolateyManagement; Trusted_Connection=True;
- `/Database`
  - Name of the SQL Server database to use. Note that if you do not also pass /ConnectionString, it will be generated using this parameter value and /SqlServerInstance (using defaults for missing parameters);
  - NOTE: Default Value: ChocolateyManagement
- `/SqlServerInstance`
  - Instance name of the SQL Server database to connect to. Note that if you do not also pass /ConnectionString, it will be generated using this parameter value and /Database (using defaults for missing parameters);
  - NOTE: Default Value: <LOCAL COMPUTER FQDN NAME>
- `/Username`
  - The username that the IIS WebApplicationPool will run under. If this is not provided the pool will run under the default account. Note that if you provide this you must also provide either the /Password or /EnterPassword parameter;
   -NOTE: Default Value: IIS APPPOOL\ChocolateyCentralManagement
- `/Password`
  - The password for the username (provided via the /Username parameter) the IIS WebApplicationPool will run under;
  - NOTE: Automatically generated secure password
- `/EnterPassword`
  - This will prompt you to enter the password, during install, for the username (provided via the /Username parameter) the IIS WebApplicationPool will run under;
  - NOTE: Default Value: Not provided


Scenario 1: You are installing the management web components on the same machine as a SQL Server Express instance

```powershell
choco install chocolatey-management-web -y --package-parameters-sensitive="'/ConnectionString=""Server=Localhost\SQLEXPRESS;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```

Scenario 2: You are installing the management web components on a server, and targeting an existing SQL Server instance in your organization

```powershell
choco install chocolatey-management-web -y --package-parammeters-sensitive="'/ConnectionString=""Server=RemoteSqlHost;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```

[[Chocolatey Central Management|CentralManagement]]
