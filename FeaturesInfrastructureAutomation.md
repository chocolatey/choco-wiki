# Configuration Management Integration

Chocolatey integrates with several infrastructure automation tools!

## Usage
With most of these tools, the interface you would interact with Chocolatey would be through the tool or through the interfaces of the tool, like in scripts.

* [Ansible](#ansible)
* [Boxstarter](#boxstarter)
* [Chef](#chef)
* [Octopus Deploy](#octopus-deploy)
* [PowerShell DSC](#powershell-dsc)
* [PowerShell PackageManagement / OneGet](#powershell-packagemanagement)
* [Puppet](#puppet)
* [Saltstack](#saltstack)
* [SCCM](#system-center-configuration-manager)

## Ansible

Ansible has the `win_chocolatey` module that manages both packages and the installation of Chocolatey itself.

~~~yaml
win_chocolatey:
  name: git
  source: https://my.internal.repository/api/v2/
~~~

[Read More...](https://bit.ly/choco_ansible)

## Boxstarter

Boxstarter is a lightweight configuration management utility.

> Repeatable, **reboot resilient** Windows environment installations made easy using Chocolatey packages. When its time to repave either **bare metal or virtualized instances**, locally or on a **remote machine**, Boxstarter can automate both trivial and highly complex installations. Compatible with all Windows versions from **Windows 7/2008 R2 forward**.

[Read more...](http://boxstarter.org/)

## Chef

Chef 12.7+ has a [built-in](https://www.chef.io/blog/2016/02/12/chef-client-12-7-released/) `chocolatey_package` resource to use with Chocolatey.

~~~ruby
chocolatey_package 'git' do
  action :install
  source 'https://my.internal.repository/api/v2'
end
~~~

[Resource - Read More...](https://docs.chef.io/resource_chocolatey_package.html)

When you need to also install Chocolatey, you would use the community cookbook to do so. The cookbook is maintained by the Chocolatey team. It has a package resource that can be used with older Chef versions.

~~~ruby
include_recipe 'chocolatey'
~~~

[Cookbook - Read More...](https://bit.ly/choco_chef)

### Example

Here's a more in depth example from [Nordstrom](https://github.com/Nordstrom/chefdk_bootstrap/blob/master/recipes/windows.rb):

~~~ruby
include_recipe 'chocolatey'

home = Dir.home
%W(
  #{home}/.chef
  #{home}/chef
  #{home}/chef/cookbooks
).each do |directory|
  directory directory
end

packages = node['chefdk_bootstrap']['package']

packages.each do |pkg, install|
  include_recipe "#{cookbook_name}::#{pkg}" if install
end
~~~

Another example from [Facebook](https://github.com/facebook/IT-CPE/tree/master/chef/cookbooks/cpe_choco):

~~~ruby
node.default['cpe_choco']['sources']['bacon'] =
  'source' => 'http://bacon.es.yummy',
}
~~~

## Octopus Deploy

Octopus is a friendly deployment automation tool for .NET developers. It integrates with lots of utilities, and they have a template for installing Chocolatey packages: https://library.octopusdeploy.com/step-template/actiontemplate-chocolatey-install-package

[Read more...](https://octopus.com/)

## PowerShell DSC

PowerShell DSC (Desired State Configuration) has a cChoco module that can manage both packages and the installation of Chocolatey itself.

~~~powershell
  cChocoInstaller installChoco
  {
    InstallDir = "c:\ProgramData\chocolatey"
  }

  cChocoPackageInstaller installGit
  {
     Name = "git"
     DependsOn = "[cChocoInstaller]installChoco"
  }
~~~

[Read More...](https://bit.ly/choco_dsc)

## PowerShell PackageManagement
**NOTE:** Chocolatey has a provider for the built-in package manager on Windows 10/Windows Server 2016. The official provider is still under development at this time and is expected to be complete in early summer 2016.

[PowerShell PackageManagement (aka OneGet)](https://github.com/OneGet/oneget) is a package manager ***aggregator*** that depends on the existence of package managers as providers to work, one of which is Chocolatey. For a pretty comprehensive post about what PackageManagement is and what it is not, see the PackageManagement Blog post on [10 things about OneGet that are completely different than you think](https://blogs.technet.microsoft.com/packagemanagement/2015/05/05/10-things-about-oneget-that-are-completely-different-than-you-think/).

## Puppet
Puppet has a [Supported module](https://forge.puppet.com/supported) coming for Chocolatey, but also has an [Approved module](https://forge.puppet.com/approved) written by the Chocolatey team. The module can manage packages and the install and configuration of Chocolatey itself.

~~~puppet
include chocolatey

package { 'git':
  ensure   => latest,
  provider => 'chocolatey',
  source   => 'https://my.internal.repository/api/v2',
}
~~~~

Puppet has some great documentation on getting started with Chocolatey. Be sure to check that out at [Using Windows modules](https://docs.puppet.com/pe/latest/windows_modules.html).

[Read More...](https://bit.ly/choco_puppet)

## Saltstack

Salt has a Chocolatey module that manages both packages and the installation of Chocolatey itself.

~~~python
salt '*' chocolatey.bootstrap
salt '*' chocolatey.install git
~~~

[Read More...](https://bit.ly/choco_salt)

## System Center Configuration Manager
Chocolatey integrates with SCCM by handling the software management, and pointing to [distribution points](https://technet.microsoft.com/en-us/library/bb680614.aspx) as the source for packages. This allows folks to get packages and larger binaries out to their network without constraints and still take advantage of Chocolatey's fantastic abilities!

To direct Chocolatey package installs, you can still write GPOs to ensure this.


