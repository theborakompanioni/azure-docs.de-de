---
title: Untersuchen von HockeyApp-Daten in Azure Application Insights | Microsoft-Dokumentation
description: Analysieren Sie die Nutzung und Leistung Ihrer Azure-App mit Application Insights.
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: bwren
ms.translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 7ea28b4989ad889ff0427e221cc26ac5ebc41f93
ms.contentlocale: de-de
ms.lasthandoff: 04/07/2017

---
# <a name="exploring-hockeyapp-data-in-application-insights"></a>Untersuchen von HockeyApp-Daten in Application Insights
[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) ist die empfohlene Plattform für die Überwachung von desktopbasierten und mobilen Live-Apps. In HockeyApp können Sie nicht nur Absturzdaten erhalten, sondern auch benutzerdefinierte und nachverfolgte Telemetriedaten senden, um die Nutzung zu überwachen und die Diagnose zu ermöglichen. Dieser Telemetriedatenstrom kann mithilfe des leistungsstarken [Analytics](app-insights-analytics.md)-Features von [Azure Application Insights](app-insights-overview.md) abgefragt werden. Darüber hinaus können Sie [die benutzerdefinierten und nachverfolgten Telemetriedaten exportieren](app-insights-export-telemetry.md). Richten Sie zum Aktivieren dieser Features eine Brücke ein, die benutzerdefinierte HockeyApp-Daten an Application Insights überträgt.

## <a name="the-hockeyapp-bridge-app"></a>Die HockeyApp-Brücken-App
Die HockeyApp-Brücken-App ist das Hauptfeature, das Ihnen über die Funktionen für Analytics und fortlaufenden Export den Zugriff auf die benutzerdefinierte Telemetrie und die Ablaufverfolgungstelemetrie von HockeyApp in Application Insights ermöglicht. Auf alle benutzerdefinierten und Ablaufverfolgungsereignisse, die von HockeyApp nach dem Erstellen der HockeyApp-Brücken-App erfasst werden, kann über diese Funktionen zugegriffen werden. Sehen wir uns an, wie eine solche Brücken-App eingerichtet wird.

Öffnen Sie in HockeyApp „Kontoeinstellungen“ &gt; [API Tokens](https://rink.hockeyapp.net/manage/auth_tokens)(API-Token). Sie können entweder ein neues Token erstellen oder ein vorhandenes Token wiederverwenden. Die erforderliche Mindestberechtigung lautet „Schreibgeschützt“. Erstellen Sie eine Kopie des API-Tokens.

![HockeyApp-API-Token abrufen](./media/app-insights-hockeyapp-bridge-app/01.png)

Öffnen Sie das Microsoft Azure-Portal, und [erstellen Sie eine Application Insights-Ressource](app-insights-create-new-resource.md). Legen Sie als Anwendungstyp „HockeyApp bridge application“ (HockeyApp-Brückenanwendung) fest:

![Neue Application Insights-Ressource](./media/app-insights-hockeyapp-bridge-app/02.png)

Sie müssen keinen Namen angeben. Er wird automatisch basierend auf dem HockeyApp-Namen festgelegt.

Die Felder für die HockeyApp-Brücke werden angezeigt. 

![Brückenfelder eingeben](./media/app-insights-hockeyapp-bridge-app/03.png)

Geben Sie das zuvor notierte HockeyApp-Token ein. Mit dieser Aktion wird das Dropdownmenü „HockeyApp Application” (HockeyApp-Anwendung) mit allen HockeyApp-Anwendungen aufgefüllt. Wählen Sie die gewünschte Anwendung aus, und füllen Sie die übrigen Felder aus. 

Öffnen Sie die neue Ressource. 

Beachten Sie, dass es eine Weile dauert, bis der Datenfluss beginnt.

![Application Insights-Ressource, die auf Daten wartet](./media/app-insights-hockeyapp-bridge-app/04.png)

Fertig! Alle benutzerdefinierten und Ablaufverfolgungsdaten, die ab diesem Zeitpunk in Ihrer mit HockeyApp instrumentierten App erfasst werden, stehen Ihnen nun auch in den Funktionen für Analytics und fortlaufenden Export von Application Insights zur Verfügung.

Sehen wir uns kurz die einzelnen verfügbaren Features an.

## <a name="analytics"></a>Analyse
Analytics ist ein leistungsstarkes Tool für die Ad-hoc-Abfrage Ihrer Daten. Es ermöglicht die Diagnose und Analyse von Telemetriedaten und die schnelle Ermittlung von Ursachen und Mustern.

![Analyse](./media/app-insights-hockeyapp-bridge-app/05.png)

* [Weitere Informationen zu Analytics](app-insights-analytics-tour.md)

## <a name="continuous-export"></a>Fortlaufendem Export
Mit der Funktion „Fortlaufender Export“ können Sie Ihre Daten in einen Azure Blob Storage-Container exportieren. Dies ist sehr nützlich, wenn Sie Ihre Daten länger speichern müssen, als dies durch die von Application Insights angebotene Aufbewahrungsdauer vorgesehen ist. Sie können die Daten im Blobspeicher speichern oder in eine SQL-Datenbank bzw. eine andere bevorzugte Data Warehousing-Lösung konvertieren.

[Weitere Informationen zum fortlaufenden Export](app-insights-export-telemetry.md)

## <a name="next-steps"></a>Nächste Schritte
* [Anwenden von Analytics auf Ihre Daten](app-insights-analytics-tour.md)


