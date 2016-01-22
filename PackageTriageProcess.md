# Package Questions/Updates/Fixes For Community Feed - Triage Process

The Community Feed (https://chocolatey.org/packages) has a collection of packages provided and maintained by the community. You may have come across a package and found you have questions about it or you have found it outdated or broken.

**NOTE**: If you are a **software vendor** wanting to maintain your own software's packages, please use the contact site admins link from the package page on the website.

## Questions about a package

Use the Contact Maintainers link on the package page of the community feed or the Disqus comment thread (at the bottom of the package page) to reach out to the maintainers. Neither require an account on Chocolatey.org. 

![contact maintainers link](https://cloud.githubusercontent.com/assets/63502/12519569/406aa70a-c105-11e5-9db2-6191ce5bc100.png)

## Package is Broken

![red ball failing testing verification](https://cloud.githubusercontent.com/assets/63502/12519534/1728a086-c105-11e5-9af7-96b188114ae7.png)

If the ball is red, the maintainer has already been informed. You may have helpful information for fixes, so please look for package source url information and file an issue if you find it (or a pull request). Otherwise use the contact maintainers link or the disqus thread from the questions process.

## Package Outdated
If you have a question about a package itself, use the contact form on the package page on chocolatey.org to contact the maintainers. From there you usually get a response and you can ask where they keep the packages repository so you can send a pull request.

If you don't hear anything within a few days back from them, it's possible they just haven't gotten to your message yet, but it's also possible that something went wrong and they didn't get the email. Use the contact site admins link. That goes to the site curators and they will try to triage the situation.

If you don't hear anything back from the curators within 2 days (usually same day response), then you send a message to the Chocolatey google group (http://groups.google.com/group/chocolatey) and we work to contact the maintainers.

If we don't hear anything back from the package maintainer within a reasonable timeframe, a TOS is started and new folks can be added to the package in question as maintainers. We don't have non-maintainer uploads in the choco/nuget infrastructure and we haven't needed them yet.

Sometimes this results in new maintainers. The next steps for these maintainers is known as the [Package Maintainer Handover/Switch](PackageMantainerHandover).