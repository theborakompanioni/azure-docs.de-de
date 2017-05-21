---
title: Azure Germany Internet of Things | Microsoft Docs
description: This provides a starting point for the IoT Suite for Azure Germany
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2017
ms.author: ralfwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: 9726fba35274d75341ebfe5afbf4f44116e9fded
ms.contentlocale: de-de
ms.lasthandoff: 04/25/2017

---
# <a name="azure-iot-services-in-azure-iot-suite"></a>Azure IoT services in Azure IoT Suite

All the required services for Azure IoT Suite are available in Azure Germany. 

## <a name="preconfigured-solutions"></a>Preconfigured solutions
You might want to start with one of the preconfigured solutions: 

### <a name="remote-monitoring"></a>Remote monitoring
The IoT Suite remote monitoring preconfigured solution is an implementation of an end-to-end monitoring solution for multiple machines running in remote locations. The solution combines key Azure services to provide a generic implementation of the business scenario. You can use the solution as a starting point for your own implementation and customize it to meet your own specific business requirements.

### <a name="predictive-maintenance"></a>Predictive maintenance
The IoT Suite predictive maintenance preconfigured solution is an end-to-end solution for a business scenario that predicts the point at which a failure is likely to occur. You can use this preconfigured solution proactively for activities such as optimizing maintenance. The solution combines key Azure IoT Suite services, such as IoT Hub, Stream analytics, and an Azure Machine Learning workspace. This workspace contains a model, based on a public sample data set, to predict the Remaining Useful Life (RUL) of an aircraft engine. The solution fully implements the IoT business scenario as a starting point for you to plan and implement a solution that meets your own specific business requirements.


### <a name="variations"></a>Variations

The start page for the Azure IoT Suite is different from global Azure. Use [http://www.azureiotsuite.de](http://www.azureiotsuite.de) and follow the instructions. 

## <a name="deploy-the-preconfigured-solutions"></a>Deploy the preconfigured solutions

Both solutions can be deployed in two ways, via website or via PowerShell.

### <a name="deploy-solution-via-website"></a>Deploy solution via website

Follow the instructions in the [tutorial for the preconfigured solutions](../iot-suite/iot-suite-getstarted-preconfigured-solutions.md) with the starting page given above.

### <a name="deploy-solution-with-powershell"></a>Deploy solution with PowerShell

There is the full version (using Resource Manager templates and Visual Studio) for the *Remote Monitoring* solution. Download from the [Azure-IoT-Remote-Monitoring repository on GitHub](https://github.com/Azure/azure-iot-remote-monitoring). The PowerShell deployment is ready for other environments like Azure Germany. Provide the *Environment* parameter "AzureGermanCloud", so it looks similar to this:

    build.cmd cloud debug AzureGermanCloud

For using the Bing Maps, you have to make an additional step since this service is currently not available in Azure Germany and therefore cannot be subscribed automatically. You can solve this by subscribing to the service in global Azure and use the service there. 

> [!NOTE]
> When using Bing Maps the way it is described here, you are leaving the environment for Azure Germany!

Here is how to do it:

1. Create a Bing Maps API in the global Azure portal by clicking `+ New`, search for *Bing Maps API for Enterprise* and follow the prompts to create.
2. Get your Bing Maps API for Enterprise QueryKey from global Azure portal: 
    1. Navigate to the Resource Group where your Bing Maps API for Enterprise is in the global Azure portal.
    2. Click **All Settings**, then **Key Management**. 
    3. You see two keys: MasterKey and QueryKey. Copy the value for QueryKey.
3. Pull down the latest code from the [Azure-IoT-Remote-Monitoring repository on GitHub](https://github.com/Azure/azure-iot-remote-monitoring)
4. Run a cloud deployment in your environment following commandline deployment guidance in the `/docs/` repository folder. 
5. After you've run the deployment, look in your root folder for the ***.user.config** file created during deployment. Open this file in a text editor. 
6. Change the following line to include the value you copied for your QueryKey: `<setting name="MapApiQueryKey" value="" />`
7. Redeploy the solution by repeating step 4.
 


## <a name="next-steps"></a>Next steps
For supplemental information and updates, subscribe to the [Azure Germany Blog](https://blogs.msdn.microsoft.com/azuregermany/).

