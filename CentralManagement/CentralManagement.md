# Chocolatey Central Mangement (CCM)

Chocolatey Central Management (CCM) provides you insights across your desktop and endpoint environments. CCM is available with Chocolatey for Business only.

Once installed and configured, you can use CCM to:

* bring reporting to the Organizational level
* quickly see all software across the Organization and see what needs attention immediately
* create reports for tracking and auditing purposes
* manage endpoints with deployments through groups and collections

![Central Management Logo](images/features/ccm/central-management.png)

This provides an overview on Chocolatey Central Mangement (CCM). It provides both setup and use of CCM.

___
<!-- TOC depthFrom:2 depthTo:5 -->

- [CCM Components](#ccm-components)
- [Getting CCM](#getting-ccm)
- [Stay Up To Date](#stay-up-to-date)
- [Links](#links)
  - [Setup / Installation](#setup--installation)
  - [Using Central Management](#using-central-management)
- [Related Articles](#related-articles)
- [Roadmap](#roadmap)
- [FAQs](#faqs)
  - [How do I take advantage of Chocolatey Central Management?](#how-do-i-take-advantage-of-chocolatey-central-management)
  - [I'm a licensed customer, now what?](#im-a-licensed-customer-now-what)
  - [Will this become available for lower editions of Chocolatey?](#will-this-become-available-for-lower-editions-of-chocolatey)
  - [What's the minimum version of the Chocolatey packages I need to use CCM?](#whats-the-minimum-version-of-the-chocolatey-packages-i-need-to-use-ccm)
  - [Where can I find all the log files for Chocolatey Central Management](#where-can-i-find-all-the-log-files-for-chocolatey-central-management)

<!-- /TOC -->

___
## CCM Components

The following are all of the Chocolatey components required for Central Management to work.

* Chocolatey (`chocolatey` package) v0.10.14+
* Chocolatey for Business (C4B) Edition.
* Chocolatey Licensed Extension (`chocolatey.extension` package) v2.0.0+
* Chocolatey Agent (`chocolatey-agent` package) v0.9.0+
* CCM Database (`chocolatey-management-database` package) v0.1.0+
  * This deploys the CCM database schema to a specified SQL Server instance
* CCM Service (`chocolatey-management-service` package) v0.1.0+
  * This installs the CCM Service, which the Chocolatey Agent will communicate with.
* CCM Website (`chocolatey-management-web` package) v0.1.0+
  * This is the CCM front end website that is the main user interface of the application

____
## Getting CCM
CCM is only available for Chocolatey for Business (C4B) customers. If you are a C4B customer, you can head to the install components section:

* [[Central Management Setup|CentralManagementSetup]]
* [[Central Management Client Setup|CentralManagementSetupClient]]

If you are not a customer yet, you can [reach out for a trial](https://chocolatey.org/contact/trial).

> :memo: **NOTE**
>
> Trials are limited to organizations. If you are personally wanting to work with CCM and other C4B components, you can purchase a C4B starter pack - see [pricing](https://chocolatey.org/pricing).

___
## Stay Up To Date
* [[Release Notes Central Management|ReleaseNotesCentralManagement]]
* [Release Announcements Only Mailing List](https://groups.google.com/group/chocolatey-announce)

___
## Links

### Setup / Installation
* [[Central Management Setup|CentralManagementSetup]]
    * [[Central Management Database Setup|CentralManagementSetupDatabase]]
    * [[Central Management Service Setup|CentralManagementSetupService]]
    * [[Central Management Web Setup|CentralManagementSetupWeb]]
* [[Central Management Client Setup|CentralManagementSetupClient]]

### Using Central Management

* [[CCM Computers|CentralManagementComputers]]
* [[CCM Software|CentralManagementSoftware]]
* [[CCM Groups|CentralManagementGroups]]
* [[CCM Deployments|CentralManagementDeployments]]
* [[CCM Reports|CentralManagementReports]]

![CCM Overview](images/features/ccm/ccm_overview.jpg)

___
## Related Articles

* [[Quick Deployment Environment (QDE)|QuickDeploymentEnvironment]]

___
## Roadmap

Chocolatey Central Management will allow:

* ~~Centralized Software Management for your entire organization.~~ Completed June 2020.
* ~~Centralized reporting of software.~~ Completed May 2019
* ~~Know immediately what software is out of date and on what machines.~~ Completed May 2019
* ~~Know within seconds the entire estate of software and what versions are installed.~~ Completed May 2019
  * ~~Including zips and archives* that do not show up in Programs and Features~~ Completed May 2019
  * ~~Including internal software* that does not show up in Programs and Features~~ Completed May 2019
* Adhoc reporting for a particular machine or set of machines
* ~~Run arbitrary Chocolatey commands against one or more machines~~ Completed June 2020.
* ~~See how many machines you are actively managing in your organization~~ Completed May 2019
* More...

\* - When deployed through Chocolatey.

____
## FAQs
### How do I take advantage of Chocolatey Central Management?
You must have a [Business edition of Chocolatey](https://chocolatey.org/compare). Business editions are great for organizations that need to manage the total software lifecycle.

### I'm a licensed customer, now what?
See [Getting CCM](#getting-ccm).

### Will this become available for lower editions of Chocolatey?
CCM will only be available in Chocolatey for Business (C4B).

### What's the minimum version of the Chocolatey packages I need to use CCM?
See [CCM Components](#ccm-components).

### Where can I find all the log files for Chocolatey Central Management
Chocolatey Central Management is made up of a number of components, so there will be a few possible places to find the log files, which you may be asked for when engaging with support.

* The Chocolatey Central Management Website log file located at `c:\tools\chocolatey-management-web\App_Data\Logs\ccm-website.log`. If you are on a version of CCM prior to 0.2.0, the log will be located at `c:\tools\chocolatey-management-web\App_Data\Logs\Logs.txt`.
* The Chocolatey Central Management service log file is located at `$env:ChocolateyInstall\logs\ccm-service.log`. If you are on a version of CCM prior to 0.2.0, the log will be located at `$env:ChocolateyInstall\lib\chocolatey-management-service\tools\service\logs\chocolatey.service.host.log`.
* The Chocolatey Agent log file is located at `$env:ChocolateyInstall\logs\chocolatey-agent.log`. If you are on a version of Chocolatey Agent prior to 0.10.0, the log will be located at `$env:ChocolateyInstall\lib\chocolatey-agent\tools\service\logs\chocolatey-agent.log`.
