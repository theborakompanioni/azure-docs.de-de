---
title: "Übersicht über Azure Time Series Insights | Microsoft-Dokumentation"
description: "Enthält eine Einführung in Azure Time Series Insights, einen neuen Dienst zum Analysieren von Zeitreihendaten und für IoT-Lösungen."
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/20/2017
ms.author: omravi
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: cc35c00bec18ee1d9ed52b29b68ddacc1136dc70
ms.lasthandoff: 04/26/2017

---

# <a name="overview-of-azure-time-series-insights"></a>Übersicht über Azure Time Series Insights

Azure Time Series Insights ist ein vollständig verwalteter Clouddienst mit Speicher-, Analyse- und Visualisierungskomponenten, die Ihnen das gleichzeitige Erfassen, Speichern, Untersuchen und Analysieren von Milliarden von Ereignissen sehr einfach machen. Mit Time Series Insights erhalten Sie globale Einblicke in Ihre Daten und können basierend darauf Ihre IoT-Lösungen schneller validieren und kostenintensive Ausfallzeiten unternehmenskritischer Geräte vermeiden, indem Sie versteckte Trends und Anomalien erkennen und die Ermittlung zugrunde liegender Ursachen nahezu in Echtzeit durchführen können. Time Series Insights erfasst Zeitreihendaten von Ereignisbrokern (z.B. IoT Hubs oder Event Hubs), indiziert die Daten und mustert Daten basierend auf einer konfigurierbaren Aufbewahrungsrichtlinie aus. Benutzer nutzen die Daten für das jeweilige Geschäftsszenario entweder über eine intuitive Benutzeroberfläche oder REST-Abfrage-APIs.

![Übersicht über Time Series Insights](media/overview/time-series-insights-overview-flow.png)

## <a name="primary-scenarios"></a>Hauptszenarien

* Überprüfung und Überwachung von IoT-Lösungen in wenigen Minuten
* Bedarfsorientierte intuitive Visualisierung und Analyse von IoT-Daten
* Schnellere Analyse der Grundursachen und Anomalieerkennung
* Erstellung einer globalen Ansicht mit mehreren Geräten, Werken und Daten

## <a name="key-capabilities-and-benefits"></a>Wichtige Funktionen und Vorteile

* **Einfacher Einstieg**: Für Azure Time Series Insights ist keine Vorbereitung der Daten erforderlich, und die Anwendung ist unglaublich schnell. Es dauert nur wenige Minuten, bis Sie eine Verbindung mit Milliarden von Ereignissen in Ihrem Azure IoT Hub oder Event Hub hergestellt haben. Sobald eine Verbindung hergestellt ist, können Sie Ihre Sensordaten innerhalb weniger Sekunden visualisieren und nutzen und Ihre IoT-Lösung schnell validieren. Time Series Insights ist unglaublich intuitiv und einfach zu verwenden. Sie können mit Ihren Daten interagieren, ohne eine einzige Codezeile zu schreiben.  Außerdem müssen Sie keine neue Sprache lernen, da Time Series Insights für fortgeschrittene Benutzer eine Oberfläche für präzise Freitextabfragen enthält, und Anfänger können die Anwendung per Zeigen und Klicken erforschen.

* **Einblicke innerhalb weniger Sekunden – nahezu in Echtzeit**: Mit den zentral an einem Ort verfügbaren Speicher-, Analyse- und Visualisierungskomponenten von Time Series Insights erzielen Sie für Ihre Zeitreihendaten einen deutlich höheren Nutzen. Mit Time Series Insights können mehrere Hundertmillionen Sensorereignisse pro Tag erfasst werden, wobei die Wartezeit eine Minute beträgt, damit Sie schnell auf Veränderungen reagieren können. Time Series Insights ermöglicht Ihnen tiefere Einblicke in Ihre Sensordaten, weil Sie Trends und Anomalien schnell erkennen und leicht komplexe Analysen der zugrunde liegenden Ursache durchführen und teure Ausfallzeiten vermeiden können. Dank der Korrelation von Echtzeit- und Verlaufsdaten können Benutzer mit Time Series Insights versteckte Trends in ihren Daten aufdecken.

* **Erstellen von benutzerdefinierten Lösungen**: Integrieren Sie die Azure Time Series Insights-Daten in Ihre bestehenden Anwendungen, oder erstellen Sie mithilfe der Time Series Insights-REST-APIs neue benutzerdefinierte Lösungen.  Darüber hinaus ist die Erstellung und Freigabe von personalisierten Ansichten sehr einfach, sodass andere Benutzer Ihre Entdeckungen leicht verfolgen können.

* **Skalierbarkeit**: Time Series Insights ist für die IoT-Nutzung ausgelegt. In der Vorschauversion können zwischen 1 Million und 100 Millionen Ereignisse pro Tag erfasst werden, und die Standardaufbewahrungsdauer beträgt 31 Tage. Mit Time Series Insights können Benutzer zusätzlich zu großen Mengen von Verlaufsdaten auch Livedatenströme nahezu in Echtzeit visualisieren und analysieren. Für die Zukunft ist geplant, die Erfassungs- und Aufbewahrungsraten weiter zu erhöhen, um für das ständige Wachstum von Unternehmen gerüstet zu sein.

## <a name="taxonomy-of-time-series-insights"></a>Taxonomie von Time Series Insights

* **Umgebung**: Eine Umgebung ist eine Azure-Ressource mit Erfassungs- und Speicherfunktionen.  Kunden stellen Umgebungen über das Azure-Portal mit der erforderlichen Kapazität bereit.
* **Ereignisquelle**: Eine Ereignisquelle leitet sich von einem Ereignisbroker, z.B. Azure Event Hubs, ab.  Time Series Insights stellt eine direkte Verbindung mit Ereignisquellen her, und der Datenstrom wird erfasst, ohne dass Benutzer eine einzige Codezeile schreiben müssen. Derzeit werden für Time Series Insights Azure Event Hubs und Azure IoT Hubs unterstützt, und in der Zukunft sollen weitere Ereignisquellen hinzugefügt werden.
* **Referenzdaten**: Mit Time Series Insights erhalten Benutzer die Möglichkeit, Zeitreihendaten mit Referenzdaten zu verknüpfen.  Referenzdaten können Metadaten zu Geräten oder andere statische Daten enthalten, die sich relativ selten ändern. Time Series Insights verknüpft die Referenzdaten mit Datenströmen, sodass Benutzer diese Daten nahezu in Echtzeit visualisieren und analysieren können.

