# Package Download Cache
Also known from the [Kickstarter](https://www.kickstarter.com/projects/ferventcoder/chocolatey-the-alternative-windows-store-like-yum/description) as the "Alternate **permanent** download location", we lovingly call this the **"No more [404](https://en.wikipedia.org/wiki/HTTP_404)s feature"**. This feature will be available ***ONLY*** for our licensed customers. Reasons answered below.

**No more 404s?**. One of the longstanding things we have always wanted in the community feed was to remove the element of package breakages due to change/movement of download locations at their original distribution point. We are now able to offer a private distribution to our licensed customers! Here are the relevant parts of the [Kickstarter](https://www.kickstarter.com/projects/ferventcoder/chocolatey-the-alternative-windows-store-like-yum/description):

> ![pro features](https://cloud.githubusercontent.com/assets/63502/12588924/a2c8d49e-c420-11e5-93a0-f271b8b7c3e3.png)

> Highlighting a few things:

> * **Pro/business only feed(s)** supporting packages that are only available to professional and above customers. 
> * Alternate **permanent** download location - A private location that contains the installers for most packages, removing the instability of downloads that are removed from their distribution point (or updated to newer versions).
> * **Virus checking during runtime.**

> The biggest features here are more **safety, trust and predictability**.

**NOTE:** Please sign up for the [Chocolatey Newsletter](http://chocolatey.us8.list-manage1.com/subscribe?u=86a6d80146a0da7f2223712e4&id=73b018498d) to be one of the first to hear about when the pro/business versions are available. http://chocolatey.us8.list-manage1.com/subscribe?u=86a6d80146a0da7f2223712e4&id=73b018498d

## FAQs
### Are you distributing these files publicly?
No, that could potentially have legal ramifications. See the next question.

### Why can't you offer this feature publicly?
Software redistribution legal reasons and costs - the short of it is that we can't have a public archive of software out there. So we require a user to be a licensed customer to be able to take advantage of this capability. 

The long of it - in the United States (maybe elsewhere as well) there are software distribution laws that determine whether an external source can redistribute software. This is typically determined in the software license if it has a clause for redistribution (many FOSS licenses do). Sometimes you can find a redistribution clause in a EULA. When these don't exist, the external source must get express written permission from the software vendor to publicly redistribute their software. The community feed offers all kinds of software with all kinds of different licenses and redistribution technicalities. To try to determine with each package if the underlying software is publicly cacheable is a very difficult process and could potentially set us up for legal ramifications if the package provides the wrong license accidentally.

As far as costs go - while folks use Chocolatey.org and its services completely free, that does not mean that there is no cost. There are infrastructure costs in general with running a high traffic site and infrastructure costs we need to support specifically in providing this feature. A **two cups of coffee per month** license helps us offset these costs and give us the ability to keep the community feed going for years to come in a self-sustaining way. So it makes sense to provide this feature as a premium add because we can't offer it for free.

### Are you modifying these files?
No, these are the original files that are/were at the original distribution location.

### Why must I be licensed? 
Really it boils down to two reasons:

* There are infrastructure support and bandwidth costs that your **two cups of coffee per month** license will help offset.
* You must be a private customer using a private service - we are not allowed to offer this publicly due to reasons stated previously.

### Does this grant me a license to the software?
No, you still need to work with the software vendor to obtain a license, if required. 

### I'm a software vendor, should I be concerned about you storing my software files?
In short, you need not be concerned. We are offering private files to private customers as a service. We are not modifying, selling, renting, leasing, lending, or sub-licensing these files. We are also not offering these files to the public in any way, shape, or form. Our customers get your original, unmodified software as it is/was provided at the original distribution point, so if there is any licensing required for the software, the customers would still need to obtain that from the vendor (you).

In most typical companies, a software store already exists on an internal file share. When there is software publicly available on the internet, the organization wants to ensure that it is always available for its constituents, so they download it to an internal file share. This ensures that if anything were to happen to the software on the internet, the constituents would still be able to successfully install the software that has already been found to be suitable for the organization.

As an organization that cares about its constituents, we are providing a private file share for our private customers who want more stability over the inherent instability of the internet. Since we don't modify the original (publicly available) files in any way or offer the downloaded files publicly, under United States law it typically means there is no violation of distribution rights. We are governed under United States law concerning software distribution and make all attempts to comply with the law. 

**Still concerned or have questions?** If you have read and understood all of this and have questions, concerns and/or would still like to opt out your software, please contact us <a href="mailto:chocolateywebadmin at googlegroups dot com?subject=[Insert Your Software Name Here] - Chocolatey Community Feed Caching&body=Please fill in details of your request (remember to include the package page url, and the software name). Please remember to change the to address to a valid email address.">here</a> (only vendor requests please).