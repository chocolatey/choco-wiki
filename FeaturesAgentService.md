# Chocolatey Agent Service

> Empower your users and give your IT folks the precious gift of time to invest into taking your organization to the next level!

The Chocolatey Agent service allows you to go further with your software management, bringing Chocolatey to desktop users in organizations that wish or have controlled environments. This provides users in controlled environments more empowerment and instant turn around on required software. This frees up IT folks and admins time to spend on making the organization even better.

## Usage

The Chocolatey agent enables two simultaneous modes of operation, one as an agent for a central console and the other as a background service for use in controlled environments. You can configure one or both modes.

### Setup

To install the background agent, you need to install the `chocolatey-agent` package. The chocolatey agent is only available for business edition customers to install from the licensed source (customers trialling the business edition will also be provided instructions on how to install).

#### Background Mode

##### Requirements

* Chocolatey v0.10.3+
* Chocolatey Licensed Edition (`chocolatey.extension`) v1.8.4+
* Chocolatey for Business edition

##### Background Mode Setup

To set Chocolatey in background mode, you need to run the following:

* `choco install chocolatey-agent <options>`
* `choco feature enable -n useBackgroundService`

### Chocolatey Background Service / Self-Service Installer

When an administrator installs the agent, they can configure Chocolatey to use background mode so that non-administrators can still perform installations of approved software as configured by an administrator.

Why this is desirable:

* Users do not need to be administrators but are still empowered to install and upgrade software.
* Users can only run install and upgrade in an administrative context.
* Shortcuts, desktop icons, etc created through Chocolatey functions will end up with the proper user.
* Users can only install approved software based on admin configured sources.
* This frees up precious IT bandwidth to work on other engagements.
* Empowers users, so they feel more in control.

This makes for happy users and happy admins as they are able to move quicker towards a better organization.

#### Self-Service Roadmap:

* Chocolatey GUI to detect background mode and adjust raising permissions.
* Admins will be able to schedule when upgrades occur.
* Admins will be able to configure what commands can be run through the background service.
* Admins will have more granular control of what certain users can install.

### Chocolatey Central Console

**NOTE:** The console is estimated to be available Q2 2017. All notes here are what we expect, but the standard disclaimer of availability and features apply.

Chocolatey will have a central core console that will allow you to manage your environments. You will need the Chocolatey Agent installed on all machines you wish to manage centrally.

#### Chocolatey Central Console Roadmap

The console will allow:

* Centralized Software Management for your entire organization.
* Centralized reporting of software.
* Know immediately what software is out of date and on what machines.
* Know within seconds the entire estate of software and what versions are installed.
  * Including zips and archives* that do not show up in Programs and Features
  * Including internal software* that does not show up in Programs and Features
* Adhoc reporting for a particular machine or set of machines
* Run arbitrary Chocolatey commands against one or more machines
* See how many machines you are actively managing in your organization
* More...

\* - When deployed through Chocolatey.

## See It In Action

coming soon

## FAQ

### How do I take advantage of Chocolatey Agent?
You must have a [Business edition of Chocolatey](https://chocolatey.org/compare). Business editions are great for organizations that need to manage the total software lifecycle.

### I'm a licensed customer, now what?
Once you have the agent service installed and Chocolatey for Business configured for background mode, most tools that use Chocolatey will automatically use the background service.

### I have Puppet or some other configuration management tool (RMM tool, infrastructure automation tool, etc.) that also runs Chocolatey. Can I configure it to skip background mode?

Yes! Add `--run-actual` to your install options. Most likely your tool won't need to be reconfigured though as it will just work with background mode. You will need Chocolatey v0.10.3+ installed across your environment so Chocolatey handles the unknown arguments appropriately.

### How does it work?
As a background service, it is able to call Chocolatey with an administrative account that is configured by you. It is secure communication that only starts once Chocolatey is configured to work with the background service.

### What's the minimum Chocolatey licenced edition version that I need to install the agent?

You need `chocolatey.extension` version 1.8.4+.

### How is it secure?

For background mode / self-service installer:

* Commands are ignored unless they come from the business edition of Chocolatey.
* Chocolatey installs can only be done from approved, configured sources.
* The background service validates commands prior to running.
* Attempted abuses of the service are logged for further review by an administrator later.

For the central console:

* Coming later when central console is more complete.
