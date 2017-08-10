---
title: Exportieren nach Power BI aus Application Insights | Microsoft Docs
description: "Analytics-Abfragen können in Power BI angezeigt werden."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: sewhee
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 02c51e6a576b5a91044eae784c72d7529497b814
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017

---
# <a name="feed-power-bi-from-application-insights"></a>Datenimport nach Power BI aus Application Insights
[Power BI](http://www.powerbi.com/) ist eine Suite aus Business Analytics-Tools zum Analysieren von Daten und Teilen von Einblicken. Auf jedem Gerät stehen leistungsfähige Dashboards zur Verfügung. Sie können Daten aus vielen Quellen kombinieren, z.B. Analytics-Abfragen aus [Azure Application Insights](app-insights-overview.md).

Es gibt drei empfohlene Methoden zum Exportieren von Application Insights-Daten nach Power BI. Diese können Sie separat oder zusammen verwenden.

* [**Power BI-Adapter**](#power-pi-adapter): Richten Sie ein vollständiges Dashboard mit den Telemetriedaten Ihrer App ein. Die Gruppe von Diagrammen ist vordefiniert, aber Sie können auch eigene Abfragen aus anderen Quellen hinzufügen.
* [**Exportieren von Analytics-Abfragen**](#export-analytics-queries): Schreiben Sie mit Analytics eine beliebige Abfrage, und exportieren Sie sie nach Power BI. Sie können diese Abfrage zusammen mit anderen Daten in einem Dashboard anordnen.
* [**Fortlaufender Export und Stream Analytics**](app-insights-export-stream-analytics.md): Hierfür fällt mehr Einrichtungsaufwand an. Dies ist hilfreich, wenn Sie Ihre Daten über längere Zeiträume aufbewahren möchten. Andernfalls sind eher die anderen Methoden zu empfehlen.

## <a name="power-bi-adapter"></a>Power BI-Adapter
Mit dieser Methode wird für Sie ein vollständiges Dashboard mit Telemetriedaten erstellt. Das anfängliche Dataset ist vordefiniert, aber Sie können weitere Daten hinzufügen.

### <a name="get-the-adapter"></a>Abrufen des Adapters
1. Melden Sie sich bei [Power BI](https://app.powerbi.com/) an.
2. Öffnen Sie **Get Data** (Daten abrufen), **Services** (Dienste), **Application Insights**.
   
    ![Abrufen aus der Application Insights-Datenquelle](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. Geben Sie die Details Ihrer Application Insights-Ressource an.
   
    ![Abrufen aus der Application Insights-Datenquelle](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. Warten Sie ein oder zwei Minuten, bis die Daten importiert wurden.
   
    ![Power BI-Adapter](./media/app-insights-export-power-bi/010.png)

Sie können das Dashboard bearbeiten und die Application Insights-Diagramme mit den Diagrammen anderer Quellen und mit Analytics-Abfragen kombinieren. Es gibt einen Katalog mit visuellen Elementen, in dem weitere Diagramme bereitgestellt werden. Jedes dieser Diagramme verfügt über Parameter, die Sie festlegen können.

Nach dem anfänglichen Import werden das Dashboard und die Berichte täglich aktualisiert. Sie können den Aktualisierungszeitplan im Dataset steuern.

## <a name="export-analytics-queries"></a>Exportieren von Analytics-Abfragen
Mit dieser Route können Sie eine beliebige Analytics-Abfrage schreiben und dann in ein Power BI-Dashboard exportieren. (Sie können dem mit dem Adapter erstellten Dashboard Elemente hinzufügen.)

### <a name="one-time-install-power-bi-desktop"></a>Einmalig: Installieren von Power BI Desktop
Zum Importieren Ihrer Application Insights-Abfrage verwenden Sie die Desktopversion von Power BI. Diese können Sie dann aber im Web oder in Ihrem Power BI-Cloudarbeitsbereich veröffentlichen. 

Installieren Sie [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Exportieren einer Analytics-Abfrage
1. [Öffnen Sie Analytics, und schreiben Sie Ihre Abfrage](app-insights-analytics-tour.md).
2. Testen und optimieren Sie die Abfrage, bis Sie mit den Ergebnissen zufrieden sind.

   **Stellen Sie sicher, dass die Abfrage ordnungsgemäß in Analytics ausgeführt wird, bevor Sie sie exportieren.**
3. Wählen Sie im Menü **Exportieren** die Option **Power BI (M)**. Speichern Sie die Textdatei.
   
    ![Exportieren der Power BI-Abfrage](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. Wählen Sie in Power BI Desktop die Option **Get Data (Daten abrufen) > Leere Abfrage**, und wählen Sie anschließend im Abfrage-Editor unter **Ansicht** die Option **Advanced Query Editor** (Editor für erweiterte Abfragen).

    Fügen Sie das exportierte M-Sprachskript in den Editor für erweiterte Abfragen ein.

    ![Editor für erweiterte Abfragen](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. Unter Umständen müssen Sie Anmeldeinformationen angeben, um für Power BI den Zugriff auf Azure zuzulassen. Verwenden Sie die Option „Organisationskonto“, um sich mit Ihrem Microsoft-Konto anzumelden.
   
    ![Angeben von Azure-Anmeldeinformationen zum Zulassen der Ausführung von Application Insights-Abfragen für Power BI](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    (Wenn Sie die Anmeldeinformationen überprüfen müssen, verwenden Sie den Menübefehl „Datenquelleneinstellungen“ im Abfrage-Editor. Sie müssen unbedingt Ihre Anmeldeinformationen für Azure verwenden, die sich von denen für Power BI möglicherweise unterscheiden.)
2. Wählen Sie eine Visualisierung für Ihre Abfrage aus, und wählen Sie die Felder für x-Achse, y-Achse und Segmentierungsdimension.
   
    ![Auswählen der Visualisierung](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. Veröffentlichen Sie den Bericht in Ihrem Power BI-Cloudarbeitsbereich. Von hier aus können Sie eine synchronisierte Version in andere Webseiten einbetten.
   
    ![Auswählen der Visualisierung](./media/app-insights-export-power-bi/publish-power-bi.png)
4. Aktualisieren Sie den Bericht in bestimmten Abständen manuell, oder richten Sie auf der Seite mit den Optionen eine geplante Aktualisierung ein.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="401-or-403-unauthorized"></a>401 oder 403: Nicht autorisiert 
Dies kann geschehen, wenn Ihr Aktualisierungstoken nicht aktualisiert wurde. Probieren Sie diese Schritte aus, um sicherzustellen, dass Sie weiterhin Zugriff haben. Wenn Sie Zugriff haben und das Aktualisieren der Anmeldeinformationen nicht funktioniert, öffnen Sie ein Supportticket.

1. Melden Sie sich beim Azure-Portal an, und stellen Sie sicher, dass Sie auf die Ressource zugreifen können.
2. Versuchen Sie, die Anmeldeinformationen für das Dashboard zu aktualisieren.

### <a name="502-bad-gateway"></a>502 Ungültiges Gateway
Dies wird normalerweise durch eine Analytics-Abfrage verursacht, die zu viele Daten zurückgibt. Probieren Sie einen kleineren Zeitbereich oder die Funktionen [ago](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) oder [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek), um nur die Felder zu [prognostizieren](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator), die Sie benötigen.

Wenn das Verkleinern des von der Analytics-Abfrage stammenden Datasets Ihre Anforderungen nicht erfüllt, sollten Sie erwägen, mithilfe der [API](https://dev.applicationinsights.io/documentation/overview) ein größeres Dataset abzurufen. Es folgen Anweisungen zum Konvertieren des M-Query-Exports zum Verwenden der API.

1. Erstellen Sie einen [API-Schlüssel](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).
2. Aktualisieren Sie das M-Skript von Power BI, das Sie aus Analytics exportiert werden, indem Sie die ARM-URL durch die AI-API ersetzen (siehe folgendes Beispiel).
   * Ersetzen Sie **https://management.azure.com/subscriptions/...**
   * durch **https://api.applicationinsights.io/beta/apps/...**
3. Aktualisieren Sie schließlich die Anmeldeinformationen in „basic“, und verwenden Sie Ihren API-Schlüssel.
  

**Vorhandenes Skript**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Aktualisiertes Skript**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>Informationen zur Stichprobenerstellung (Sampling)
Wenn für Ihre Anwendung große Datenmengen gesendet werden, wird ggf. die Funktion für die Adaptive Stichprobenerstellung verwendet und nur ein prozentualer Anteil der Telemetriedaten gesendet. Dasselbe gilt, wenn Sie die Stichprobenerstellung entweder im SDK oder bei der Erfassung manuell festgelegt haben. [Erfahren Sie mehr über das Erstellen von Stichproben.](app-insights-sampling.md)


## <a name="next-steps"></a>Nächste Schritte
* [Power BI – Informationen](http://www.powerbi.com/learning/)
* [Analytics-Tutorial](app-insights-analytics-tour.md)


