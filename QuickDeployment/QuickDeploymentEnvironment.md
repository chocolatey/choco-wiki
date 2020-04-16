# Chocolatey Quick Deployment Environment (QDE)

## Summary

This is an overview on the Chocolatey Quick Deployment Environment (QDE). It provides a single virtual machine appliance to be imported into your hypervisor-of-choice, which contains most of the various components of a Chocolatey organizational solution.

![QDE Architechture](images/quickdeploy/QDE-architecture.gif)

<!-- TOC -->

- [Summary](#summary)
- [QDE Components](#qde-components)
- [Links](#links)
- [Related Articles](#related-articles)
- [FAQs](#faqs)
  - [What does the QDE (Quick Deployment Environment) include?](#what-does-the-qde-quick-deployment-environment-include)
  - [What do I need to do make this happen in my environment?](#what-do-i-need-to-do-make-this-happen-in-my-environment)
  - [How much time does this save?](#how-much-time-does-this-save)
  - [What if I have a larger environment? (> 1k nodes)](#what-if-i-have-a-larger-environment--1k-nodes)
  - [Can we brag about how fast we were able to get configured?](#can-we-brag-about-how-fast-we-were-able-to-get-configured)

<!-- /TOC -->

## QDE Components

The QDE appliance provides a unified architecture containing the following components:

* **Sonatype Nexus Repository OSS** (pre-configured with repositories) - This is your repository, where you'll be storing your Chocolatey packages (nupkg files).
* **Jenkins** (pre-configured with internalizer jobs) - This is your automation engine, that lets you run tasks on-demand and on a schedule. It will host the PowerShell scripts to help you auto-internalize packages from the Chocolatey Community Repository
* **Chocolatey Central Management** - This is your web dashboard for Chocolatey, that will allow you to track and monitor Chocolatey packages on your endpoint clients. You can see what packages are installed where, and whether or not they are out-of-date.
* **Scripts for Internal Deployment** - Various scripts to help you configure this solution are included, for your convenience.

> :warning: **WARNING**: This solution targets environments up to about **1000 nodes**.
If you need a solution for a larger environment, QDE would still be suitable as a proof-of-concept, however, best practices would recommmend a distributed infrastructure, separating each component into its own discrete node.
If you find yourself in need of a more scalable solution, please contact Support and we'll be more than happy to provide guidance for larger solutions.

## Links

* [[Quick Deployment Environment Setup|QuickDeploymentSetup]]
* Setting up proper DNS on QDE (DHCP by default; you'll want to switch to static)
* [[QDE Desktop ReadMe File|QuickDeploymentDesktopReadme]] (included here for convenience)
* [[QDE SSL/TLS Setup|QuickDeploymentSslSetup]]
* [[QDE Firewall Changes|QuickDeploymentFirewallChanges]]
* [[QDE Client Setup|QuickDeploymentClientSetup]] (setting up your client machines)
* Opening up for remote access (for QDE)

## Related Articles

A lot of what is done in QDE compresses the work or completely removes the work found in these related articles.

* [[Organizational Deployment Guide|How-To-Setup-Offline-Installation]]
* [Client Install](https://chocolatey.org/install#organization)
* [[Chocolatey Commercial Installation|Installation-Licensed]]
* [[Automate Package Internalization|How-To-Setup-Internal-Package-Repository]]

**NOTE**: If you find that QDE is only good for a POC in your environment as you have thousands of endpoints, you will want to understand how to scale out that infrastructure. The above articles really help address that.

## FAQs

### What does the QDE (Quick Deployment Environment) include?

* Repository Server - Nexus (pre-configured with repositories)
* Automation Pipelines - Jenkins (pre-configured with internalizer jobs)
* Chocolatey Central Management
* Package Builder / Package Internalizer
* All the goodness of that sweet, sweet automation in Chocolatey.
* Scripts for Internal Deployment

### What do I need to do make this happen in my environment?

Fill out the form on [this page](https://chocolatey.org/contact/quick-deployment) and we will reach out with all the necessary information to get you started.

### How much time does this save?

Typically, setting up a proper Chocolatey Central Management server and any accompanying infrastructure takes somewhere between 1-5 days, even when you have everything you need to get started. Setting up a new piece of infrastructure can be pretty cumbersome, we know. The QDE contains pretty much everything you need to get started in a single image, all ready to go. At most, it should only take a couple of hours to get everything ready to go from there.

### What if I have a larger environment? (> 1k nodes)

While this solution will be great to get a glimpse of what Chocolatey can really do for you, by its nature it isn't the silver bullet for every environment.

If you have a larger environment, we would strongly recommend taking the time to set up the various services on separate hosts to make them easier to manage and ensure they have the necessary resources to handle heavier loads.

### Can we brag about how fast we were able to get configured?

Please do! :-)
