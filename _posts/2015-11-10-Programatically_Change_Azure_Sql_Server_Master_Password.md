---
title: Programatically Change Azure Sql Server Master Password
layout: post
---

Every Azure Sql database that is created has to live on a server instance. Every server has a server admin account and 
a password for that account. The password for this account is given at the time of provisioning the server. The password can
be changed later from the Azure portal. However, I came across a scenario wherein I had to do this programatically. The below is the
REST API call that needs to be made to achieve this.

    HTTP POST 
    URL: "https://sql2.ext.azure.com/api/Server/Update"
    HEADERS: 
      Authorization: OAUTH token
	  Content-Type: application/json
    BODY:  {
    "resourceId": [path to azure sql server],
    "location": [Location of azure sql server],
    "administratorLoginPassword": [Password],
    "confirmAdministratorLoginPassword": [Password]
		} 
			 
This should achieve the job.