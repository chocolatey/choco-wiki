# Central Management Web setup

<!-- TOC depthFrom:2 -->

- [Step 1: Install Central Management Web Package](#step-1-install-central-management-web-package)
    - [Dependency Installation](#dependency-installation)

<!-- /TOC -->

## Step 1: Install Central Management Web Package

> :warning: **WARNING**: Ensure you have completed installing the database package first on whatever machine the database is on.

> :memo: **NOTE**: At this time we don't recommend opening internet access to CCM web. However, if you choose to, you will want to set up SSL/TLS certificates to ensure communication is encrypted over the internet.

### Dependency Installation

The CCM Service and Web packages are built on ASP .Net Core, and as such we need to ensure that it is installed on the server for proper function. Note, that the codebase is currently locked to version `2.2.7` of these packages, and it is critical that you install these right, otherwise you will encounter errors.

```powershell
choco install IIS-WebServer -s windowsfeatures -y
choco install IIS-ApplicationInit -s windowsfeatures -y
choco install aspnetcore-runtimepackagestore --version 2.2.7 -y
choco install dotnetcore-windowshosting --version 2.2.7 -y
```

> :note: **NOTE** In 90% of cases, accepting the defaults this package provides is sufficient, however if you have specific use cases warranting you change anything will the installation the following parameters can be provided:

- `/ConnectionString`
  - The SQL Server database connection string to be used to connect to the CCM Database;
  - NOTE: Default Value: Server=<LOCAL COMPUTER FQDN NAME>; Database=ChocolateyManagement; Trusted_Connection=True;
- `/Database`
  - Name of the SQL Server database to use. Note that if you do not also pass /ConnectionString, it will be generated using this parameter value and /SqlServerInstance (using defaults for missing parameters);
  - NOTE: Default Value: ChocolateyManagement
- `/SqlServerInstance`
  - Instance name of the SQL Server database to connect to. Note that if you do not also pass /ConnectionString, it will be generated using this parameter value and /Database (using defaults for missing parameters);
  - NOTE: Default Value: <LOCAL COMPUTER FQDN NAME>
- `/Username`
  - The username that the IIS WebApplicationPool will run under. If this is not provided the pool will run under the default account. Note that if you provide this you must also provide either the /Password or /EnterPassword parameter;
   -NOTE: Default Value: IIS APPPOOL\ChocolateyCentralManagement
- `/Password`
  - The password for the username (provided via the /Username parameter) the IIS WebApplicationPool will run under;
  - NOTE: Automatically generated secure password
- `/EnterPassword`
  - This will prompt you to enter the password, during install, for the username (provided via the /Username parameter) the IIS WebApplicationPool will run under;
  - NOTE: Default Value: Not provided

Scenario 1: You are installing the management web components on the same machine as a SQL Server Express instance

```powershell
choco install chocolatey-management-web -y --package-parameters-sensitive="'/ConnectionString=""Server=Localhost\SQLEXPRESS;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```

Scenario 2: You are installing the management web components on a server, and targeting an existing SQL Server instance in your organization

```powershell
choco install chocolatey-management-web -y --package-parammeters-sensitive="'/ConnectionString=""Server=RemoteSqlHost;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```

[[Central Management Setup|CentralManagementSetup]] | [[Chocolatey Central Management|CentralManagement]]
