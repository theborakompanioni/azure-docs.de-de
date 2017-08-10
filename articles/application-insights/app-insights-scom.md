---
title: SCOM-Integration in Application Insights | Microsoft Docs
description: "Nutzen Sie als SCOM-Benutzer Application Insights zur Überwachung der Leistung und zur Diagnose von Problem. Umfassende Dashboards, intelligente Warnungen, leistungsstarke Diagnosetools und Analyseabfragen."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/12/2016
ms.author: sewhee
ms.translationtype: Human Translation
ms.sourcegitcommit: dea21a59b189d1d3d474cbc5e67f64df485a1981
ms.openlocfilehash: 68ec072b972e38d8cd020adda4dcc85cdaccfb76
ms.contentlocale: de-de
ms.lasthandoff: 11/17/2016

---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a>Überwachung der Anwendungsleistung mithilfe von Application Insights für SCOM
Falls Sie Ihre Server mithilfe von System Center Operations Manager (SCOM) verwalten, können Sie zum Überwachen der Leistung sowie zum Diagnostizieren von Leistungsproblemen [Azure Application Insights](app-insights-asp-net.md)verwenden. Application Insights überwacht eingehende Anforderungen, ausgehende REST- und SQL-Aufrufe sowie Ausnahmen und Protokollablaufverfolgungen Ihrer Webanwendung. Die Lösung stellt Metrikdiagramme und intelligente Warnungen für Dashboards bereit und bietet leistungsfähige diagnostische Suchfunktionen und analytische Abfragen für die Telemetriedaten. 

Die Application Insights-Überwachung kann mithilfe eines SCOM-Management Packs aktiviert werden.

## <a name="before-you-start"></a>Vorbereitung
Wir treffen folgende Annahmen:

