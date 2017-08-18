---
title: "Azure Application Insights für ASP.NET Core | Microsoft-Dokumentation"
description: "Überwachen Sie Webanwendungen auf Verfügbarkeit, Leistung und Auslastung."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: 43e9e864a11cd6601ca9599f5a193793d344e0e5
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---
# <a name="application-insights-for-aspnet-core"></a>Application Insights für ASP.NET Core
[Mit Application Insights ](app-insights-overview.md)können Sie Ihre Webanwendung auf Verfügbarkeit, Leistung und Nutzung überwachen. Mit dem Feedback zur Leistung und Effektivität der App in der Praxis können Sie in jedem Entwicklungslebenszyklus eine fundierte Entscheidung für die Richtung des Entwurfs treffen.

![Beispiel](./media/app-insights-asp-net-core/sample.png)

Sie benötigen ein Abonnement für [Microsoft Azure](http://azure.com). Melden Sie sich mit einem Microsoft-Konto an, das Sie möglicherweise für Windows, XBox Live oder andere Microsoft-Clouddienste verwenden. Falls Ihr Team über ein Unternehmensabonnement für Azure verfügt, können Sie den Besitzer bitten, Sie über Ihr Microsoft-Konto hinzuzufügen.

## <a name="getting-started"></a>Erste Schritte

* Klicken Sie im Projektmappen-Explorer von Visual Studio mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **Application Insights konfigurieren** oder **Hinzufügen > Application Insights** aus. [Weitere Informationen](app-insights-asp-net.md).
* Wenn diese Befehle im Menü nicht aufgeführt werden, befolgen Sie den [Leitfaden zu den manuellen ersten Schritten](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started). Dies ist möglicherweise erforderlich, wenn das Projekt mit einer Version von Visual Studio vor 2017 erstellt wurde.

## <a name="using-application-insights"></a>Verwenden von Application Insights
Melden Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com) an, wählen Sie **Alle Ressourcen** oder **Application Insights** aus, und wählen Sie dann die Ressource aus, die Sie zum Überwachen Ihrer App erstellt haben.

Nutzen Sie die App eine Zeit lang in einem separaten Browserfenster. Nach und nach werden Daten in den Application Insights-Diagrammen angezeigt. (Möglicherweise müssen Sie auf „Aktualisieren“ klicken.) Während der Entwicklung werden nur wenige Daten angezeigt. Wenn Sie die App veröffentlichen und sie von vielen Benutzern verwendet wird, werden diese Diagramme jedoch mit zahlreichen Daten gefüllt. 

Auf der Übersichtsseite werden wichtige Leistungsdiagramme angezeigt: Serverantwortzeiten, Seitenladezeiten und Anzahl der Anforderungen mit Fehlern. Klicken Sie auf ein beliebiges Diagramm, um weitere Diagramme und Daten anzuzeigen.

Die Ansichten im Portal sind in drei Hauptkategorien unterteilt:

* Im [Metrik-Explorer](app-insights-metrics-explorer.md) werden Diagramme und Graphen von Metriken und Zahlen angezeigt, beispielsweise Antwortzeiten, Fehlerraten oder Metriken, die Sie selbst über die [API](app-insights-api-custom-events-metrics.md) erstellt haben. Filtern und segmentieren Sie die Daten nach Eigenschaftswerten, um sich einen besseren Überblick über Ihre App und ihre Benutzer zu verschaffen.
* Im [Suchexplorer](app-insights-diagnostic-search.md) sind einzelne Ereignisse aufgeführt, beispielsweise bestimmte Anforderungen, Ausnahmen, Ablaufprotokolle oder Ereignisse, die Sie selbst über die [API](app-insights-api-custom-events-metrics.md) erstellt haben. Filtern und durchsuchen Sie die Ereignisse, und navigieren Sie zwischen zugehörigen Ereignissen, um Probleme zu untersuchen.
* [Analytics](app-insights-analytics.md) ein leistungsfähiges Tool für Analyse und Diagnose, mit dem Sie SQL-ähnliche Abfragen für Ihre Telemetriedaten durchführen können.

## <a name="alerts"></a>Warnungen
* Sie erhalten automatisch [proaktive Diagnosewarnungen](app-insights-proactive-diagnostics.md), die Sie über anomale Änderungen bei Fehlerraten und anderen Metriken informieren.
* Richten Sie [Verfügbarkeitstests](app-insights-monitor-web-app-availability.md) ein, um Ihre Website fortwährend an Standorten auf der ganzen Welt zu testen und um E-Mails zu erhalten, sobald bei einem Test Fehler auftreten.
* Richten Sie [Metrikwarnungen](app-insights-monitor-web-app-availability.md) ein, um informiert zu werden, wenn Metriken wie Antwortzeiten oder Ausnahmeraten außerhalb zulässiger Grenzwerte liegen.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a>Open Source
[Lesen und Hinzufügen von Code](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a>Nächste Schritte
* [Fügen Sie Ihren Webseiten Telemetrie hinzu](app-insights-javascript.md) , um die Seitennutzung und Leistung zu überwachen.
* [Überwachen Sie Abhängigkeiten](app-insights-asp-net-dependencies.md) , um zu überprüfen, ob REST, SQL oder andere externe Ressourcen die Leistung beeinträchtigen.
* [Verwenden Sie die API](app-insights-api-custom-events-metrics.md) , um Ihre eigenen Ereignisse und Metriken für eine detailliertere Ansicht der Leistung und Nutzung Ihrer App zu senden.
* [Availability tests](app-insights-monitor-web-app-availability.md) wird Ihre App fortwährend an Standorten auf der ganzen Welt überprüft. 


