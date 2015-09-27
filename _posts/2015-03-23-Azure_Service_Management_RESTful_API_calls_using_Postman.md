---
title: Azure Service Management RESTful API calls using Postman
layout: post
---

Postman is a chrome app that can be used to make RESTful API calls. This makes it very ideal if you want to quickly test any Web API. 
You can install Postman from [here](https://www.getpostman.com/). 
In this post I will describe how to set up your machine so as to use Postman to make calls to Azure Service Management API.

Step 1:-

If you are a Windows user,

1. Use the makecert tool to create a self-signed certificate. The tool comes with your Visual Studio installation. You will find the instructions to do that here.

2. If you do not have Visual Studio, you can download the tool by installing the Windows SDK from here.

3. Next, Generate a .pfx file from the .cer file by following the steps listed [here](https://technet.microsoft.com/en-us/library/dd261744.aspx).

If you are a Linux user,

1. Use the steps described [here]("https://www.linux.com/learn/tutorials/392099-creating-self-signed-ssl-certificates-for-apache-on-linux") to generate a .crt file. Rename the .crt file with a .cer extension.

2. Use the command given below to generate a .pfx file

3. openssl pkcs12 -export -out pfxfilename.pfx -inkey securekey.key -in cerfilename.cer

Step 2:-

4. Upload the .cer file to the [azure management portal]("https://manage.windowsazure.com") in Settings > Management Certificates

5. Add the .pfx file to Chrome as shown [here]("https://support.globalsign.com/customer/portal/articles/1211541-install-client-digital-certificate---windows-using-chrome").

You are all set to begin using Postman in Chrome to interact with the Azure Service Management API. The documentation for the REST API is [here]("https://msdn.microsoft.com/en-us/library/azure/ee460799.aspx").
