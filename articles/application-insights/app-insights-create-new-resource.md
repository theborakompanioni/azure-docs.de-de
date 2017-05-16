---
title: Erstellen einer neuen Azure Application Insights-Ressource | Microsoft Docs
description: "Richten Sie manuell die Application Insights-Überwachung für eine neue Liveanwendung ein."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 003db6e1479be1007dd292555ce5997f1c138809
ms.openlocfilehash: f73a24993fdeaced422b2f7a1283722a82c2be77
ms.contentlocale: de-de
ms.lasthandoff: 12/13/2016


---
# <a name="create-an-application-insights-resource"></a>Erstellen einer Application Insights-Ressource
Azure Application Insights zeigt Daten über Ihre Anwendung in einer Microsoft Azure-*Ressource* an. Die Erstellung einer neuen Ressource gehört daher zur [Einrichtung von Application Insights für das Überwachen einer neuen Anwendung][start]. In vielen Fällen kann dies automatisch über die IDE erfolgen. Diese Methode wird nach Möglichkeit empfohlen. In einigen Fällen erstellen Sie eine Ressource jedoch manuell – z.B. um separate Ressourcen für die Entwicklungs- und Produktionsbuilds der Anwendung verwenden zu können.

Nach dem Erstellen der Ressource erhalten Sie den Instrumentationsschlüssel und verwenden ihn zum Konfigurieren des SDK in der Anwendung. Dadurch wird die Telemetrie an die Ressource gesendet.

## <a name="sign-up-to-microsoft-azure"></a>Anmelden bei Microsoft Azure
Wenn Sie noch kein [Microsoft-Konto besitzen, erstellen Sie jetzt eins](http://live.com). (Wenn Sie Dienste wie Outlook.com, OneDrive, Windows Phone oder XBox Live verwenden, besitzen Sie bereits ein Microsoft-Konto.)

Außerdem benötigen Sie ein [Microsoft Azure](http://azure.com)-Abonnement. Wenn Ihr Team oder Ihre Organisation über ein Azure-Abonnement verfügt, kann der Besitzer Sie anhand Ihrer Windows Live ID hinzufügen. Ihnen wird nur die tatsächliche Verwendung berechnet. Darüber hinaus erlaubt der Basic-Standardplan die kostenlose Nutzung für Testzwecke bis zu einem bestimmten Volumen.

Wenn Sie Zugang zu einem Abonnement besitzen, melden Sie sich über Ihre Live ID unter [http://portal.azure.com](https://portal.azure.com)bei Application Insights an.

## <a name="create-an-application-insights-resource"></a>Erstellen einer Application Insights-Ressource
Fügen Sie unter [portal.azure.com](https://portal.azure.com)eine neue Application Insights-Ressource hinzu:

![Klicken Sie auf "Neu > Application Insights"](./media/app-insights-create-new-resource/01-new.png)

* **Anwendungstyp** bestimmt den Inhalt des Blatts „Übersicht“ und die im [Metrik-Explorer][metrics] verfügbaren Eigenschaften. Wird Ihr App-Typ nicht angezeigt, wählen Sie „Allgemein“.
* **Abonnement** ist Ihr Zahlungskonto in Azure.
* **Ressourcengruppe** ist eine benutzerfreundliche Möglichkeit zum Verwalten von Eigenschaften wie der Zugriffssteuerung. Wenn Sie bereits andere Azure-Ressourcen erstellt haben, können Sie diese neue Ressource in derselben Gruppe platzieren.
* **Speicherort** ist der Ort, an dem Ihre Daten aufbewahrt werden.
* **An Dashboard anheften** legt eine Schnellzugriffskachel für die Ressource auf Ihrer Azure-Startseite ab. Empfohlen.

Wenn Ihre App erstellt wurde, wird ein neues Blatt geöffnet. Dort werden die Leistungs- und Nutzungsdaten über Ihre App angezeigt. 

Um bei der nächsten Anmeldung bei Azure dorthin zurückzugelangen, suchen Sie im Startmenü (Startseite) nach der Schnellstart-Kachel für Ihre App. Klicken Sie alternativ dazu auf "Durchsuchen", um sie zu finden.

## <a name="copy-the-instrumentation-key"></a>Kopieren des Instrumentationsschlüssels
Der Instrumentationsschlüssel identifiziert die Ressource, die Sie erstellt haben. Sie müssen ihn an das SDK übergeben.

![Klicken Sie auf "Essentials", klicken Sie auf den Instrumentierungsschlüssel, STRG+C.](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-the-sdk-in-your-app"></a>Installieren des SDK in Ihrer App
Installieren Sie das Application Insights-SDK in Ihrer App. Dieser Schritt hängt stark von der Art der Anwendung ab. 

Konfigurieren Sie das [SDK, das Sie in Ihrer Anwendung installieren][start] mithilfe des Instrumentierungsschlüssels.

Das SDK enthält die Standardmodule, die Telemetriedaten senden, ohne dass Sie Code schreiben müssen. Um Benutzeraktionen nachzuverfolgen oder Probleme im Detail zu diagnostizieren, [verwenden Sie die API][api] zum Senden eigener Telemetriedaten.

## <a name="monitor"></a>Anzeigen von Telemetriedaten
Schließen Sie das Schnellstartblatt, um zum Blatt Ihrer Anwendung im Azure-Portal zurückzukehren.

Klicken Sie auf die Kachel „Search“, um die [Diagnosesuche][diagnostic] zu öffnen. Dort werden die ersten Ereignisse angezeigt. 

Klicken Sie nach einigen Sekunden auf "Aktualisieren", wenn Sie mehr Daten erwarten.

## <a name="creating-a-resource-automatically"></a>Automatisches Erstellen einer Ressource
Sie können ein [PowerShell-Skript](app-insights-powershell.md) schreiben, um eine Ressource automatisch zu erstellen.

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines Dashboards](app-insights-dashboards.md)
* [Diagnosesuche](app-insights-diagnostic-search.md)
* [Untersuchen von Metriken](app-insights-metrics-explorer.md)
* [Schreiben von Analytics-Abfragen](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md


