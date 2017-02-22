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
ms.date: 02/01/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: f0292efd50721ef58028df778052eb0ed6fcda84
ms.openlocfilehash: 724eba50b7428b0012e8f062e264ce057e2a5287


---
# <a name="job-diagnostic-logs"></a>Auftragsdiagnoseprotokolle

## <a name="introduction"></a>Einführung
Stream Analytics bietet zwei Typen von Protokollen: 
* [Aktivitätsprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), die immer aktiviert sind und Einblicke in Vorgänge bieten, die im Rahmen des Auftrags ausgeführt werden;
* [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs), die vom Benutzer konfigurierbar sind und umfangreichere Einblicke in alle Vorgänge bieten, die im Rahmen des Auftrags ausgeführt werden, beginnend beim Erstellen und Aktualisieren über die Ausführung bis zum Löschen.

## <a name="how-to-enable-diagnostic-logs"></a>Aktivieren von Diagnoseprotokollen
Die Diagnoseprotokolle sind standardmäßig **deaktiviert**. Gehen Sie folgendermaßen vor, um sie zu aktivieren:

Melden Sie sich beim Azure-Portal an, navigieren Sie zu dem Blatt „Streamingauftrag“, und verwenden Sie das Blatt „Diagnoseprotokolle“ unter „Überwachung“.

![Blatt „Navigation zu Diagnoseprotokollen“](./media/stream-analytics-job-diagnostic-logs/image1.png)  

Klicken Sie dann auf den Link „Diagnose aktivieren“.

![Aktivieren der Diagnoseprotokolle](./media/stream-analytics-job-diagnostic-logs/image2.png)

Ändern Sie in der geöffneten Diagnose den Status in „Nein“.

![Ändern des Status der Diagnoseprotokolle](./media/stream-analytics-job-diagnostic-logs/image3.png)

Konfigurieren Sie das gewünschte Archivierungsziel (Speicherkonto, Ereignishub, Log Analytics), und wählen Sie die Kategorien der Protokolle, die Sie erfassen möchten (Ausführung, Erstellung). Speichern Sie dann die neue Diagnosekonfiguration.

Nach dem Speichern dauert es ungefähr 10 Minuten, bis die Konfiguration wirksam ist, und danach werden Protokolle im konfigurierten Archivierungsziel angezeigt, die Sie auf dem Blatt „Diagnoseprotokolle“ sehen können:

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
resourceId | Die ID der Ressource, auf der der Vorgang stattfand, in Großbuchstaben. Beinhaltet Abonnement-ID, Ressourcengruppe und Auftragsname. Beispiel: `/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT.STREAMANALYTICS/STREAMINGJOBS/MYSTREAMINGJOB`
category | Die Protokollkategorie, entweder `Execution` oder `Authoring`.
operationName | Der Name des protokollierten Vorgangs. Beispiel: `Send Events: SQL Output write failure to mysqloutput`
status | Der Status des Vorgangs. Beispiel: `Failed, Succeeded`.
Einstellung | Protokollebene. Beispiel: `Error, Warning, Informational`
Eigenschaften | Spezifisches Detail des Protokolleintrags; als JSON-Zeichenfolge serialisiert; weitere Informationen finden Sie im Folgenden

### <a name="execution-logs-properties-schema"></a>Eigenschaftsschema der Ausführungsprotokolle
Ausführungsprotokolle enthalten Informationen zu Ereignissen, die während der Ausführung des Stream Analytics-Auftrags aufgetreten sind.
Je nach Typ der Ereignisse weisen Eigenschaften unterschiedliche Schemata auf. Derzeit gibt es folgende Typen:

### <a name="data-errors"></a>Datenfehler
Jeder Fehler in der Verarbeitung von Daten fällt unter diese Kategorie von Protokollen. Wird hauptsächlich während des Datenlesens sowie Serialisierungs- und Schreibvorgängen produziert. Sie umfasst nicht Konnektivitätsfehler, die als generische Ereignisse behandelt werden.

Name | Beschreibung
------- | -------
Quelle | Name der Auftragseingabe oder -Ausgabe, bei der der Fehler aufgetreten ist.
Nachricht | Mit dem Fehler verknüpfte Meldung.
Typ | Der Fehlertyp. Beispiel: `DataConversionError, CsvParserError, ServiceBusPropertyColumnMissingError` etc.
Daten | Enthält Daten, die hilfreich sind, um die Ursache des Fehlers genau zu lokalisieren. Unterliegt je nach Größe Kürzungen.

Je nach dem Wert **OperationName** entsprechen Datenfehler folgendem Schema:
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
Typ | Typ der Nachricht, Zuordnung zur internen Kategorisierung von Fehlern: z.B. JobValidationError, BlobOutputAdapterInitializationFailure usw.
Korrelations-ID | Ein [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)-Wert, der die Auftragsausführung eindeutig identifiziert. Alle Ausführungsprotokolleinträge, die vom Beginn des Auftrags bis zu seinem Ende produziert werden, haben die gleiche „Korrelations-ID“.



## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)




<!--HONumber=Feb17_HO1-->


