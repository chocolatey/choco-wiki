# Chocolatey Central Mangement Setup

<!-- TOC depthFrom:2 depthTo:5 -->

- [Summary](#summary)
- [High Level Requirements](#high-level-requirements)
- [Step 1: Internalize Packages](#step-1-internalize-packages)
- [Step 2: Setup Central Management Database](#step-2-setup-central-management-database)
- [Step 3: Setup Central Mangement Windows Service(s)](#step-3-setup-central-mangement-windows-services)
- [Step 4: Setup Central Management Website](#step-4-setup-central-management-website)

<!-- /TOC -->

## Summary

Installing CCM takes a little more pre-thought than simply running the package installs.
While it is envisioned that CCM will be installed across multiple servers (split installation), it is certainly possible to run CCM on a single server (monolithic).

When setting up Central ManagementCurrently, the CCM packages do not provision the SQL Server Database Permissions that are required for the CCM components to function.  It is assumed that the necessary permissions have already been provided (see the [FAQ](#how-can-i-add-sql-server-permissions-through-powershell) for one method of doing it).

> :memo: **NOTE**
>
> Please read through all of this prior to running installation as you could run into issues that require support to help you correct later.


> :warning: **WARNING**
>
> Unless otherwise noted, please follow these steps in ***exact*** order. These steps build on each other and need to be completed in order.

> :memo: **NOTE**
>
> If this seems like a lot to set up, you have the ability to get access to the [[Quick Deployment Environment (QDE)|QuickDeploymentEnvironment]]. It comes preloaded with Central Management and other Chocolatey recommended infrastructure. Please see [[Quick Deployment Environment (QDE)|QuickDeploymentEnvironment]].

## High Level Requirements
Central Management packages require at a minimum:

* Chocolatey for Business (C4B) Edition
* .NET Framework 4.6.1+
* Windows Server 2012+

Each package further defines dependencies that they include.

___
## Step 1: Internalize Packages

The complete installation of CCM requires several packages that are available from the community repository. Let's get them internalized. We will internalize them to a `C:\packages` directory. It is highly recommended that you push the packages to an internal repository before continuing with other steps in this guide. Change the values in the last line of this script to match what you need in your environment.

```powershell
# Remote the < >
$YourInternalRepositoryPushUrl = '<INSERT REPOSITORY URL HERE>'
$YourInternalRepositoryApiKey = '<YOUR API KEY HERE>'
$YourBusinessLicenseGuid = '<INSERT NON-TRIAL C4B LICENSE GUID HERE>'

if(!(Test-Path C:\packages)){
  $null = New-Item C:\packages -ItemType Directory
}

# This is for Chocolatey and other Community Related Items
choco download chocolatey chocolateygui dotnet4.5.2 dotnet4.6.1 --force --internalize --internalize-all-urls --append-use-original-location --source="'https://chocolatey.org/api/v2/'" --output-directory="'C:\packages'"


# This is for SQL Server Express
# Not necessary if you already have SQL Server
@('sql-server-express','sql-server-management-studio') | Foreach-Object {
  choco download $_ --force --internalize --internalize-all-urls --append-use-original-location --source="'https://chocolatey.org/api/v2/'" --output-directory="'C:\packages'"
}

# We must use the 2.2.7 versions of these packages, so we need to download/internalize these specific items
@('aspnetcore-runtimepackagestore','dotnetcore-windowshosting') | Foreach-Object {
  choco download $_ --version 2.2.7 --force --internalize --internalize-all-urls --append-use-original-location --source="'https://chocolatey.org/api/v2/'" --output-directory="'C:\packages'"
}

# Internalize Licensed Packages
# Trial? You have download links, download the files - then place them in the c:\packages folder. Comment out this section
choco download chocolatey-agent chocolatey.extension chocolatey-management-database chocolatey-management-service chocolatey-management-web --force --internalize --internalize-all-urls --append-use-original-location --source="'https://licensedpackages.chocolatey.org/api/v2/'" --ignore-dependencies --output-directory="'C:\packages'"  --user="'user'" --password="'$YourBusinessLicenseGuid'"

# Push all downloaded packages to your internal repository
Get-ChildItem C:\packages -Recurse -Filter *.nupkg | Foreach-Object { choco push $_.Fullname --source="'$YourInternalRepositoryPushUrl'" --api-key="'$YourInternalRepositoryApiKey'"}
```

___

## Step 2: Setup Central Management Database

Please see [[Central Management Database Setup|CentralManagementSetupDatabase]].

> :memo: **NOTE**: While we'd like to support different database engines at some point in the distant future, currently only SQL Server is supported.

___

## Step 3: Setup Central Mangement Windows Service(s)

Please see [[Central Management Service Setup|CentralManagementSetupService]].

> :memo: **NOTE**: If Step 1 is not succesful, do not move on to this step until you resolve issues with database setup.

___

## Step 4: Setup Central Management Website

Please see [[Central Management Web Setup|CentralManagementSetupWeb]].

> :memo: **NOTE**: If Step 1 or 2 is not succesful, do not move on to this step until you resolve issues with previous steps.


[[Chocolatey Central Management|CentralManagement]]
