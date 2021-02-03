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

```powershell
Set-AzureGroupOwners -OwnerSourceGroup [GUID] -UserGroups [GUID] -AzureADAutomationCertificate AzureADAppCertName -AzureADAppId [GUID] -AzureADTenantId [GUID]
```

In this example the script uses App-only 'Modern' authentication for access to Exchange Online and Azure AD. This will be the only support way to run unattended scripts into the future. 


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
<# 
.SYNOPSIS
This script automates the population of members from one group as owners on another group.  

.DESCRIPTION
This has been created with the primary goal of allowing scoped administrators to manage licence assignment via groups which have been enabled for group-based licensing.  

## Set-AzureGroupOwners [-OwnerSourceGroup <string[ObjectID]>] [-UserGroups <Array[ObjectID]>] 

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

.PARAMETER AzureADAutomationCertificate
 The EXOAutomationCertificate parameter defines which Azure Automation Certificate you would like to use which grants access to Exchange Online. 

.PARAMETER AzureADAppId
The EXOAppId parameter specifies the application ID of the service principal. Parameter must be used with -AzureADAutomationCertificate. 

.PARAMETER AzureADTenantId
The AzureADTenantId parameter You must specify the TenantId parameter to authenticate as a service principal or when using Microsoft account. Populate by using the Tenant GUID. 


.EXAMPLE
Set-AzureGroupOwners -OwnerSourceGroup '7b7c4926-c6d7-4ca8-9bbf-5965751022c2' -UserGroups '0e55190c-73ee-e811-80e9-005056a31be6'

-- SET OWNERS FOR AZURE GROUP --

In this example the script will add users (members of Group '7b7c4926-c6d7-4ca8-9bbf-5965751022c2') as owners to group '0e55190c-73ee-e811-80e9-005056a31be6'

.EXAMPLE
Set-AzureGroupOwners -OwnerSourceGroup '7b7c4926-c6d7-4ca8-9bbf-5965751022c2' -UserGroups "0e55190c-73ee-e811-80e9-005056a31be6","0e55190c-73ee-e811-80e9-005056a3" -DifferentialScope 20

-- SET OWNERS FOR 2 AZURE GROUPS & INCREASE DIFFERENTIAL SCOPE TO 20 --

In this example the script will add users (members of Group '7b7c4926-c6d7-4ca8-9bbf-5965751022c2') as owners to group 0e55190c-73ee-e811-80e9-005056a31be6 and 0e55190c-73ee-e811-80e9-005056a3 with an increased scope of 20 changes.

.EXAMPLE
Set-AzureGroupOwners -OwnerSourceGroup [GUID] -UserGroups [GUID] -AzureADAutomationCertificate AzureADAppCertName -AzureADAppId [GUID] -AzureADTenantId [GUID]

-- USE APP AUTH TO SET GROUP OWNERS --

In this example the script uses App-only 'Modern' authentication for access to Exchange Online and Azure AD. This will be the only support way to run unattended scripts into the future. 

.EXAMPLE
Set-AzureGroupOwners -OwnerSourceGroup '7b7c4926-c6d7-4ca8-9bbf-5965751022c2' -UserFilter "startswith(DisplayName, 'GRP License Assignment -')" | Where-Object{$_.Description -eq 'This group has been created to assign licenses to BU1'}" -DifferentialScope 20

-- SET OWNERS VIA FILTER & INCREASE DIFFERENTIAL SCOPE TO 20 --

Still in dev as I had issues with the filter CMDlet... For another day when I have more time. A Static list for now :) 

.LINK

Azure AD Group-Based Licensing - https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal

Log Analytics Workspace - https://docs.microsoft.com/en-us/azure/azure-monitor/learn/quick-create-workspace

App-only authentication for unattended scripts in the EXO V2 module - https://docs.microsoft.com/en-us/powershell/exchange/app-only-auth-powershell-v2?view=exchange-ps

.NOTES
This function requires that you have already created your Azure AD Groups.

App Only Auth requires registering the Azure Application and required certificates. 

Please note, when using Azure Automation with more than one user group the array should be set to JSON for example ['ObjectID','ObjectID']

We used AzureADPreview 2.0.2.89 when testing this script. 

[AUTHOR]
Joshua Bines, Consultant

Find me on:
* Web:     https://theinformationstore.com.au
* LinkedIn:  https://www.linkedin.com/in/joshua-bines-4451534
* Github:    https://github.com/jbines
  
[VERSION HISTORY / UPDATES]
0.0.1 20190305 - JBINES - Created the bare bones
0.0.2 20190311 - JBines - Tried filter options via OData filters.  
0.0.5 20190314 - JBines - [BUGFIX] Removed Secure String and Begin Process End as it is not supported in azure automation. 
                        - [Feature] Added a write-output when AutomationPSCredential is using in the write-log function
1.0.0 20190314 - JBines - [MAJOR RELEASE] Other than that it works like a dream... 
1.0.1 20190430 - JBines - [BUGFIX] Changed the DifferentialScope to apply for each group rather than the total number of changes. Better when to apply to a number of groups. 
1.0.2 20191001 - CGarvin - Changed variable $OwnerSourceGroup from String type to $OwnerSourceGroups of Array type for maximum flexibility.
1.0.3 20191021 - JBines - [BUGFIX] Added Select-Object -Unique on the $OwnerSourceGroups Array.
1.0.4 20210203 - JBINES - [Feature] Added support for the use of Service Principles using Certificates based authenication for Azure AD. Also updated AzureADPreview to 2.0.2.89

[TO DO LIST / PRIORITY]
MED - Automatic group assignment Azure Group Filter
#>
```
