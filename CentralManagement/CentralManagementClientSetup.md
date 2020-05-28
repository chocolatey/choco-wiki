# Chocolatey Central Mangement Client Setup

> :warning: **WARNING**: This is a Work in Progress. Please check back later
>
> Also see [[Chocolatey Central Management in Features|FeaturesChocolateyCentralManagement]].

<!-- TOC depthFrom:2 -->

- [Summary](#summary)
- [Setup](#setup)
  - [Configuration](#configuration)
  - [Config Settings](#config-settings)
  - [Features](#features)
- [Common Errors And Resolutions](#common-errors-and-resolutions)
  - [Unable to report computer information to CCM](#unable-to-report-computer-information-to-ccm)
  - [Unable to check for deployments from CCM](#unable-to-check-for-deployments-from-ccm)

<!-- /TOC -->

## Summary

> :warning: **WARNING**: This is a Work in Progress. Please check back later
>
> Also see [[Chocolatey Central Management in Features|FeaturesChocolateyCentralManagement]].



## Setup

First you need Chocolatey Agent installed. As there may be some steps involved with the install of the agent, please see [[Chocolatey Agent Setup|FeaturesAgentService]].


At a minimum you need the following items set to be able to have a Chocolatey Agent be "opted-in" for both checking into Central Management and Deployments:

~~~powershell
choco config set CentralManagementServiceUrl https://<FQDN_CCM_SERVICE>:24020/ChocolateyManagementService
choco feature enable --name="'useChocolateyCentralManagement'"
# Requires Chocolatey Licensed Extension v2.1.0+, Chocolatey-Agent v0.10.0+, and Chocolatey Central Management v0.2.0+:
choco feature enable --name="'useChocolateyCentralManagementDeployments'"
~~~

> :memo: **NOTE**
>
> As these features have security considerations as it is enabling cross-machine communication, they must be turned on explicitly.
> If you decide you want to open this up for over the internet communication, you should also set `centralManagementClientCommunicationSaltAdditivePassword` and `centralManagementServiceCommunicationSaltAdditivePassword` - see [Configuration](#configuration) below.


### Configuration

Also found at [[Chocolatey Configuration|ChocolateyConfiguration]].

### Config Settings
* `centralManagementServiceUrl` = **' '** - The URL that should be used to communicate with Chocolatey Central Management. It should look something like https://servicemachineFQDN:24020/ChocolateyManagementService.  See https://chocolatey.org/docs/features-chocolatey-central-management#fqdn-usage.  Available in business editions v2.0.0+ only.
* `centralManagementReportPackagesTimerIntervalInSeconds` = **'1800'** - Amount of time, in seconds, between each execution of the background service to report installed and outdated packages to Chocolatey Central Management.  Available in business editions v2.0.0+ only.
* `centralManagementReceiveTimeoutInSeconds` = **'30'** - The amount of time, in seconds, that the background agent should wait to receive information from Chocolatey Central Management.  Available in business editions v2.0.0+ only.
* `centralManagementSendTimeoutInSeconds` = **'30'** - The amount of time, in seconds, that the background agent should wait to send information to Chocolatey Central Management.  Available in business editions v2.0.0+ only.
* `centralManagementCertificateValidationMode` = **'PeerOrChainTrust'** - The certificate mode that is used in communication to Chocolatey Central Management.  Available in business editions v2.0.0+ only.
* `centralManagementMaxReceiveMessageSizeInBytes` = **'2147483647'** - The size of the permitted message, in bytes, which can be exchanged between the Chocolatey Background Agent and Chocolatey Central Management. Available in business editions v2.0.3+ only.
* `centralManagementDeploymentCheckTimerIntervalInSeconds` = **'180'** - Amount of time, in seconds, between each execution of the background service to check for a new deployment step from Chocolatey Central Management. Available in business editions v2.1.0+ only.
* `centralManagementClientCommunicationSaltAdditivePassword` = **' '** - Chocolatey Central Management Client Communication Salt Additive - The salt additive to use in the salt recipe for encrypting and verifying communication from an agent TO an instance of Central Management Service (will need to be set the same on all clients contacting that service AND the instance of the management service itself). When not set a default encryption phrase set by the system will be used. When set the unencrypted value must match exactly with what is set in the configuration for Central Management Service and every client contacting that instance of Central Management Service. Value is not shared over the wire. Because this is a much more involved process, it is recommended only setting this if you are transmitting messages over the internet. Defaults to ''. Needs to be at least 8 characters long if set or it will throw errors and use the default. Available in business editions v2.1.0+ only. Requires Chocolatey Agent v0.10.0+ and Central Management 0.2.0+. IMPORTANT: If this value is set, agents less than v0.10.0 will be unable to contact Central Management to report in.
* `centralManagementServiceCommunicationSaltAdditivePassword` = **' '** - Chocolatey Central Management Communication Salt Additive - The salt additive to use in the salt recipe for encrypting and verifying communication FROM an instance of Central Management Service to an agent (will need to be set the same on all clients contacting that service AND the instance of the management service itself). When not set a default encryption phrase set by the system will be used. When set the unencrypted value must match exactly with what is set in the configuration for Central Management Service and every client contacting that instance of Central Management Service. Value is not shared over the wire. Because this is a much more involved process, it is recommended only setting this if you are transmitting messages over the internet. Defaults to ''. Needs to be at least 8 characters long if set or it will throw errors and use the default. Available in business editions v2.1.0+ only. Requires Chocolatey Agent v0.10.0+ and Central Management 0.2.0+.

### Features
* [ ] `useChocolateyCentralManagement` - Use Chocolatey Central Management - Lists of installed and outdated packages will be reported to the chosen Chocolatey Central Management server.  Business editions only (version 2.0.0+). See https://chocolatey.org/docs/features-chocolatey-central-management
* [ ] `useChocolateyCentralManagementDeployments` - Use Chocolatey Central Management Deployments - Centrally managed deployments of packages and scripts can be sent from Chocolatey Central Management.  Business editions only (version 2.1.0+).  See https://chocolatey.org/docs/features-chocolatey-central-management

## Common Errors And Resolutions

### Unable to report computer information to CCM

You may see messaging like the following in the chocolatey-agent.log:

~~~sh
[INFO ] - Creating secure channel to https://ccmserver:24020/ChocolateyManagementService ahead of CCM check-in...
[ERROR] - Unable to report computer information to CCM.:
 The message with Action 'http://tempuri.org/IChocolateyManagementService/report_computer_information' cannot be
 processed at the receiver, due to a ContractFilter mismatch at the EndpointDispatcher. This may be because of either a
 contract mismatch (mismatched Actions between sender and receiver) or a binding/security mismatch between the sender
 and the receiver.  Check that sender and receiver have the same contract and the same binding (including security
 requirements, e.g. Message, Transport, None).
~~~

This is due to having a Chocolatey Agent that is v0.10.0+ versus an older Central Management Service (< v0.2.0). Newer agents are incompatible because they use newer and more secure methods of communication. Please upgrade Central Management to v0.2.0+ at your earliest convenience

### Unable to check for deployments from CCM

This will provide similar messaging as the above. The fix is the same, upgrade Chocolatey Central Management to v0.2.0+.



[[Chocolatey Central Management|CentralManagement]]
