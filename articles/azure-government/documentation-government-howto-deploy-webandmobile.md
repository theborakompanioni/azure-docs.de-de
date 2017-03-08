---
title: Bereitstellen in Azure App Service mithilfe von Visual Studio 2015 | Microsoft Docs
description: Dieser Artikel beschreibt, wie Web-Apps, API-Apps oder mobile Apps mithilfe von Visual Studio 2015 und dem Azure SDK in Azure Government bereitgestellt werden.
services: azure-government
cloud: gov
documentationcenter: 
author: sdubeymsft
manager: zakramer
ms.assetid: 8f9a3700-b9ee-43b7-b64d-2e6c3b57d4c0
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 01/03/2016
ms.author: sdubeymsft
translationtype: Human Translation
ms.sourcegitcommit: 1cb57e5156dab976599ddfa9a58f26ca8ef1ee0e
ms.openlocfilehash: 159c891dedb66dfaa238981b395b4bba074c5716
ms.lasthandoff: 02/22/2017


---
# <a name="deploy-to-azure-app-services-using-visual-studio-2015"></a>Bereitstellen in Azure App Service mithilfe von Visual Studio 2015
Dieser Artikel beschreibt, wie Azure App Service-Apps (API-Apps, Web-Apps, mobile Apps) mithilfe von Visual Studio 2015 in Azure Government bereitgestellt werden.

## <a name="prerequisites"></a>Voraussetzungen
* Informationen zum Installieren und Konfigurieren von Visual Studio 2015 und Azure SDK finden Sie unter [Voraussetzungen für Visual Studio] (../app-service-api/app-service-api-dotnet-get-started.md#prerequisites).
* Führen Sie [diese Anweisungen] (documentation-government-manage-subscriptions.md) aus, um Visual Studio für die Verbindung mit einem Azure Government-Konto zu konfigurieren.

## <a name="open-app-project-in-visual-studio"></a>Öffnen des App-Projekts in Visual Studio
* Öffnen Sie die App-Lösung bzw. das App-Projekt in Visual Studio, erstellen Sie anhand [dieser Anweisungen] (../app-service-web/web-sites-dotnet-get-started.md#create-a-web-application) ein Projekt, oder laden Sie die Beispiel-App gemäß [dieser Schritte] (../app-service-api/app-service-api-dotnet-get-started.md#download-the-sample-application) herunter.
* Führen Sie die App in Visual Studio aus, um sicherzustellen, dass sie lokal funktioniert.

## <a name="deploy-to-azure-government"></a>Bereitstellen in Azure Government
* Nachdem **Visual Studio für die Verbindung mit einem Azure Government-Konto** konfiguriert wurde (bereits im Abschnitt „Voraussetzungen“ erfolgt), sind die Anweisungen für das Bereitstellen in App Services identisch mit denen für Azure Public.
* Befolgen Sie zum Bereitstellen der App [diese Schritte] (../app-service-api/app-service-api-dotnet-get-started.md#a-idcreateapiappa-create-an-api-app-in-azure-and-deploy-code-to-it).

### <a name="references"></a>Referenzen
* [Bereitstellen einer ASP.NET-Web-App in Azure App Service mit Visual Studio] (../app-service-web/web-sites-dotnet-get-started.md)
* Andere Bereitstellungsarten finden Sie unter [Bereitstellen der App in Azure App Service] (../app-service-web/web-sites-deploy.md).
* Die allgemeine App Services-Dokumentation finden Sie unter [App Service – API-Apps-Dokumentation] (../app-service-api/index.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den [Microsoft Azure Government-Blog](https://blogs.msdn.microsoft.com/azuregov/) abonnieren.

