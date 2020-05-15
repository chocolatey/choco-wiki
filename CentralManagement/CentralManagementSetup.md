# Chocolatey Central Mangement Setup

> :warning: **WARNING**: This is a Work in Progress. Please check back later

<!-- TOC depthFrom:2 -->

- [Summary](#summary)
- [Step 0: Complete Prerequisites](#step-0-complete-prerequisites)
  - [Step 0.1: Install Or Prepare SQL Server](#step-01-install-or-prepare-sql-server)
- [Step 1: Install Central Management Database Package](#step-1-install-central-management-database-package)
- [Step 2: Set up SQL Server Logins And Users](#step-2-set-up-sql-server-logins-and-users)
- [Step 3: Install Central Mangement Service Package](#step-3-install-central-mangement-service-package)
- [Step 4: Install Central Management Web Package](#step-4-install-central-management-web-package)

<!-- /TOC -->

## Summary
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

### Step 0.1: Install Or Prepare SQL Server

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


___
## Step 1: Install Central Management Database Package

> :warning: **WARNING**: CCM packages do ***NOT*** install SQL Server. You must take care of that in the prerequisites. Do not even start on central management installs until you have a SQL Server instance up and ready. I repeat, SQL Server engine must be already installed.

The CCM database package will add or update a database to an existing SQL Server instance.

When you run this package installation, you will want to do so as integrated security, or with Windows Authentication. When you run the other two package installations, you will want to do so providing a connection string.

___
## Step 2: Set up SQL Server Logins And Users

Once we have the database, we can create logins and map those logins to users in the database.

> :warning: **WARNING**: CCM packages do ***NOT*** configure SQL Server access either.

The difference between a login and a user when it comes to SQL Server accounts has long confused folks. Simply put:

* Login (Authentication) - A login is at instance level (the credentials) - https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/create-a-login
* User (Authorization) - A user is that login being mapped to a database and given roles/privileges (an instance can contain multiple databases) - https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/create-a-database-user


Notes:
* Grant `db_reader` and `db_writer` to the accounts you create for the web and service.
* You can share the same login for the two accounts, unless your internal best practices dictate using different passwords.


___
## Step 3: Install Central Mangement Service Package

> :warning: **WARNING**: Ensure you have completed installing the database package and have set up sql server logins and accounts.

> :memo: **NOTE**: You could be installing several of these if your environment is large enough.

___
## Step 4: Install Central Management Web Package

> :warning: **WARNING**: Ensure you have completed installing the database package first on whatever machine the database is on.

> :memo: **NOTE**: At this time we don't recommend opening internet access to CCM web. However, if you choose to, you will want to set up SSL/TLS certificates to ensure communication is encrypted over the internet.
