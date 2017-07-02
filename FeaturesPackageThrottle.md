# Package Throttle (Licensed Editions Only)

WORK IN PROGRESS, CHECK BACK FOR UPDATES

> Use Chocolatey in low bandwidth areas

By default, Chocolatey downloads packages and any resources the packages use as fast as it can. In most cases, this is exactly the behavior you want. However, low bandwidth areas would be overwhelmed with this behavior and so you need to slow Chocolatey down. Package Throttle covers these low bandwidth scenarios automatically.

<!-- TOC -->

- [Usage](#usage)
  - [Requirements](#requirements)
  - [Setup](#setup)
- [See It In Action](#see-it-in-action)
- [Options and Switches](#options-and-switches)
- [FAQ](#faq)
  - [How do I take advantage of this feature?](#how-do-i-take-advantage-of-this-feature)
  - [I'm a licensed customer, now what?](#im-a-licensed-customer-now-what)
  - [How does it work?](#how-does-it-work)
  - [Package Throttle didn't slow down the downloads of resources in my package](#package-throttle-didnt-slow-down-the-downloads-of-resources-in-my-package)

<!-- /TOC -->

## Usage

### Requirements

* Chocolatey (`chocolatey` package) v0.10.7+.
* Chocolatey for Business (C4B) Edition.
* Chocolatey Licensed Extension (`chocolatey.extension` package) v1.10.0+.

### Setup

Coming Soong!

## See It In Action

Coming soon!

## Options and Switches

With `choco install`/`choco upgrade`:

~~~sh
     --bps, --maxdownloadrate, --max-download-rate, --maxdownloadbitspersecond, --max-download-bits-per-second, --maximumdownloadbitspersecond, --maximum-download-bits-per-second=VALUE
     Maximum Download Rate Bits Per Second - The maximum download rate in
       bits per second. '0' or empty means no maximum. A number means that will
       be the maximum download rate in bps. Defaults to config setting of '0'.
       Available in [licensed editions](https://chocolatey.org/compare) v1.10+ only. See https://chocolate-
       y.org/docs/features-download-throttle
~~~

## FAQ

### How do I take advantage of this feature?
You must have a [licensed edition of Chocolatey](https://chocolatey.org/pricing) (Pro, MSP, or Business) and use the community package repository to install/upgrade packages. Pro is a personal, named license that costs about the price of a lunch outing per month and comes with several other features. Business editions are great for organizations that need to manage the total software management lifecycle. MSP editions are for managed service providers and contain the same features as Pro (minus VirusTotal integration).

### I'm a licensed customer, now what?
It works to slow the download rate when you provide the maximum download rate as an argument or as a set feature.

### How does it work?

### Package Throttle didn't slow down the downloads of resources in my package
Package Throttle only works for resources downloaded with built-in Chocolatey PowerShell functions. It will not be able to slow down downloads using WebClient, and it's an anti-pattern in packaging to use WebClient anyways.
