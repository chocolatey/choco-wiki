# Install The Licensed Edition

Congratulations on your recent purchase of a licensed edition of Chocolatey! If you are trialing, please pay particular attention to that section:

<!-- TOC -->

- [Some Administrative Actions](#some-administrative-actions)
- [How Do I Know When the License is Installed?](#how-do-i-know-when-the-license-is-installed)
- [See It In Action](#see-it-in-action)
- [How Do I Install The Licensed Edition?](#how-do-i-install-the-licensed-edition)
  - [How Do I Install The Trial Edition?](#how-do-i-install-the-trial-edition)
- [Upgrading](#upgrading)
  - [How Do I Upgrade The Trial Edition?](#how-do-i-upgrade-the-trial-edition)
- [Installing / Upgrading In Secure Environments / Without Internet Access](#installing--upgrading-in-secure-environments--without-internet-access)
- [Set Up Licensed Edition With Puppet](#set-up-licensed-edition-with-puppet)
- [Install the Chocolatey Agent Service](#install-the-chocolatey-agent-service)
- [Common Errors and Resolutions](#common-errors-and-resolutions)
  - [Exception of type 'Rhino.Licensing.LicenseNotFoundException' was thrown.](#exception-of-type-rhinolicensinglicensenotfoundexception-was-thrown)
  - [ERROR: The term 'Install-ChocolateyWindowsService' is not recognized as the name of a cmdlet, function, script file, or operable program.](#error-the-term-install-chocolateywindowsservice-is-not-recognized-as-the-name-of-a-cmdlet-function-script-file-or-operable-program)

<!-- /TOC -->

## Some Administrative Actions

* The email the license is sent to will automatically be subscribed to the customer advisory list. If there are other folks who need to be on that list for important notifications, please have them sign up at [Chocolatey Customers](http://eepurl.com/b6zpGv).
* Please sign up for software announcements at [Chocolatey Announce](https://groups.google.com/d/forum/chocolatey-announce). Direct others to sign up for the list as well.
* For support, remember to use the support email address (below).
* Learn how to install below. Installation is also found at https://chocolatey.org/docs/installation-licensed.


## How Do I Know When the License is Installed?

Installing a licensed edition requires two parts

* The properly placed license file
* Installing/upgrading the licensed package - Chocolatey Licensed Extension (`chocolatey.extension`)

When you have performed all parts correctly, running `choco` will produce a message like one of the following:

* `Chocolatey v0.10.7 Professional`
* `Chocolatey v0.10.7 Business`

If you do not see the edition next to the name and version, you need to check over your installation procedure again to ensure you've completed it properly. See below.

## See It In Action
Here's the whole process for installing your license and installing the licensed code.

![install](https://raw.githubusercontent.com/wiki/chocolatey/choco/images/gifs/choco_install_pro.gif)

## How Do I Install The Licensed Edition?

 1. Install a recent version of Chocolatey (0.9.10+) - `choco upgrade chocolatey`.
 1. You received a license file in email.
 1. Take that license file and copy it to your Chocolatey install folder `license` subdirectory (you may need to create it first). For most folks that path would be `"C:\ProgramData\chocolatey\license"`. Alternatively, you can put the license in your user profile directory, e.g. `"C:\Users\YourUserName\chocolatey.license.xml"`

  ![license](https://cloud.githubusercontent.com/assets/63502/12741281/84a3df8e-c940-11e5-8818-456801728cf5.png)
 1. Run this command: `choco upgrade chocolatey.extension` (or you can call `install` instead of `upgrade`). You will see an error you can safely ignore.

  ![install/upgrade](https://cloud.githubusercontent.com/assets/63502/13052159/e6d1be92-d3c2-11e5-8856-d7580e51e3b6.png)
 1. That's it! You are good to go.

### How Do I Install The Trial Edition?

If you've received a trial license, you will also receive a link to download a recent version of the `chocolatey.extension` package. You will not be able to install or upgrade the licensed edition through regular means. Chocolatey may add the licensed source, but your license will not be recognized on the server.

 * Follow all of the instructions above except the `choco upgrade chocolatey.extension` command, that will not work with the trial license as the license will not be recognized by the licensed source.
 * Instead download the `chocolatey.extension` (licensed package) from the provided download link location and remember where you saved it.
 * Now run this command: `choco upgrade chocolatey.extension --pre --source c:\folder\where\downloaded\nupkg\resides` (or you can use `install` instead of `upgrade`). **Note**: Source location is not `--source c:\downloads\chocolatey.extension.1.8.1.nupkg`, it is `--source c:\downloads`.

## Upgrading

To upgrade the licensed edition just run the following code:

* `choco upgrade chocolatey.extension`

Your license automatically adds the licensed source.

### How Do I Upgrade The Trial Edition?

You will not be able to upgrade through regular means - please reach back out to the Chocolatey Software folks to get an updated edition (and possibly an extended trial license).

## Installing / Upgrading In Secure Environments / Without Internet Access

Once you have the license down and the licensed edition extension intstalled the first time, you will have access to `choco download`. This will allow you to download the licensed edition and put it on your internally hosted repository.

From a machine that will have access to do this you simply run:

* `choco download chocolatey.extension --source https://licensedpackages.chocolatey.org/api/v2/ --ignore-dependencies`
* Whatever followup command you need to push that downloaded package to your internal package repository.

You can even script this or add it to a CI job that would automatically make the newer edition available.

**NOTE**: The licensed source that is automatically added can be disabled, but it cannot be removed. So just run `choco source disable -n chocolatey.licensed` to disable it or set that up in your configuration management solution scripts. Some of them, like Puppet, have a resource dedicated strictly to this:

~~~puppet
chocolateysource {'chocolatey.licensed':
  ensure   => disabled,
  require  => File['C:/ProgramData/chocolatey/license/chocolatey.license.xml'],
}
~~~

## Set Up Licensed Edition With Puppet

Most organizations using Chocolatey and Puppet are going to do so with zero internet access. Here is what a completely offline ensurance of Chocolatey looks like (complete with a Chocolatey.Server instance):

~~~puppet
# ensure Chocolatey is installed - host the package internally
class {'chocolatey':
  chocolatey_download_url         => 'https://internalurl/to/chocolatey.nupkg',
}

# ensure installation of the Chocolatey Simple Server package repository
class {'chocolatey_server':
  server_package_source => 'https://internalurl/odata/server',
}

file { ['C:/ProgramData/chocolatey','C:/ProgramData/chocolatey/license']:
  ensure => directory,
}

file {'C:/ProgramData/chocolatey/license/chocolatey.license.xml':
  ensure             => file,
  source             => 'puppet:///modules/choco_internal/chocolatey.license.xml',
  source_permissions => ignore,
}

# configure sources
chocolateysource {'chocolatey':
  ensure   => absent,
}

chocolateysource {'chocolatey.licensed':
  ensure   => disabled,
  require  => File['C:/ProgramData/chocolatey/license/chocolatey.license.xml'],
}

chocolateysource {'internal_chocolatey':
  ensure   => enabled,
  location => 'http://internal/server',
  priority => 1,
}

# set features appropriately
chocolateyfeature {'useFipsCompliantChecksums':
  ensure => enabled,
}

# https://chocolatey.org/docs/features-automatically-recompile-packages
chocolateyfeature {'internalizeAppendUseOriginalLocation':
  ensure => enabled,
  require => Package['chocolatey.extension'],
}

# configuration
chocolateyconfig {'cacheLocation':
  value  => 'c:\ProgramData\choco-cache',
}

# ensure licensed edition is installed
package { 'chocolatey.extension':
  ensure   => latest,
  source   => 'internal_chocolatey',
  require  => File['C:/ProgramData/chocolatey/license/chocolatey.license.xml'],
}
~~~

## Install the Chocolatey Agent Service

For C4B (Chocolatey for Business) customers, there is an optional agent service you can install. The agent service allows for things like background mode, setting up maintenance windows (future), and hooking up to Chocolatey Central Management (future).

To be able to install it, you must first have the licensed edition properly installed. Once you have [verified your install](#how-do-i-know-when-the-license-is-installed), you can run the following:

* `choco install chocolatey-agent <options>`

For more on the agent service, please see [[Agent Service|FeaturesAgentService]].

## Common Errors and Resolutions

When you are attempting to install the licensed edition, you may run into some errors depending on your configuration. Here are some common ones we've seen.

### Exception of type 'Rhino.Licensing.LicenseNotFoundException' was thrown.

If you run into this error, it means you are on an older version of Chocolatey and you need to upgrade.

1. Remove the license file.
1. Upgrade the `chocolatey` package - `choco upgrade chocolatey <options>`
1. Set the license file back and proceed with the install or upgrade process as above.

### ERROR: The term 'Install-ChocolateyWindowsService' is not recognized as the name of a cmdlet, function, script file, or operable program.

You can get this if you are attempting to install the `chocolatey-agent` package, but you don't have a verified installation of a licensed edition of Chocolatey. You must resolve that prior to attempting to install the `chocolatey-agent` package. See [How Do I Know When the License is Installed?](#how-do-i-know-when-the-license-is-installed).
