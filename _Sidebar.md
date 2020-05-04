 - [[Home|Home]]
 - [[Why Chocolatey?|Why]]
 - [Choco 0.9.8.x & below](https://github.com/chocolatey/chocolatey/wiki)

#### General
 * [[Troubleshooting|Troubleshooting]]
 * [[FAQs|ChocolateyFAQs]]
 * [[Security|Security]]
 * [[Roadmap|RoadMap]]
 * [Submitting Issues](https://github.com/chocolatey/choco/blob/master/README.md#submitting-issues)
 * [Contributing](https://github.com/chocolatey/choco/blob/master/CONTRIBUTING.md)
 * [[Release Notes|ReleaseNotes]]
 * [[Licensed Release Notes|ReleaseNotesLicensed]]
 * [[Set Up Fully Internal Chocolatey Deployment (For Organizations)|How-To-Setup-Offline-Installation]]

 - [[Chocolatey.org Moderation|Moderation]]
 - [[Chocolatey.org Packages Disclaimer|CommunityPackagesDisclaimer]]
 - [[Software Vendor?|PackageTriageProcess#are-you-a-software-vendor]]

#### Features

 - <u>**Free**</u>
 - [[Create your own packages|CreatePackages]]
 - [[Host packages internally|How-To-Host-Feed]]
 - [[Integrates with everything|FeaturesInfrastructureAutomation]]
 - [[Package Shims - Automatically Add exes to PATH w/out Clutter|FeaturesShim]]
 - [[Create Custom Package Templates|How-To-Create-Custom-Package-Templates]]
 - [[Package Extensions - Extend Chocolatey With PowerShell Modules (extensions)|How-To-Create-Extensions]]
 - Automatic Uninstaller
 - <u>**Paid**</u>
 - [[Runtime Malware Protection (Pro+)|FeaturesVirusCheck]]
 - [[Download CDN Cache (Pro+)|FeaturesPrivateCdn]]
 - [[Package Reducer (Pro+) - reduce package installation space usage|FeaturesPackageReducer]]
 - [[Ubiquitous Install Directory Option (Pro+)|FeaturesInstallDirectoryOverride]]
 - [[Package Throttle (Pro+)|FeaturesPackageThrottle]]
 - [[Package Synchronizer (Pro+ Auto / C4B Sync Command) - Sync with Programs and Features|FeaturesSynchronize]]
 - [[Self-Service / Background Mode (C4B) - Allow non-administrators to install/upgrade software|FeaturesAgentService]]
 - [[Package Builder (C4B) - Create packages automatically from installers|FeaturesCreatePackagesFromInstallers]]
 - [[Package Internalizer (C4B) - Convert existing packages for complete offline / reliable use|FeaturesAutomaticallyRecompilePackages]]
 - Direct Installer (C4B) - install/upgrade directly from msi / exe installers
 - [[Package Audit (C4B) - know who installed what and when|FeaturesPackageAudit]]
 - Windows Service Management PowerShell Functions (C4B)
 - [[Branding Chocolatey Applications (C4B)|FeaturesBranding]]
 - In progress, check back for updates!

#### Quick Deployment Environment
* [[Quick Deployment Environment|QuickDeploymentEnvironment]]
* [[QDE Setup|QuickDeploymentSetup]]
* [[QDE Desktop ReadMe File|QuickDeploymentDesktopReadme]] (online version of the desktop readme, online is most up to date)
* [[QDE SSL/TLS Setup|QuickDeploymentSslSetup]]
* [[QDE Firewall Changes|QuickDeploymentFirewallChanges]]
* [[QDE Client Setup|QuickDeploymentClientSetup]] (setting up your client machines)

#### Usage

[[How does choco work?|GettingStarted#how-does-chocolatey-work]]

 - [[How to install|installation]]
 - [[How to install licensed edition|installation-licensed]]
 - [[Configuration / chocolatey.config|ChocolateyConfiguration]]
 - [[How to uninstall|Uninstallation]]
 - [[Getting Started|GettingStarted]]
 - [[Proxy Settings|Proxy-Settings-for-Chocolatey]]
 - **Commands:**
  - [[Passing args to choco|CommandsReference#how-to-pass-options--switches]]
  - [[Complete Reference|CommandsReference]]
  - [[List / Search|CommandsList]]
  - [[Info|CommandsInfo]]
  - [[Install|CommandsInstall]]
  - [[Pin|CommandsPin]]
  - [[Outdated|CommandsOutdated]]
  - [[Upgrade|CommandsUpgrade]]
  - [[Uninstall|CommandsUninstall]]
  - [[Config|CommandsConfig]]
  - [[Source / Sources|CommandsSources]]
  - [[Feature|CommandsFeature]]
  - [[Download|CommandsDownload]]
  - Packaging
    - [[New|CommandsNew]]
    - [[Pack|CommandsPack]]
    - [[Apikey|CommandsApiKey]]
    - [[Push|CommandsPush]]

#### Creating Packages

 - [[Create Packages|CreatePackages]]
 - [[Quick Start|CreatePackagesQuickStart]]
 - **Commands:**
   - [[New|CommandsNew]]
   - [[Pack|CommandsPack]]
   - [[Apikey|CommandsApiKey]]
   - [[Push|CommandsPush]]
   - [[Download|CommandsDownload]]
 - [[Automatic Packaging|AutomaticPackages]]
 - **PowerShell Reference:**
   - [[Function and Variable Reference|HelpersReference]]
   - [[Install-ChocolateyPackage|HelpersInstallChocolateyPackage]]
   - [[Install-ChocolateyZipPackage|HelpersInstallChocolateyZipPackage]]
   - [[Install-ChocolateyVsixPackage|HelpersInstallChocolateyVsixPackage]]
   - [[Get-ChocolateyWebFile|HelpersGetChocolateyWebFile]]
   - [[Install-ChocolateyInstallPackage|HelpersInstallChocolateyInstallPackage]]
   - [[Get-ChocolateyUnzip|HelpersGetChocolateyUnzip]]
   - [[Install-ChocolateyPath|HelpersInstallChocolateyPath]]
   - [[Install-ChocolateyEnvironmentVariable|HelpersInstallChocolateyEnvironmentVariable]]
   - [[Install-ChocolateyShortcut|HelpersInstallChocolateyShortcut]]
   - [[Install-ChocolateyFileAssociation|HelpersInstallChocolateyFileAssociation]]

#### How To's

 - [[Use Chocolatey w/Proxy Server|Proxy-Settings-for-Chocolatey]]
 - [[Change Download Cache Location aka Don't use TEMP for downloads|How-To-Change-Cache]]
 - [[Install/Upgrade a Package w/out running install scripts|How-To-Install-Upgrade-Package-Without-Scripts]]
 - [[Request Package Fixes/Updates|PackageTriageProcess]]
 - [[Manually Recompile Packages, Embedding/Internalizing Remote Resources|How-To-Recompile-Packages]]
 - [[Request Package|PackageTriageProcess]]
 - [[Maintain Packages for My Software|PackageTriageProcess#i-want-to-take-overhelp-with-package-maintenance-for-my-software]]
 - [[Become a Maintainer|PackageTriageProcess]]
 - [[Take Over Package Maintenance Exclusively|PackageMaintainerHandover]]
 - [[Parse Package Parameters|How-To-Parse-PackageParameters-Argument]]
 - [[Mount Iso|How-To-Mount-An-Iso-In-Chocolatey-Package]]
 - [[Create Custom Package Templates|How-To-Create-Custom-Package-Templates]]
 - [[Extend Chocolatey With PowerShell Modules (extensions)|How-To-Create-Extensions]]
 - [[Deprecate a Package|How-To-Deprecate-A-Chocolatey-Package]]
 - [[Install licensed edition|Installation-Licensed]]
 - [[Host Your Own Package Repository Server|How-To-Host-Feed]]
 - [[Set up the Chocolatey.Server|How-To-Set-Up-Chocolatey-Server]]
 - [[Set up Chocolatey for internal/organizational use|How-To-Setup-Offline-Installation]]
 - [[Use Package Internalizer To Create Internal Package Source (Business Editions Only)|How-To-Setup-Internal-Package-Repository]]

#### Use Cases

 - [[Development Environment|DevelopmentEnvironmentSetup]]
 - [[Host on MyGet|Hosting-Chocolatey-Packages-on-MyGet]]

#### Learning Resources

 - [[Resources|Resources]]
 - [[Videos|Videos]]
 - [[Presentations|Presentations]]

#### Other

 - [[Why Chocolatey?|Why]]
 - [[Legal|Legal]]
 - [[History|History]]
