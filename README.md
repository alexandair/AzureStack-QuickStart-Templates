# AzureStack-QuickStart-Templates
Quick start ARM templates that deploy on Microsoft Azure Stack

######################################
# CONNECT TO AZURE STACK ENVIRONMENT
######################################

### Add specific Azure Stack Environment
```PowerShell
$AadTenantId = "3dc25382-d7d1-4e5a-ad19-2fb47f1571c2" #GUID Specific to the AAD Tenant
Add-AzureRmEnvironment -Name 'Azure Stack' `
    -ActiveDirectoryEndpoint ("https://login.windows.net/$AadTenantId/") `
    -ActiveDirectoryServiceEndpointResourceId "https://azurestack.local-api/" `
    -ResourceManagerEndpoint ("https://api.azurestack.local/") `
    -GalleryEndpoint ("https://gallery.azurestack.local:30016/") `
    -GraphEndpoint "https://graph.windows.net/"
```

### Get Azure Stack Environment Information
```PowerShell
$env = Get-AzureRmEnvironment 'Azure Stack'
```

### Authenticate to AAD with Azure Stack Environment
```PowerShell
Add-AzureRmAccount -Environment $env -Verbose
```

### Get Azure Stack Environment Subscription
```PowerShell
$SubName = "Best Sub"
Get-AzureRmSubscription -SubscriptionName $SubName | Select-AzureRmSubscription
```

##################################
# DEPLOY TEMPLATE TO AZURE STACK #
##################################
### Set Deployment Variables
```PowerShell
$myNum = "001" #Modify this per deployment
$RGName = "myRG$myNum"
$myLocation = "local"
$myBlobStorageEndpoint = "blob.azurestack.local"
```

### Create Resource Group for Template Deployment
```PowerShell
New-AzureRMResourceGroup -Name $RGName -Location $myLocation
```

### Deploy Simple IaaS Template 
```PowerShell
New-AzureRmResourceGroupDeployment `
    -Name "myDeploymen$myNum" `
    -ResourceGroupName $RGName `
    -TemplateFile "c:\templates\azuredeploy-101-simple-windows-vm-withDNS.json" `
    -deploymentLocation $myLocation `
    -blobStorageEndpoint $myBlobStorageEndpoint `
    -newStorageAccountName "mystorage$myNum" `
    -dnsNameForPublicIP "mydns$myNum" `
    -adminUsername "admin" `
    -adminPassword ("User@123" | ConvertTo-SecureString -AsPlainText -Force) `
    -vmName "myVM$myNum" `
    -windowsOSVersion "2012-R2-Datacenter"
```    
    
########################################
# DEPLOY TEMPLATE USING DSC EXTENSION #
########################################

### Set Deployment Variables
```PowerShell
$vmName = 'myVM001'
$RGName = 'myRG001'
$depName = 'myDSCDeployment001'
```

### Deploy DSC Extension Template
```PowerShell
New-AzureRmResourceGroupDeployment `
    -Name $depName `
    -ResourceGroupName $RGName `
    -TemplateFile "c:\templates\azuredeploy-dsc.json" `
    -TemplateParameterFile "C:\templates\azuredeploy-dsc.parameters.json" `
    -vmName $vmName `
    -timestamp (Get-Date)
```

######################################
# DEPLOY TEMPLATE USING CUSTOM SCRIPT #
######################################

### Set Deployment Variables
```PowerShell
$vmName = 'myVM001'
$RGName = 'myRG001'
$depName = 'myCSDeployment001'
```

### Deploy Custom Script Template
```PowerShell
New-AzureRmResourceGroupDeployment `
    -Name $depName `
    -ResourceGroupName $RGName `
    -TemplateFile "c:\templates\azuredeploy-customscript-command.json" `
    -vmName $vmName
```

###############################
# DEPLOY TEMPLATE USING BGINFO #
###############################

### Set Deployment Variables
```PowerShell
$vmName = 'myVM001'
$RGName = 'myRG001'
$depName = 'myBGIDeployment001'
```

### Deploy BGInfo Template
```PowerShell
New-AzureRmResourceGroupDeployment `
    -Name $depName `
    -ResourceGroupName $RGName `
    -TemplateFile "c:\templates\azuredeploy-bginfo.json" `
    -vmName $vmName
```

##################################
# DEPLOY STORAGE ACCOUNT TEMPLATE #
##################################

### Set Deployment Variables
```PowerShell
$storageacct = 'mystorage001'
$RGName = 'myRG001'
$depName = 'mySADeployment001'
```

### Deploy Storage Account Template
```PowerShell
New-AzureRmResourceGroupDeployment `
    -Name $depName `
    -ResourceGroupName $RGName `
    -TemplateFile "c:\templates\azuredeploy-storageacct.json" `
    -newStorageAccountName $storageacct
```

########################
# DEPLOY vNET TEMPLATE #
########################

### Set Deployment Variables
```PowerShell
$RGName = 'myRG001'
$depName = 'myVNDeployment001'
```

### Deploy vNet Template
```PowerShell
New-AzureRmResourceGroupDeployment `
    -Name $depName `
    -ResourceGroupName $RGName `
    -TemplateFile "c:\templates\azuredeploy-vNet.json"
    
```
