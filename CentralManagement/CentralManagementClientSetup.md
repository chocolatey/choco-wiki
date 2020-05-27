# Chocolatey Central Mangement Client Setup

> :warning: **WARNING**: This is a Work in Progress. Please check back later

<!-- TOC depthFrom:2 -->

- [Summary](#summary)
- [Common Errors And Resolutions](#common-errors-and-resolutions)
  - [Unable to report computer information to CCM](#unable-to-report-computer-information-to-ccm)
  - [Unable to check for deployments from CCM](#unable-to-check-for-deployments-from-ccm)

<!-- /TOC -->

## Summary


## Common Errors And Resolutions

### Unable to report computer information to CCM

You may see messaging like the following:

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
