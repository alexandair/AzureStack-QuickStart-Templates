## VM IOSTORM WORKLOAD FOR AZURESTACK (LOCAL) ##

<b>DESCRIPTION</b>

This template deploys requested number of VMs and a controller VM with public IP address in same virtual network. Controller VM synchronizes IO workload on all VMs, collects and analyze results and upload it to the storage account.

NOTE: There is a 90 minutes Azure time-out which you can hit if large number of VMs are deployed. To circumvent that, all these operations are done using Scheduled Task which gets created by a DSC Script Resource by a Controller VM.


<b>RESULTS</b>

VM iostorm results file is uploaded to Unique Azure Storage Account ('uniqueStorageAccountName' parameter provided by you) as a blob with name 'VMIOResult.log.ps1.zip'


<b>DEPLOY</b>


Login to AzureStack portal

Click 'New' -> 'Custom' -> 'Template Deployment'

Copy content in azuredeploy.json, click 'Edit Template', paste all the content and click 'Save'

Fill in the parameters

Click 'Create New' to create a new 'Resource Group'

Click 'Create'

Wait for results to appear in 'Storage Account' of a given 'Resource Group' parameter name resource


<b>PARAMETERS</b>

azureUser: Tenant user name used for azure portal login

azurePassword: Tenant user password used for azure portal login

azureApplicationId: "https://azurestack.local-api/"

tenantId: Tenant id of AzureStack e.g. (Get-AzureRmSubscription).TenantId

uniqueDnsNameForPublicIP: <Choose any string value unique across Azure> e.g. "vmbootdns"

uniqueStorageAccountName: <Choose any string value unique across Azure> e.g. "vmbootsa"

location: "local"

vmAdminUsername: <Your VM admin username> e.g. "vmbootadmin"

vmAdminPassword: <Your VM secure admin password>

vmCount: Number of VMs to deploy and bootstorm

vmOsSku: Operating System sku to be deployed in VMs e.g. "2012-R2-Datacenter"