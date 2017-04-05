---
title: Azure Stream Analytics-Diagnoseprotokolle | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Diagnoseprotokolle von Stream Analytics-Aufträgen in Microsoft Azure analysieren."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 0dac2cc79de884def8d4cf0ee89dc2f645d35b34
ms.lasthandoff: 03/28/2017


---
# <a name="job-diagnostic-logs"></a>Auftragsdiagnoseprotokolle

## <a name="introduction"></a>Einführung
Stream Analytics bietet zwei Typen von Protokollen: 
* [Aktivitätsprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), die immer aktiviert sind und Einblicke in Vorgänge bieten, die im Rahmen des Auftrags ausgeführt werden;
* [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs), die konfigurierbar sind und umfangreichere Einblicke in alle Vorgänge bieten, die im Rahmen des Auftrags ausgeführt werden, beginnend beim Erstellen und Aktualisieren über die Ausführung bis zum Löschen.

> [!NOTE]
> Beachten Sie, dass die Verwendung von Diensten wie Azure Storage, Event Hubs und Log Analytics zum Analysieren nicht konformer Daten anhand des Preismodells für diese Dienste in Rechnung gestellt werden.

## <a name="how-to-enable-diagnostic-logs"></a>Aktivieren von Diagnoseprotokollen
Die Diagnoseprotokolle sind standardmäßig **deaktiviert**. Gehen Sie folgendermaßen vor, um sie zu aktivieren:

Melden Sie sich beim Azure-Portal an, und wechseln Sie zum Blatt „Streamingauftrag“. Klicken Sie unter „Überwachung“ auf das Blatt „Diagnoseprotokolle“.

![Blatt „Navigation zu Diagnoseprotokollen“](./media/stream-analytics-job-diagnostic-logs/image1.png)  

Klicken Sie dann auf den Link „Diagnose aktivieren“.

![Aktivieren der Diagnoseprotokolle](./media/stream-analytics-job-diagnostic-logs/image2.png)

Ändern Sie in der geöffneten Diagnose den Status in „Ein“.

![Ändern des Status der Diagnoseprotokolle](./media/stream-analytics-job-diagnostic-logs/image3.png)

Konfigurieren Sie das gewünschte Archivierungsziel (Speicherkonto, Ereignishub, Log Analytics), und wählen Sie die Kategorien der Protokolle, die Sie erfassen möchten (Ausführung, Erstellung). Speichern Sie dann die neue Diagnosekonfiguration.

Nach dem Speichern dauert es etwa 10 Minuten, bis die Konfiguration übernommen wird. Anschließend werden die Protokolle nach und nach im konfigurierten Archivierungsziel (auf dem Blatt „Diagnoseprotokolle“ zu sehen) angezeigt:

![Blatt „Navigation zu Diagnoseprotokollen“](./media/stream-analytics-job-diagnostic-logs/image4.png)

