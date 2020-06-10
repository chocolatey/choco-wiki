# Chocolatey Central Mangement (CCM)

> :warning: **WARNING**: This is a Work in Progress. Please check back later
>
> Also see [[Chocolatey Central Management in Features|FeaturesChocolateyCentralManagement]].

## Summary
Chocolatey Central Management (CCM) provides you insights across your desktop and endpoint environments.

Once installed and configured, you can use CCM to:

* bring reporting to the Organizational level
* quickly see all software across the Organization and see what needs attention immediately
* create reports for tracking and auditing purposes
* manage endpoints with deployments through groups and collections

![CCM Overview](images/features/ccm/ccm_overview.jpg)


This provides an overview on Chocolatey Central Mangement (CCM). It provides both setup and use of CCM.

<!-- TOC depthFrom:2 depthTo:5 -->

- [Summary](#summary)
- [CCM Components](#ccm-components)
- [Getting CCM](#getting-ccm)
- [Stay Up To Speed](#stay-up-to-speed)
- [Links](#links)
  - [Setup / Installation](#setup--installation)
  - [Using Central Management](#using-central-management)
- [Related Articles](#related-articles)
- [FAQs](#faqs)
  - [Will this become available for lower editions of Chocolatey?](#will-this-become-available-for-lower-editions-of-chocolatey)
  - [What's the minimum version of the Chocolatey packages I need to use CCM?](#whats-the-minimum-version-of-the-chocolatey-packages-i-need-to-use-ccm)

<!-- /TOC -->

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

## Getting CCM
CCM is only available for Chocolatey for Business (C4B) customers. If you are a C4B customer, you can head to the install components section:

* [[Central Management Setup|CentralManagementSetup]]
* [[Central Management Client Setup|CentralManagementClientSetup]]

If you are not a customer yet, you can [reach out for a trial](https://chocolatey.org/contact/trial).

> :memo: **NOTE**
>
> Trials are limited to organizations. If you are personally wanting to work with CCM and other C4B components, you can purchase a C4B starter pack - see [pricing](https://chocolatey.org/pricing).

## Stay Up To Speed
* [[Release Notes Central Management|ReleaseNotesCentralManagement]]
* [Release Announcements Only Mailing List](https://groups.google.com/group/chocolatey-announce)

## Links

### Setup / Installation
* [[Central Management Setup|CentralManagementSetup]]
    * [[Central Management Database Setup|CentralManagementSetupDatabase]]
    * [[Central Management Service Setup|CentralManagementSetupService]]
    * [[Central Management Web Setup|CentralManagementSetupWeb]]
* [[Central Management Client Setup|CentralManagementClientSetup]]

### Using Central Management

* [[CCM Computers|CentralManagementComputers]]
* [[CCM Software|CentralManagementSoftware]]
* [[CCM Groups|CentralManagementGroups]]
* [[CCM Deployments|CentralManagementDeployments]]
* [[CCM Reports|CentralManagementReports]]


## Related Articles

* [[Quick Deployment Environment (QDE)|QuickDeploymentEnvironment]]

## FAQs
### Will this become available for lower editions of Chocolatey?

CCM will only be available in Chocolatey for Business (C4B).

### What's the minimum version of the Chocolatey packages I need to use CCM?

See [CCM Components](#ccm-components).
