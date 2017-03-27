---
title: "Wechseln zwischen Ansichts- und Bearbeitungsmodus für Berichte in Azure Power BI Embedded | Microsoft-Dokumentation"
description: "Lernen Sie, wie Sie zwischen Ansichts- und Bearbeitungsmodus für Berichte in Power BI Embeddedd wechseln."
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
ms.openlocfilehash: f73bf05b41523a5833cc9366fb84cb7021b4b7a9
ms.lasthandoff: 03/14/2017

---
# <a name="toggle-between-view-and-edit-mode-for-reports-in-power-bi-embedded"></a>Wechseln zwischen Ansichts- und Bearbeitungsmodus für Berichte in Power BI Embedded

Lernen Sie, wie Sie zwischen Ansichts- und Bearbeitungsmodus für Berichte in Power BI Embeddedd wechseln.

## <a name="creating-an-access-token"></a>Erstellen eines Zugriffstokens

Sie müssen ein Zugriffstoken erstellen, das Ihnen ermöglicht, einen Bericht sowohl anzuzeigen als auch zu bearbeiten. Zum Bearbeiten und Speichern eines Berichts benötigen Sie die Tokenberechtigung **Report.ReadWrite**. Weitere Informationen finden Sie unter [Authentifizieren und Autorisieren in Power BI Embedded](power-bi-embedded-app-token-flow.md).

> [!NOTE]
> Dies ermöglicht Bearbeiten und Speichern von Änderungen eines vorhandenen Berichts. Wenn Sie auch **Speichern unter** unterstützen möchten, müssen Sie zusätzliche Berechtigungen angeben. Weitere Informationen finden Sie unter [Authentifizieren und Autorisieren mit Power BI Embedded](power-bi-embedded-app-token-flow.md#scopes).

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Report.ReadWrite";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="embed-configuration"></a>Einbettungskonfiguration

Sie müssen Berechtigungen und einen viewMode (Ansichtsmodus) angeben, um im Bearbeitungsmodus die „Speichern“-Schaltfläche anzuzeigen. Weitere Informationen finden Sie unter [Embed configuration details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details) (Details der Einbettungskonfiguration).

Beispielsweise in JavaScript:

```
   <div id="reportContainer"></div>

    // Get models. Models, it contains enums that can be used.
    var models = window['powerbi-client'].models;

    // Embed configuration used to describe the what and how to embed.
    // This object is used when calling powerbi.embed.
    // This also includes settings and options such as filters.
    // You can find more information at https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details.
    var config= {
        type: 'report',
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        id:  '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
        permissions: models.Permissions.ReadWrite /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.View,
        settings: {
            filterPaneEnabled: true,
            navContentPaneEnabled: true
        }
    };

    // Get a reference to the embedded report HTML element
    var reportContainer = $('#reportContainer')[0];

    // Embed the report and display it within the div container.
    var report = powerbi.embed(reportContainer, config);
```

Dies gibt an, den Bericht basierend darauf, dass **viewMode** auf **models.ViewMode.View** festgelegt wird, im Ansichtsmodus einzubetten.

## <a name="view-mode"></a>Ansichtsmodus

Sie können den folgenden JavaScript-Code zum Wechsel in den Ansichtsmodus verwenden, wenn Sie sich im Bearbeitungsmodus befinden.

```
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to view mode.
report.switchMode("view");

```

## <a name="edit-mode"></a>Bearbeitungsmodus

Sie können den folgenden JavaScript-Code zum Wechsel in den Bearbeitungsmodus verwenden, wenn Sie sich im Ansichtsmodus befinden.

```
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to edit mode.
report.switchMode("edit");

```

## <a name="see-also"></a>Weitere Informationen

[Erste Schritte mit dem Beispiel](power-bi-embedded-get-started-sample.md)  
[Einbetten eines Berichts](power-bi-embedded-embed-report.md)  
[Authentifizieren und Autorisieren in Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[JavaScript-Einbettungsbeispiel](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[PowerBI-CSharp Git-Repository](https://github.com/Microsoft/PowerBI-CSharp)  
[PowerBI-Node Git-Repository](https://github.com/Microsoft/PowerBI-Node)  
Weitere Fragen? [Power BI-Community ausprobieren](http://community.powerbi.com/)

