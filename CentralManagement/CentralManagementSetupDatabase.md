# Chocolatey Central Management Database Setup

<!-- TOC depthFrom:2 -->

- [Step 1: Install Or Prepare SQL Server](#step-1-install-or-prepare-sql-server)
- [Step 2: Install Central Management Database Package](#step-2-install-central-management-database-package)
  - [Package Parameters](#package-parameters)
  - [Scenarios](#scenarios)
- [Step 3: Set up SQL Server Logins And Access](#step-3-set-up-sql-server-logins-and-access)
- [FAQ](#faq)
  - [Can I use MySQL (or PostgreSQL)?](#can-i-use-mysql-or-postgresql)
- [Common Errors and Resolutions](#common-errors-and-resolutions)

<!-- /TOC -->

## Step 1: Install Or Prepare SQL Server

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
>Unless you are an expert in hooking things up to SQL Server, its probably best to stick with SQL Server Mixed Mode Authentication.
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

## FAQ

### Can I use MySQL (or PostgreSQL)?
Unfortunately only SQL Server SKUs work with Chocolatey Central Management at this time. You can use SQL Server Express in smaller shops.

## Common Errors and Resolutions

[[Central Management Setup|CentralManagementSetup]] | [[Chocolatey Central Management|CentralManagement]]
