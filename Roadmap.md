# Roadmap
Heading down a sweet road.


**NOTE:** This is not always going to be updated, one can take a look at the issues list here and in the chocolatey.org repositories.

## Chocolatey

* Validation on packaging (subset of the package-validator)
* Official support for OneGet/PackageManagement - Summer 2016
* GPG package signing - likely sometime in 2017
* Validating authenticodes - see [#643](https://github.com/chocolatey/choco/issues/643)
* ~~Not allowing package installs that don't have checksums by default when using non-secure resources like HTTP/FTP - see [#112](https://github.com/chocolatey/choco/issues/112)~~ released in 0.10.0
* Nuspec enhancements - supported versions of Windows, etc
* Packaging enhancements - like package version, architecture
* [Virtual packages](https://github.com/chocolatey/chocolatey/issues/7)
* WSA Support 
* Windows Nano Support - likely late 2016/early 2017 (switching to .NET Standard/Core)

## Licensed Chocolatey

* Self-Service Installs (Non-Admins)
* Package Modernizer
* Package Sync command
  * finding existing packages
  * building packages on the fly with sync (business edition)
* Package Builder - create packages from Programs and Features
* Right Click `Create Chocolatey Package...` (business editions)
* Package Builder UI (business edition)
* Package Builder UI (pro edition without the auto detection)
* Possibly a business edition gallery with features specific to business needs.

## Chocolatey.org

* Adding back in search box
* Converting rest of site
* Kickstarter backers
* Add blog
* ~~VirusTotal scans of all packages and downloaded binaries~~ - Completed Feb 2016
* Locking down HTTP without checksums
* Locking down packages without checksums

## Thoughts for future directions (undecided directions)

* Pluggable compositional model


## Legal
**Disclaimer:** The information included in this roadmap does not constitute, and should not be construed as, a promise or commitment by RealDimensions Software, LLC (Chocolatey) to develop, market, or deliver any particular product, feature, or function. The timing and content of RealDimensions Software, LLC (Chocolatey) future product releases could differ materially from the expectations discussed in this document. RealDimensions Software, LLC (Chocolatey) reserves the right to change it product plans or roadmap at any time. 