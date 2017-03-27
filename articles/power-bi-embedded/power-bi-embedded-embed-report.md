---
title: Einbetten eines in Azure Power BI Embedded befindlichen Berichts | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen in Power BI Embedded befindlichen Bericht in Ihre Anwendung einbetten.
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 3d865af2418c9c557c861a379766a125d75cebf1
ms.lasthandoff: 03/14/2017


---
# <a name="embed-a-report-in-power-bi-embedded"></a>Einbetten eines in Power BI Embedded befindlichen Berichts

Erfahren Sie, wie Sie einen in Power BI Embedded befindlichen Bericht in Ihre Anwendung einbetten.

Wir werden die Schritte zum Einbetten eines Berichts in Ihre Anwendung praktisch durchführen. Dies setzt voraus, dass Sie bereits über einen Bericht verfügen, der in einem Arbeitsbereich Ihrer Arbeitsbereichssammlung vorliegt. Wenn Sie diesen Schritt noch nicht ausgeführt haben, lesen Sie [Erste Schritte mit Microsoft Power BI Embedded](power-bi-embedded-get-started.md).

Sie können das SDK für .NET (C#) oder Node.js zusammen mit JavaScript verwenden, um Ihre Anwendung mühelos mit Power BI Embedded zu erstellen. 

## <a name="using-the-access-keys-to-use-rest-apis"></a>Verwenden von REST-APIs mithilfe von Zugriffsschlüsseln

Um die REST-API aufzurufen, können Sie den Zugriffsschlüssel übergeben, den Sie vom Azure-Portal für eine bestimmte Arbeitsbereichssammlung abrufen können. Weitere Informationen finden Sie unter [Erste Schritte mit Power BI Embedded](power-bi-embedded-get-started.md).

## <a name="get-a-report-id"></a>Abrufen einer Berichts-ID

Jedes Zugriffstoken basiert auf einem Bericht. Sie müssen die jeweilige Berichts-ID für den Bericht abrufen, den Sie einbetten möchten. Dies kann basierend auf Aufrufen der [Get Reports](https://msdn.microsoft.com/library/azure/mt711510.aspx)-REST-API erfolgen. Hiermit werden die Berichts-ID und die Einbettungs-URL zurückgegeben. Dies kann mithilfe des Power BI .NET SDK oder durch direkten Aufruf der REST-API erfolgen.

### <a name="using-the-power-bi-net-sdk"></a>Verwenden des Power BI .NET SDK

Wenn Sie das .NET SDK verwenden, müssen Sie Tokenanmeldeinformationen erstellen, die auf dem Zugriffsschlüssel basieren, den Sie aus dem Azure-Portal erhalten. Hierzu müssen Sie das [Power BI API-NuGet-Paket](https://www.nuget.org/profiles/powerbi) installieren.

**NuGet-Paketinstallation**

```
Install-Package Microsoft.PowerBI.Api
```

**C#-Code**

```
using Microsoft.PowerBI.Api.V1;
using Microsoft.Rest;

var credentials = new TokenCredentials("{access key}", "AppKey");
var client = new PowerBIClient(credentials);
client.BaseUri = new Uri(https://api.powerbi.com);

var reports = (IList<Report>)client.Reports.GetReports(workspaceCollectionName, workspaceId).Value;

// Select the report that you want to work with from the collection of reports.
```

### <a name="calling-the-rest-api-directly"></a>Direktes Aufrufen der REST-API

```
System.Net.WebRequest request = System.Net.WebRequest.Create("https://api.powerbi.com/v1.0/collections/{collectionName}/workspaces/{workspaceId}/Reports") as System.Net.HttpWebRequest;

request.Method = "GET";
request.ContentLength = 0;
request.Headers.Add("Authorization", String.Format("AppKey {0}", accessToken.Value));

using (var response = request.GetResponse() as System.Net.HttpWebResponse)
{
    using (var reader = new System.IO.StreamReader(response.GetResponseStream()))
    {
        // Work with JSON response to get the report you want to work with.
    }

}
```

## <a name="create-an-access-token"></a>Erstellen eines Zugriffstokens

Power BI Embedded verwendet Einbettungstoken, die HMAC-signierte JSON Web Token sind. Die Token werden mit dem Zugriffsschlüssel aus Ihrer Azure Power BI Embedded-Arbeitsbereichssammlung signiert. Einbettungstoken werden standardmäßig verwendet, um schreibgeschützten Zugriff auf einen Bericht zum Einbetten in eine Anwendung bereitzustellen. Einbettungstoken werden für einen bestimmten Bericht ausgegeben und sollten einer Einbettungs-URL zugeordnet werden.

Zugriffstoken sollten auf dem Server erstellt werden, da die Zugriffsschlüssel zum Signieren/Verschlüsseln von Token verwendet werden. Informationen zum Erstellen eines Zugriffstokens finden Sie unter [Authentifizieren und Autorisieren mit Power BI Embedded](power-bi-embedded-app-token-flow.md). Sie können sich auch über die [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)-Methode informieren. Das folgende Beispiel zeigt Ihnen das Vorgehen mit dem .NET SDK für Power BI.

Sie verwenden die Berichts-ID, die Sie zuvor abgerufen haben. Sobald das Einbettungstoken erstellt ist, verwenden Sie den Zugriffsschlüssel zum Generieren des Tokens, das Sie aus der JavaScript-Perspektive verwenden können. Die *PowerBIToken-Klasse* erfordert die Installation des [Power BI Core-NuGut-Pakets](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

**NuGet-Paketinstallation**

```
Install-Package Microsoft.PowerBI.Core
```

**C#-Code**

```
using Microsoft.PowerBI.Security;

// rlsUsername, roles and scopes are optional.
embedToken = PowerBIToken.CreateReportEmbedToken(workspaceCollectionName, workspaceId, reportId, rlsUsername, roles, scopes);

var token = embedToken.Generate("{access key}");
```

### <a name="adding-permission-scopes-to-embed-tokens"></a>Hinzufügen von Berechtigungsbereichen zu Einbettungstoken

Bei der Verwendung von Einbettungstoken möchten Sie vielleicht die Nutzung der Ressourcen beschränken, auf die Sie Zugriff gewähren. Aus diesem Grund können Sie ein Token mit bereichsbezogenen Berechtigungen generieren. Weitere Informationen finden Sie unter [Authentifizieren und Autorisieren mit Power BI Embedded](power-bi-embedded-app-token-flow.md#scopes).

## <a name="embed-using-javascript"></a>Einbetten mit JavaScript

Sobald Sie über Zugriffstoken und Berichts-ID verfügen, können wir den Bericht mithilfe von JavaScript einbetten. Hierzu müssen Sie das NuGet-[Power BI JavaScript-Paket](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/) installieren. Die embedUrl lautet „https://embedded.powerbi.com/appTokenReportEmbed“.

> [!NOTE]
> Mit dem [JavaScript Report Embed Sample](https://microsoft.github.io/PowerBI-JavaScript/demo/) (Beispiel zur Berichtseinbettung mit JavaScript) können Sie die Funktionalität testen. Es enthält außerdem Codebeispiele für die verschiedenen Vorgänge, die verfügbar sind.

**NuGet-Paketinstallation**

```
Install-Package Microsoft.PowerBI.JavaScript
```

**JavaScript-Code**

```
<script src="/scripts/powerbi.js"></script>
<div id="reportContainer"></div>

var embedConfiguration = {
    type: 'report',
    accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
    id: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed'
};

var $reportContainer = $('#reportContainer');
var report = powerbi.embed($reportContainer.get(0), embedConfiguration);
```

### <a name="set-the-size-of-embedded-elements"></a>Festlegen der Größe eingebetteter Elemente

Der Bericht wird automatisch basierend auf der Größe seines Containers eingebettet. Um die Standardgröße der Einbettungen zu überschreiben, fügen Sie einfach ein CSS-Klassenattribut oder Inlineformatvorlagen für Breite und Höhe hinzu.

## <a name="see-also"></a>Weitere Informationen

[Erste Schritte mit dem Beispiel](power-bi-embedded-get-started-sample.md)  
[Authentifizieren und Autorisieren in Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[JavaScript-Einbettungsbeispiel](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI JavaScript-Paket](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
[Power BI API-NuGet-Paket](https://www.nuget.org/profiles/powerbi)
[Power BI Core-NuGut-Paket](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[PowerBI-CSharp Git-Repository](https://github.com/Microsoft/PowerBI-CSharp)  
[PowerBI-Node Git-Repository](https://github.com/Microsoft/PowerBI-Node)  
Weitere Fragen? [Power BI-Community ausprobieren](http://community.powerbi.com/)

