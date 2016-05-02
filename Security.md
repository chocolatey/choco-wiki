# Security
## How we address security

### Overall

Chocolatey has grown up quite a bit with the release of 0.9.9+ series and has been moving to a more secure by default approach. What that means is that Chocolatey will set the more secure defaults and the user has to do something (e.g. set a switch, choose to install Chocolatey to a less secure location) to reduce the overall security of Chocolatey.

1. Requires elevated permissions to install to the default location (`C:\ProgramData\chocolatey`). This reduces escalation of privilege attacks.
1. Requires elevated permissions to run `choco.exe` in the default installed location. This reduces escalation of privilege attacks.
1. Requires administrative permission to add to the Machine PATH environment variable. This reduces escalation of privilege attacks.
1. choco by default will stop and ask you to confirm before changing state of the system, showing you the script it wants to execute.
1. choco.exe supports a `-whatif` scenario (aka `--noop`) to 0.9.9 so you can get a feel for what a package would do to your system.
1. To reduce MITM (Man in the middle) attacks, package installs support [[checksums|HelpersInstallChocolateyPackage]], so that when downloading from a remote location, you can verify you are getting what you believe you should be getting before executing it on the system. We're making steps to allow for [users to supply the checksum as well](https://github.com/chocolatey/choco/issues/112).
1. Choco will not allow you to push to the community feed without using SSL/TLS (HTTPS). This reduces DNS poisoning issues.
1. When you host internal packages, those packages can embed software and/or point to internal shares. You are not subject to software distribution rights like the packages on the community feed, so you can create packages that are more reliable and secure. See [[What are Chocolatey Packages|GettingStarted#what-are-chocolatey-packages]] for more details.

### Chocolatey binaries and the Chocolatey package
 
The binary `choco.exe` can be trusted (at least as far as you trust the Chocolatey maintainers and RealDimensions Software, LLC). 

*  Starting with 0.9.10, both the binaries and the PowerShell scripts are Authenticode signed. This certificate is only held by the lead Chocolatey maintainer (Rob). This provides quite a bit of trust that you are getting Chocolatey from the source and as intended.

Using PowerShell, you can verify the binary (the path below is the default install location, adjust if necessary).
~~~sh
C:\ PS> (Get-AuthenticodeSignature -FilePath C:\ProgramData\chocolatey\choco.exe).SignerCertificate | Format-List


Subject      : CN="RealDimensions Software, LLC", O="RealDimensions Software,
               LLC", L=Topeka, S=Kansas, C=US
Issuer       : CN=DigiCert SHA2 Assured ID Code Signing CA, OU=www.digicert.com,
               O=DigiCert Inc, C=US
Thumbprint   : C9F7FD1A91F078DB6BFCFCCE28B9749F8F2A0C38
FriendlyName :
NotBefore    : 3/23/2016 7:00:00 PM
NotAfter     : 3/28/2017 7:00:00 AM
Extensions   : {System.Security.Cryptography.Oid,
               System.Security.Cryptography.Oid,
               System.Security.Cryptography.Oid,
               System.Security.Cryptography.Oid...}
~~~

* Although not the best security method, one can also verify choco based on the strong name. choco.exe is strong named with a key that is known only to the lead maintainer of Chocolatey (Rob). Verify the strong name of the official choco binary with the `sn.exe` utility - the public key should be `79d02ea9cad655eb`.

Using a Visual Studio Command Prompt, you can verify the binary (the path below is the default install location, adjust if necessary). You can also download sn separately if necessary.
~~~sh
C:\Program Files (x86)\Microsoft Visual Studio 10.0\VC>sn -T c:\ProgramData\chocolatey\choco.exe

Microsoft (R) .NET Framework Strong Name Utility  Version 4.0.30319.1
Copyright (c) Microsoft Corporation.  All rights reserved.

Public key token is 79d02ea9cad655eb
~~~

* Choco will warn if it is not signed with the right key (the FOSS project has a default key so that it can build appropriately) and require a user to pass `--allow-unofficial-build`. Over time we are going to increase this so that more places will restrict this (those a user can't just go change source of choco on and build).
* The code for Chocolatey is [open source](https://github.com/chocolatey/choco), so you can inspect to visually be sure it is not doing anything malicious to your machine - https://github.com/chocolatey/choco.

For more information on the specifics, see [#36](https://github.com/chocolatey/choco/issues/36) and [#501](https://github.com/chocolatey/choco/issues/501).

### Chocolatey.org (the community feed)

**NOTE: For Organizational Use of Chocolatey** - First of all, it goes without stating that if you are a business and you are using Chocolatey, you should think long and hard before trusting an external source you have no control over (chocolatey.org packages, in addition to all of the binaries that download from official distribution channels over the internet). It is too easy to set up your [[own private feed|How-To-Host-Feed]] where you can vet packages and have complete control over the binaries and what gets installed. This is what we recommend for businesses that use Chocolatey in production scenarios (and what many of them do). There is a [great article written up](https://puppetlabs.com/blog/chocolatey-hosting-your-own-server) on the reasoning and options for hosting your own server.

1. Users can report malicious packages/software directly to the site administrators using a form found on every package page.
1. Everything is enforced as HTTPS where it should be. This reduces DNS poisoning attacks.
1. As of October 2014, packages submitted to the community feed (https://chocolatey.org) are moderated and approved before they become live. 
1. Some packages move into a trusted status. This is usually when the package maintainer is also the software maintainer, but can also occur when the maintainer(s) are trusted and the package is submitted without issues.
1. Packages that download binaries (installers, zip archives) are checked to ensure that the binary is coming from the official distribution source. 
1. If the package has a checksum, it provides a further integrity check that the downloadable the maintainer/moderator checked is the same binary that the user gets.
1. Packages are run through Virus Total to produce a second opinion on the relative safety of the package and underlying software that is contained or downloaded by the package.

### Future Chocolatey enhancements

1. Virus checking is being added to the pro/business offering - we highly recommend a security conscious company look to the business routes for more security (and locking down of components, like locking down folders even more and other nice tweaks that a business would want to make).
1. Moderators will cryptographically sign packages with a GPG key that they own. This will allow folks to trust moderators. 
1. Users will also cryptographically sign packages so we can provide authenticity that the package came from them.
1. We are looking at more secure checksums and showing those checksums on packages that have executables contained in them. https://github.com/chocolatey/choco/issues/113
1. We'll show the package checksum on the website for folks that want to verify the package is brought down appropriately.
1. We've implemented checksumming in the package, but we'll verify that against what the website reports for the checksum for additional integrity checks. We do this now to a naive degree.
1. A user can optionally pass their own checksums that must be validated for downloaded software - https://github.com/chocolatey/choco/issues/112

## History

Some folks may state that Chocolatey *is* insecure. That is based on older information and is incorrect to be stated in that way. Feel free to correct the person with "You mean Chocolatey ***used*** to be insecure." and then point them to this page. It is correct that there ***were*** security concerns. However, all known concerns have been corrected and/or have a plan to be resolved (e.g. package signing). As we learn of new security concerns we put together a plan to resolve those issues with a priority that each CVE (common vulnerabilities and exposures) requires.

An acquaintance of mine was asked to do a security audit for Chocolatey (he does penetration testing for a living, I'd tell you more but I'm not sure I have permission to name him) for a company and he found several things that have all been corrected. He went as far as filing CVEs but CERT decided not to release them publicly at the time (this was in March 2014).

### Security concerns and how they have been addressed

1. Installs without prompting for confirmation  - not true as of 0.9.9. Chocolatey by default will stop and ask you to confirm before changing state of the system, showing you the script it wants to execute.
1. Anybody can put packages up on the community feed and they could be malicious - we put package moderation in place in October 2014. All packages coming in are now moderated BEFORE they are open to the public. See http://codebetter.com/robreynolds/2014/10/27/chocolatey-now-has-package-moderation/ for more details.
1. Downloads packages from S3 over HTTP (subject to DNS poisoning) - this was corrected in March 2014 (https://github.com/chocolatey/chocolatey.org/issues/70)
1. Site doesn't require HTTPS (could be subject to DNS poisoning) - https://github.com/chocolatey/chocolatey.org/issues/126 (closed completely in November 2014)
1. Downloads from internet files with no integrity check - we've added checksumming, but we haven't yet enforced it for package maintainers. At some point we will flip a switch and users won't be able to install a package without a checksum by default. They will need to specify a switch
1. Poor permissions with `c:\Chocolatey` at root (allows attacker to gain Admin perms through specially crafted exes dropped in bin folder, among other things) - we don't install here by default anymore. We install to `C:\ProgramData\chocolatey` by default for more secure permissions. 

### What about a non-administrative installation of Chocolatey? Is it secure?

In a word, it depends on where you install Chocolatey.

Keep in mind by default that Chocolatey requires elevated rights. 

1. The default install location (`C:\ProgramData\chocolatey`) requires elevated rights to install to. 
2. It (`C:\ProgramData\chocolatey`) also requires elevated rights to install packages. To ease this a bit, we add the installing user's ACE with modify access (the user still needs to be elevated/admin at the time of installing/upgrading Chocolatey).
3. Adding system-wide environment variables (e.g. Chocolatey's bin directory to System PATH) requires administrative rights to set.

Now with that in mind, let's talk about a non-administrative install of Chocolatey.

1. A non-admin user installs Chocolatey. They have to select a different install location that they can write to.
2. When they install Chocolatey, it only adds USER environment variables. That means they only appear systemwide for that user alone.

Chocolatey supports both of these types of installs. Note the first is secure by default, but the second is also an option and can be secure depending on where the user decides to install Chocolatey.

A non-administrative user should choose to install Chocolatey in a directory somewhere under `C:\Users\<username>` to avoid the most security risk. Ensure that Everyone/Users do not have modify access to the folder by checking the ACL (security tab of Folder properties).

### Security Scenarios to Keep in Mind / Avoid

1. Administrative user choose to install Chocolatey to an insecure location (like the root of the system drive, e.g. `C:\Chocolatey`). Now anyone that has access to that computer has an attack vector. This is very bad, **DO NOT DO THIS.** It still requires an administrative execution context to exploit, but it has a high possibility and high impact.
1. Non-admin user choose to install Chocolatey to an insecure location (like the root of the system drive, e.g. `C:\Chocolatey`). Now anyone that has access to that computer has an attack vector for that user alone. This has a medium possibility and low impact.
1. Installing user is admin during install, but then the admin privileges are removed. That user can still install portable packages that will end up on PATH. This can lead to escalation of privilege attacks. This is an unlikely scenario but one to consider if you reduce privileges for users in your organization. This has a low possibility but a high impact.
