---
title: "Einführung in Stream Analytics | Microsoft Docs"
description: "Erfahren Sie mehr über Stream Analytics, einen verwalteten Dienst, der Ihnen bei der Analyse der Streamingdaten aus dem Internet der Dinge (IoT) in Echtzeit hilft."
keywords: Analysen als Dienst, verwaltete Dienste, Streamingverarbeitung, Stream Analytics, was ist Stream Analytics
services: stream-analytics
documentationcenter: 
author: jenniehubbard
manager: jhubbard
editor: cgronlun
ms.assetid: 613c9b01-d103-46e0-b0ca-0839fee94ca8
ms.service: stream-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/08/2017
ms.author: jhubbard
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: 5081ecaf569aef9e2f99cc27e91c4b64a25b0deb
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---

# <a name="what-is-stream-analytics"></a>Was ist Stream Analytics?

Azure Stream Analytics ist ein vollständig verwaltetes Modul für die Ereignisverarbeitung, mit dem Sie analytische Echtzeitberechnungen von Datenströmen einrichten können. Die Daten können von Geräten, Sensoren, Websites, Feeds sozialer Medien, Anwendungen, Infrastruktursystemen usw. stammen. 

## <a name="what-can-i-do-with-stream-analytics"></a>Wofür kann ich Stream Analytics verwenden?

Mit Stream Analytics können Sie hohe Datenvolumina untersuchen, die von Geräten oder Prozessen ausgehen, Informationen aus dem Datenstrom extrahieren und nach Mustern, Trends und Beziehungen suchen. Je nach Inhalt der Daten können Sie dann Anwendungsaufgaben durchführen. Beispielsweise können Sie Warnungen auslösen, Automatisierungsworkflows starten, Informationen an ein Berichtstool wie Power BI leiten oder Daten zur späteren Untersuchung speichern. 

Beispiele:

* Personalisierte Aktienhandelsanalysen und Warnungen von Finanzdienstleistern in Echtzeit
* Echtzeit-Betrugserkennung basierend auf der Auswertung von Transaktionsdaten 
* Daten- und Identitätsschutzdienste
* Analyse von Daten, die von Sensoren und Stellgliedern physischer Objekte generiert werden (Internet der Dinge (IoT))
* Webklickstream-Analyse
* CRM-Anwendungen (Customer Relationship Management), z.B. Ausgabe von Warnungen, wenn die Benutzerfreundlichkeit innerhalb eines Zeitraums beeinträchtigt wird

## <a name="how-does-stream-analytics-work"></a>Funktionsweise von Stream Analytics

In der folgenden Abbildung ist die Stream Analytics-Pipeline dargestellt. Es wird gezeigt, wie Daten erfasst, analysiert und dann zur Anzeige oder Bearbeitung gesendet werden. 

![Stream Analytics-Pipeline](./media/stream-analytics-introduction/stream_analytics_intro_pipeline.png)

Stream Analytics beginnt mit einer Quelle mit Streamingdaten. Die Daten können in Azure von einem Gerät erfasst werden, indem ein Azure Event Hub oder IoT Hub verwendet wird. Die Daten können auch aus einem Datenspeicher abgerufen werden, z.B. Azure Blob Storage. 

Zum Untersuchen des Datenstroms erstellen Sie einen Stream Analytics-*Auftrag*, in dem angegeben ist, woher die Daten stammen. Im Auftrag ist auch eine *Transformation*&mdash; dazu angegeben, wie nach Daten, Mustern oder Beziehungen gesucht werden soll. Für diese Aufgabe unterstützt Stream Analytics eine SQL-ähnliche Abfragesprache, mit der Sie Streamingdaten für einen bestimmten Zeitraum filtern, sortieren, aggregieren und zusammenführen können.

Schließlich ist im Auftrag auch noch der Ausgabeort angegeben, an den die transformierten Daten gesendet werden. So können Sie steuern, was als Reaktion auf die analysierten Informationen zu tun ist. Aus der Analyse können sich beispielsweise folgende Schritte ergeben:

* Senden eines Befehls zum Ändern der Einstellungen eines Geräts 
* Senden von Daten an eine Warteschlange, die von einem Prozess überwacht wird, bei dem je nach den gefundenen Ergebnissen Maßnahmen ergriffen werden 
* Senden von Daten an ein Power BI-Dashboard zur Berichterstellung
* Senden von Daten an einen Speicher, z.B. Data Lake Store, SQL Server-Datenbank oder Azure Blob/Table Storage

Während der Ausführung eines Auftrags können Sie ihn überwachen und anpassen, wie viele Ereignisse pro Sekunde verarbeitet werden. Außerdem können Sie einrichten, dass für Aufträge Diagnoseprotokolle für die Problembehandlung erstellt werden.

## <a name="key-capabilities-and-benefits"></a>Wichtige Funktionen und Vorteile

Stream Analytics ist darauf ausgelegt, leicht verwendbar, flexibel, skalierbar auf jede Auftragsgröße und wirtschaftlich zu sein.

### <a name="connectivity-to-many-inputs-and-outputs"></a>Konnektivität mit vielen Ein- und Ausgaben

Stream Analytics stellt eine Direktverbindung mit [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) und [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) (Datenstromerfassung) und mit dem [Azure Blob Storage-Dienst](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage-accounts) (Erfassung von Verlaufsdaten) her. Wenn Sie Daten aus Event Hubs abrufen, können Sie Stream Analytics mit anderen Datenquellen und Verarbeitungsmodulen kombinieren.

