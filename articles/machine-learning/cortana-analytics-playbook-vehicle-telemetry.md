---
title: "Lösungs-Playbook zur Fahrzeugtelemetrieanalyse | Microsoft Docs"
description: "Verwenden Sie die Funktionen von Cortana Intelligence zur Echtzeitgewinnung von prädiktiven Einblicken in Fahrzeugzustand und Fahrgewohnheiten."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2016
ms.author: bradsev
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 55c85d95a16e0c0ea7bb11d6f47ae85aeb80577d


---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Lösungs-Playbook zur Fahrzeugtelemetrieanalyse
Dieses **Menü** bietet Links zu den Kapiteln dieses Playbooks. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Übersicht
Supercomputer stehen nicht mehr im Labor, sondern sind jetzt in Ihrer Garage geparkt! Diese modernen Fahrzeuge enthalten unzählige Sensoren, sodass sie Millionen von Ereignissen pro Sekunde verfolgen und überwachen können. Wir gehen davon aus, dass die meisten dieser Fahrzeuge bis 2020 mit dem Internet verbunden sein werden. Stellen Sie sich vor, wie Sie auf der Basis dieser riesigen Datensammlung erstklassige Funktionen für Sicherheit, Zuverlässigkeit und ein besseres Fahrgefühl bereitstellen können. Microsoft hat diesen Traum mit Cortana Intelligence Wirklichkeit werden lassen.

Cortana Intelligence von Microsoft ist eine vollständig verwaltete Big Data- und erweiterte Analyse-Suite, die es Ihnen ermöglicht, dank Ihrer Daten intelligente Maßnahmen zu ergreifen. Wir möchten Ihnen die Cortana Intelligence Lösungsvorlage für Vehicle Telemetry Analytics (Fahrzeugtelemetrieanalyse) vorstellen. Diese Lösung veranschaulicht, wie Automobilhändler, Automobilhersteller und Versicherungsgesellschaften die Funktionen von Cortana Intelligence verwenden können, um in Echtzeit und prädiktiv Einblick in Fahrzeugzustand und Fahrgewohnheiten zu gewinnen. 

Die Lösung ist als ein [Lambda-Architekturmuster](https://en.wikipedia.org/wiki/Lambda_architecture) implementiert und verfügt dadurch über das vollständige Potential der Cortana Intelligence-Plattform für Echtzeit- und Batchverarbeitung. Die Lösung: 

* stellt einen Fahrzeugtelematiksimulator bereit.
* nutzt Event Hubs zum Erfassen von Millionen simulierter Fahrzeug-Telemetrieereignisse in Azure. 
* nutzt Stream Analytics, um in Echtzeit Einblick in den Fahrzeugzustand zu gewinnen.
* behält die Daten für eine umfangreichere Batchanalyse langfristig im Speicher. 
* nutzt Machine Learning für die Erkennung von Anomalien in Echtzeit und Batchverarbeitung zum Gewinnen prädiktiver Einblicke.
* nutzt HDInsight für skalierte Datentransformationen und Data Factory für Orchestrierung, Planung, Ressourcenverwaltung und Überwachung der Batchverarbeitungs-Pipeline. 
* bietet ein umfassendes Dashboard für Visualisierungen von Echtzeitdaten und prädiktiver Analyse mit Power BI.

## <a name="architecture"></a>Architektur
![](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Abbildung 1 – Lösungsarchitektur für die Fahrzeugtelemetrieanalyse*

Diese Lösung umfasst die folgenden **Cortana Intelligence-Komponenten** und zeigt ihre End-to-End-Integration:

* **Event Hubs** für das Erfassen von Millionen von Telemetrie-Ereignissen in Azure.
* **Stream Analytics** für das Gewinnen von Einblicken in Echtzeit hinsichtlich des Fahrzeugzustands und Speichern dieser Daten in beständigem Speicher zur umfangreicheren Batchanalyse.
* **Machine Learning** für die Erkennung von Anomalien in Echtzeit und Batchverarbeitung zum Gewinnen prädiktiver Einblicke.
* **HDInsight** wird verwendet, um Daten nach Maß zu transformieren.
* **Data Factory** übernimmt die Orchestrierung, Planung, Ressourcenverwaltung und Überwachung der Batchverarbeitungs-Pipeline.
* **Power BI** bietet dieser Lösung ein umfassendes Dashboard für Visualisierungen von Echtzeitdaten und Predictive Analytics.

Diese Lösung greift auf die folgenden zwei unterschiedlichen **Datenquellen**zu: 

* **Simulierte Fahrzeugsignale und -diagnose:**Ein Fahrzeugtelematiksimulator gibt Diagnoseinformationen und Signale aus, die dem Zustand des Fahrzeugs und dem Fahrverhalten zu einem bestimmten Zeitpunkt entsprechen. 
* **Fahrzeugkatalog:**Der Fahrzeugkatalog ist ein Referenzdataset mit der Fahrgestellnummer zur Modellierung der Zuordnung.




<!--HONumber=Nov16_HO3-->


