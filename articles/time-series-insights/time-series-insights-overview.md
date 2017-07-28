---
title: "Übersicht über Azure Time Series Insights | Microsoft-Dokumentation"
description: "Einführung in Azure Time Series Insights, einen neuen Dienst zum Analysieren von Zeitreihendaten und für IoT-Lösungen"
keywords: 
services: tsi
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: 
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: omravi
ms.translationtype: HT
ms.sourcegitcommit: 8021f8641ff3f009104082093143ec8eb087279e
ms.openlocfilehash: abd66208ab7ac30831f3f1eddb2891ed7bcd3995
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---

# <a name="what-is-azure-time-series-insights"></a>Was ist Azure Time Series Insights?

Azure Time Series Insights ist ein verwalteter Clouddienst mit Speicher-, Analyse- und Visualisierungskomponenten zur einfachen gleichzeitigen Erfassung, Speicherung, Untersuchung und Analyse unzähliger Ereignisse. Mit Time Series Insights erhalten Sie globale Einblicke in Ihre Daten und können auf dieser Grundlage Ihre IoT-Lösungen schneller validieren und kostenintensive Ausfallzeiten von Geräten vermeiden, indem Sie versteckte Trends und Anomalien erkennen und die Ermittlung zugrunde liegender Ursachen nahezu in Echtzeit durchführen. Time Series Insights erfasst Zeitreihendaten von Ereignisbrokern (z.B. IoT Hubs oder Event Hubs), indiziert die Daten und mustert Daten basierend auf einer konfigurierbaren Aufbewahrungsrichtlinie aus. Benutzer verwenden die Daten entweder über eine intuitive Benutzeroberfläche oder über REST-Abfrage-APIs.

![Übersicht über Time Series Insights](media/overview/time-series-insights-overview-flow.png)

## <a name="primary-scenarios"></a>Hauptszenarien

* Überwachen und Validieren von IoT-Lösungen innerhalb weniger Minuten
* Visualisieren und Analysieren von IoT-Daten
* Beschleunigen von Ursachenanalyse und Anomalieerkennung
* Erstellen einer globalen Ansicht mit mehreren Geräten, Werken und Daten

## <a name="capabilities-and-benefits"></a>Funktionen und Vorteile

* **Einfacher Einstieg:** Azure Time Series Insights erfordert keine Vorbereitung der Daten und ist überaus schnell. Stellen Sie binnen weniger Minuten eine Verbindung mit unzähligen Ereignissen in Ihrem Azure IoT Hub oder Event Hub her. Sobald eine Verbindung besteht, können Sie in Sekundenschnelle Sensordaten visualisieren, mit ihnen interagieren und Ihre IoT-Lösungen validieren. Time Series Insights ist benutzerfreundlich: Sie können mit Ihren Daten interagieren, ohne eine einzige Codezeile zu schreiben.  Sie müssen keine neue Sprache lernen: Für fortgeschrittene Benutzer steht in Time Series Insights eine Oberfläche für präzise Freitextabfragen zur Verfügung, alle anderen können Daten per Point&Click untersuchen.

* **Einblicke nahezu in Echtzeit**: Mit Time Series Insights können pro Tag unzählige Sensorereignisse erfasst werden – mit einer Wartezeit von lediglich einer Minute, sodass Sie schnell auf Veränderungen reagieren können. Time Series Insights bietet umfassende Einblicke in Ihre Sensordaten. So können Sie Trends und Anomalien schnell erkennen, problemlos komplexe Ursachenanalysen durchführen und teure Ausfallzeiten vermeiden. Dank der Korrelation von Echtzeit- und Verlaufsdaten können Sie mit Time Series Insights versteckte Trends in den Daten aufdecken.

* **Erstellen von benutzerdefinierten Lösungen**: Integrieren Sie die Azure Time Series Insights-Daten in Ihre bestehenden Anwendungen, oder erstellen Sie mithilfe der Time Series Insights-REST-APIs neue benutzerdefinierte Lösungen. Erstellen und teilen Sie personalisierte Ansichten mit anderen, um ihnen die Untersuchung Ihrer Entdeckungen zu ermöglichen.

* **Skalierbarkeit**: Time Series Insights ist für die skalierte IoT-Nutzung ausgelegt. In der Vorschauversion können zwischen einer Million und 100 Millionen Ereignisse pro Tag erfasst werden, und die Standardaufbewahrungsdauer beträgt 31 Tage. Sie können sowohl große Mengen an Verlaufsdaten als auch Livedatenströme nahezu in Echtzeit visualisieren und analysieren. Für die Zukunft ist geplant, die Erfassungs- und Aufbewahrungsraten weiter zu erhöhen, um für das ständige Wachstum von Unternehmen gerüstet zu sein.

## <a name="time-series-insights-glossary"></a>Glossar für Time Series Insights

* **Umgebung**: Eine Umgebung ist eine Azure-Ressource mit Erfassungs- und Speicherfunktionen.  Kunden stellen Umgebungen über das Azure-Portal mit der erforderlichen Kapazität bereit.
* **Ereignisquelle**: Eine Ereignisquelle leitet sich von einem Ereignisbroker, z.B. Azure Event Hubs, ab.  Time Series Insights stellt eine direkte Verbindung mit Ereignisquellen her, und der Datenstrom wird erfasst, ohne dass Sie eine einzige Codezeile schreiben müssen. Zurzeit unterstützt Time Series Insights Azure Event Hubs und Azure IoT Hubs.
* **Referenzdaten**: Mit Time Series Insights erhalten Benutzer die Möglichkeit, Zeitreihendaten mit Referenzdaten zu verknüpfen.  Referenzdaten können Metadaten zu Geräten oder andere statische Daten enthalten, die sich relativ selten ändern. Time Series Insights verknüpft die Referenzdaten mit Datenströmen, sodass Benutzer diese Daten nahezu in Echtzeit visualisieren und analysieren können.