Weitere Informationen zum Konfigurieren der Diagnose finden Sie auf der Seite [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="diagnostic-logs-categories"></a>Diagnoseprotokollkategorien
Es gibt zwei Kategorien von Diagnoseprotokollen, die wir derzeit erfassen:

* **Erstellung** erfasst die Protokolle zu Auftragserstellungsvorgängen: Erstellen, Hinzufügen und Löschen von Eingaben und Ausgaben, Hinzufügen und Aktualisieren der Abfrage, Starten und Beenden des Auftrags.
* **Ausführung** erfasst, was während der Ausführung eines Auftrags geschieht.
    * Konnektivitätsfehler;
    * Datenverarbeitungsfehler;
        * Ereignisse, die nicht mit der Abfragedefinition übereinstimmen (nicht übereinstimmende Feldtypen und Werte, fehlende Felder usw.);
        * Fehler bei der Auswertung von Ausdrücken;
    * und weitere

## <a name="diagnostic-logs-schema"></a>Schema „Diagnoseprotokolle“
Alle Protokolle werden im JSON-Format gespeichert, und jeder Eintrag hat die folgenden allgemeinen Zeichenfolgenfelder:

Name | Beschreibung
------- | -------
in | Der Zeitstempel (UTC) des Protokolls.
resourceId | Die ID der Ressource, auf der der Vorgang stattfand, in Großbuchstaben. Beinhaltet Abonnement-ID, Ressourcengruppe und Auftragsname. Beispiel: **/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT.STREAMANALYTICS/STREAMINGJOBS/MYSTREAMINGJOB**.
category | Die Protokollkategorie: **Ausführung** oder **Erstellung**.
operationName | Der Name des protokollierten Vorgangs. Beispiel **Ereignisse senden: Fehler beim Schreiben der SQL-Ausgabe nach mysqloutput**
status | Der Status des Vorgangs. Beispiele: **Fehler, Erfolg**.
Einstellung | Protokollebene. Beispiele: **Fehler, Warnung, Information**.
Eigenschaften | Details je nach Protokolleintrag, als JSON-Zeichenfolge serialisiert; weitere Informationen finden Sie im Folgenden.

### <a name="execution-logs-properties-schema"></a>Eigenschaftsschema der Ausführungsprotokolle
Ausführungsprotokolle enthalten Informationen zu Ereignissen, die während der Ausführung des Stream Analytics-Auftrags aufgetreten sind.
Je nach Typ der Ereignisse weisen Eigenschaften unterschiedliche Schemata auf. Derzeit gibt es folgende Typen:

### <a name="data-errors"></a>Datenfehler
Jeder Fehler in der Verarbeitung von Daten fällt unter diese Kategorie von Protokollen. Wird hauptsächlich während des Datenlesens sowie Serialisierungs- und Schreibvorgängen produziert. Sie umfasst nicht Konnektivitätsfehler, die als generische Ereignisse behandelt werden.

Name | Beschreibung
------- | -------
Quelle | Name der Auftragseingabe oder -Ausgabe, bei der der Fehler aufgetreten ist.
Nachricht | Mit dem Fehler verknüpfte Meldung.
Typ | Der Fehlertyp. Beispiele **DataConversionError, CsvParserError und ServiceBusPropertyColumnMissingError**.
Daten | Enthält Daten, die hilfreich sind, um die Ursache des Fehlers genau zu lokalisieren. Unterliegt je nach Größe Kürzungen.

Je nach dem Wert **operationName** entsprechen Datenfehler folgendem Schema:
* **Serialisieren von Ereignissen** – geschieht während Ereignislesevorgängen, wenn die Daten in der Eingabe das Abfrageschema nicht erfüllen:
    * Typenkonflikt während des (De)serialisierens von Ereignissen: Feld, das den Fehler verursacht hat.
    * Ereignis kann nicht gelesen werden, ungültige Serialisierung: Informationen zu der Stelle in den Eingabedaten, wo der Fehler aufgetreten ist: Blobname für Blobeingabe, Offset und eine Stichprobe der Daten.
* **Senden von Ereignissen** – Schreibvorgänge: Streamingereignis, das den Fehler verursacht hat.

### <a name="generic-events"></a>Generische Ereignisse
Alle anderen

Name | Beschreibung
-------- | --------
Error | (optional) Fehlerinformationen, in der Regel Ausnahmeinformationen, sofern verfügbar.
Nachricht| Protokollmeldung.
Typ | Typ der Nachricht, wird der internen Kategorisierung von Fehlern zugeordnet: z.B. **JobValidationError, BlobOutputAdapterInitializationFailure** usw.
Korrelations-ID | Ein [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)-Wert, der die Auftragsausführung eindeutig identifiziert. Alle Ausführungsprotokolleinträge, die vom Beginn des Auftrags bis zu seinem Ende produziert werden, weisen dieselbe Korrelations-ID auf.



## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)


