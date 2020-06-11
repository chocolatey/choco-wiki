# Chocolatey Central Management Database Setup

<!-- TOC depthFrom:2 depthTo:5 -->

- [Summary](#summary)
- [Step 1: Complete Prerequisites](#step-1-complete-prerequisites)
  - [Step 1.1: Install SQL Server](#step-11-install-sql-server)
    - [Install SQL Server Express](#install-sql-server-express)
  - [Step 1.2: Prepare SQL Server](#step-12-prepare-sql-server)
    - [Script to Prepare SQL Server Express](#script-to-prepare-sql-server-express)
- [Step 2: Install Central Management Database Package](#step-2-install-central-management-database-package)
  - [Package Parameters](#package-parameters)
  - [Scenarios](#scenarios)
- [Step 3: Set up SQL Server Logins And Access](#step-3-set-up-sql-server-logins-and-access)
- [Step 4: Verify Installation](#step-4-verify-installation)
- [FAQ](#faq)
  - [Can I use MySQL (or PostgreSQL)?](#can-i-use-mysql-or-postgresql)
- [Common Errors and Resolutions](#common-errors-and-resolutions)
  - [Chocolatey Central Management database package installs without error, but ChocolateyManagement database is not created](#chocolatey-central-management-database-package-installs-without-error-but-chocolateymanagement-database-is-not-created)
  - [The term 'Install-ChocolateyAppSettingsJsonFile' is not recognized as the name of a cmdlet, function, script file, or operable program.](#the-term-install-chocolateyappsettingsjsonfile-is-not-recognized-as-the-name-of-a-cmdlet-function-script-file-or-operable-program)
  - [Cannot process command because of one or more missing mandatory parameters: FilePath](#cannot-process-command-because-of-one-or-more-missing-mandatory-parameters-filepath)

<!-- /TOC -->

## Summary

At the end of this, we should have a fully ready to go SQL Server:

* Installed
* Configured
* Database package deployed creating the database
* Permissions added

> :warning: **WARNING**
>
> Unless otherwise noted, please follow these steps in ***exact*** order. These steps build on each other and need to be completed in order.

___
## Step 1: Complete Prerequisites

* SQL Server

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
> * **Central Management Web installation** - You'll need to use an Active Directory (LDAP) account. See the install options for how to pass that through to be set with the IIS Application Pool.
>
> :memo: Incorrect credentials to the database is 90% of support tickets related to Central Management.
>
>Unless you are an expert in hooking things up to SQL Server, its probably best to stick with SQL Server Mixed Mode Authentication.
> See https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/authentication-in-sql-server


### Step 1.1: Install SQL Server

You may need to install SQL Server as part of this. There are all kinds of ways to do that and different SKUs to choose from. If you already have SQL Server implemented and you simply want to add the Chocolatey Management Database to that, you can skip this step (and possibley 1.2 as well).

#### Install SQL Server Express
You may have other methods for getting SQL Server installed, but if you are looking for a quick way of installing SQL Server Express, you can use the Chocolatey packages we internalized earlier in this process.

The quickest option to get going with the database is to use the Community Repository, or an internalized version of a sql server package from the community repository For SQL Server Express 2019, run the following:

```powershell
choco install sql-server-express -y
```

You will also want to have the management tools installed, which can be installed with:

```powershell
choco install sql-server-management-studio -y
```

### Step 1.2: Prepare SQL Server

In preparing SQL Server, you need to do the following:

* Turn on Named Pipes and TCP Server protocols
* Ensure the TcpPort is 1433 (or know what it is you need to connect to)
* Set SQL Server Mixed Mode Authentication
* Restart SQL Server
* Open Windows Firewall ports for TCP access (and SQL Server Browser in most cases)

We've prepared a handy script (that may turn into a package later) to help you ensure you have SQL Server set up properly.

#### Script to Prepare SQL Server Express

The following is a script for SQL Server Express. You may be configuring a default instance. This should be run on the computer that has SQL Server Express installed as it will have the right binaries necessary for accessing SQL Server programmatically

```powershell
# https://docs.microsoft.com/en-us/sql/tools/configuration-manager/tcp-ip-properties-ip-addresses-tab
Write-Output "SQL Server: Configuring Remote Acess on SQL Server Express."
$assemblyList = 'Microsoft.SqlServer.Management.Common', 'Microsoft.SqlServer.Smo', 'Microsoft.SqlServer.SqlWmiManagement', 'Microsoft.SqlServer.SmoExtended'

foreach ($assembly in $assemblyList) {
  $assembly = [System.Reflection.Assembly]::LoadWithPartialName($assembly)
}

$wmi = New-Object Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer # connects to localhost by default
$instance = $wmi.ServerInstances | Where-Object { $_.Name -eq 'SQLEXPRESS' }

$np = $instance.ServerProtocols | Where-Object { $_.Name -eq 'Np' }
$np.IsEnabled = $true
$np.Alter()

$tcp = $instance.ServerProtocols | Where-Object { $_.Name -eq 'Tcp' }
$tcp.IsEnabled = $true
$tcp.Alter()

$tcpIpAll = $tcp.IpAddresses | Where-Object { $_.Name -eq 'IpAll' }

$tcpDynamicPorts = $tcpIpAll.IpAddressProperties | Where-Object { $_.Name -eq 'TcpDynamicPorts' }
$tcpDynamicPorts.Value = ""
$tcp.Alter()

$tcpPort = $tcpIpAll.IpAddressProperties | Where-Object { $_.Name -eq 'TcpPort' }
$tcpPort.Value = "1433"
$tcp.Alter()

Write-Output "SQL Server: Setting Mixed Mode Authentication."
New-ItemProperty 'HKLM:\Software\Microsoft\Microsoft SQL Server\MSSQL12.SQLEXPRESS\MSSQLServer\' -Name 'LoginMode' -Value 2 -Force

Write-Output "SQL Server: Forcing Restart of Instance."
Restart-Service -Force 'MSSQL$SQLEXPRESS'

Write-Output "SQL Server: Setting up SQL Server Browser and starting the service."
Set-Service 'SQLBrowser' -StartupType Automatic
Start-Service 'SQLBrowser'

Write-Output "Firewall: Enabling SQLServer TCP port 1433."
netsh advfirewall firewall add rule name="SQL Server 1433" dir=in action=allow protocol=TCP localport=1433 profile=any enable=yes service=any
#New-NetFirewallRule -DisplayName "Allow inbound TCP Port 1433" –Direction inbound –LocalPort 1433 -Protocol TCP -Action Allow

Write-Output "Firewall: Enabling SQL Server browser UDP port 1434."
netsh advfirewall firewall add rule name="SQL Server Browser 1434" dir=in action=allow protocol=UDP localport=1434 profile=any enable=yes service=any
#New-NetFirewallRule -DisplayName "Allow inbound UDP Port 1434" –Direction inbound –LocalPort 1434 -Protocol UDP -Action Allow
```

___
## Step 2: Install Central Management Database Package

> :warning: **WARNING**: CCM packages do ***NOT*** install SQL Server. You must take care of that in the prerequisites. Do not even start on central management installs until you have a SQL Server instance up and ready. I repeat, SQL Server engine must be already installed.

The CCM database package will add or update a database to an existing SQL Server instance.

> :memo: **NOTE**: When you run this package installation, you will want to do so as integrated security, or with Windows Authentication. When you run the other two package installations, you will want to do so providing a connection string.

### Package Parameters
Note items with "`:`" mean a value should be provided, items without are simply switches.

* `/ConnectionString:` - The SQL Server database connection string to be used to connect to the Chocolatey Central Management database. Defaults to default or explicit values for `/SqlServiceInstance` and `/Database`, along with Integrated Security (`Server=<LOCAL COMPUTER FQDN NAME>; Database=ChocolateyManagement; Trusted_Connection=True;`). The account should have `db_owner` access to the database ([database owner](https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/database-level-roles#fixed-database-roles)).
* `/SqlServerInstance:` - Instance name of the SQL Server database to connect to. Alternative to passing full connection string with `/ConnectionString`. Uses `/Database` (below) to build a connection string. Defaults to `<LOCAL COMPUTER FQDN NAME>`.
* `/Database:` - Name of the SQL Server database to use. Alternative to passing full connection string with `/ConnectionString`. Uses `/SqlServerInstance` (above) to build a connection string. Defaults to `ChocolateyManagement`.
* `/SkipDatabasePermissionCheck` - By default, a check will be completed to ensure that the installing user has access to create a new database, based on the provided/computed connection string. If this check isn't required, for example, the database has already been created or permissions will error, this step can be skipped using this parameter. Available with CCM v0.2.0+.


### Scenarios

Scenario 1: You are installing the database package on the same machine as a SQL Server Express installation:

```powershell
choco install chocolatey-management-database -y --package-paramaeters-sensitive="'/ConnectionString=""Server=Localhost\SQLEXRESS;Database=ChocolateyManagement;Trusted_Connection=true;""'"
```

Scenario 2: You are installing the database package on a single server, but connecting to an existing SQL Server in your environment:

```powershell
choco install chocolatey-management-database -y --package-parameters-sensitive="'/ConnectionString=""Server=DatabaseServer;Database=ChocolateyManagement;Trusted_Connection=true;""'"
```

___
## Step 3: Set up SQL Server Logins And Access

Once we have the database, we can create logins and map those logins to users in the database.

> :warning: **WARNING**: CCM packages do ***NOT*** configure SQL Server access either.

The difference between a login and a user when it comes to SQL Server accounts has long confused folks. Simply put:

- Login (Authentication) - A login is at instance level (the credentials or Windows-based accounts) - [https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/create-a-login](https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/create-a-login)
- User (Authorization) - A user is that login being mapped to a database and given roles/privileges (an instance can contain multiple databases) - [https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/create-a-database-user](https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/create-a-database-user)

Notes:

- Grant `db_reader` and `db_writer` to the accounts you create for the web and service.
- You can share the same login for the two accounts, unless your internal best practices dictate using different passwords.


```powershell
function Add-DatabaseUserAndRoles {
  param(
   [parameter(Mandatory=$true)][string] $Username,
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
IF EXISTS(SELECT * FROM msdb.sys.syslogins WHERE UPPER([name]) = UPPER('$Username'))
  BEGIN
    DROP LOGIN [$Username]
  END
CREATE LOGIN [$Username] $LoginOptions WITH DEFAULT_DATABASE=[$DatabaseName]

USE [$DatabaseName]
IF EXISTS(SELECT * FROM sys.sysusers WHERE UPPER([name]) = UPPER('$Username'))
  BEGIN
    DROP USER [$Username]
  END

CREATE USER [$Username] FOR LOGIN [$Username]

"@

  foreach ($DatabaseRole in $DatabaseRoles) {
    $addUserSQLCommand += @"

ALTER ROLE [$DatabaseRole] ADD MEMBER [$Username]
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

# Add Sql Server Login / User:
Add-DatabaseUserAndRoles -Username 'ChocoUser' -SqlUserPassword '<SUPER HARD PASSWORD>' -CreateSqlUser -DatabaseRoles @('db_datareader', 'db_datawriter')
# Add Local Windows User:
Add-DatabaseUserAndRoles -Username "$(hostname)\ChocolateyLocalAdmin" -DatabaseRoles @('db_datareader', 'db_datawriter')
# Add Active Directory Domain User:
Add-DatabaseUserAndRoles -Username "<DomainName>\<Username>" -DatabaseRoles @('db_datareader', 'db_datawriter')
```

___
## Step 4: Verify Installation

The purpose of the `chocolatey-management-database` package is to create and deploy the schema for the database that is used by the CCM Service and Website.  This can be verified by using something like SQL Server Management Studio to connect to the SQL Server Instance and:

* Check that a database (by default named `ChocolateyManagement`) has been created
* That a set of tables have been created within this database
* That permissions have been set for accounts

___
## FAQ

### Can I use MySQL (or PostgreSQL)?
Unfortunately only SQL Server SKUs work with Chocolatey Central Management at this time. You can use SQL Server Express in smaller shops.

___
## Common Errors and Resolutions

### Chocolatey Central Management database package installs without error, but ChocolateyManagement database is not created

In the beta version of the Chocolatey Central Management database package, if there was an error during the creation of the database, no exit code was used.  As a result, the package could state that it was installed correctly, but the database would not have been created.  This has been corrected in the 0.1.0 release of Chocolatey Central Management.

When this occurs, the problem is typically the connection string being used to connect to the database.  The advice is to verify that the connecting string is valid, and attempt the installation again.

### The term 'Install-ChocolateyAppSettingsJsonFile' is not recognized as the name of a cmdlet, function, script file, or operable program.

In the beta version of Chocolatey.Extension, there was a Cmdlet named Install-ChocolateyAppSettingsJsonFile and this was used in the 0.1.0-beta-20181009 release of the Chocolatey Central Management components. In the final released version of the Chocolatey.Extension, this was renamed to Install-AppSettingsJsonFile.

As a result, the Chocolatey Central Management beta no longer works with the released version of Chocolatey.Extension. This will be corrected once the next release of the Chocolatey Central Management components is completed.

### Cannot process command because of one or more missing mandatory parameters: FilePath

During the creation of Chocolatey Central Management, some additional PowerShell cmdlets were created, and these are installed as part of the Chocolatey Extension package.  These cmdlets went through a number of iterations, and as a result, different combinations of Chocolatey Central Management packages were incompatible with the Chocolatey Extension package, resulting in the error:

`Cannot process command because of one or more missing mandatory parameters: FilePath`

The guidance in this case is either to pin to the specific version of the Chocolatey Extension package required by the version of Chocolatey Central Management being used, or, update to the latest versions of all packages, where the situation should be addressed.

[[Central Management Setup|CentralManagementSetup]] | [[Chocolatey Central Management|CentralManagement]]
