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
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 24ccafb4df95e0010416485199e19f81e1ae31aa
ms.openlocfilehash: 11017c7c0a761569892aebcd085d5d3fb2d67a69
ms.contentlocale: de-de
ms.lasthandoff: 02/14/2017


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

## <a name="about-sampling"></a>Informationen zur Stichprobenerstellung (Sampling)
Wenn für Ihre Anwendung große Datenmengen gesendet werden, wird ggf. die Funktion für die Adaptive Stichprobenerstellung verwendet und nur ein prozentualer Anteil der Telemetriedaten gesendet. Dasselbe gilt, wenn Sie die Stichprobenerstellung entweder im SDK oder bei der Erfassung manuell festgelegt haben. [Erfahren Sie mehr über das Erstellen von Stichproben.](app-insights-sampling.md)

## <a name="next-steps"></a>Nächste Schritte
* [Power BI – Informationen](http://www.powerbi.com/learning/)
* [Analytics-Tutorial](app-insights-analytics-tour.md)


