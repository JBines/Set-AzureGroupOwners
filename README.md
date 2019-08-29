# Set-AzureGroupOwners
This script automates the population of members from one group as owners on another group.  

This has been created with the primary goal of allowing scoped administrators to manage licence assignment via groups which have been enabled for group-based licensing. 

```powershell
 Set-AzureGroupOwners -OwnerSourceGroup "<string[ObjectID]>" -UserGroups "<Array[ObjectID]>" -DifferentialScope "Int[Number]" -AutomationPSCredential "<string[Cred]>"
```

### Examples 
```powershell
Set-AzureGroupOwners -OwnerSourceGroup '7b7c4926-c6d7-4ca8-9bbf-5965751022c2' -UserGroups '0e55190c-73ee-e811-80e9-005056a31be6'
```
In this example the script will add users (members of Group '7b7c4926-c6d7-4ca8-9bbf-5965751022c2') as owners to group '0e55190c-73ee-e811-80e9-005056a31be6'

```powershell
Set-AzureGroupOwners -OwnerSourceGroup '7b7c4926-c6d7-4ca8-9bbf-5965751022c2' -UserGroups "0e55190c-73ee-e811-80e9-005056a31be6","0e55190c-73ee-e811-80e9-005056a3" -DifferentialScope 20
```
In this example the script will add users (members of Group '7b7c4926-c6d7-4ca8-9bbf-5965751022c2') as owners to group 0e55190c-73ee-e811-80e9-005056a31be6 and 0e55190c-73ee-e811-80e9-005056a3 with an increased scope of 20 changes.

### Final Notes
This function requires that you have already created your Azure AD Groups.

We used AzureADPreview Version: 2.0.2.5 ()

Please note, when using Azure Automation with more than one user group the array should be set to JSON for example ['ObjectID','ObjectID']

### Author
Joshua Bines, Consultant

Find me on:
* Web:     https://theinformationstore.com.au
* LinkedIn:  https://www.linkedin.com/in/joshua-bines-4451534
* Github:    https://github.com/jbines

```powershell
.PARAMETER OwnerSourceGroup 
The OwnerSourceGroup parameter details the ObjectId of the Azure Group which contains all the desired owners as members of one group.

.PARAMETER UserGroups
The UserGroups parameter details all the ObjectId of the target groups. This setting will remove and add owners listed in the -UserGroup array so they match the users listed as members in the -OwnerSourceGroup. 

.PARAMETER UserGroupsFilter
The UserGroupsFilter parameter details a pattern that could be used to automate the adding of addiosial groups to be managed. Please be aware that an incorrect 

.PARAMETER DifferentialScope
The DifferentialScope parameter defines how many objects can be added or removed from the UserGroups in a single operation of the script. The goal of this setting is throttle bulk changes to limit the impact of misconfiguration by an administrator. What value you choose here will be dictated by your userbase and your script schedule. The default value is set to 10 Objects. 

.PARAMETER AutomationPSCredential
The DifferentialScope parameter defines how many objects can be added or removed from the UserGroups in a single operation of the script. The goal of this setting is throttle bulk changes to limit the impact of misconfiguration by an administrator. What value you choose here will be dictated by your userbase and your script schedule. The default value is set to 10 Objects. 

.EXAMPLE
Set-AzureGroupOwners -OwnerSourceGroup '7b7c4926-c6d7-4ca8-9bbf-5965751022c2' -UserGroups '0e55190c-73ee-e811-80e9-005056a31be6'

-- SET OWNERS FOR AZURE GROUP --

In this example the script will add users (members of Group '7b7c4926-c6d7-4ca8-9bbf-5965751022c2') as owners to group '0e55190c-73ee-e811-80e9-005056a31be6'

.EXAMPLE
Set-AzureGroupOwners -OwnerSourceGroup '7b7c4926-c6d7-4ca8-9bbf-5965751022c2' -UserGroups "0e55190c-73ee-e811-80e9-005056a31be6","0e55190c-73ee-e811-80e9-005056a3" -DifferentialScope 20

-- SET OWNERS FOR 2 AZURE GROUPS & INCREASE DIFFERENTIAL SCOPE TO 20 --

In this example the script will add users (members of Group '7b7c4926-c6d7-4ca8-9bbf-5965751022c2') as owners to group 0e55190c-73ee-e811-80e9-005056a31be6 and 0e55190c-73ee-e811-80e9-005056a3 with an increased scope of 20 changes.

.EXAMPLE
Set-AzureGroupOwners -OwnerSourceGroup '7b7c4926-c6d7-4ca8-9bbf-5965751022c2' -UserFilter "startswith(DisplayName, 'GRP License Assignment -')" | Where-Object{$_.Description -eq 'This group has been created to assign licenses to BU1'}" -DifferentialScope 20

-- SET OWNERS VIA FILTER & INCREASE DIFFERENTIAL SCOPE TO 20 --

Still in dev as I had issues with the filter CMDlet... For another day when I have more time. A Static list for now :) 

.LINK

Azure AD Group-Based Licensing - https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal

Log Analytics Workspace - https://docs.microsoft.com/en-us/azure/azure-monitor/learn/quick-create-workspace

.NOTES
This function requires that you have already created your Azure AD Groups.

We used AzureADPreview Version: 2.0.2.5 ()

Please note, when using Azure Automation with more than one user group the array should be set to JSON for example ['ObjectID','ObjectID']

[AUTHOR]
Joshua Bines, Consultant

Find me on:
* Web:     https://theinformationstore.com.au
* LinkedIn:  https://www.linkedin.com/in/joshua-bines-4451534
* Github:    https://github.com/jbines
```
