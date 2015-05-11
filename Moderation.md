# Moderation of the Community Feed

## Definitions

- package - the nuget package
- sofware - the underlying software that the package assists in installing
- installer - the native installer, usually packaged as MSI, NSIS, InstallShield, Wise, Squirrel, or some other flavor.

### Roles
- Reviewer - Able to review packages but not approve/reject them
- Moderator - Able to set/remove package maintainers, review packages, approve/reject them, able to unlist packages.
- Administrator - Has access to administrative sections of the site. Can perform all functions that a moderator can perform.

#### Becoming a Reviewer
TBD

#### Becoming a Moderator
There is no set process for becoming a moderator yet. Usually it is having many approved packages and understanding the process of creating choco packages. Eventually it will be something you earn through your reputation on the site.
- Make awesome packages
- Work on the disqus threads and mailing list.
- Have a desire to better the quality of Chocolatey
- Know a little PowerShell. More is better but yeah
- Be friendly and customer service-oriented

#### Becoming an Admin
This is not an achievable status.

## Package Review Process

When reviewing new and existing packages, you should go based on the following requirements and guidelines.

### Existing Packages
#### Requirements

Anything that falls under requirements will hold up package approval until they are corrected. Always be explicit that you are waiting on the maintainer to fix and resubmit the same version of the package so you can move the review process along.

- ProjectUrl - It's almost absolutely required for the community feed
- Is the title appropriate?
- At least something written in the description. It should be sufficient to explain the software.
- The authors field (software author) is not being used for the maintainers field (exception: when the maintainer is also the author)
- Tags are appropriate and do not include "chocolatey" (with the exception of the chocolatey packages) - note this doesn't mean they have what you believe are missing tags they could have, that's a guideline.
- Look over the package files
  - If binaries are included in the package, does the maintainer have distribution rights? If they have explicit permission, a copy of that in PDF should be in the package contents.
  - Install/Uninstall scripts
    - Do the scripts try to do anything malicious? This is almost always immediate grounds for banning the maintainer and deleting their packages.
    - Do the scripts set good defaults for silent args?
    - Is there anything there that would not work with POSH v2?
    - If it is a download, is it getting it from the proper location? Use the project site (projectUrl) to determine where the download for the file is coming from and it should match the one in the package files. If not there needs to be a really, really good reason for not doing so.
    - Does the download version match the package version?
    - Are the original commented lines in there? Those must be cleaned up.
    - Flag the use of any of the following: `$nugetChocolateyPath`, `$nugetPath`, `$nugetExePath`, `$nugetLibPath`, `$chocInstallVariableName`, `$nugetExe`
    - Does the script try to use `choco install/upgrade/uninstall`?
    - Does the package try to do anything that an existing Chocolatey function already covers? The maintainers would need a really good reason for diverging from that.
- Does the package install correctly?
- Does the package uninstall correctly?

#### Guidelines
For guidelines, a package can be approved with comments. Be sure when you have a combination of guidelines versus requirements that you let the maintainer(s) know what exactly is required for approval and what falls into the "nice to have category".

- LicenseUrl is nearly a requirement. The only reason it sits in guidelines is that not all software has a page out there containing it's license information. We request that in those cases they point to the url for the FOSS license of the software.
- We really want to see the IconUrl being used, and some moderators want to see it being used properly, using the rawgit CDN. However it is a guideline, and something to note for a maintainer to fix up next time, not currently. Some software doesn't have
- Suggest description get really filled out and they take full advantage of the use of markdown.
- Summary is important, but it doesn't show up on the package page.
- Tags could always use suggestions to add.
- Look over the package files
  - Install / Uninstall Scripts
    - Be familiar with things that have been deprecated and add a gentle reminder about those things for them to clean up.
- Something in the releaseNotes section would be great.

### New Packages
New packages are packages that have all versions in unapproved status. There are packages out there that were pre-existing before moderation was turned on. Those will have some versions (not prereleases) that are exempted. Prereleases are always exempted.

#### Requirements
New packages must meet the existing package requirements and the following additional requirements.

- Package naming - if the naming doesn't follow our conventions, it is grounds for rejecting immediately with the suggestion they resubmit with *suggested name*. Note that they may have had prereleases already, and it's still okay to move forward with the rejected status as long as the name of the name of package hasn't been previously approved.
  - suggest the name split if over 25 chars with no -
  - flag on "."" in name (unless .portable/.install)
- not a package duplicating another existing package

#### Guidelines
TDB

### Process
Typically a package goes into the moderation queue when submitted.You can get to that by signing in and going to the packages page like you normally would.

 1. You should see a new drop down near the top that allows you to change your view. This is the moderation queue. ![Moderation Queue Dropdown](https://cloud.githubusercontent.com/assets/63502/7542991/b5032a38-f586-11e4-991a-7c7602d508aa.png)

 2. You will see items arranged in order based on reviewed and resubmitted at the top, items ready for review in order based on when they were submitted, and at the end of the queue, you will see items that are waiting for maintainer response. ![Moderation Queue](https://cloud.githubusercontent.com/assets/63502/7543076/58d5530c-f587-11e4-8d73-1325074d6e58.png)
 3. You grab a package and head in and review it based on the following items in the requirements and guidelines.
 ![Review Box](https://cloud.githubusercontent.com/assets/63502/7543237/8a9845ec-f588-11e4-9931-eb67d31f4958.png)
 4. Review the previous comments if there are any. ![image](https://cloud.githubusercontent.com/assets/63502/7543258/c2c0abbc-f588-11e4-8cac-4c57b03671f8.png)
 5. Look through the package files ![image](https://cloud.githubusercontent.com/assets/63502/7543284/ddaa41e0-f588-11e4-817a-9d7bd1130a84.png)
 6. Leave comments in the review box if you have any. Note that you can use markdown here.
 7. If you are approving the package, change Package Status to Approved, and Rejected otherwise.
 8. Click Save. You should get a message that the message was sent successfully.
 9. The maintainers receive an email noting the comments. They may follow up by email, which is chocolatey-moderators at google groups dot com.
 10. If the package is updated, it will show up in the top of the queue. At that time, please review it and make sure the maintainers made all changes requested.
 11. In the package review box, leave the original review, but add this heading over it `#### ORIGINAL YYYYMMDD:` e.g. `#### ORIGINAL 20150501:`. The date of the original review is to the left under Published Date. Then add additional comments above it with a heading of `#### UPDATE YYYYMMDD:`.


### New Reviewers / Moderators
- Become familiar with the package guidelines and all of the different Chocolatey functions available.
- Join the chocolatey-moderators at google groups dot com mailing list. This is required for communication with maintainers.
- Join Gitter and hang out in the [Choco Gitter Room](https://gitter.im/chocolatey/choco) from time to time.