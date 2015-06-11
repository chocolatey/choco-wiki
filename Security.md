There "were" all kinds of security concerns. However that was awhile back and almost all if not all has been corrected. 

An acquaintance of mine was asked to do a security audit for Chocolatey (he does penetration testing for a living, I'd tell you more but I'm not sure I have permission to name him) for a company and he found several things that have all been corrected. He went as far as filing CVEs but CERT decided not to release them publicly at the time (this was in March 2014).

### Security concerns and how they have been addressed

1. Installs without prompting for confirmation  - not true as of 0.9.9. Chocolatey by default will stop and ask you to confirm before changing state of the system, showing you the script it wants to execute.
1. Anybody can put packages up on the community feed and they could be malicious - we put package moderation in place in October 2014. All packages coming in are now moderated BEFORE they are open to the public. See http://codebetter.com/robreynolds/2014/10/27/chocolatey-now-has-package-moderation/ for more details.
1. Downloads packages from S3 over HTTP (subject to DNS poisoning) - this was corrected in March 2014 (https://github.com/chocolatey/chocolatey.org/issues/70)
1. Site doesn't require HTTPS (could be subject to DNS poisoning) - https://github.com/chocolatey/chocolatey.org/issues/126 (closed completely in November 2014)
1. Downloads from internet files with no integrity check - we've added checksumming, but we haven't yet enforced it for package maintainers. At some point we will flip a switch and users won't be able to install a package without a checksum by default. They will need to specify a switch
1. Poor permissions with c:\Chocolatey at root (allows attacker to gain Admin perms through specially crafted exes dropped in bin folder, among other things) - we don't install here by default anymore. We install to C:\programdata\chocolatey by default for more secure permissions. 

For any that deal with the community feed or downloading packages over the internet - a company should never be doing that in the first place and should have their own internal feed. We'd never recommend a company do that, we mention that several times on the about page - https://chocolatey.org/about


### More security-related items
1. The community feed (https://chocolatey.org) is now moderated before packages become live.
1. Choco v0.9.9 is signed with a key that only Rob has. Choco will warn if it is not signed with that key and require a user to pass --allow-unofficial-build. Over time we are going to increase this so that more places will restrict this (those a user can't just go change source of choco on and build). https://github.com/chocolatey/choco/issues/36
1. We have added -whatif (aka --noop) to 0.9.9 so you can get a feel for what a package would do to your system.

### Future security:
1. Virus checking is being added to the pro/business offering - a plug but I highly recommend a security conscious company look to the business routes for more security (and locking down of components, like locking down folders even more and other nice tweaks that a business would want to make).
1. Moderators will cryptographically sign packages with a GPG key that they own. This will allow folks to trust moderators. We are strongly considering going this route with users as well so they can provide authenticity that the package came from them.
1. We are looking at more secure checksums and showing those checksums on packages that have executables contained in them. https://github.com/chocolatey/choco/issues/113
1. We'll show the package checksum on the website for folks that want to verify the package is brought down appropriately.
1. We've implemented a checksum in the package, but we'll verify that against what the website reports for the checksum for additional integrity checks.
1. A user can optionally pass their own checksums that must be validated for downloaded software - https://github.com/chocolatey/choco/issues/112
