# Chocolatey Central Management (CCM) (Business Editions Only)

CCM provides you insights across your desktop and endpoint environments.

Once installed and configured, you can use CCM to:

* bring reporting to the Organizational level
* quickly see all software across the Organization and see what needs attention immediately
* create reports for tracking and auditing purposes
* manage endpoints with deployments through groups and collections

Please see [[Chocolatey Central Management|CentralManagement]].


<!-- TOC depthTo:6 -->

- [Roadmap](#roadmap)
- [Setup](#setup)
- [FQDN Usage](#fqdn-usage)
- [Chocolatey Clients](#chocolatey-clients)
- [Complete Installation Script](#complete-installation-script)
- [FAQ](#faq)
  - [Will this become available for lower editions of Chocolatey?](#will-this-become-available-for-lower-editions-of-chocolatey)
  - [Where can I find all the log files for Chocolatey Central Management](#where-can-i-find-all-the-log-files-for-chocolatey-central-management)
    - [Website](#website)
    - [Chocolatey Management Service](#chocolatey-management-service)
    - [Chocolatey Agent Service](#chocolatey-agent-service)

<!-- /TOC -->

## Roadmap

Please see [[Chocolatey Central Management|CentralManagement#roadmap]].

## Setup

Please see [[Central Management Setup|CentralManagementSetup]].


## FQDN Usage

Please see [[Central Management Service Setup|CentralManagementSetupService#fqdn-usage]].


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
