# Configuration Management Integration

Chocolatey integrates with several infrastructure automation tools!

## Usage
With most of these tools, the interface you would interact with Chocolatey would be through the tool or through the interfaces of the tool, like in scripts.

* [Ansible](#ansible)
* [Chef](#chef)
* [PowerShell DSC](#powershell-dsc)
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

## Chef

Chef has a built-in `chocolatey_package` resource to use with Chocolatey.

~~~ruby
chocolatey_package 'git' do
  action :install
  source 'https://my.internal.repository/api/v2'
end
~~~

[Resource - Read More...](https://docs.chef.io/resource_chocolatey_package.html)

When you need to also install Chocolatey, you would use the community cookbook to do so. The cookbook is maintained by the Chocolatey team.

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