* Sie sind mit SCOM vertraut und verwalten Ihre IIS-Webserver mithilfe von SCOM 2012 R2 oder 2016.
* Sie haben auf Ihren Servern bereits eine Webanwendung installiert, die Sie mit Application Insights überwachen möchten.
* Die App-Framework-Version ist mindestens .NET 4.5.
* Sie haben Zugriff auf ein Abonnement in [Microsoft Azure](https://azure.com) und können sich beim [Azure-Portal](https://portal.azure.com) anmelden. Unter Umständen verfügt Ihre Organisation über ein Abonnement und kann diesem Ihr Microsoft-Konto hinzufügen.

(Das Entwicklerteam kann das [Application Insights SDK](app-insights-asp-net.md) in die Web-App integrieren. Durch diese Instrumentation zur Buildzeit haben Entwickler mehr Freiheiten beim Schreiben von benutzerdefinierter Telemetrie. Dies spielt hier jedoch keine Rolle: Die beschriebenen Schritte können auch ausgeführt werden, wenn das SDK integriert wurde.)

## <a name="one-time-install-application-insights-management-pack"></a>Installieren des Application Insights-Management Packs (einmaliger Schritt)
Gehen Sie auf dem Computer, auf dem Operations Manager ausgeführt wird, wie folgt vor:

1. Deinstallieren Sie sämtliche alte Versionen des Management Packs:
   1. Öffnen Sie in Operations Manager „Verwaltung“ > „Management Packs“. 
   2. Löschen Sie die alte Version.
2. Laden Sie das Management Pack aus dem Katalog herunter, und installieren Sie es.
3. Starten Sie Operations Manager neu.

## <a name="create-a-management-pack"></a>Erstellen eines Management Packs
1. Öffnen Sie in Operations Manager **Konfiguration**, **.NET...mit Application Insights**, **Assistent zum Hinzufügen von Monitoren**, und wählen Sie erneut **.NET...mit Application Insights** aus.
   
    ![](./media/app-insights-scom/020.png)
2. Benennen Sie die Konfiguration nach Ihrer App. (Apps müssen immer einzeln instrumentiert werden.)
   
    ![](./media/app-insights-scom/030.png)
3. Erstellen Sie auf der gleichen Seite des Assistenten entweder ein neues Management Pack, oder wählen Sie ein zuvor für Application Insights erstelltes Management Pack aus.
   
     (Das Application Insights- [Management Pack](https://technet.microsoft.com/library/cc974491.aspx) ist eine Vorlage für die Erstellung einer Instanz. Die gleiche Instanz kann später erneut verwendet werden.)

    ![Geben Sie auf der Registerkarte „Allgemeine Eigenschaften“ den Namen Ihrer App ein. Klicken Sie auf „Neu“, und geben Sie einen Namen für ein Management Pack ein. Klicken Sie auf „OK“ und anschließend auf „Weiter“.](./media/app-insights-scom/040.png)

1. Wählen Sie eine zu überwachende App aus. Das Suchfeature durchsucht die auf Ihren Servern installierten Apps.
   
    ![Klicken Sie auf der Registerkarte zur Angabe der zu überwachenden Elemente auf „Hinzufügen“, geben Sie einen Teil des App-Namens ein, klicken Sie auf „Suchen“, wählen Sie die App aus, klicken Sie auf „Hinzufügen“, und klicken Sie anschließend auf „OK“.](./media/app-insights-scom/050.png)
   
    Falls die App nicht auf allen Servern überwacht werden soll, können Sie mithilfe des optionalen Felds „Überwachungsbereich“ eine Teilmenge Ihrer Server angeben.
2. Auf der nächsten Seite des Assistenten müssen Sie zunächst Ihre Anmeldeinformationen für die Anmeldung bei Microsoft Azure angeben.
   
    Auf dieser Seite wählen Sie die gewünschte Application Insights-Ressource für die Analyse und Darstellung der Telemetriedaten aus. 
   
   * Falls die Anwendung während der Entwicklung für Application Insights konfiguriert wurde, wählen Sie die vorhandene Ressource aus.
   * Falls nicht, erstellen Sie eine neue, nach der App benannte Ressource. Sind weitere Apps vorhanden, bei denen es sich um Komponenten des gleichen Systems handelt, fügen Sie sie der gleichen Ressourcengruppe hinzu, um die Verwaltung des Telemetriedatenzugriffs zu vereinfachen.
     
     Diese Einstellungen können später geändert werden.
     
     ![Klicken Sie auf der Registerkarte mit den Einstellungen für Application Insights auf „Anmelden“, und geben Sie die Anmeldeinformationen Ihres Microsoft-Kontos für Azure an. Wählen Sie dann ein Abonnement, eine Ressourcengruppe und eine Ressource aus.](./media/app-insights-scom/060.png)
3. Schließen Sie den Assistenten ab.
   
    ![Klicken Sie auf „Erstellen“.](./media/app-insights-scom/070.png)

Wiederholen Sie dieses Verfahren für jede App, die Sie überwachen möchten.

Falls Sie die Einstellungen später ändern möchten, öffnen Sie im Konfigurationsfenster die Eigenschaften des Monitors.

![Wählen Sie in der Konfiguration die Option für die .NET-Anwendungsleistungsüberwachung mit Application Insights sowie Ihren Monitor aus, und klicken Sie anschließend auf „Eigenschaften“.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a>Überprüfen der Überwachung
Der installierte Monitor durchsucht jeden Server nach Ihrer App. Wo die App gefunden wird, wird der Application Insights-Statusmonitor für die Überwachung der App konfiguriert. Gegebenenfalls wird der Statusmonitor zuvor auf dem Server installiert.

Sie können überprüfen, welche Instanzen der App gefunden wurden:

![Öffnen Sie Application Insights (unter „Überwachung“).](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a>Anzeigen von Telemetriedaten in Application Insights
Navigieren Sie im [Azure-Portal](https://portal.azure.com)zur Ressource für Ihre App. Hier werden [Diagramme mit Telemetriedaten](app-insights-dashboards.md) aus Ihrer App angezeigt. (Falls sie noch nicht auf der Hauptseite angezeigt wird, klicken Sie auf „Live Metrics Stream“.)

## <a name="next-steps"></a>Nächste Schritte
* [Richten Sie ein Dashboard ein](app-insights-dashboards.md) , um die wichtigsten Diagramme zur Überwachung dieser und anderer Apps zusammenzuführen.
* [Weitere Informationen zu Metriken](app-insights-metrics-explorer.md)
* [Einrichten von Warnungen](app-insights-alerts.md)
* [Diagnostizieren von Leistungsproblemen](app-insights-detect-triage-diagnose.md)
* [Leistungsstarke Analytics-Abfragen](app-insights-analytics.md)
* [Verfügbarkeitswebtests](app-insights-monitor-web-app-availability.md)


