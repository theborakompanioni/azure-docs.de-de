---
title: Problembehandlung bei Azure Stream Analytics mit Diagnoseprotokollen | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Diagnoseprotokolle von Stream Analytics-Aufträgen in Microsoft Azure analysieren."
keywords: 
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: ea90a62ffee9c766985f76e1c0abc1585bebc69b
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---
# <a name="troubleshoot-azure-stream-analytics-by-using-diagnostics-logs"></a>Problembehandlung bei Azure Stream Analytics mit Diagnoseprotokollen

In einigen Fällen beendet ein Azure Stream Analytics-Auftrag unerwartet die Verarbeitung. Es ist wichtig, diese Art von Ereignissen behandeln zu können. Das Ereignis wird möglicherweise durch ein unerwartetes Abfrageergebnis, die Verbindung zu Geräten oder einen unerwarteten Dienstausfall verursacht. Durch die Diagnoseprotokolle in Stream Analytics können Sie die Ursache der Probleme bei ihrem Auftreten feststellen und die Wiederherstellungszeit verkürzen.

## <a name="log-types"></a>Protokolltypen

Stream Analytics bietet zwei Typen von Protokollen: 
* [Aktivitätsprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) (Always On). Aktivitätsprotokolle bieten Einblicke in die Vorgänge. die bei Aufträgen durchgeführt werden.
* [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) (konfigurierbar). Diagnoseprotokolle bieten umfangreichere Einblicke in sämtliche Vorgänge eines Auftrags. Diagnoseprotokolle starten, wenn der Auftrag erstellt wurde, und enden, wenn der Auftrag gelöscht wird. Sie behandeln Ereignisse bei der Aktualisierung des Auftrags und während der Ausführung.

> [!NOTE]
> Sie können Dienste wie Azure Storage, Azure Event Hubs und Azure Log Analytics verwenden, um nicht konforme Daten zu analysieren. Gebühren werden basierend auf dem Preismodell für diese Dienste in Rechnung gestellt.
>

## <a name="turn-on-diagnostics-logs"></a>Aktivieren von Diagnoseprotokollen

Diagnoseprotokolle sind standardmäßig **deaktiviert**. Um die Diagnoseprotokolle zu aktivieren, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim Azure-Portal an, und wechseln Sie zum Blatt „Streamingauftrag“. Wählen Sie unter **Überwachung** die Option **Diagnoseprotokolle** aus.

    ![Blattnavigation zu Diagnoseprotokollen](./media/stream-analytics-job-diagnostic-logs/image1.png)  

2.  Wählen Sie **Diagnose aktivieren**.

    ![Aktivieren von Diagnoseprotokollen](./media/stream-analytics-job-diagnostic-logs/image2.png)

3.  Setzen Sie auf der Seite **Diagnoseeinstellungen** die Option **Status** auf **Ein**.

    ![Ändern des Status für Diagnoseprotokolle](./media/stream-analytics-job-diagnostic-logs/image3.png)

4.  Richten Sie das gewünschte Archivierungsziel (Storage-Konto, Event Hub, Log Analytics) ein. Wählen Sie dann die Kategorien von Protokollen aus, die erfasst werden sollen (Ausführung, Erstellung). 

5.  Speichern Sie die neue Diagnosekonfiguration.

Es dauert etwa 10 Minuten, bis die Diagnosekonfiguration angewendet wird. Anschließend werden die Protokolle nach und nach im konfigurierten Archivierungsziel (auf dem Blatt **Diagnoseprotokolle** zu sehen) angezeigt:

![Blattnavigation zu Diagnoseprotokollen – Archivierungsziele](./media/stream-analytics-job-diagnostic-logs/image4.png)

