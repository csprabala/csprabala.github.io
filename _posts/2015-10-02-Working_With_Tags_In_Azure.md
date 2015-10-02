---
title: Working with Tags in Azure
layout: post
---

Tags offer an efficient mechanism for associating organizational/user metadata to Azure resources. A typical use case could be to add your environment 
information such as DEV, TST, PRD to a resource. Another example could be adding BU information such as Finance, HR, Marketing etc.. to a resource. 
Tags flow down into billing data and can form the basis for your chargeback model. Tags can be created using PowerShell and REST api. A few details 
on how that can be done are provided [here](https://azure.microsoft.com/en-us/documentation/articles/resource-group-using-tags/) and 
[here](http://msdn.microsoft.com/library/azure/dn790568.aspx). 

Below are a couple of code samples that illustrate how tagging can be done at the individual
resource level. A point of note is that tagging at the individual resource level works for the V2 versions of those resources. These resource can be created
using the ARM (Azure Resource Management) workflows as opposed to using the Classic workflows. For the moment to use the ARM workflow using PowerShell command


    Switch-AzureMode -Name AzureResourceManagement

*Using PowerShell to tag Resources:-*

Obtain the Resource ID. Every resource in Azure has a unique URI that can be used to reference it and fetch its metadata. To obtain the Resource ID for 
say a VM (TagTestVM) in Resource Group (TagTestResourceGroup), you may use the following PowerShell command. 

    $resource_id = (Get-AzureVM -ResourceGroupName TagTestResourceGroup -Name TagTestVM).ID
    Set-AzureResource -ResourceId $resource_id -Tag @(@{ Name="TestTag"; Value="TestValue" }) -Force

*Using REST API to tag Resources:-*

To tag resources using REST API, you need to do submit a HTTP PATCH request to the resource URL

1. Construct the URL, It is a combination of https://management.azure.com and the Resource ID of the resource you would like to tag. A fully constructed
URL for VM will look like 

	```
	https://management.azure.com/subscriptions/{%raw%}{subscription-id}{%endraw%}/resourceGroups/{%raw%}{resource-group-name}{%endraw%}/providers/Microsoft.Compute/virtualMachines/{%raw%}{VM-Name}{%endraw%}
	```

2. The following headers have to be passed along with the request in the header

		a. Authorization: Bearer {%raw%}{OAUTH TOKEN}{%endraw%}
		b. Content-Type: application/json

3. The body of the request has to be json formatted supplying the tag names in the following way.

		{
		"tags": 
		    {
		        "TestTag": "TestValue"
		    }
		} 
		
	A point of note, if there are already any tags defined, fetch them and add them to the request body along with the new tags. Otherwise, the older tags
	will be overwritten.

4. Submit the request and you should see your tags added to the resource. 
