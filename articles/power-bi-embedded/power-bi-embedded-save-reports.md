---
title: Speichern von in Azure Power BI Embedded befindlichen Berichten | Microsoft-Dokumentation
description: "Lernen Sie, wie Sie in Power BI befindliche Berichte speichern. Um dies erfolgreich durchzuführen, benötigen Sie die entsprechenden Berechtigungen."
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
ms.openlocfilehash: ad895004cc2972f2ded81566186325a16d401151
ms.lasthandoff: 03/14/2017

---
# <a name="save-reports-in-power-bi-embedded"></a>Speichern von in Power BI Embedded befindlichen Berichten

Lernen Sie, wie Sie in Power BI befindliche Berichte speichern. Um dies erfolgreich durchzuführen, benötigen Sie die entsprechenden Berechtigungen.

In Power BI Embedded können Sie vorhandene Berichte bearbeiten und speichern. Sie können auch einen neuen Bericht erstellen und als neuen Bericht speichern, um einen Bericht zu erstellen.

Zum Speichern eines Berichts müssen Sie zunächst ein Token für den entsprechenden Bericht mit den richtigen Bereichen erstellen:

* Zum Aktivieren von „Speichern“ ist der Bereich „Report.ReadWrite“ erforderlich.
* Zum Aktivieren von „Speichern unter“ sind die Bereiche „Report.Read“ und „Workspace.Report.Copy“ erforderlich.
* Zum Aktivieren von „Speichern“ und „Speichern unter“ sind die Bereiche „Report.ReadWrite“ und „Workspace.Report.Copy“ erforderlich.

Um jeweils die richtigen Schaltflächen für „Speichern/Speichern unter“ im Dateimenü zu aktivieren, müssen Sie die richtige Berechtigung in der Einbettungskonfiguration angeben, wenn Sie den Bericht einbetten:

* models.Permissions.ReadWrite
* models.Permissions.Copy
* models.Permissions.All

> [!NOTE]
> Ihr Zugriffstoken benötigt auch die entsprechenden Bereiche. Weitere Informationen finden Sie unter [Authentifizieren und Autorisieren mit Power BI Embedded](power-bi-embedded-app-token-flow.md#scopes).

## <a name="embed-report-in-edit-mode"></a>Einbetten von Berichten im Bearbeitungsmodus

Nehmen wir an, dass Sie einen Bericht im Bearbeitungsmodus in Ihre App einbetten möchten. Sie übergeben einfach nur die richtigen Eigenschaften an die Einbettungskonfiguration und rufen „powerbi.embed()“ auf. Sie müssen Berechtigungen und einen viewMode (Ansichtsmodus) angeben, um im Bearbeitungsmodus die „Speichern“- und „Speichern unter“-Schaltfläche anzuzeigen. Weitere Informationen finden Sie unter [Embed configuration details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details) (Details der Einbettungskonfiguration).

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
        permissions: models.Permissions.All /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.Edit,
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

Jetzt wird ein Bericht im Bearbeitungsmodus in Ihre App eingebettet.

## <a name="save-report"></a>Bericht speichern

Nach dem Einbetten des Berichts im Bearbeitungsmodus mit dem richtigen Token und den richtigen Berechtigungen können Sie den Bericht über Dateimenü oder JavaScript speichern:

```
 // Get a reference to the embedded report.
    report = powerbi.get(reportContainer);

 // Save report
    report.save();
```

## <a name="save-as"></a>Speichern unter

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
> Ein neuer Bericht wird erst nach *Speichern unter* erstellt. Nach dem Speichern wird in der Canvas weiterhin der alte Bericht im Bearbeitungsmodus und nicht der neue Bericht angezeigt. Sie müssen den neuen Bericht, der erstellt wurde, einbetten. Dies erfordert ein neues Zugriffstoken, da dieses pro Bericht erstellt wird.

Sie müssen dann den neuen Bericht nach *Speichern unter* laden. Dies ähnelt dem Einbetten eines beliebigen Berichts.

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

## <a name="see-also"></a>Weitere Informationen

[Erste Schritte mit dem Beispiel](power-bi-embedded-get-started-sample.md)  
[Einbetten eines Berichts](power-bi-embedded-embed-report.md)  
[Erstellen eines neuen Berichts aus einem Dataset](power-bi-embedded-create-report-from-dataset.md)  
[Authentifizieren und Autorisieren in Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript-Einbettungsbeispiel](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
Weitere Fragen? [Power BI-Community ausprobieren](http://community.powerbi.com/)


