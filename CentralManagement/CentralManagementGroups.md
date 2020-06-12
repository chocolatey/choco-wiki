# Chocolatey Central Management - Groups

Central Management's Groups are the basis on which a given [[Deployment|CentralManagementDeployments]] operates.
In Central Management, a Group may contain one or more computers and/or other groups.
Currently, Central Management's groups are entirely self-contained, and cannot be directly mapped from Active Directory groups.

The **Groups** page can be accessed from the left-hand navigation menu on your Central Management Dashboard by selecting the **Groups** menu item.
If you do not see this menu entry, verify with your administrator whether your Central Management account has the View Groups role assigned..

![Groups menu entry on the CCM Dashboard](images/groups/ccm-groups-menu.png)

<!-- TOC depthFrom:2 -->

- [Creating a Group](#creating-a-group)
- [Editing a Group](#editing-a-group)
- [Deleting a Group](#deleting-a-group)
- [Eligibility for Deployments](#eligibility-for-deployments)
- [Related Topics](#related-topics)

<!-- /TOC -->

## Creating a Group

On the main Groups page, select the **+ Create New Group** button.

![Create New Group button on the Groups page](images/groups/ccm-groups-new.png)

Fill in a Name for the group and (optionally) a Description in the appropriate fields in the _Create New Group_ modal.
Then, select the computer(s) or existing group(s) you would like to include in the new group and click the **>** button in the _Available Groups/Computers_ column to add the selected items, or click the **>>** button to add all available groups and computers into the new group.

![New Group Modal](images/groups/ccm-groups-modal-new.png)

Click **:floppy_disk: Save** to close the modal and create the new group.

## Editing a Group

> :information_source: **NOTE**
>
> If you do not see the **Edit** menu entry or the **:gear: Actions** buttons, consult your administrator to determine if you have the Edit Groups role assigned.

On the main Groups page, find the group you want to edit.
You can enter a search term in the text field to filter results by typing in part of their Name or Description and clicking the :mag: button.
Select the **:gear: Actions** button on the right-hand side of the group, and then select **Edit** to open the _Edit Group_ modal.

![Edit menu entry in group actions flyout menu](images/groups/ccm-groups-edit.png)

From the **Edit Group** modal, you can modify the group name and description, and modify members by adding or removing groups and/or computers.

## Deleting a Group

> :information_source: **NOTE**
>
> If you do not see the **Delete** menu entry or the **:gear: Actions** buttons, consult your administrator to determine if you have the appropriate role assigned to your account.

On the main Groups page, find the group you want to edit.
You can enter a search term in the text field to filter results by typing in part of their Name or Description and clicking the :mag: button.
Similar to the [Edit Group](#editing-a-group) action, select the **:gear: Actions** button on the right-hand side of the group, and instead select **Delete**.
You will be prompted to confirm the deletion.

## Eligibility for Deployments

The Create / Edit Group modals display groups or computers that are ineligible for Deployments in either red or orange, depending on the reason for their ineligibility.
**All** groups and computers in a given group must have their eligibility clear in order for that group to be used as part of a Deployment.
If a Deployment is targeting ineligible groups, the deployment cannot be started until the eligibility status(es) of the affected computers has been resolved.

![Group eligibility legend](images/groups/ccm-groups-eligibility.png)

## Related Topics

* [[Chocolatey Central Management|CentralManagement]]
* [[Central Management - Deployments|CentralManagementDeployments]]
* [[Central Management - Computers|CentralManagementComputers]]

___
[[Chocolatey Central Management|CentralManagement]]
