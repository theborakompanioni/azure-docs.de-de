---
title: Microsoft Dynamics CRM und Azure Application Insights | Microsoft-Dokumentation
description: "Abrufen von Telemetriedaten aus Microsoft Dynamics CRM Online mithilfe von Application Insights Exemplarische Vorgehensweise für das Einrichten, Abrufen von Daten, Visualisieren und Exportieren."
services: application-insights
documentationcenter: 
author: mazharmicrosoft
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 948c894cba1b8bb513a26d720cbe433b38353d03
ms.lasthandoff: 04/17/2017


---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Exemplarische Vorgehensweise: Aktivieren von Telemetriedaten für Microsoft Dynamics CRM Online mithilfe von Application Insights
Dieser Artikel beschreibt, wie Sie Telemetriedaten aus [Microsoft Dynamics CRM Online](https://www.dynamics.com/) mit [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) abrufen können. Der vollständige Vorgang des Hinzufügens eines Application Insights-Skripts zu Ihrer Anwendung, das Erfassen von Daten und die Visualisierung von Daten werden erläutert.

> [!NOTE]
> [Durchsuchen Sie die Beispielprojektmappe](https://dynamicsandappinsights.codeplex.com/).
> 
> 

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Hinzufügen von Application Insights zu einer neuen oder vorhandenen CRM Online-Instanz
Zum Überwachen Ihrer Anwendung fügen Sie Ihrer Anwendung ein Application Insights-SDK hinzu. Das SDK sendet Telemetriedaten an das [Application Insights-Portal](https://portal.azure.com), in dem Sie unsere leistungsstarken Analyse- und Diagnosetools verwenden oder die Daten in den Speicher exportieren können.

### <a name="create-an-application-insights-resource-in-azure"></a>Erstellen einer Application Insights-Ressource in Azure
1. Fordern Sie [ein Konto in Microsoft Azure](http://azure.com/pricing)an. 
2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und fügen Sie eine neue Application Insights-Ressource hinzu. Hier werden die Daten verarbeitet und angezeigt.
   
    ![Klicken Sie auf "+", "Entwicklerdienste", "Application Insights".](./media/app-insights-sample-mscrm/01.png)
   
    Wählen Sie als Anwendungstyp "ASP.NET" aus.
3. Öffnen Sie die Seite „Erste Schritte“ und dann „Überwachung und Diagnose der clientseitigen Anwendung“.
   
    ![Codeausschnitt zum Einfügen in Ihre Webseite](./media/app-insights-sample-mscrm/03.png)

**Lassen Sie die Codeseite geöffnet** , während Sie den nächsten Schritt in einem anderen Browserfenster ausführen. Sie benötigen den Code bald. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Erstellen einer JavaScript-Webressource in Microsoft Dynamics CRM
1. Öffnen Sie Ihre CRM Online-Instanz, und melden Sie sich mit Administratorrechten an.
2. Öffnen Sie „Microsoft Dynamics CRM Settings“, „Anpassungen“, „System anpassen“.
   
    ![Microsoft Dynamics CRM Settings](./media/app-insights-sample-mscrm/04.png)
   
    ![Einstellungen > Anpassungen](./media/app-insights-sample-mscrm/05.png)

    ![Option „System anpassen“](./media/app-insights-sample-mscrm/06.png)

1. Erstellen Sie eine JavaScript-Ressource.
   
    ![Dialogfeld für neue Webressource](./media/app-insights-sample-mscrm/07.png)
   
    Geben Sie ihr einen Namen, wählen Sie **Skript (JScript)** , und öffnen Sie den Text-Editor.
   
    ![Text-Editor öffnen](./media/app-insights-sample-mscrm/08.png)
2. Kopieren Sie den Code aus Application Insights. Achten Sie beim Kopieren darauf, Skript-Tags zu ignorieren. Siehe Screenshot unten:
   
    ![Instrumentationsschlüssel festlegen](./media/app-insights-sample-mscrm/09.png)
   
    Der Code enthält den Instrumentationsschlüssel, der Ihre Application Insights-Ressource identifiziert.
3. Speichern und veröffentlichen Sie die Ressource.
   
    ![Speichern und veröffentlichen](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Instrumentieren von Formularen
1. Öffnen Sie in Microsoft CRM Online das Formular "Firma".
   
    ![Konto-Formular](./media/app-insights-sample-mscrm/11.png)
2. Öffnen Sie "Formulareigenschaften".
   
    ![Formulareigenschaften](./media/app-insights-sample-mscrm/12.png)
3. Fügen Sie die JavaScript-Webressource hinzu, die Sie erstellt haben.
   
    ![Menü „Hinzufügen“](./media/app-insights-sample-mscrm/13.png)
   
    ![Die Webressource hinzufügen](./media/app-insights-sample-mscrm/14.png)
4. Speichern und veröffentlichen Sie die Formularanpassungen.

## <a name="metrics-captured"></a>Erfasste Metriken
Sie haben jetzt Telemetriedatenerfassung für das Formular eingerichtet. Immer wenn es verwendet wird, werden Daten an die Application Insights-Ressource gesendet.

Im Folgenden finden Sie Beispiele für die Daten, die Sie sehen werden.

#### <a name="application-health"></a>Anwendungsintegrität
![Beispiel für Seitenladezeit](./media/app-insights-sample-mscrm/15.png)

![Beispieldigramm für Seitenaufrufe](./media/app-insights-sample-mscrm/16.png)

Browserausnahmen:

![Diagramm der Browserausnahmen](./media/app-insights-sample-mscrm/17.png)

Klicken Sie auf das Diagramm, um weitere Details zu erhalten:

![Liste von Ausnahmen](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Verwendung
![Benutzer, Sitzungen und Seitenaufrufe](./media/app-insights-sample-mscrm/19.png)

![Sitzungsdiagramme](./media/app-insights-sample-mscrm/20.png)

![Browserversionen](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Browser
![Aufschlüsselung der Seitenladezeit](./media/app-insights-sample-mscrm/22.png)

![Anzahl der Sitzungen nach Browserversion](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Geolocation
![Anzahl der Sitzungen nach Land](./media/app-insights-sample-mscrm/24.png)

![Sitzungen und Benutzer nach Land](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Details zu Anforderungen von Seitenansichten
![Zusammenfassung der Seitenaufrufe](./media/app-insights-sample-mscrm/26.png)

![Suche in den Seitenaufrufereignissen](./media/app-insights-sample-mscrm/27.png)

![Ähnliche Seitenaufrufe](./media/app-insights-sample-mscrm/28.png)

![Eigenschaften von Seitenansichten](./media/app-insights-sample-mscrm/29.png)

![Seiten pro Sitzung](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Beispielcode
[Durchsuchen Sie den Beispielcode](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI
Sie können sogar eine tiefer gehende Analyse durchführen, wenn Sie [die Daten nach Microsoft Power BI exportieren](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Microsoft Dynamics CRM-Beispiellösung
[Hier ist die in Microsoft Dynamics CRM implementierte Beispiellösung](https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Weitere Informationen
* [Was ist Application Insights?](app-insights-overview.md)
* [Application Insights für Webseiten](app-insights-javascript.md)
* [Weitere Beispiele und exemplarische Vorgehensweisen](app-insights-code-samples.md)

