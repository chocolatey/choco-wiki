# Chocolatey Central Management - Computers
Chocolatey Central Management gives you visibility into what's installed on a given computer, as well as their last check-in to Central Management and IP Address.

The **Computers** page can be accessed from the Central Management Dashboard via the menu entry in the left-hand sidebar.

![Computers menu entry on the CCM Dashboard](images/computers/ccm-computers-nav.png)


___
<!-- TOC depthFrom:2 -->

- [Registering a New Computer](#registering-a-new-computer)
- [Viewing Installed Software on a Computer](#viewing-installed-software-on-a-computer)
- [Removing a Computer from Central Management](#removing-a-computer-from-central-management)
- [Related Topics](#related-topics)

<!-- /TOC -->

___
## Registering a New Computer

Client computers (agents) will show up in Central Management automatically as long as long as these conditions are met for the client computer:

1. The `chocolatey`, `chocolatey.extension`, and `chocolatey-agent` packages are installed, alongside a valid Chocolatey for Business license.
1. The `useChocolateyCentralManagement` feature is enabled.
1. The `centralManagementServiceUrl` is set correctly in the chocolatey configuration file (typically to `https://<FQDN_to_CCM_service_host>:24020/ChocolateyManagementService`)
1. The client has access to the above URL (this may require opening the port in your firewall, etc.) so that it can resolve the SSL certificate necessary to communicate with the CCM service.

Please see [[Central Management Client Setup|CentralManagementSetupClient]] for more details and setup.

___
## Viewing Installed Software on a Computer

From the main Computers page in Central Management, locate the computer of interest in the list or by providing a search term in the table filter.
Select the **:gear: Actions** menu in the corresponding right-hand column, and click **Details**.

![Finding a computer's details menu option](images/computers/ccm-computers-details-menu.png)

You will be presented with a list of the installed software packages for the machine, similar to below.

![Computer details screen showing installed software](images/computers/ccm-computers-details.png)

___
## Removing a Computer from Central Management

> :information_source: **NOTE**
>
> Unless you first uninstall (at minimum) the `chocolatey-agent` or disable Central Management by disabling the feature setting, the deleted computer will reappear when the Chocolatey Agent performs its next check-in.

From the main Computers page in Central Management, locate the computer of interest in the list or by providing a search term in the table filter.
Select the **:gear: Actions** menu in the corresponding right-hand column, and click **Delete**.

![Deleting a computer in Central Management](images/computers/ccm-computers-delete-menu.png)

You will be prompted to confirm the deletion.

![Prompt to confirm deletion of a computer in Central Management](images/computers/ccm-computers-delete-confirm.png)

___
## Related Topics

* [[Chocolatey Central Management|CentralManagement]]
* [[Central Management - Software|CentralManagementSoftware]]
* [[Central Management - Groups|CentralManagementGroups]]
* [[Central Management - Reports|CentralManagementReports]]

___
[[Chocolatey Central Management|CentralManagement]]
