# Chocolatey Central Mangement Setup

> :warning: **WARNING**: This is a Work in Progress. Please check back later
>
> Also see [[Chocolatey Central Management in Features|FeaturesChocolateyCentralManagement]].

<!-- TOC depthFrom:2 depthTo:5 -->

- [Summary](#summary)
- [Step 0: Internalize Packages](#step-0-internalize-packages)
- [Step 1: Setup Central Management Database](#step-1-setup-central-management-database)
- [Step 2: Setup Central Mangement Windows Service(s)](#step-2-setup-central-mangement-windows-services)
- [Step 3: Setup Central Management Website](#step-3-setup-central-management-website)

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
## Step 0: Internalize Packages

The complete installation of CCM requires several packages that are available from the community repository. Let's get them internalized. We will internalize them to a `C:\packages` directory. It is highly recommended that you push the packages to an internal repository before continuing with other steps in this guide. Change the values in the last line of this script to match what you need in your environment.

```powershell
# Remote the < >
$YourInternalRepositoryPushUrl = '<INSERT REPOSITORY URL HERE>'
$YourInternalRepositoryApiKey = '<YOUR API KEY HERE>'
$YourBusinessLicenseGuid = '<INSERT NON-TRIAL C4B LICENSE GUID HERE>'

if(!(Test-Path C:\packages)){
  $null = New-Item C:\packages -ItemType Directory
}

# This is for SQL Server Express
# Not necessary if you already have SQL Server
@('sql-server-express','sql-server-management-studio') | Foreach-Object {
  choco download $_ --internalize  --force --output-directory="'C:\packages'"
}

# We must use the 2.2.7 versions of these packages, so we need to download/internalize these specific items
@('aspnetcore-runtimepackagestore','dotnetcore-windowshosting') | Foreach-Object {
  choco download $_ --version 2.2.7 --force --output-directory="'C:\packages'"
}

# Internalize Licensed Packages
# Trial? You have download links, download the files - then place them in the c:\packages folder. Comment out this section
choco download chocolatey-agent chocolatey.extension chocolatey-management-database chocolatey-management-service chocolatey-management-web --internalize --append-use-original-location --source="'https://licensedpackages.chocolatey.org/api/v2/'" --ignore-dependencies --force --output-directory="'C:\packages'"  --user="'user'" --password="'$YourBusinessLicenseGuid'"

# Push all downloaded packages to your internal repository
Get-ChildItem C:\packages -Recurse -Filter *.nupkg | Foreach-Object { choco push $_.Fullname --source="'$YourInternalRepositoryPushUrl'" --api-key="'$YourInternalRepositoryApiKey'"}
```
___

## Step 1: Setup Central Management Database

Please see [[Central Management Database Setup|CentralManagementSetupDatabase]].

___

## Step 2: Setup Central Mangement Windows Service(s)

Please see [[Central Management Service Setup|CentralManagementSetupService]].

___

## Step 3: Setup Central Management Website

Please see [[Central Management Web Setup|CentralManagementSetupWeb]].



[[Chocolatey Central Management|CentralManagement]]
