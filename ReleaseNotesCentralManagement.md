# Chocolatey Release Notes - Chocolatey Central Management
## Summary
This covers the release notes for the Chocolatey Central Management (`chocolatey-management-database`, `chocolatey-management-service`, and `chocolatey-management-web`) packages, which covers Central Management server-side functionality. For more information, installation options, etc, please refer to [[Chocolatey Central Management|FeaturesChocolateyCentralManagement]].

**NOTE**: This package is available to Chocolatey for Business (C4B) customers only.

## Other Release Notes
* Refer to [[Open Source Release Notes|ReleaseNotes]] as commercial editions build on top of open source.
* Chocolatey for Business (C4B) customers - also refer to [[Chocolatey Licensed Extension Release Notes|ReleaseNotesExtension]] and [[Chocolatey Agent Release Notes|ReleaseNotesAgent]].

## Known Issues
* Please see https://github.com/chocolatey/chocolatey-licensed-issues/labels/CentralManagement
* Some issues may be held internally, please follow your support routes to learn more.

## 0.1.0 (May 22, 2019)

Initial preview release

### FEATURES
* Reports - Ability to view and generate report (Excel/PDF) for all currently outdated software
* Dashboard - Provide a dashboard screen with key KPI values
* Overview - Show number of machines checking into CCM and compare to number currently licensed

### BUG FIXES
* Fix - Packaging - Before upgrading Web Package ensure that dotnet process isn't running
* Fix - Web Site - Ensure that minified versions of all assets are used
* Fix - Web Site - Ensure consistent Date/Time Formatting used everywhere
* Fix - Web Site - Corrected duplicate display of search input box on some screens
* Fix - Web Site - Error when attempting to sort by any column in table on Computer Details screen
* Fix - Web Site - Erorr when attempting to sort by Name or Package Title column in table on Software screen
* Fix - Web Site - Tab does not sort by oudated first on Software screen
* Fix - Web Site - Timezone modification doesn't provide useful information to user
* Fix - Web Site - Only show Software that is installed on at least one machine
* Fix - Web Site - Excel Export generates errors when DateTime values are included
* Fix - Versioning - Ensure correct version number is stamped on all generated assemblies
* Fix - Service - Correct usage of default port number, which should be 24020
* Fix - Service - New-SelfSignedCertificate usage doesn't work on earlier PowerShell versions
* Fix - Service - Ensure correct error handling for incorrect/missing SQL Server credentials
* Fix - Database - Ensure SQL Server 2008 support
* Fix - Database - Migrator doesn't exit with non-zero exit code when there is an error
* Fix - Installation - Ensure usage of FQDN for all components

### IMPROVEMENTS
* Logging - Provided better logging during Service Certificate installation
* Installation - Verify and usage persisted appsettings.json file during upgrade
* Installation - Reduce issues unpacking web package by shortening paths in packaging
* Uninstallation - Remove modifications that were done as part of installation
* Database - Don't attempt to seed database tables everytime application starts
* Packaging - Removed unneccessary files from package, making it much smaller
* Packaging - Added requied dependencies to packages
* Packaging - Add information about available installation parameters to package description
* Service - Allow modification of configuration settings without the need to restart Windows Service


## 0.1.0-beta-20181009 (October 9, 2018)
### FEATURES
* [Security] Installation - Provide encryption for all persisted configuration data
* [Security] Installation - Sign all PowerShell Scripts and assemblies shipped as part of release
* [Security] Web Site - Provide full RBAC to site and API
* Audit - Provide ability to list all computers that are currently in use across environment
* Audit - Provide ability to list all software that is currently installed across environment
* Reports - Ability to view and generate report (Excel/PDF) for all currently installed software
* Reports - Ability to view and generate report (Excel/PDF) for all computers currently in use
