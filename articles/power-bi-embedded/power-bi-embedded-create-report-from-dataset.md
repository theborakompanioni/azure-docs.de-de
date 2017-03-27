---
title: Erstellen eines neuen Berichts aus einem Dataset in Azure Power BI Embedded | Microsoft-Dokumentation
description: "Power BI Embedded-Berichte können jetzt aus einem Dataset in Ihrer eigenen Anwendung erstellt werden."
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
ms.openlocfilehash: 457f53aa76059dbb2faed6b264102f1f59b9918a
ms.lasthandoff: 03/14/2017

---
# <a name="create-a-new-report-from-a-dataset-in-power-bi-embedded"></a>Erstellen eines neuen Berichts aus einem Dataset in Power BI Embedded

Power BI Embedded-Berichte können jetzt aus einem Dataset in Ihrer eigenen Anwendung erstellt werden. 

Die Authentifizierungsmethode ähnelt dem Einbetten von Berichten. Sie basiert auf Zugriffstoken, die für ein Dataset spezifisch sind. Für „PowerBI.com“ verwendete Token werden von Azure Active Directory (AAD) ausgestellt und Power BI Embedded-Token von Ihrem eigenen Dienst.

Beim Erstellen eines Embedded-Berichts sind die ausgestellten Token für ein bestimmtes Dataset bestimmt. Token sollten der Einbettungs-URL auf dem gleichen Element zugeordnet sein, um sicherzustellen, dass jedes ein eindeutiges Token hat. Um einen Embedded-Bericht zu erstellen, müssen die Bereiche *Dataset.Read und Workspace.Report.Create* im Zugriffstoken bereitgestellt werden.

## <a name="create-access-token-needed-to-create-new-report"></a>Erstellen von Zugriffstoken, die zum Erstellen des neuen Berichts erforderlich sind

Power BI Embedded verwendet Einbettungstoken, die HMAC-signierte JSON Web Token sind. Die Token werden mit dem Zugriffsschlüssel aus Ihrer Azure Power BI Embedded-Arbeitsbereichssammlung signiert. Einbettungstoken werden standardmäßig verwendet, um schreibgeschützten Zugriff auf einen Bericht zum Einbetten in eine Anwendung bereitzustellen. Einbettungstoken werden für einen bestimmten Bericht ausgegeben und sollten einer Einbettungs-URL zugeordnet werden.

Zugriffstoken sollten auf dem Server erstellt werden, da die Zugriffsschlüssel zum Signieren/Verschlüsseln von Token verwendet werden. Informationen zum Erstellen eines Zugriffstokens finden Sie unter [Authentifizieren und Autorisieren mit Power BI Embedded](power-bi-embedded-app-token-flow.md). Sie können sich auch über die [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)-Methode informieren. Das folgende Beispiel zeigt Ihnen das Vorgehen mit dem .NET SDK für Power BI.

In diesem Beispiel geben wir unsere Dataset-ID an, zu der wir den neuen Bericht erstellen möchten. Wir müssen auch die Bereiche für *Dataset.Read und Workspace.Report.Create* hinzufügen.

Die *PowerBIToken-Klasse* erfordert die Installation des [Power BI Core-NuGut-Pakets](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

**NuGet-Paketinstallation**

```
Install-Package Microsoft.PowerBI.Core
```

**C#-Code**

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Dataset.Read Workspace.Report.Create";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="create-a-new-blank-report"></a>Erstellen eines neuen leeren Berichts

Zum Erstellen eines neuen Berichts muss die Erstellungskonfiguration bereitgestellt werden. Dazu gehören Zugriffstoken, embedURL und datasetID, auf deren Basis wir den Bericht erstellen möchten. Hierzu müssen Sie das NuGet-[Power BI JavaScript-Paket](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/) installieren. Die embedUrl lautet „https://embedded.powerbi.com/appTokenReportEmbed“.

> [!NOTE]
> Mit dem [JavaScript Report Embed Sample](https://microsoft.github.io/PowerBI-JavaScript/demo/) (Beispiel zur Berichtseinbettung mit JavaScript) können Sie die Funktionalität testen. Es enthält außerdem Codebeispiele für die verschiedenen Vorgänge, die verfügbar sind.

**NuGet-Paketinstallation**

```
Install-Package Microsoft.PowerBI.JavaScript
```

**JavaScript-Code**

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);
```

Beim Aufrufen von *powerbi.createReport()* wird im *div*-Element eine leere Canvas im Bearbeitungsmodus angezeigt.

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-create-new-report.png)

## <a name="save-new-reports"></a>Speichern von neuen Berichten

Der Bericht wird erst dann erstellt, wenn Sie den **Speichern unter**-Vorgang aufrufen. Dies kann über das Menü „Datei“ oder JavaScript erfolgen.

```
 // Get a reference to the embedded report.
    report = powerbi.get(reportContainer);
    
    var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);
```

> [!IMPORTANT]
> Ein neuer Bericht wird erst nach dem Aufruf von **Speichern unter** erstellt. Nach dem Speichern wird in der Canvas weiterhin das Dataset im Bearbeitungsmodus und nicht der Bericht angezeigt. Sie müssen den neuen Bericht wie jeden anderen Bericht neu laden.

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-save-new-report.png)

## <a name="load-the-new-report"></a>Laden des neuen Berichts

Zur Interaktion mit dem neuen Bericht müssen Sie ihn in gleicher Weise einbetten, wie die Anwendung einen regulären Bericht einbettet, d.h. ein neues Token muss speziell für den neuen Bericht ausgestellt werden, und rufen Sie dann die Einbettungsmethode auf.

```
<div id="reportContainer"></div>
  
var embedConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MJ',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: '5dac7a4a-4452-46b3-99f6-a25915e0fe54',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
```

## <a name="automate-save-and-load-of-a-new-report-using-the-saved-event"></a>Automatisches Speichern und Laden eines neuen Berichts mithilfe des „saved“-Ereignisses

Zum Automatisieren des „Speichern unter“-Prozesses und anschließenden Laden des neuen Berichts können Sie das „saved“-Ereignis verwenden. Dieses Ereignis wird ausgelöst, wenn der Speichervorgang abgeschlossen ist, und es gibt ein JSON-Objekt zurück, das die neue reportId, den Berichtsnamen und die alte reportId (falls vorhanden) enthält, sowie die Information, ob ein „saveAs“- oder „save“-Vorgang stattgefunden hat.

```
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

Um den Vorgang zu automatisieren, können Sie auf das „saved“-Ereignis lauschen, die neue reportId übernehmen, das neue Token erstellen und den neuen Bericht damit einbetten.

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);


   var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);

    // report.on will add an event handler which prints to Log window.
    report.on("saved", function(event) {
        
         // get new Token
         var newReportId =  event.detail.reportObjectId;

        // create new Token. This is a function that the application should provide
        var newToken = createAccessToken(newReportId,scopes /*provide the wanted scopes*/);
        
        
    var embedConfiguration = {
        accessToken: newToken ,
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: newReportId,
    };

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
       
   // report.off removes a given event handler if it exists.
   report.off("saved");
    });
```

## <a name="see-also"></a>Weitere Informationen

[Erste Schritte mit dem Beispiel](power-bi-embedded-get-started-sample.md)  
[Speichern von Berichten](power-bi-embedded-save-reports.md)  
[Einbetten eines Berichts](power-bi-embedded-embed-report.md)  
[Authentifizieren und Autorisieren in Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript-Einbettungsbeispiel](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI Core-NuGut-Paket](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[Power BI JavaScript-Paket](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
Weitere Fragen? [Power BI-Community ausprobieren](http://community.powerbi.com/)
