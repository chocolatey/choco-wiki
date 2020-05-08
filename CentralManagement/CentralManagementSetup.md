# Chocolatey Central Mangement Setup

> :warning: **WARNING**: This is a Work in Progress. Please check back later

<!-- TOC depthFrom:2 -->

- [Summary](#summary)
- [Step 0: Complete Prerequisites](#step-0-complete-prerequisites)
  - [Step 0.1: Install Or Prepare SQL Server](#step-01-install-or-prepare-sql-server)
- [Step 1: Install Central Management Database Package](#step-1-install-central-management-database-package)
- [Step 2: Install Central Mangement Service Package](#step-2-install-central-mangement-service-package)
- [Step 3: Install Central Management Web Package](#step-3-install-central-management-web-package)

<!-- /TOC -->

## Summary
Installing CCM takes a little more pre-thought than simply running the package installs.

> :memo: **NOTE**
>
> Please read through all of this prior to running installation as you could run into issues that require support to help you correct later.


> :warning: **WARNING**
>
> Unless otherwise noted, please follow these steps in ***exact*** order. These steps build on each other and need to be completed in order.


___
## Step 0: Complete Prerequisites

### Step 0.1: Install Or Prepare SQL Server
CCM will not install or take a dependency on a database engine install as there are different editions that could be installed and multiple packages out there. At this time, it is expected that you have this ready. This is required before you can continue to other steps.

* You need a SQL Server set up somewhere. Editions really don't matter until you have a large number of computers checking in.
* SQL Server should support mixed mode for logins (unless you are going to use AD authentication). 98% of the time you are going to want mixed mode authentication for SQL Server unless you hit options.
* You need to create the user access to the database (logins at the server level/users at the db level).


> :memo: **NOTE**: While we'd like to support different database engines at some point in the distant future, currently only SQL Server is supported.


___
## Step 1: Install Central Management Database Package


___
## Step 2: Install Central Mangement Service Package

> :warning: **WARNING**: Ensure you have completed installing the database package first on whatever machine the database is on.


___
## Step 3: Install Central Management Web Package

> :warning: **WARNING**: Ensure you have completed installing the database package first on whatever machine the database is on.
