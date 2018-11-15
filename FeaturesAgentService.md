# Chocolatey Agent Service (Business Editions Only)

> Empower your users and give your IT folks the precious gift of time to invest into taking your organization to the next level!

The Chocolatey Agent service allows you to go further with your software management, bringing Chocolatey to desktop users in organizations that have controlled environments. This provides users in controlled environments more empowerment and instant turn around on required software. This frees up IT folks and admins time to spend on making the organization even better.

<!-- TOC depthTo:5 -->

- [Usage](#usage)
  - [Requirements](#requirements)
  - [Setup](#setup)
    - [Background Mode Setup](#background-mode-setup)
      - [Chocolatey Agent Install Options](#chocolatey-agent-install-options)
      - [Command Customization Consideration](#command-customization-consideration)
      - [Interactive Self-Service Consideration](#interactive-self-service-consideration)
  - [Chocolatey Background Service / Self-Service Installer](#chocolatey-background-service--self-service-installer)
    - [Self-Service Roadmap:](#self-service-roadmap)
  - [Chocolatey Central Console](#chocolatey-central-console)
    - [Chocolatey Central Console Roadmap](#chocolatey-central-console-roadmap)
- [See It In Action](#see-it-in-action)
- [FAQ](#faq)
  - [How do I take advantage of Chocolatey Agent?](#how-do-i-take-advantage-of-chocolatey-agent)
  - [I'm a licensed customer, now what?](#im-a-licensed-customer-now-what)
  - [Will this become available for lower editions of Chocolatey?](#will-this-become-available-for-lower-editions-of-chocolatey)
  - [I have Puppet or some other configuration management tool (RMM tool, infrastructure automation tool, etc.) that also runs Chocolatey. Can I configure it to skip background mode?](#i-have-puppet-or-some-other-configuration-management-tool-rmm-tool-infrastructure-automation-tool-etc-that-also-runs-chocolatey-can-i-configure-it-to-skip-background-mode)
  - [How does it work?](#how-does-it-work)
  - [What's the minimum Chocolatey licensed extension version that I need to install the agent?](#whats-the-minimum-chocolatey-licensed-extension-version-that-i-need-to-install-the-agent)
  - [How is it secure?](#how-is-it-secure)
  - [Do you have an example of a message that goes across the agent service named pipe, from the client?](#do-you-have-an-example-of-a-message-that-goes-across-the-agent-service-named-pipe-from-the-client)
  - [What is the purpose of the hash that is used to protect the named pipe?](#what-is-the-purpose-of-the-hash-that-is-used-to-protect-the-named-pipe)
  - [Does the agent service or Chocolatey stop installation from unconfigured sources?](#does-the-agent-service-or-chocolatey-stop-installation-from-unconfigured-sources)
  - [We want to set up the chocolatey agent service to use a domain account that will have local admin on each box. Can we do this?](#we-want-to-set-up-the-chocolatey-agent-service-to-use-a-domain-account-that-will-have-local-admin-on-each-box-can-we-do-this)
  - [Is the password stored anywhere?](#is-the-password-stored-anywhere)
  - [We are going to use our own account with a rotating password. When we rotate the password for the account that we use for the Chocolatey Agent, what do we need to do?](#we-are-going-to-use-our-own-account-with-a-rotating-password-when-we-rotate-the-password-for-the-account-that-we-use-for-the-chocolatey-agent-what-do-we-need-to-do)
  - [Tell me more about the Chocolatey managed password.](#tell-me-more-about-the-chocolatey-managed-password)
  - [Is the managed password stored or logged anywhere?](#is-the-managed-password-stored-or-logged-anywhere)
  - [Is the managed password the same on every machine?](#is-the-managed-password-the-same-on-every-machine)
  - [How would someone potentially get access to the managed password?](#how-would-someone-potentially-get-access-to-the-managed-password)
  - [Do you rotate the managed password on a schedule?](#do-you-rotate-the-managed-password-on-a-schedule)
  - [Can I take advantage of Chocolatey managed passwords with my own Windows services?](#can-i-take-advantage-of-chocolatey-managed-passwords-with-my-own-windows-services)
- [Common Errors and Resolutions](#common-errors-and-resolutions)
  - [Installs from custom source locations are not allowed in background mode. Please remove custom source and try again using default (configured) package source locations.](#installs-from-custom-source-locations-are-not-allowed-in-background-mode-please-remove-custom-source-and-try-again-using-default-configured-package-source-locations)
  - [I'm getting the following: "There are no sources enabled for packages and none were passed as arguments."](#im-getting-the-following-there-are-no-sources-enabled-for-packages-and-none-were-passed-as-arguments)
  - [I'm having trouble seeing packages on a file share source](#im-having-trouble-seeing-packages-on-a-file-share-source)

<!-- /TOC -->

## Usage

The Chocolatey agent enables two simultaneous modes of operation, one as an agent for a central console (upcoming) and the other as a background service for use in controlled environments. You can configure one or both modes.

### Requirements

* Chocolatey (`chocolatey` package) v0.10.3+ - v0.10.4+ for better compatibility. Chocolatey v0.10.7+ is recommended and required in newer versions.
* Chocolatey for Business (C4B) Edition
* Chocolatey Licensed Extension (`chocolatey.extension` package) v1.8.4+. For chocolatey-agent v0.5.0+, licensed extension v1.9.0+. For `chocolatey-agent` v0.7.0+, licensed extension v1.11.0+.
* Chocolatey Agent Service (`chocolatey-agent` package) - 0.7.0+ is recommended.

For use with Chocolatey GUI, you must be on Chocolatey v0.10.7+, Chocolatey Licensed Extension v1.11.0+, and Chocolatey Agent v0.7.0+.

### Setup

To install the Chocolatey agent service, you need to install the `chocolatey-agent` package. The Chocolatey Agent is only available for business edition customers to install from the licensed source (customers trialling the business edition will also be provided instructions on how to install).

#### Background Mode Setup

To set Chocolatey in background mode, you need to run the following:

* `choco upgrade chocolatey-agent <options>` (see [agent install options](#chocolatey-agent-install-options))
* `choco feature disable --name=showNonElevatedWarnings` - requires Chocolatey v0.10.4+ to set.
* `choco feature enable --name=useBackgroundService`
* You also need to opt in sources in for self-service packages. See [[choco source|CommandsSource]] (and `--allow-self-service`). You can also run `choco source -?` to get the help menu.
    * OPTIONAL (not recommended): Alternatively, you can allow any configured source to be used for self-service by running the following: `choco feature disable --name=useBackgroundServiceWithSelfServiceSourcesOnly` (requires Chocolatey Extension v1.10.0+). We do not recommend this as it could be a security finding if you shut it off.
* OPTIONAL (highly recommended): If you want self-service to apply only to non-administrators, run `choco feature enable --name=useBackgroundServiceWithNonAdministratorsOnly` (requires Chocolatey Extension v1.11.1+). Do understand this means that a real non-administrator, not an administrator in a non-elevated UAC context (that scenario will go the normal route and will not go through background mode).
* OPTIONAL (varied recommendations): If you want to configure custom commands (not just install/upgrade), use something like `choco config set backgroundServiceAllowedCommands "install,upgrade,pin,sync"` (with the commands you want to allow, requires Chocolatey Extension v1.12.4+). See [commands consideration](#command-customization-consideration) below.
* OPTIONAL (highly recommended): For use with Chocolatey GUI, you need Chocolatey Extension v1.12.4+, and at least Chocolatey GUI v0.15.0. **Uninstall any version of the GUI you already have installed first**, then run `choco upgrade chocolateygui -y --allow-downgrade` (you will also need at least .NET 4.5.2 installed)
* DOES NOT WORK WITH UAC, DO NOT USE UNTIL [FIX IS ANNOUNCED](https://groups.google.com/group/chocolatey-announce)! OPTIONAL (recommended if you use installers that are not completely silent): If you want self-service to interactively manage installations, run `choco feature enable --name=useBackgroundServiceInteractively` (requires Chocolatey Extension v1.12.10+). This requires that you use the `ChocolateyLocalAdmin` account with the Chocolatey-managed password as passwords are not stored and the service would need to produce that at runtime. There are some security considerations and why this is not turned on by default. Please see [interactive self-service consideration](#interactive-self-service-consideration).

**NOTE**: Once you are all setup, please review the [Common Errors and Resolutions](#common-errors-and-resolutions) section so you will be familiar if you run into any issues with working with sources.

An example script:

This carries our typical recommendations, but you could adjust from above.

~~~powershell
choco upgrade chocolatey-agent -y
choco feature disable --name="'showNonElevatedWarnings'"
choco feature enable --name="'useBackgroundService'"
choco feature enable --name="'useBackgroundServiceWithNonAdministratorsOnly'"

# TODO: opt in your sources with --allow-self-service - run choco source -? for details
~~~

> Best practices in scripts are noted here:
> * Use `upgrade` instead of `install` - upgrade is more making the script reusable when newer versions are available.
> * Always use `-y` to ensure nothing stops and prompts for more than 30 seconds.
> * When using options prefer a longer name (`--name` versus the short `-n`) to make the scripts more self-documenting
> * When using options that have a value passed, add an `=` between and surround the value with `"''"` (`--name="'value'"`). This ensures that the argument is not split between different versions/editions of Chocolatey. This also ensures that values like `.` and `\\` are not escaped by PowerShell.

##### Chocolatey Agent Install Options

Starting with Chocolatey Agent v0.8.0+, the service will install as a local administrative user `ChocolateyLocalAdmin` by default (and manage the password as well). However you can specify your own user with package parameters (or have it use `LocalSystem`). Using a local administrator account allows for more things to be installed without issues. It also will allow easier shortcuts and other items to be put back on the correct user (the original requestor). You can specify a domain account as well. Prior to `v0.8.0`, Chocolatey Agent would install as LocalSystem (`SYSTEM`) and would require additional customization.

**NOTE:** If you are using file shares for sources, you may want to ensure the account or computer has network access permissions for the file share(s).

**Package Parameters**:

* `/Username:` - provide username - intead of using the default 'ChocolateyLocalAdmin' user.
* `/Password:` - optional password for the user.
* `/EnterPassword` - receive the password at runtime as a secure string
* `/UseDefaultChocolateyConfigUser` - use the default username from Chocolatey's configuration. This may be LocalSystem.
* `/NoRestartService` - do not shut down and restart the service. You will need to restart later to take advantage of new service information.

**Chocolatey Managed Password**

When Chocolatey manages the password for a local administrator, it creates a very complex password:

* It is 32 characters long.
* It uses uppercase, lowercase, numbers, and symbols to meet very stringent complexity requirements.
* The password is different for every machine.
* Due to the way that it is generated, it is completely unguessable.
* No one at Chocolatey Software could even tell you what the password is for a particular machine without local access.

See [FAQ](#faq) below for more discussion on security aspects.

##### Command Customization Consideration

Starting with Chocolatey Licensed Extension v1.12.4, you are allowed to configure what commands can be routed through the background service. Please note that Chocolatey Licensed defaults to `install` and `upgrade` as that is the most secure experience. However you can add uninstall and some other commands as well. Uninstall does have some security considerations as it would allow a non-administrator to remove software that you may have installed, including the background service itself.

**Available Commands**:

* info - do not add if you want sources hidden from non-admins
* list/search - do not add if you want sources hidden from non-admins
* outdated - do not add if you want sources hidden from non-admins
* install - default
* upgrade - default
* uninstall - keep in mind there may be security implications for this
* optimize
* pin
* sync
* download - Chocolatey Licensed Extension v1.12.12+ - keep in mind if you have shut off a non-admin's ability to run this they still won't be able to without also disabling the `adminOnlyExecutionForDownloadCommand` feature.

**Blacklisted Commands**:

* config
* feature
* source
* apikey

Chocolatey does not allow for configuration changing commands to be routed through the background service as that would allow users to be able to change configuration and that could be detrimental. For instance, a user could add a local source with a package they've created that promotes themselves to an administrator (escalation of privilege). As that constitutes a security issue, we do not allow it.

For the same reason, we do not recommend allowing sources you do not control to be allowed for self-service.

##### Interactive Self-Service Consideration
When using the self-service with `useBackgroundServiceInteractively`, it is similar to "Run As". Microsoft used to allow Windows services to interact with the desktop but removed the functionality (of "Allow Service to Interact with the Desktop") in Windows Vista and limited all services into what is known as Session 0 isolation. In Session 0, those services can access a desktop, but not the interactive user's desktop (at least it is very, very difficult to do so). Microsoft did this as a security consideration as allowing a privileged account to run executables in a non-administrative user context opens the potential to allow a non-admin to gain privileges to a system (known as escalation of privilege). Depending on what is allowed for folks to install, as long as those package installations do not open a command shell and wait for user input, the potential is quite low. However it is something to keep in mind and assess before turning on this feature (and why it is off by default).

However sometimes you work with installers that refuse to be silent. You might get them down to unattended (meaning they still have required input or window pop ups, but they are all handled and automatically closed), but they won't get to silent without a different option. Here are some options from most preferred to least preferred in getting those installers that are not fully silent to work:

* (Silent) Find a portable version of the tool that doesn't run a badly behaved installer. Looked for a zipped up version, you can easily create a Chocolatey package from these.
* (Silent) Find an alternative that does the same thing, but has silent installers. It's a consideration, but maybe not something you can or are willing to explore. Moving on...
* (Silent) Use MSI repackaging to produce a completely silent installer. It works by recording everything that happens when you run the install and creates an MSI to do the same. This would need to be done for every version. There are tools out there that can do this, some of which are VERY expensive.
* (Unattended/Interactive) Use AutoHotKey or AutoIT (or some other tool) to automate the key presses and values being sent to the interfaces. This can be brittle, but can typically be quickly implemented.

If you must run in the context of working with "unattended", non-silent installations, you need to take the above security consideration into account if you want to be able to manage those installations using the background service.

### Chocolatey Background Service / Self-Service Installer
When an administrator installs the agent, they can configure Chocolatey to use background mode so that non-administrators can still perform installations of approved software as configured by an administrator.

Why this is desirable:

* Users do not need to be administrators but are still empowered to install and upgrade software (functions are configurable with Chocolatey Extension v1.12.4+)
* Users can ***only*** run install and upgrade in an administrative context by default. This is configurable to other commands as of Chocolatey Licensed Extension 1.12.4+..
* Shortcuts, desktop icons, etc created through Chocolatey functions will end up with the proper user (still coming).
* Users can only install approved software based on admin configured sources.
* This frees up precious IT bandwidth to work on other engagements.
* Empowers users, so they feel more in control.

This makes for happy users and happy admins as they are able to move quicker towards a better organization.

#### Self-Service Roadmap:

* Admins will be able to schedule when upgrades occur (maintenance windows).
* ~~Admins will be able to configure what commands can be run through the background service.~~ Completed with Chocolatey Extension v1.12.4.
* Admins will have more granular control of what certain users can install.

### Chocolatey Central Console

**NOTE:** The console is estimated to be available by end of 2018. All notes here are what we expect, but the standard disclaimer of availability and features apply.

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
- Background mode only for install / upgrade by default
- GUI on roadmap

This image shows running `choco install adobereader -y`.
-->

Once you've configured background mode and configured approved sources for installation, a user can install only those approved applications using the command line or the Chocolatey GUI.

Now, if a user wants to install from a non-approved source, they are met with the following message:
![Not able to install from custom source](images/features/features_non_admin_custom_source_error.png)

This ensures non-admin users can only install from sources that you configure.


## FAQ

### How do I take advantage of Chocolatey Agent?
You must have a [Business edition of Chocolatey](https://chocolatey.org/compare). Business editions are great for organizations that need to manage the total software lifecycle.

### I'm a licensed customer, now what?
Once you have the agent service installed and Chocolatey for Business configured for background mode (see [Setup](#setup) above), most tools that use Chocolatey will automatically use the background service.

### Will this become available for lower editions of Chocolatey?
The background service and Central Management UI Console will only be available in C4B (Chocolatey for Business).

### I have Puppet or some other configuration management tool (RMM tool, infrastructure automation tool, etc.) that also runs Chocolatey. Can I configure it to skip background mode?
Yes! Add `--run-actual` to your install options. Most likely your tool won't need to be reconfigured though as it will just work with background mode. You will need Chocolatey v0.10.3+ installed across your environment so Chocolatey handles the unknown arguments appropriately.

Another (possibly better) way to handle this as of Chocolatey Extension v1.12.0, turn on the `useBackgroundServiceWithNonAdministratorsOnly` feature to make Self-Service apply only to non-administrators. See [Background Mode Setup](#background-mode-setup) for details.

### How does it work?
As a background service, it is able to call Chocolatey with an administrative account that is configured by you. It is secure communication that only starts once Chocolatey is configured to work with the background service.

### What's the minimum Chocolatey licensed extension version that I need to install the agent?
You need `chocolatey.extension` version 1.8.4+.

### How is it secure?
For Background Mode / Self-Service Installer:

* Commands are ignored unless they come from the business edition of Chocolatey.
* Chocolatey installs can only be done from approved, configured sources.
* The background service validates commands prior to running.
* Attempted abuses of the service are logged for further review by an administrator later.

For the Central Management UI / Console:

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

### We want to set up the chocolatey agent service to use a domain account that will have local admin on each box. Can we do this?
Yes, absolutely. You will pass those credentials through at install/upgrade time, and you will also want to turn on the feature `useRememberedArgumentsForUpgrades` (see [[configuration|ChocolateyConfiguration#features]]) so that future upgrades will have that information available. The remembered arguments are stored encrypted on the box (that encryption is reversible so you may opt to pass that information each time).

* `/Username:` - provide username - intead of using the default 'ChocolateyLocalAdmin' user.
* `/Password:` - optional password for the user.
* `/EnterPassword` - receive the password at runtime as a secure string

You would pass something like `choco install chocolatey-agent -y --params="'/Username:domain\account /EnterPassword'"` to securely pass the password at runtime. You could also run `choco install chocolatey-agent -y --params="'/Username:<domain\account>'" --package-parameters-sensitive="'/Password:<password>'"` (or do it as part of `choco upgrade`).

With Puppet, this could look something like:

~~~puppet
package {'chocolatey-agent':
  ensure          => latest,
  install_options => ['-pre','--params="',"'/Username:<domain\account>'", '"','--package-parameters-sensitive="', "'/Password:<password>'", '"'],
  require         => Chocolateyfeature['useLocalSystemForServiceInstalls'],
}
~~~

where `<domain\account>` and `<password>` could be pulled from Hiera or somewhere else. If you have access to the Puppet secrets type, then you can use that here as well.

### Is the password stored anywhere?
No, that would reduce the security of the password. It exists in memory long enough to set the value on user and the service and then it is cleared.

There is no storage of the password anywhere other than how Windows stores passwords.

### We are going to use our own account with a rotating password. When we rotate the password for the account that we use for the Chocolatey Agent, what do we need to do?
Like with any service that uses rotating passwords, you will need to redeploy the service or go into the services management console and update the password. As it is much faster to deploy out that update, you can do something like `choco upgrade chocolatey-agent -y --params="'/Username:domain\account'" --package-parameters-sensitive="'/Password:newpassword'" --force` (the `--force` ensures the code is redeployed).

### Tell me more about the Chocolatey managed password.
So you've seen from above that

* It is 32 characters long.
* It uses uppercase, lowercase, numbers, and symbols to meet very stringent complexity requirements.
* The password is different for every machine.
* Due to the way that it is generated, it is completely unguessable.
* No one at Chocolatey Software could even tell you what the password is for a particular machine without local access.

Chocolatey uses something unique about each system, along with an encrypted value in the licensed code base to generate base password, then it makes some other changes to ensure that the password meets complexity requirements. We won't give you the full algorithm of how the password is generated as knowing the algorithm would be a security issue - like having a partial picture of a key, you could start working on how to break in. Unlike a picture of a key, even knowing the full algorithm doesn't get you everything you need as you would need local access to each box to determine the password for ***each*** machine.

### Is the managed password stored or logged anywhere?
No, that would reduce the security of the password. It exists in memory long enough to set the value on user and the service and then it is cleared.

There is no storage of the password anywhere other than how Windows stores passwords.

### Is the managed password the same on every machine?
No, it is different for every machine it is deployed to.

### How would someone potentially get access to the managed password?
The Chocolatey licensed code base is encrypted, so only people that work at Chocolatey Software would be able to determine the password for a particular box (just that one) ***IF*** they have local access to that box. Even with all of the information and the algorithm, it's still going to take our folks a while to determine the password. That gets them access to one machine. Of course, Chocolatey folks are not going to do this for obvious reasons.

So let's realize this to its full potential - If someone were able to hack the Chocolatey licensed codebase, they would be able to determine the full password algorithm. Then they'd also need to hack into your infrastructure and get local access to every box that they wanted to get the Chocolatey-managed password so they could get admin access to just that box. Taking this out a bit further, it's reasonable to assume that if someone has hacked into your infrastructure, it's highly unlikely they are going to be using a non-administrator account to get local access to a box so they can get the password for an administrator account for just that one box. It's more likely they would would already have a local admin account for the boxes they are attacking, and are likely to seek other attack vectors that are much less sophisticated.

### Do you rotate the managed password on a schedule?
We are looking to do this in a future release. We may make the schedule configurable.

### Can I take advantage of Chocolatey managed passwords with my own Windows services?
Yes, absolutely. If you use C4B's PowerShell Windows Services code, you will be able to install services and have Chocolatey manage the password for those as well.


## Common Errors and Resolutions

### Installs from custom source locations are not allowed in background mode. Please remove custom source and try again using default (configured) package source locations.
You can not pass custom source arguments to Chocolatey, it will error. You need to set up sources in the Chocolatey configuration and any that are marked as allowed for self-service will be passed by the background service.

**NOTE:** If you have run `choco feature disable -n useBackgroundServiceWithSelfServiceSourcesOnly`, then all configured sources will be passed by the background service.

### I'm getting the following: "There are no sources enabled for packages and none were passed as arguments."
This means you need to opt a source into self-service (new in Chocolatey Extension v1.10).

This just involves ensuring a source is set so that it allows self-service. To do this you run `choco source add -n name -s location <--other details need repeated> --allow-self-service`. Editing a source happens when the name is the same in `choco source add`.

To change this behavior back to the way it was previously, simply run `choco disable -n useBackgroundServiceWithSelfServiceSourcesOnly`. For feature options, run `choco feature list` or see [[Self-Service Feature Configuration|ChocolateyConfiguration#self-service-background-mode]]

### I'm having trouble seeing packages on a file share source
Starting with Chocolatey Agent v0.8.0+, the service will default an install to a local administrative user `ChocolateyLocalAdmin`, but before that it installed as LocalSystem by default. These accounts may not have network access to UNC shares. We recommend changing the service to a named account that is a local admin that would also have network access (or setting machines into an active directory group and explicitly giving that group read permissions to the share and ACL). To specify your own user, you can do that at install time with [package parameters](#chocolatey-agent-install-options), or you can do that in the Service Manager properties for the service itself (future upgrades would need you to pass that user/pass at least the first time).

So if you've set up a source like `choco source add -n="'name'" -s="'\\unc\packages'" --priority=1`, by default this may not work with the Chocolatey Agent. You would need to grant access to machines or anonymous access to the share (Everyone Read is likely not enough).

A great read on your options can be found at the following Stack Exchange links:

* https://serverfault.com/q/135867/79259
* https://serverfault.com/q/41130/79259

A way to do this with LocalSystem:

1. Create a global group on the Domain
    * add all machines to this group
1. Add this group to the share permissions with "Read" Access
1. Add this group to the NTFS permissions with "Read" Access

**Note**:  You'll need to add this group itself and not nest it inside of another one.
