# Azure vwan-autolab  

This one is a bit different, this deploys a pair logic apps with the API connector as well. One logicapp to create the lab environment at the start of every day, and another to delete the lab resource group at the end of the day. 

After deploying the logic apps, you'll need to give the managed identities of both contributor access to your subscription to be able to create and delete the resource groups. 
You can run these powershell commands in the cloudshell to do that:  
    $id1 = (Get-AzADServicePrincipal -DisplayName lab-vwan-creator).id  
    $id2 = (Get-AzADServicePrincipal -DisplayName lab-vwan-killer).id  
    $subid = (Get-AzSubscription).Id  
    New-AzRoleAssignment -ObjectId $id1 -RoleDefinitionName Contributor -Scope /subscriptions/$subid  
    New-AzRoleAssignment -ObjectId $id2 -RoleDefinitionName Contributor -Scope /subscriptions/$subid  

This creates a vwan and a couple of spoke vnets with VM's connected to the vhub. You'll be prompted for the resource group name to deploy the logicapps in, a location for that resource group, your public ip and username and password to use for the VM's and a resource group for the lab resources to be in, this resource group will be created and deleted everyday. JIT access is setup for 8 hours from allowing RDP/ssh access from your public ip. This also creates a logic app that will delete the resource group in 24hrs.

The topology of the lab will look like this:
![wvanlab](https://github.com/user-attachments/assets/99d90b13-282a-4bc4-82f0-afec7f311822)


You can deploy it all right here:  

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fquiveringbacon%2FAzurevwan-autolab%2Fmain%2Flabberapp--vwanlab.json)
