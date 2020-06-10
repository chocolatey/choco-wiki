# Chocolatey Central Management - Computers

<!-- TOC depthFrom:2 -->

- [Summary](#summary)
- [Registering a New Computer](#registering-a-new-computer)
- [Viewing Installed Packages on a Computer](#viewing-installed-packages-on-a-computer)
- [Removing a Computer from Central Management](#removing-a-computer-from-central-management)
- [Related Topics](#related-topics)

<!-- /TOC -->

## Summary

Chocolatey Central Management gives you visibility into what's installed on a given computer, as well as their last check-in to Central Management and IP Address.

The **Computers** page can be accessed from the Central Management Dashboard via the menu entry in the left-hand sidebar.

![Computers menu entry on the CCM Dashboard](images/computers/ccm-computers-nav.png)

## Registering a New Computer

Client computers will show up in Central Management automatically as long as long as these conditions are met for the client computer:

1. The `chocolatey`, `chocolatey.extension`, and `chocolatey.agent` packages are installed, alongside a valid Chocolatey for Business license.
1. The `useChocolateyCentralManagement` feature is enabled.
1. The `centralManagementServiceUrl` is set correctly in the chocolatey configuration file (typically to `https://hostname:24020/CentralManagementService`)
1. The client has access to the above URL (this may require opening the port in your firewall, etc.) so that it can resolve the SSL certificate necessary to communicate with the CCM service.

## Viewing Installed Packages on a Computer

From the main Computers page in Central Management, locate the computer of interest in the list or by providing a search term in the table filter.
Select the **:cog: Actions** menu in the corresponding right-hand column, and click **Details**.

![Finding a computer's details menu option](images/computers/ccm-computers-details-menu.png)

You will be presented with a list of the installed software packages for the machine, similar to below.

![Computer details screen showing installed software](images/computers/ccm-computers-details.png)

## Removing a Computer from Central Management

> :information_source: **NOTE**
>
> Unless you first uninstall (at minimum) the `chocolatey-agent` or disable Central Management by disabling the feature setting, the deleted computer will reappear when the Chocolatey Agent performs its next check-in.

From the main Computers page in Central Management, locate the computer of interest in the list or by providing a search term in the table filter.
Select the **:cog: Actions** menu in the corresponding right-hand column, and click **Delete**.

[Deleting a computer in Central Management](images/computers/ccm-computers-delete-menu.png)

You will be prompted to confirm the deletion.

![Prompt to confirm deletion of a computer in Central Management](images/computers/ccm-computers-delete-confirm)

## Related Topics

[[Chocolatey Central Management|CentralManagement]]

[[Chocolatey Central Management - Features|FeaturesChocolateyCentralManagement]]

[[Central Management - Groups|CentralManagementGroups]]

[[Central Management - Software|CentralManagementSoftware]]
