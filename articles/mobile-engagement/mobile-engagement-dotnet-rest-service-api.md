---
title: Zugreifen auf Azure Mobile Engagement-Dienst-APIs mit der REST-API
description: "Beschreibt, wie die Mobile Engagement-REST-APIs für den Zugriff auf Azure Mobile Engagement-Dienst-APIs verwendet werden kann."
services: mobile-engagement
documentationcenter: mobile
author: wesmc7777
manager: erikre
editor: 
ms.assetid: e8df4897-55ee-45df-b41e-ff187e3d9d12
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.translationtype: Human Translation
ms.sourcegitcommit: 8f987d079b8658d591994ce678f4a09239270181
ms.openlocfilehash: e381eef22b68e89d5407fb452c05eba089b98780
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="using-rest-to-access-azure-mobile-engagement-service-apis"></a>Zugreifen auf Azure Mobile Engagement-Dienst-APIs mit REST
Azure Mobile Engagement stellt die [Azure Mobile Engagement-REST-API](https://msdn.microsoft.com/library/azure/mt683754.aspx) zur Verfügung, mit der Sie Geräte, Reichweiten-/Pushkampagnen usw. verwalten können.

Wenn Sie die REST-APIs nicht direkt verwenden möchten, stellen wir Ihnen auch eine [Swagger-Datei](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) bereit, die Sie mit Tools zum Generieren von SDKs für Ihre bevorzugte Sprache verwenden können. Wir empfehlen die Verwendung des Tools [AutoRest](https://github.com/Azure/AutoRest) , um Ihr SDK aus unserer Swagger-Datei zu generieren. Auf ähnliche Weise haben wir ein .NET SDK erstellt, das Ihnen die Interaktion mit diesen APIs mit einem C#-Wrapper erlaubt, ohne dass Sie das Authentifizierungstoken selbst aushandeln und aktualisieren müssen. Weitere Informationen zur Verwendung des .NET SDK für die API finden Sie unter [.NET SDK-Beispiel für die Dienst-API](mobile-engagement-dotnet-sdk-service-api.md).

