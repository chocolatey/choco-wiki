# Synchronize with Programs And Features (Licensed Editions Only)

Chocolatey maintains its own state of the world, while Windows maintains the state of Programs and Features. If an application is upgraded or uninstalled outside of Chocolatey, such as is the case with Google Chrome and its auto updating utility, Chocolatey doesn't know about the change. The synchronize feature keeps Chocolatey's state in sync with Programs and Features, removing possible system-installed state drift.

* [Automatic Sync](#automatic-synchronize)
* [Synchronize Command](#sync-command)

# Automatic Synchronize

In licensed editions of Chocolatey, synchronize for existing packages that are tracking to software installed in Programs and Features happens automatically and takes effect prior to the command running.

## Usage

![Synchronize - if you are on https://chocolatey.org/docs/features-synchronize, see commented html below for detailed description of image](images/features/features_synchronize.png)

<!--
Text in the image above:

Synchronize with Programs and Features

- Chocolatey for Business - any Chocolatey command will trigger synchronization
- Synchronizes state between Chocoaltey and Programs and Features
- Currently supports manual software removals
- Adding upgrade tracking

This image shows running `choco list -lo`. Chocolatey for Business automatically detects that 1Password has been manually uninstalled and synchronizes Chocolatey's state.
-->

## See It In Action

![auto package creation/synchronize](images/gifs/choco_business_features.gif)

## Options and Switches

N/A

## FAQ

### How do I take advantage of this feature?
You must have a [licensed edition of Chocolatey](https://chocolatey.org/pricing) (Pro, Business, or MSP). Pro is a personal, named license that costs about the price of a lunch outing per month and comes with several other features. Business editions are great for organizations that need to manage the total software management lifecycle. MSP editions contain a subset of the Business edition features.

### I'm a licensed customer, now what?
It just works.

### How does it work?
Chocolatey tracks applications that it installs, so it is able to keep up with those applications as they are upgraded and uninstalled, even outside of Chocolatey.

# Sync Command

Starting in 1.9.0 of the licensed extension, sync has been added as a preview feature for organizations to try out.

Sync looks at all software that is in Programs and Features that is not being managed with Chocolatey packages and brings them under management. This means you can run one command and suddenly, all of the software installed on a machine is under management by Chocolatey!

## Usage

To synchronize your system, Simply call `choco sync` and Chocolatey will ensure that all software in Programs and Features comes under Chocolatey management and provides you the packages/package sources so you can add them to source control for managing those packages over time.

### Setup
At 1.9.0, sync is in preview. You need to turn it on by enabling the feature  `allowPreviewFeatures`:

* `choco feature enable -n allowPreviewFeatures`

## See it in action

We've prepared a short video to show sync in action:
[![Chocolatey's Package Synchronizer - Sync Command](https://cloud.githubusercontent.com/assets/63502/22050108/ade28e5a-dcfd-11e6-8835-afc9f699e400.png)](https://youtu.be/tzSsYHYsjf4 "Chocolatey's Package Synchronizer - Sync Command")

In the following image, sync is run on a system that has 18 applications installed and a base Chocolatey for Business install. Note after running sync, all software on the machine is now being managed by Chocolatey.

![Chocolatey's Package Synchronizer Sync Command - if you are on https://chocolatey.org/docs/features-synchronize, see commented html below for detailed description of image](images/features/features_choco_sync.png)

<!--
Text in the image above:

Package Synchronizer's Sync Command

- Brings unmanaged software under Chocolatey management
- Provides you the source packaging and package output
- Automate existing systems in under 90 seconds!
- Machine parseable with `-r`
- Links existing packages not tracking to Programs and Features

This image shows running `choco sync`. It shows first a system that has 18 applications installed outside of Chocolatey, then runs `choco sync` and watches Chocolatey generate packages and baseline the system. Then it shows `choco list -lo --include-programs` again, which shows that all software on the machine is now being managed by Chocolatey.

-->

## Options and Switches

N/A at this time. More switches will be added as this feature is enhanced

## FAQ

### How do I take advantage of this feature?
You must have the [business edition of Chocolatey](https://chocolatey.org/pricing). Business editions are great for organizations that need to manage the total software management lifecycle.

### I'm a business customer, now what?
You would periodically run `choco sync`.

### How does it work?
Chocolatey takes a look at all software in Programs and Features that is not under Chocolatey management, generates packages on the fly and baselines them under the Chocolatey install, ensuring all of the links are tracked.

### Do I get the packages to add to source?

Yes! Chocolatey will tell you the location of the sync files so you can put them into source control.

### Some packages have a TODO list
Generating packages on the fly from Programs and Features for non-MSI installers doesn't provide everything necessary to ensure an actual install. So when you take those packages back to source, you will need to finish out the packaging for those so that later when you upgrade, things will work appropriately.

### How do I get machine parseable output?

Use `-r`. `choco sync -r`. Requires Chocolatey v0.10.4+.

### What if I have an existing package that is just not tracking to Programs and Features?

Synchronize can recognize existing packages and sync to those as long as the name of the package is a close match to the software name (e.g. Google Chrome becomes either google-chrome or googlechrome).

## Sync Command Known issues

* Any packages you've installed side by side (`-m`) will show up every time during sync.
* If you have both a 64-bit and 32-bit version of some software installed, sync will track to one on the first run and the other on the next run. This is not a normal scenario.
