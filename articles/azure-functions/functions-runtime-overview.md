---
title: "Übersicht über Azure Functions Runtime | Microsoft-Dokumentation"
description: "Übersicht über Azure Functions Runtime Preview"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: d9e655b6ebbdb02bae21fa7a8fa7ee5b46d6e63b
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="azure-functions-runtime-overview"></a>Übersicht über Azure Functions Runtime

Azure Functions Runtime bietet Ihnen eine neue Möglichkeit, die Einfachheit und Flexibilität des Azure Functions-Programmiermodels lokal zu nutzen. Azure Functions Runtime basiert auf den gleichen Open Source-Grundlagen wie Azure Functions, wird allerdings lokal bereitgestellt, um eine fast identische Entwicklungsumgebung wie mit dem Clouddienst zu ermöglichen.

![Portal für Azure Functions Runtime Preview][1]

Azure Functions Runtime bietet Ihnen eine Möglichkeit, mit Azure Functions zu arbeiten, bevor Sie die Cloud einsetzen. Auf diese Weise können die Codeobjekte, die Sie erstellen, bei der Migration in die Cloud übernommen werden.  Die Runtime eröffnet außerdem neue Optionen, z.B. die Nutzung überschüssiger Rechenleistung Ihrer lokalen Computer, um Batchprozesse über Nacht auszuführen. Sie können auch Geräte in Ihrer Organisation verwenden, um bedingt Daten an andere Systeme zu senden, lokal und in der Cloud.

Azure Functions Runtime besteht aus zwei Teilen:
* Azure Functions Runtime-Verwaltungsrolle
* Azure Functions Runtime-Workerrolle

## <a name="azure-functions-management-role"></a>Azure Functions-Verwaltungsrolle

Die Azure Functions-Verwaltungsrolle stellt einen Host für die lokale Verwaltung von Funktionen bereit. Diese Rolle führt die folgenden Aufgaben aus:

* Hosten des Azure Functions-Verwaltungsportals, dies entspricht dem Portal, das im [Azure-Portal](https://portal.azure.com) angezeigt wird. Dadurch können Sie die Funktionen auf die gleiche Weise wie im Azure-Portal entwickeln.
* Verteilen von Funktionen auf mehrere Functions-Worker.
* Bereitstellen eines Endpunkts für die Veröffentlichung, damit Sie Ihre Funktionen direkt über Microsoft Visual Studio veröffentlichen können.

## <a name="azure-functions-worker-role"></a>Azure Functions-Workerrolle

Die Azure Functions-Workerrollen werden in Windows-Containern bereitgestellt, und hier wird der Funktionscode ausgeführt.  Sie können in Ihrer Organisation mehrere Workerrollen bereitstellen. Dies ist eine wichtige Möglichkeit für Kunden, überschüssige Rechenleistung zu nutzen.

## <a name="minimum-requirements"></a>Mindestanforderungen

Um mit der Nutzung von Azure Functions Runtime zu beginnen, benötigen Sie einen Computer mit **Windows Server 2016 oder Windows 10 Creators Update** mit Zugriff auf eine **SQL Server**-Instanz.

## <a name="next-steps"></a>Nächste Schritte

Installieren von [Azure Functions Runtime Preview](https://aka.ms/azafr)

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png

