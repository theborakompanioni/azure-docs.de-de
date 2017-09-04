---
title: Was ist mit dem Clouddienstprojekt passiert? | Microsoft Docs
description: Erfahren Sie, was nach dem Herstellen einer Verbindung mit einem Azure-Speicherkonto mithilfe von verbundenen Visual Studio-Diensten in einem Clouddienstprojekt passiert.
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 4c9de56f6daf07097c0f593db37d14dce3bce05f
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Was ist mit dem Clouddienstprojekt passiert (verbundene Visual Studio-Dienste für Azure Storage)?
## <a name="references-added"></a>Verweise wurden hinzugefügt
Das Azure Storage NuGet-Paket wurde zu Ihrem Visual Studio-Projekt hinzugefügt.  
Dieses Paket fügt die folgenden .NET-Verweise hinzu:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Die Verbindungszeichenfolge für Azure Storage wurde hinzugefügt
Elemente mit der Verbindungszeichenfolge und dem Schlüssel des ausgewählten Speicherkontos wurden erstellt. An den folgenden Dateien wurden Änderungen vorgenommen:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**


