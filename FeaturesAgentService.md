# Chocolatey Agent Service

> Empower your users and give your IT folks the precious gift of time to invest into taking your organization to the next level!

The Chocolatey Agent service allows you to go further with your software management, bringing Chocolatey to desktop users in organizations that have controlled environments. This provides users in controlled environments more empowerment and instant turn around on required software. This frees up IT folks and admins time to spend on making the organization even better.

<!-- TOC -->

- [Usage](#usage)
  - [Requirements](#requirements)
  - [Setup](#setup)
    - [Background Mode Setup](#background-mode-setup)
  - [Chocolatey Background Service / Self-Service Installer](#chocolatey-background-service--self-service-installer)
    - [Self-Service Roadmap:](#self-service-roadmap)
  - [Chocolatey Central Console](#chocolatey-central-console)
    - [Chocolatey Central Console Roadmap](#chocolatey-central-console-roadmap)
- [See It In Action](#see-it-in-action)
- [FAQ](#faq)
  - [How do I take advantage of Chocolatey Agent?](#how-do-i-take-advantage-of-chocolatey-agent)
  - [I'm a licensed customer, now what?](#im-a-licensed-customer-now-what)
  - [I have Puppet or some other configuration management tool (RMM tool, infrastructure automation tool, etc.) that also runs Chocolatey. Can I configure it to skip background mode?](#i-have-puppet-or-some-other-configuration-management-tool-rmm-tool-infrastructure-automation-tool-etc-that-also-runs-chocolatey-can-i-configure-it-to-skip-background-mode)
  - [How does it work?](#how-does-it-work)
  - [What's the minimum Chocolatey licensed extension version that I need to install the agent?](#whats-the-minimum-chocolatey-licensed-extension-version-that-i-need-to-install-the-agent)
  - [How is it secure?](#how-is-it-secure)
  - [Do you have an example of a message that goes across the agent service named pipe, from the client?](#do-you-have-an-example-of-a-message-that-goes-across-the-agent-service-named-pipe-from-the-client)
  - [What is the purpose of the hash that is used to protect the named pipe?](#what-is-the-purpose-of-the-hash-that-is-used-to-protect-the-named-pipe)
  - [Does the agent service or Chocolatey stop installation from unconfigured sources?](#does-the-agent-service-or-chocolatey-stop-installation-from-unconfigured-sources)

<!-- /TOC -->

## Usage

The Chocolatey agent enables two simultaneous modes of operation, one as an agent for a central console and the other as a background service for use in controlled environments. You can configure one or both modes.

### Requirements

* Chocolatey v0.10.3+ - v0.10.4+ is recommended for better compatibility.
* Chocolatey for Business (C4B) Edition
* Chocolatey Licensed Extension (`chocolatey.extension`) v1.8.4+. For chocolatey-agent v0.5.0+, licensed edition v1.9.0+.

### Setup

To install the Chocolatey agent service, you need to install the `chocolatey-agent` package. The chocolatey agent is only available for business edition customers to install from the licensed source (customers trialling the business edition will also be provided instructions on how to install).

#### Background Mode Setup

To set Chocolatey in background mode, you need to run the following:

* `choco install chocolatey-agent <options>`
* `choco feature enable -n useBackgroundService`
* `choco feature disable -n showNonElevatedWarnings` - Chocolatey v0.10.4+

This will install Chocolatey Agent as LocalSystem (`SYSTEM`). To change the user, edit the username/password in the services management console on `Chocolatey Agent` properties and restart the service. Currently you will need to do this on upgrade as well.

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

Here's a short 8 minute walkthrough (preview):

[![Chocolatey's Self-Service Install - Background Mode (Preview)](https://cloud.githubusercontent.com/assets/63502/21634430/d8b94416-d21c-11e6-80c6-6a1b6def72fc.png)](https://www.youtube.com/watch?v=6HRmbTQ9wNM "Chocolatey's Self-Service Install - Background Mode (Preview)")


Consider the following image:

![Attempting to install software as non-admin - if you are on https://chocolatey.org/docs/features-agent-service, see commented html below for detailed description of image](images/features/features_non_admin_installer.png)

<!--
Text in the image above:

Non-administrators need an administrator to perform installs.

This image shows attempting to install VLC as a non-administrator and having the computer show the question for an administrator username and password to continue.
-->

This is the status quo for a non-administrative user. Can't install software without the help of an administrator. That takes up time, time for both the user waiting to get work done and the IT admin that performs the work.

Now, how does that change once we have background mode?

![Installing software with Chocolatey's background mode from the command line. - if you are on https://chocolatey.org/docs/features-agent-service, see commented html below for detailed description of image](images/features/features_non_admin_selfservice.png)

<!--
Text in the image above:

Background Mode / Self-Service Installer

- Non-admin can install only from approved, configured sources
- Chocolatey Agent Service validates commands prior to running
- Output streams as it happens
- Attempted abuses are logged for further review
- Background mode only for install / upgrade (configuring commands allowed on roadmap)
- GUI on roadmap

This image shows running `choco install adobereader -y`.
-->

Once you've configured background mode and configured approved sources for installation, a user can install only those approved applications using the command line or the Chocolatey GUI (coming soon).


Now, if a user wants to install from a non-approved source, they are met with the following message:
![Not able to install from custom source](images/features/features_non_admin_custom_source_error.png)

This ensures non-admin users can only install from sources that you configure.

## FAQ

### How do I take advantage of Chocolatey Agent?
You must have a [Business edition of Chocolatey](https://chocolatey.org/compare). Business editions are great for organizations that need to manage the total software lifecycle.

### I'm a licensed customer, now what?
Once you have the agent service installed and Chocolatey for Business configured for background mode, most tools that use Chocolatey will automatically use the background service.

### I have Puppet or some other configuration management tool (RMM tool, infrastructure automation tool, etc.) that also runs Chocolatey. Can I configure it to skip background mode?

Yes! Add `--run-actual` to your install options. Most likely your tool won't need to be reconfigured though as it will just work with background mode. You will need Chocolatey v0.10.3+ installed across your environment so Chocolatey handles the unknown arguments appropriately.

### How does it work?
As a background service, it is able to call Chocolatey with an administrative account that is configured by you. It is secure communication that only starts once Chocolatey is configured to work with the background service.

### What's the minimum Chocolatey licensed extension version that I need to install the agent?

You need `chocolatey.extension` version 1.8.4+.

### How is it secure?

For background mode / self-service installer:

* Commands are ignored unless they come from the business edition of Chocolatey.
* Chocolatey installs can only be done from approved, configured sources.
* The background service validates commands prior to running.
* Attempted abuses of the service are logged for further review by an administrator later.

For the central console:

* Coming later when central console is more complete.
* Communication will be done over TLS (w/self-signed certificate) or another medium with message encryption

### Do you have an example of a message that goes across the agent service named pipe, from the client?

The message is a serialized object that contains:

* hashed passcode - SHA512 Hash of args, user, current directory, and a salt value only shared with agent and licensed edition
* command arguments to run - verified against validation checks
* username - this is what we'll use to ensure things like desktop shortcuts, etc
* timeout - how long before the command should timeout (from choco config)
* working directory - where is the context of this being executed, in case there are things to place relative to current directory

Here is the interface:

`void run_choco_command(string passcode, IEnumerable<string> arguments, string userName, int timeout, string workingDirectory);`

Keep in mind this message is only put on localhost. It does not go over any networks.

### What is the purpose of the hash that is used to protect the named pipe?
You may notice the hash changes every time based on what command is called. This is a security measure to ensure the call is coming from a configured Chocolatey client and not from another source. The agent will ignore anything that does not match up.

### Does the agent service or Chocolatey stop installation from unconfigured sources?

The agent stops unconfigured sources from installation. Right now it simply logs those abuses to the log file (that is locked down to admins for modify). The log file can be slurped into a tool like Splunk. Alternatively considering this is preview and we are waiting for feedback, we can look to providing those alerts in a different way, like the event log. We welcome any feedback on how you might like to see this.

Chocolatey doesn't stop unconfigured sources for install, it lets the agent do so. Once Chocolatey is in background mode, all commands for install/upgrade go through the agent service.

The one exception is when someone calls `--run-actual` in their arguments. But there is no escalation of privilege here because they would be running that under their own user context and thus only have the permissions granted to them already.

