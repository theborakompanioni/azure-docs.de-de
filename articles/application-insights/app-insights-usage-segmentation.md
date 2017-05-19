---
title: Benutzer-, Sitzungs- und Ereignisanalyse in Azure Application Insights | Microsoft-Dokumentation
description: Demografische Analyse der Benutzer Ihrer Web-App.
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 912e3e25680034828aee93d23c6a725c2cec0d45
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Benutzer-, Sitzungs- und Ereignisanalyse in Azure Application Insights

Ermitteln Sie, wann Personen Ihre Web-App verwenden, für welche Seiten sie sich am meisten interessieren, wo sich die Benutzer befinden, welche Browser und Betriebssysteme sie verwenden. Analysieren Sie Geschäfts- und Nutzungstelemetriedaten mithilfe von [Azure Application Insights](app-insights-overview.md).

## <a name="get-started"></a>Erste Schritte

Wenn Sie im Application Insights-Portal auf den Blättern „Benutzer“, „Sitzungen“ und „Ereignissen“ noch keine Daten sehen, [erfahren Sie hier mehr zum Einstieg in die Nutzungstools](app-insights-usage-overview.md).

## <a name="the-users-sessions-and-events-segmentation-tool"></a>Das Segmentierungstool für Benutzer, Sitzungen und Ereignisse

Drei der Nutzungsblätter verwenden dasselbe Tool zum Aufteilen von Telemetriedaten aus Ihrer Web-App aus drei Perspektiven. Durch Filtern und Aufteilen der Daten gewinnen Sie Einblicke in die relative Nutzung verschiedener Seiten und Funktionen.

* **Benutzertool**: Wie viele Personen haben Ihre App und zugehörige Funktionen verwendet?  Benutzer werden mithilfe von anonymen, in Browsercookies gespeicherten IDs gezählt. Eine einzelne Person, die verschiedene Browser oder Computer benutzt, wird als mehrere Benutzer gezählt.
* **Sitzungstool**: Bei wie vielen Benutzeraktivitätssitzungen wurden bestimmte Seiten und Funktionen der App verwendet? Eine Sitzung wird nach einer halben Stunde der Benutzerinaktivität oder nach 24 Stunden ununterbrochener Nutzung gezählt.
* **Ereignistool**: Wie oft werden bestimmte Seiten und Funktionen der App verwendet? Eine Seitenansicht wird gezählt, wenn eine Seite Ihrer App in einem Browser geladen wird, vorausgesetzt, Sie haben sie [instrumentiert](app-insights-javascript.md). 

    Ein benutzerdefiniertes Ereignis stellt eine Aktion in Ihrer App dar, häufig eine Benutzerinteraktion wie das Klicken auf eine Schaltfläche oder den Abschluss einer Aufgabe. Sie fügen Code in Ihre App ein, um [benutzerdefinierte Ereignisse zu generieren](app-insights-api-custom-events-metrics.md#trackevent).

![Nutzungstool](./media/app-insights-usage-segmentation/users.png)

## <a name="querying-for-certain-users"></a>Abfragen für bestimmte Benutzer 

Untersuchen Sie verschiedene Benutzergruppen, indem Sie die Abfrageoptionen oben im Benutzertools anpassen: 

* Verwendet von: Wählen Sie benutzerdefinierte Ereignisse und Seitenansichten. 
* Während: Wählen Sie einen Zeitbereich. 
* Von: Wählen Sie, wie Datenbuckets erstellt werden, entweder anhand einer Zeitspanne oder einer anderen Eigenschaft, z.B. Browser oder Ort. 
* Teilen nach: Wählen Sie eine Eigenschaft, anhand derer die Daten aufgeteilt oder segmentiert werden. 
* Filter hinzufügen: Beschränken Sie die Abfrage auf bestimmte Benutzer, Sitzungen oder Ereignisse basierend auf deren Eigenschaften, z.B. Browser oder Ort. 
 
## <a name="saving-and-sharing-reports"></a>Speichern und Freigeben von Berichten 
Sie können Benutzerberichte entweder privat nur für Sie selbst im Abschnitt „Meine Berichte“ oder im Abschnitt „Freigegebene Berichte“ für alle Benutzer speichern, die Zugriff auf diese Application Insights-Ressource haben.  
 
Wählen Sie beim Speichern eines Berichts oder beim Bearbeiten seiner Eigenschaften die Option „Aktueller relativer Zeitbereich“ aus, damit der gespeicherte Bericht rückwirkend für einen bestimmten Zeitraum fortlaufend Daten aktualisiert.  
 
Wählen Sie „Aktueller absoluter Zeitbereich“ um einen Bericht mit einem festen Satz von Daten zu speichern. Beachten Sie, dass Daten in Application Insights nur 90 Tage gespeichert werden, d.h. ein Bericht mit absolutem Zeitbereich wird nach mehr als 90 Tagen nach der Speicherung leer angezeigt. 
 
## <a name="example-instances"></a>Beispielinstanzen

Der Abschnitt mit Beispielinstanzen zeigt Informationen zu einigen einzelnen Benutzern, Sitzungen oder Ereignissen, die der aktuellen Abfrage entsprechen. Das Berücksichtigen und Untersuchen des Verhaltens einzelner Personen neben Aggregaten kann Erkenntnisse über die tatsächliche Verwendung Ihrer App durch Benutzer bieten. 
 
## <a name="insights"></a>Erkenntnisse 

Die Insights-Randleiste zeigt große Cluster von Benutzern, die gemeinsame Eigenschaften aufweisen. Diese Cluster können überraschende Trends bei der Verwendung Ihrer App durch Benutzer aufdecken. Beispielsweise, dass 40% der gesamten Nutzung Ihrer App auf Personen entfallen, die eine einzelne Funktion verwenden.  


## <a name="next-steps"></a>Nächste Schritte

* [Nutzungsübersicht](app-insights-usage-overview.md)
* [Aufbewahrung](app-insights-usage-retention.md)
* [Codierung benutzerdefinierte Ereignisse](app-insights-api-custom-events-metrics.md)