Weitere Informationen zur Konfiguration von Diagnosen finden Sie unter [Erfassen und Verwenden von Diagnosedaten Ihrer Azure-Ressourcen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="diagnostics-log-categories"></a>Kategorien der Diagnoseprotokolle

Derzeit werden zwei Kategorien von Diagnoseprotokollen erfasst:

* **Erstellung**. Erfasst Protokollereignisse zu Auftragserstellungsvorgängen: Erstellen von Aufträgen, Hinzufügen und Löschen von Eingaben und Ausgaben, Hinzufügen und Aktualisieren der Abfrage, Starten und Beenden des Auftrags.
* **Ausführung**. Erfasst Ereignisse, die während der Ausführung eines Auftrags auftreten:
    * Verbindungsfehler
    * Datenverarbeitungsfehler wie Folgende:
        * Ereignisse, die nicht mit der Abfragedefinition übereinstimmen (nicht übereinstimmende Feldtypen und Werte, fehlende Felder usw.)
        * Fehler bei der Auswertung von Ausdrücken
    * Andere Ereignisse und Fehler

## <a name="diagnostics-logs-schema"></a>Diagnoseprotokollschema

Alle Protokolle werden im JSON-Format gespeichert. Jeder Eintrag enthält folgende allgemeine Zeichenfolgenfelder:

Name | Beschreibung
------- | -------
in | Zeitstempel (UTC) des Protokolls.
resourceId | ID der Ressource, über die der Vorgang stattfand, in Großbuchstaben. Beinhaltet die Abonnement-ID, die Ressourcengruppe und den Auftragsnamen. Beispiel: **/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT.STREAMANALYTICS/STREAMINGJOBS/MYSTREAMINGJOB**.
category | Protokollkategorie: **Ausführung** oder **Erstellung**.
operationName | Der Name des protokollierten Vorgangs. Beispiel: **Ereignisse senden: Fehler beim Schreiben der SQL-Ausgabe nach mysqloutput**.
status | Der Status des Vorgangs. Beispiele: **Fehlgeschlagen** oder **Erfolgreich**.
Einstellung | Protokollebene. Beispiele: **Fehler**, **Warnung** oder **Information**.
properties | Spezifisches Detail des Protokolleintrags; als JSON-Zeichenfolge serialisiert. Weitere Informationen finden Sie in den folgenden Abschnitten.

### <a name="execution-log-properties-schema"></a>Eigenschaftsschema der Ausführungsprotokolle

Ausführungsprotokolle enthalten Informationen zu Ereignissen, die während der Ausführung des Stream Analytics-Auftrags aufgetreten sind. Das Schema der Eigenschaften variiert je nach Ereignistyp. Derzeit sind folgende Typen von Ausführungsprotokollen verfügbar:

### <a name="data-errors"></a>Datenfehler

Alle Fehler, die auftreten, während der Auftrag Daten in dieser Kategorie von Protokollen verarbeitet. Diese Protokolle werden am häufigsten bei Lese-, Serialisierungs- und Schreibvorgängen von Daten erstellt. Diese Protokolle enthalten keine Verbindungsfehler. Verbindungsfehler werden als generische Ereignisse behandelt.

Name | Beschreibung
------- | -------
Quelle | Name der Auftragseingabe oder -ausgabe, bei der der Fehler aufgetreten ist.
Nachricht | Mit dem Fehler verknüpfte Meldung.
Typ | Fehlertyp. Beispiele: **DataConversionError**, **CsvParserError** oder **ServiceBusPropertyColumnMissingError**.
Daten | Enthält Daten, die hilfreich sind, um die Ursache des Fehlers genau zu lokalisieren. Unterliegt je nach Größe Kürzungen.

Je nach dem Wert für **operationName** entsprechen Datenfehler folgendem Schema:
* **Serialisieren von Ereignissen**. Die Serialisierung von Ereignissen tritt bei Lesevorgängen von Ereignissen auf. Sie treten auf, wenn die Daten bei der Eingabe aus einem der folgenden Gründen nicht das Abfrageschema erfüllen:
    * *Typenkonflikt während des (De-)Serialisierens von Ereignissen*: Identifiziert das Feld, das den Fehler verursacht hat.
    * *Ein Ereignis kann nicht gelesen werden; ungültige Serialisierung*: Enthält Informationen über die Stelle in den Eingabedaten, an der der Fehler aufgetreten ist. Enthält den Blobnamen für die Eingabe mit formlosen Objekten, Offset und eine Stichprobe der Daten.
* **Senden von Ereignissen**. Das Senden von Ereignissen tritt bei Schreibvorgängen auf. Diese identifizieren das Streamingereignis, das den Fehler verursacht hat.

### <a name="generic-events"></a>Generische Ereignisse

Generische Ereignisse verarbeiten alles andere.

Name | Beschreibung
-------- | --------
Error | (optional) Fehlerinformationen. In der Regel sind dies Ausnahmeinformationen, sofern diese verfügbar sind.
Nachricht| Protokollmeldung.
Typ | Meldungstyp. Wird der internen Kategorisierung von Fehlern zugeordnet. Beispiele: **JobValidationError** oder **BlobOutputAdapterInitializationFailure**.
Korrelations-ID | Ein [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)-Wert, der die Auftragsausführung eindeutig identifiziert. Alle Ausführungsprotokolleinträge ab dem Zeitpunkt, an dem der Auftrag gestartet wird, bis zum Beenden des Auftrags weisen denselben Wert für **Korrelations-ID** auf.

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Referenz zur Stream Analytics-Abfragesprache](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur REST-API für die Stream Analytics-Verwaltung](https://msdn.microsoft.com/library/azure/dn835031.aspx)