Die Auftragseingabe kann auch Referenzdaten enthalten (statische oder sich langsam ändernde Daten). Sie können Streamingdaten mit diesen Referenzdaten zusammenführen, um Suchvorgänge auf die gleiche Weise durchzuführen, wie Sie dies mit Datenbankabfragen tun.

Leiten Sie die Stream Analytics-Auftragsausgabe in viele verschiedene Richtungen weiter. Sie können in Speicher schreiben, z.B. Azure Storage-Blobs oder -Tabellen, Azure SQL DB, Azure Data Lake Stores oder Azure Cosmos DB. Von dort aus können die Daten beispielsweise zur Batchanalyse per Azure HDInsight weitergeleitet werden. Sie können die Ausgabe zur Nutzung durch einen anderen Prozess an einen anderen Dienst senden, z.B. Event Hubs, Azure Service Bus-Themen oder Warteschlangen. Sie können die Ausgabe auch zur Visualisierung an Power BI senden.

### <a name="ease-of-use"></a>Einfache Bedienung

Zum Definieren von Transformationen setzen Sie eine einfache, deklarative [Stream Analytics-Abfragesprache](https://msdn.microsoft.com/library/azure/dn834998.aspx) ein, mit der Sie anspruchsvolle Analysen ohne Programmierung erstellen können. Die Abfragesprache nutzt Streamingdaten als Eingabe. Anschließend können Sie die Daten filtern und sortieren, Werte aggregieren, Berechnungen durchführen, Daten zusammenführen (innerhalb eines Datenstroms oder mit Referenzdaten) und Geofunktionen verwenden. Sie können Abfragen im Portal bearbeiten, indem Sie IntelliSense und die Syntaxprüfung nutzen, und Abfragen mithilfe von Datenstichproben testen, die Sie aus dem Livestream extrahieren.

### <a name="extensible-query-language"></a>Erweiterbare Abfragesprache

Sie können die Funktionen der Abfragesprache erweitern, indem Sie zusätzliche Funktionen definieren und aufrufen. Definieren Sie Funktionsaufrufe im Azure Machine Learning-Dienst, um Azure Machine Learning-Lösungen zu nutzen. Außerdem können Sie benutzerdefinierte JavaScript-Funktionen (UDFs) integrieren, um im Rahmen einer Stream Analytics-Abfrage komplexe Berechnungen durchzuführen.

### <a name="scalability"></a>Skalierbarkeit

Stream Analytics kann bis zu 1 GB eingehende Daten pro Sekunde verarbeiten. Die Integration von [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) und [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) macht es für Aufträge möglich, Millionen von Ereignissen pro Sekunde zu erfassen, die von verbundenen Geräten, aus Klickstreams, aus Protokolldateien usw. stammen. Indem Sie die Partitionsfunktion von Event Hubs verwenden, können Sie Berechnungen in logische Schritte partitionieren, die bei Bedarf jeweils weiter partitioniert werden können, um die Skalierbarkeit zu erhöhen.

### <a name="low-cost"></a>Niedrige Kosten

Stream Analytics ist als Clouddienst optimiert, damit Sie ohne größere Kosten starten können. Für Sie gilt die nutzungsbasierte Zahlung basierend auf der Streamingeinheitennutzung sowie auf den vom System verarbeiteten Daten. Die Nutzung wird auf Grundlage der Menge an verarbeiteten Ereignissen und dem Umfang der im Cluster bereitgestellten Rechenleistung berechnet, um die Stream Analytics-Aufträge zu verarbeiten.

### <a name="reliability-quick-recovery-and-repeatability"></a>Zuverlässigkeit, schnelle Wiederherstellung und Wiederholbarkeit

Als verwalteter Dienst in der Cloud trägt Stream Analytics zur Verhinderung von Datenverlust und Sicherstellung der Geschäftskontinuität bei. Wenn es zu Ausfällen kommt, stellt der Dienst integrierte Wiederherstellungsfunktionen bereit. Mit der Möglichkeit, den Zustand intern beizubehalten, kann der Dienst wiederholbare Ergebnisse bereitstellen, wobei sichergestellt wird, dass Ereignisse archiviert und die Verarbeitung in Zukunft erneut angewendet werden können und immer dieselben Ergebnisse erzielt werden. Dies ermöglicht es Ihnen, zeitlich zurückzugehen und Berechnungen bei Ursachenanalysen, Was-wäre-wenn-Analysen usw. zu untersuchen.

## <a name="next-steps"></a>Nächste Schritte

* Experimentieren Sie mit Eingaben und Abfragen: [Erste Schritte mit Azure Stream Analytics zum Verarbeiten der Daten von IoT-Geräten](stream-analytics-get-started-with-azure-stream-analytics-to-process-data-from-iot-devices.md).
* Erstellen Sie eine [Stream Analytics-End-to-End-Lösung](stream-analytics-real-time-fraud-detection.md), mit der Telefonmetadaten auf betrügerische Anrufe untersucht werden.
* Informieren Sie sich über die SQL-ähnliche Abfragesprache für Stream Analytics und einzigartige Konzepte wie die [Fensterfunktionen](stream-analytics-window-functions.md).
* Erfahren Sie, wie Sie [Stream Analytics-Aufträge skalieren](stream-analytics-scale-jobs.md). 
* Erfahren Sie, wie Sie [Stream Analytics und Azure Machine Learning integrieren](stream-analytics-machine-learning-integration-tutorial.md).
* Antworten auf Ihre Fragen zu Stream Analytics finden Sie im [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).


