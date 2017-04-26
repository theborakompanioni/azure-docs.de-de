---
title: Azure Germany Databases | Microsoft Docs
description: This provides a comparision of database services for Azure Germany
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
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: 392f5f3a12a650f80a84f97c93873b11f554399f
ms.lasthandoff: 04/25/2017

---

# <a name="azure-germany-database-services"></a>Azure Germany database services
## <a name="azure-sql-database"></a>Azure SQL Database
Refer to the [Microsoft Security Center for SQL Database Engine](https://msdn.microsoft.com/en-us/library/bb510589.aspx) and [Azure SQL Database global documentation](../sql-database/index.md) for additional guidance on metadata visibility configuration and protection best practices.

### <a name="variations"></a>Variations
Azure SQL V12 Database is generally available in Azure Germany.

The address for Azure SQL Databases in Azure Germany is different from global Azure:

| Service Type | global Azure | Azure Germany |
| --- | --- | --- |
| Azure SQL Database | *.database.windows.net | *.database.cloudapi.de |


## <a name="azure-redis-cache"></a>Azure Redis Cache
For details on this service and how to use it, see [Azure Redis Cache global documentation](../redis-cache/index.md).

### <a name="variations"></a>Variations
The URLs for accessing and managing Azure Redis Cache in Azure Germany are different from global Azure:

| Service Type | global Azure | Azure Germany |
| --- | --- | --- |
| Cache endpoint | *.redis.cache.windows.net | *.redis.cache.cloudapi.de |
| Azure portal | https://portal.azure.com | https://portal.microsoftazure.de |

> [!NOTE]
> All of your scripts and code need to account for the appropriate endpoints and environments. For more information, see [How to connect to Azure Germany Cloud](../redis-cache/cache-howto-manage-redis-cache-powershell.md).
> 
> 


## <a name="next-steps"></a>Next steps
For supplemental information and updates, subscribe to the [Azure Germany Blog](https://blogs.msdn.microsoft.com/azuregermany/).







