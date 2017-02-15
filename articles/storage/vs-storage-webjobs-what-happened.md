---
title: "Was ist mit dem WebJob-Projekt passiert (verbundene Visual Studio-Dienste für Azure Storage)? | Microsoft Docs"
description: Erfahren Sie, was nach dem Herstellen einer Verbindung mit einem Speicherkonto mithilfe von verbundenen Visual Studio-Diensten in einem Azure-WebJob-Projekt passiert.
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e4ae7ff320791ff22a7b77a2ffb47b96506bdc06


---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Was ist mit dem WebJob-Projekt passiert (verbundene Visual Studio-Dienste für Azure Storage)?
## <a name="references-added"></a>Verweise wurden hinzugefügt
Das Azure Storage NuGet-Paket wurde zu Ihrem Visual Studio-Projekt hinzugefügt oder aktualisiert.  
Dieses Paket fügt die folgenden .NET-Verweise hinzu:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Die Verbindungszeichenfolge für Azure Storage wurde hinzugefügt
In der Datei „App.config“ Ihres Projekts wurden die Einträge **AzureWebJobsStorage** und **AzureWebJobsDashboard** mit der Verbindungszeichenfolge und dem Schlüssel des ausgewählten Speicherkontos aktualisiert.

Weitere Informationen finden Sie unter [Dokumentationsressourcen für Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).




<!--HONumber=Nov16_HO3-->


