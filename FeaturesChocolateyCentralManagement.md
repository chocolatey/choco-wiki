# Chocolatey Central Management (CCM) (Business Editions Only)

CCM provides you insights across your desktop and endpoint environments.

Once installed and configured, you can use CCM to:

* bring reporting to the Organizational level
* quickly see all software across the Organization and see what needs attention immediately
* create reports for tracking and auditing purposes
* manage endpoints with deployments through groups and collections

![CCM Overview](images/features/ccm/ccm_overview.jpg)


<!-- TOC depthTo:6 -->

- [Usage](#usage)
- [Roadmap](#roadmap)
- [Setup](#setup)
- [FQDN Usage](#fqdn-usage)
- [Chocolatey Configuration for CCM](#chocolatey-configuration-for-ccm)
  - [centralManagementReportPackagesTimerIntervalInSeconds](#centralmanagementreportpackagestimerintervalinseconds)
  - [centralManagementServiceUrl](#centralmanagementserviceurl)
  - [centralManagementReceiveTimeoutInSeconds](#centralmanagementreceivetimeoutinseconds)
  - [centralManagementSendTimeoutInSeconds](#centralmanagementsendtimeoutinseconds)
  - [centralManagementCertificateValidationMode](#centralmanagementcertificatevalidationmode)
- [Chocolatey Clients](#chocolatey-clients)
- [Complete Installation Script](#complete-installation-script)
- [FAQ](#faq)
  - [Will this become available for lower editions of Chocolatey?](#will-this-become-available-for-lower-editions-of-chocolatey)
  - [Where can I find all the log files for Chocolatey Central Management](#where-can-i-find-all-the-log-files-for-chocolatey-central-management)
    - [Website](#website)
    - [Chocolatey Management Service](#chocolatey-management-service)
    - [Chocolatey Agent Service](#chocolatey-agent-service)
  - [How can I increase the level of logging for Chocolatey Central Management](#how-can-i-increase-the-level-of-logging-for-chocolatey-central-management)

<!-- /TOC -->

## Usage

Chocolatey Central Management (CCM) works in conjunction with [Chocolatey Agent](https://chocolatey.org/docs/features-agent-service) to bring full details of all Chocolatey controlled machines in your environment into one location, which is then accessible by the CCM Website.  Chocolatey Agent will regularly report information about what is installed on each machine, and whether any of that software is outdated, based on packages in available sources.

## Roadmap

Please see [[Chocolatey Central Management|CentralManagement]].

## Setup

Please see [[Central Management Setup|CentralManagementSetup]].


While it is envisioned that CCM will be installed across multiple servers, it is certainly possible to run CCM on a single server.

Currently, the CCM packages do not provision the SQL Server Database Permissions that are required for the CCM components to function.  It is assumed that the necessary permissions have already been provided (see the [FAQ](#how-can-i-add-sql-server-permissions-through-powershell) for one method of doing it).  By default, two users will require read/write permissions to the CCM Database:

* `ChocolateyLocalAdmin` - which, by default, runs the CCM Service
* `IIS APPPOOL\ChocolateyCentralManagement` - which, by default, runs the CCM IIS Application Pool

**NOTE:** If either of these users are changed during the installation of these components, the database permissions will need to be updated to reflect this.

Or, the required username/password for connecting to the CCM database are added as part of the connection string that is passed into the CCM packages during installation.


## FQDN Usage

Please see [[Central Management Service Setup|CentralManagementSetupService#fqdn-usage]].



## Chocolatey Configuration for CCM

The following configuration values, with their default values, are added into the chocolatey.config file after installing CCM and it's dependent packages.

### centralManagementReportPackagesTimerIntervalInSeconds

This is the length of time, in seconds, that the Chocolatey Background Agent will wait between each attempt to report into CCM.

**Default Value:** 1800

### centralManagementServiceUrl

This is the URL that is used by the Chocolatey Background Agent to report into CCM, and also by the CCM Service to register the URL that it is listening for incoming reports on.

**Default Value:** _blank_

**NOTE:** If left blank, the CCM Service will construct a URL based on the default Port number which is 24020, and the FQDN of the machine that the service is being executed on.  However, Chocolatey Agent will not be able to report into CCM, if a value is not provided.

**NOTE:** If the Chocolatey Background Agent is installed on the same machine that has the CCM Service installed, it can only report into that CCM Service.

### centralManagementReceiveTimeoutInSeconds

This is the length of time, in seconds, that a connection to CCM can remain inactive, during which no application messages are received, before it is dropped.

**Default Value:** 30

### centralManagementSendTimeoutInSeconds

This is the length of time, in seconds, that a write operation against CCM has to complete before the transport raises an exception.

**Default Value:** 30

### centralManagementCertificateValidationMode

This captures the options for determining the validity of the CCM Service certificate obtained using SSL/TLS negotiation.

**Default Value:** PeerOrChainTrust

**Valid Values:** None, PeerTrust, ChainTrust, PeerOrChainTrust, Custom

## Chocolatey Clients

Once CCM has been set up and configured, each machine that you want to report into CCM will have to be enabled.  This can be done by doing the following:

~~~powershell
choco config set CentralManagementServiceUrl https://MACHINE1:24020/ChocolateyManagementService
choco feature enable --name="'useChocolateyCentralManagement'"
~~~

Here, the full URL, including the port number, to where the CCM service was installed to is being set, and then the `useChocolateyCentralManagement` feature is being enabled. In your environment you would replace `MACHINE1:24020` with the FQDN name of your server and the port being used.

**NOTE:** By default, this feature is disabled, and will need to be turned on.

**NOTE:** If not set, the CCM Service will construct a URL based on the default Port number which is 24020, and the FQDN of the machine that the service is being executed on.  However, Chocolatey Agent will not be able to report into CCM, if a value is not provided.

[Additional configuration](#chocolatey-configuration-for-chocolatey-central-management) exists for CCM Service, which allows fine grained control of how Chocolatey Agent will report into CCM.

## Complete Installation Script

The following is a complete installation script that can be used as an **example** (which should be modified to fit with your environment before executing) of how to install all necessary CCM components and configuration on a **single machine**, using all the **default values**.

To use values other than the default, see the relevant parameters section for the [chocolatey-management-database](#package-parameters), [chocolatey-management-service](#package-parameters-1) and [chocolatey-management-web](#package-parameters-2) packages.  And refer to the [Chocolatey Configuration](#chocolatey-configuration-for-ccm) for further information about global settings for CCM.

~~~powershell
# Find FDQN for current machine
$hostName = [System.Net.Dns]::GetHostName()
$domainName = [System.Net.NetworkInformation.IPGlobalProperties]::GetIPGlobalProperties().DomainName

if(-Not $hostName.endswith($domainName)) {
  $hostName += "." + $domainName
}

# Pre-requisites
# These three packages will be required on any machine that is going to be reporting into CCM
choco upgrade chocolatey -y --version 0.10.15
choco upgrade chocolatey.extension -y --version 2.0.2
choco upgrade chocolatey-agent -y --version 0.9.1

# CCM Database
# This package is only required on the machine that is going to host the CCM Database
choco upgrade chocolatey-management-database -y --version 0.1.0

# CCM Service
# This package is only required on the machine that is going to host the CCM Service
choco upgrade chocolatey-management-service -y --version 0.1.0

# CCM Website
# These packages are only required on the machine that is going to host the CCM Web Site
choco upgrade aspnetcore-runtimepackagestore -y --version 2.2.7
choco upgrade dotnetcore-windowshosting -y --version 2.2.7
choco upgrade chocolatey-management-web -y --version 0.1.0

# CCM Configuration
# These steps are required on any machine that is going to be reporting into CCM
choco config set --name="'centralManagementReportPackagesTimerIntervalInSeconds'" --value="'1800'"
choco config set --name="'centralManagementServiceUrl'" --value="'https://$($hostname):24020/ChocolateyManagementService'"
choco config set --name="'centralManagementReceiveTimeoutInSeconds'" --value="'60'"
choco config set --name="'centralManagementSendTimeoutInSeconds'" --value="'60'"
choco config set --name="'centralManagementCertificateValidationMode'" --value="'PeerOrChainTrust'"
choco feature enable --name="'useChocolateyCentralManagement'"
~~~

> Best practices in scripts are noted here:
> * Use `upgrade` instead of `install` - upgrade is more making the script reusable when newer versions are available.
> * Always use `-y` to ensure nothing stops and prompts for more than 30 seconds.
> * When using options prefer a longer name (`--name` versus the short `-n`) to make the scripts more self-documenting
> * When using options that have a value passed, add an `=` between and surround the value with `"''"` (`--name="'value'"`). This ensures that the argument is not split between different versions/editions of Chocolatey. This also ensures that values like `.` and `\\` are not escaped by PowerShell.


## FAQ

### Will this become available for lower editions of Chocolatey?

CCM will only be available in Chocolatey for Business (C4B).


### Where can I find all the log files for Chocolatey Central Management

Due to the fact that Chocolatey Central Management is made up of a number of components, there are a few places that log files exist, and you may be asked for these when engaging with support.

#### Website

Log file is located at `C:\tools\chocolatey-management-web\App_Data\Logs\Logs.txt`

#### Chocolatey Management Service

Log file is located at `C:\ProgramData\chocolatey\lib\chocolatey-management-service\tools\service\logs\chocolatey.service.host.log`

#### Chocolatey Agent Service

Log file is located at `C:\ProgramData\chocolatey\lib\chocolatey-agent\tools\service\logs\chocolatey-agent.log`

### How can I increase the level of logging for Chocolatey Central Management

This can be done by changing the level value, which should be currently INFO, to use DEBUG, as per the following:

~~~
<root>
  <level value="DEBUG" />
  <appender-ref ref="ColoredConsoleAppender" />
  <appender-ref ref="RollingLogFileAppender" />
</root>
~~~

In the following files:

* C:\ProgramData\chocolatey\lib\chocolatey.management.service\tools\service\chocolatey.console.service.host.exe.config
* C:\ProgramData\chocolatey\lib\chocolatey-agent\tools\service\chocolatey-agent.exe.config

