<!-- TOC depthFrom:2 -->

- [Package Parameters](#package-parameters)
- [Scenarios](#scenarios)
- [FAQ](#faq)
- [Common Errors and Resolutions](#common-errors-and-resolutions)

<!-- /TOC -->

> :warning: **WARNING**: Ensure you have completed installing the [[database package|CentralManagementSetupDatabase]] and have set up sql server [[logins and access|CentralManagementSetupDatabase#step-2-set-up-sql-server-logins-and-access]].

> :memo: **NOTE**: You could be installing several of these if your environment is large enough.

> :warning: **WARNING**
>
> Timezones are super important here and time synchronization is really important when generating SSL Certificates. You want to make sure you have this correct and good. Otherwise there is a potential edge case you could generate an SSL Certificate that is not yet valid. As the service package could generate an SSL certificate if you don't pass an existing thumbprint, its best to ensure that time synchronization is not an issue with the machine you are installing this on.

> :note: **NOTE**
The installation of this package will assume that a new SSL certificate will need to created and used. This will be generated using the fully-qualified domain name of the system on which you are installing this package. It will also be assumed that port 24020 will be used to facilitate communication between an agent and the management service. The following parameters can be optionally provided to override this default behavior:

## Package Parameters

- `/Username`
  - Username to install the management service as;
NOTE: Default Value: ChocolateyLocalAdmin
- `/Password`
  - Password to use for the management service account;
  - NOTE: Automatically generated secure password
- `/EnterPassword`
  - This will prompt you to enter the password, during install, for the username (provided via the /Username parameter) the management service will run under;
  - NOTE: Default Value: Not provided
- `/ConnectionString`
  - The SQL Server database connection string to be used to connect to the CCM database;
  - NOTE: Default Value: Server=<LOCAL COMPUTER FQDN NAME>; Database=ChocolateyManagement; Trusted_Connection=True;
- `/Database`
  - Name of the SQL Server database to use. Note that if you do not also pass /ConnectionString, it will be generated using this parameter value and /SqlServerInstance (using defaults for missing parameters);
  - NOTE: Default Value: ChocolateyManagement
- `/SqlServerInstance`
  -Instance name of the SQL Server database to connect to. Note that if you do not also pass /ConnectionString, it will be generated using this parameter value and /Database (using defaults for missing parameters);
  - NOTE: Default Value: <LOCAL COMPUTER FQDN NAME>
- `/PortNumber`
  - The port the Chocolatey Management Service will listen on. This will automatically create a rule to open the firewall on this port;
  - NOTE: Default Value 24020
- `/CertificateDnsName`
  - The DNS name of the self-signed certificate that is generated if no existing certificate thumbprint is provided using the /CertificateThumbprint parameter is provided;
  - NOTE: Default Value: <LOCAL COMPUTER FQDN NAME>
- `/CertificateThumbprint`
  - By default the CCM Service uses a self-signed SSL certificate to secure communication with the clients. Use this parameter to provide the thumbprint of a certificate to use instead. Note that if you use this the certificate must already be in the LocalMachine\TrustedPeople Certificate Store on the Chocolatey Management Service computer;
  - NOTE: Default Value: Not applicable as if not provided, a new Self Signed Certificate will be generated
- `/NoRestartService`
  - Explicit request not to restart the service
  - NOTE: Default Value: Not provided
- `/DoNotReinstallService`
  - Explicit request not to reinstall the service
  - NOTE: Default Value: Not provided

Once you have completed the steps necessary to allow Sql access for the user in Step 2, we can install the management service.

## Scenarios

Scenario 1: You are installing the management service on the same machine as a SQL Server Express instance

```powershell
choco install chocolatey-management-service -y --package-parameters-sensitive="'/ConnectionString=""Server=Localhost\SQLEXPRESS;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```

Scenario 2: You are installing the management service on a server, and targeting an existing SQL Server instance in your organization

```powershell
choco install chocolatey-management-service -y --package-parammeters-sensitive="'/ConnectionString=""Server=RemoteSqlHost;Database=ChocolateyManagement;User ID=ChocoUser;Password='Ch0c0R0cks';""'"
```

## FAQ

## Common Errors and Resolutions

[[Central Management Setup|CentralManagementSetup]] | [[Chocolatey Central Management|CentralManagement]]
