---
title: 'Stream Analytics: Betrugserkennung in Echtzeit | Microsoft Docs'
description: "Erfahren Sie, wie Sie mit Stream Analytics eine Lösung zur Betrugserkennung in Echtzeit erstellen können. Verwenden Sie einen Event Hub für die Ereignisverarbeitung in Echtzeit."
keywords: Erkennung von Anomalien, Betrugserkennung, Erkennung von Anomalien in Echtzeit
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: c10dd53f-d17a-4268-a561-cb500a8c04eb
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 11/14/2016
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 18156086ca3eaf3ee99dfeb61096bc1eee49458b
ms.openlocfilehash: 3d32518bed64ebe3dd1790bb511949624b5dcac1


---
# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Erste Schritte mit Azure Stream Analytics: Betrugserkennung in Echtzeit
Erfahren Sie, wie Sie mit Azure Stream Analytics eine End-to-End-Lösung zur Betrugserkennung in Echtzeit erstellen können. Binden Sie Ereignisse in Azure Event Hubs ein, schreiben Sie Stream Analytics-Abfragen für das Erfassen von Daten oder für Warnungen, und senden Sie das Ergebnis an eine Ausgabesenke, um in Echtzeit Dateneinblicke zu erhalten. Anomalienerkennung in Echtzeit für Telekommunikation wird erläutert, aber die Beispieltechnik eignet sich gleichermaßen für andere Arten von Betrugserkennung, z.B. Kreditkarten- oder Identitätsdiebstahlszenarios.

Stream Analytics ist ein vollständig verwalteter Dienst, der eine geringe Latenz, hohe Verfügbarkeit und eine skalierbare komplexe Ereignisverarbeitung für das Streaming von Daten in der Cloud bietet. Weitere Informationen finden Sie unter [Einführung in Azure Stream Analytics](stream-analytics-introduction.md).

## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Szenario: Telekommunikation und SIM-Betrugserkennung in Echtzeit
Ein Telekommunikationsunternehmen hat eine große Datenmenge durch eingehende Anrufe. Das Unternehmen möchte an den Daten folgende Aktionen durchführen:

* Reduzieren dieser Daten auf eine verwaltbare Menge und Gewinnen von Erkenntnissen über die Kundennutzung anhand der geografischen Regionen und über einen Zeitraum
* Erkennen von SIM-Betrug (mehrere mehr oder weniger gleichzeitige Anrufe derselben Person von geografisch unterschiedlichen Standorten) in Echtzeit, damit das Unternehmen problemlos reagieren und die Kunden benachrichtigen oder den Dienst einstellen kann

Kanonische IoT-Szenarios (Internet der Dinge) enthalten sehr viel Telemetrie- oder Sensordaten. Kunden wünschen, die Daten zu aggregieren oder Warnungen zu Anomalien in Echtzeit zu erhalten.

## <a name="prerequisites"></a>Voraussetzungen
* Herunterladen von [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) aus dem Microsoft Download Center
* Optional: Quellcode des Ereignisgenerators von [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator)

## <a name="create-azure-event-hubs-input-and-consumer-group"></a>Erstellen einer Azure Event Hub-Eingabe- und Consumergruppe
Die Beispielanwendung generiert Ereignisse und überträgt sie mithilfe von Push an eine Event Hub-Instanz für die Echtzeitverarbeitung. Service Bus Event Hubs sind die bevorzugte Methode der Ereigniserfassung für Stream Analytics. Weitere Informationen zu Event Hubs finden Sie unter [Azure Dokumentation zu Service Bus](/azure/service-bus/).

So erstellen Sie einen Event Hub:

1. Klicken Sie im [Azure-Portal](https://manage.windowsazure.com/) auf **NEU** > **APP SERVICES** > **SERVICE BUS** > **EVENT HUB** > **SCHNELLERFASSUNG**. Geben Sie einen Namen, eine Region und den neuen oder vorhandenen Namespace an, um einen neuen Event Hub zu erstellen.  
2. Als bewährte Methode sollte jeder Stream Analytics-Auftrag aus einer einzelnen Event Hub-Verbrauchergruppe lesen. Wir führen Sie später durch den Prozess des Erstellens einer Consumergruppe. [Erfahren Sie mehr über Consumergruppen](https://msdn.microsoft.com/library/azure/dn836025.aspx). Um eine Consumergruppe zu erstellen, wechseln Sie zu dem neu erstellten Event Hub, klicken Sie auf die Registerkarte **CONSUMERGRUPPEN** und anschließend auf **ERSTELLEN** am unteren Rand der Seite. Geben Sie einen Namen für die Consumergruppe ein.
3. Um Zugriff auf den Event Hub zu gewähren, müssen wir eine Richtlinie für gemeinsamen Zugriff erstellen. Klicken Sie auf die Registerkarte **KONFIGURIEREN** Ihres Event Hubs.
4. Erstellen Sie unter **RICHTLINIEN FÜR GEMEINSAMEN ZUGRIFF** eine neue Richtlinie mit Berechtigungen zum **VERWALTEN**.

   ![Unter "Gemeinsame Zugriffsrichtlinien" können Sie eine neue Richtlinie mit Verwaltungsberechtigungen erstellen.](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-shared-access-policies.png)
5. Klicken Sie unten auf der Seite auf **Speichern** .
6. Wechseln Sie zum **DASHBOARD**, klicken Sie unten auf der Seite auf **VERBINDUNSINFORMATIONEN**, und kopieren und speichern Sie anschließend die Verbindungsinformationen.

## <a name="configure-and-start-the-event-generator-application"></a>Konfigurieren und starten der Ereignisgenerator-Anwendung
Wir haben eine Clientanwendung bereitgestellt, die Beispielmetadaten für eingehende Anrufe generiert und diese per Push an den Event Hub übermittelt. Gehen Sie folgendermaßen vor, um diese Anwendung einzurichten.  

1. Laden Sie die Datei [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) herunter, und entzippen Sie diese in ein Verzeichnis.

   > [!NOTE]
   > Windows blockiert möglicherweise die heruntergeladene ZIP-Datei. Klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie **Eigenschaften**. Wenn die Meldung „Die Datei stammt von einem anderen Computer. Der Zugriff wurde aus Sicherheitsgründen eventuell blockiert.“ angezeigt wird, aktivieren Sie das Feld **Nicht mehr blockieren**, und klicken Sie für die ZIP-Datei auf „Übernehmen“.
   >
   > 
2. Ersetzen Sie die Werte „Microsoft.ServiceBus.ConnectionString“ und „EventHubName“ in „telcodatagen.exe.config“ durch die Verbindungszeichenfolge und den Namen Ihres Event Hubs.

   Die Verbindungszeichenfolge, die Sie vom Azure-Portal kopiert haben, stellt den Namen der Verbindung ans Ende. Entfernen Sie „;EntityPath=<value>“ aus dem Feld „add key=“.
3. Starten Sie die Anwendung. Die Nutzung lautet wie folgt:

   telcodatagen.exe [#NumCDRsPerHour] [SIM-Kartenbetrugswahrscheinlichkeit] [#DurationHours]

Im folgenden Beispiel werden im Lauf von zwei Stunden 1.000 Ereignisse mit einer Betrugswahrscheinlichkeit von 20 Prozent generiert.

    telcodatagen.exe 1000 .2 2

Sie sehen, dass Datensätze an Ihren Event Hub gesendet werden. Einige wichtige Felder dieser Anwendung zur Betrugserkennung in Echtzeit werden hier definiert:

| Datensatz      | Definition                               |
| ----------- | ---------------------------------------- |
| CallrecTime | Zeitstempel für die Startzeit des Anrufs.       |
| SwitchNum   | Die für die Anrufverbindung verwendete Vermittlungsstelle. |
| CallingNum  | Die Rufnummer des Anrufers.              |
| CallingIMSI | International Mobile Subscriber Identity (IMSI).  Die eindeutige ID des Anrufers. |
| CalledNum   | Die Rufnummer des Empfängers.      |
| CalledIMSI  | International Mobile Subscriber Identity (IMSI).  Die eindeutige ID des Empfängers. |

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags
Nun, da wir einen Datenstrom von Telekommunikationsereignissen haben, können wir einen Stream Analytics-Auftrag einrichten, um diese Ereignisse in Echtzeit zu analysieren.

### <a name="provision-a-stream-analytics-job"></a>Bereitstellen eines Stream Analytics-Auftrags
1. Klicken Sie im Azure-Portal auf **NEU** > **DATA SERVICES** > **STREAM ANALYTICS** > **SCHNELLSTART**.
2. Geben Sie die folgenden Werte an, und klicken Sie dann auf **STREAM ANALYTICS-AUFTRAG ERSTELLEN**:

   * **AUFTRAGSNAME**: Geben Sie einen Auftragsnamen ein.
   * **REGION**: Wählen Sie die Region aus, in der der Auftrag ausgeführt werden soll. Ziehen Sie es in Betracht, den Auftrag und das Event Hub in derselben Region zu platzieren, um für eine bessere Leistung zu sorgen und sicherzustellen, dass Sie nicht für die Übertragung von Daten zwischen Regionen bezahlen.
   * **SPEICHERKONTO**: Wählen Sie das Azure-Speicherkonto aus, das Sie zum Speichern von Überwachungsdaten für alle Stream Analytics-Aufträge verwenden möchten, die innerhalb dieser Region ausgeführt werden. Sie haben die Möglichkeit, ein vorhandenes Speicherkonto auszuwählen oder ein neues zu erstellen.
3. Klicken Sie im linken Bereich auf **STREAM ANALYTICS**, um die Stream Analytics-Aufträge aufzulisten.

   ![Symbol des Stream Analytics-Diensts](./media/stream-analytics-real-time-fraud-detection/stream-analytics-service-icon.png)

   Der neue Auftrag wird mit dem Status **ERSTELLT**aufgeführt. Beachten Sie, dass die Schaltfläche **START** am unteren Seitenrand deaktiviert ist. Sie müssen die Auftragseingabe, -ausgabe und -abfrage konfigurieren, bevor Sie den Auftrag starten können.

### <a name="specify-job-input"></a>Festlegen der Auftragseingabe
1. Klicken Sie in Ihrem Stream Analytics-Auftrag auf **EINGABEN** am oberen Rand der Seite, und klicken Sie dann auf **EINGABE HINZUFÜGEN**. Das aufgerufene Dialogfeld führt Sie durch eine Reihe von Schritten, um Ihre Eingabe einzurichten.
2. Klicken Sie auf **DATENSTROM** und anschließend auf die rechte Schaltfläche.
3. Klicken Sie auf **EVENT HUB**, und dann mit der rechten Maustaste.
4. Geben Sie die folgenden Werte auf der dritten Seite ein, oder wählen Sie sie aus:

   * **INPUT ALIAS**: Geben Sie einen Anzeigenamen für diese Auftragseingabe ein, z.B. *CallStream*. Beachten Sie, dass Sie diesen Namen später in der Abfrage verwenden werden.
   * **EVENT HUB**: Wenn sich der Event Hub, den Sie erstellt haben, im gleichen Abonnement wie der Stream Analytics-Auftrag befindet, wählen Sie den Namespace aus, in dem sich der Event Hub befindet.

     Wenn sich Ihr Event Hub in einem anderen Abonnement befindet, wählen Sie **Event Hub aus einem anderen Abonnement verwenden** aus, und geben Sie manuell die Informationen für **SERVICE BUS-NAMESPACE**, **EVENT HUB-NAME**, **NAME DER EVENT HUB-RICHTLINIE**, **SCHLÜSSEL DER EVENT HUB-RICHTLINIE** und **EVENT HUB-PARTITIONSANZAHL** ein.
   * **EVENT HUB-NAME**: Wählen Sie den Namen des Event Hubs aus.
   * **NAME DER EVENT HUB-RICHTLINIE**: Wählen Sie die Event Hub-Richtlinie aus, die Sie zuvor in diesem Lernprogramm erstellt haben.
   * **EVENT HUB-CONSUMERGRUPPE**: Geben Sie den Namen der Consumergruppe ein, die Sie zuvor in diesem Tutorial erstellt haben.
5. Klicken Sie auf die rechte Taste.
6. Geben Sie die folgenden Werte an:

   * **EVENT SERIALIZER FORMAT**: JSON
   * **CODIERUNG**: UTF8
7. Klicken Sie auf das **Kontrollkästchen**, um diese Quelle hinzuzufügen und um zu überprüfen, ob Stream Analytics erfolgreich mit dem Event Hub verbunden werden kann.

### <a name="specify-job-query"></a>Festlegen der Auftragsabfrage
Stream Analytics unterstützt ein einfaches, deklaratives Abfragemodell zum Beschreiben der Transformationen für Echtzeitverarbeitung. Weitere Informationen zur Sprache finden Sie in der [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/dn834998.aspx)(in englischer Sprache). Dieses Tutorial hilft Ihnen beim Erstellen und Testen mehrerer Abfragen über Ihren Anrufdatenstrom in Echtzeit.

#### <a name="optional-sample-input-data"></a>Optional: Beispieleingabedaten
Um Ihre Abfrage mit den tatsächlichen Auftragsdaten zu überprüfen, können Sie die Funktion **BEISPIELDATEN** verwenden, um Ereignisse aus Ihrem Datenstrom zu extrahieren und eine JSON-Datei der Ereignisse zum Testen zu erstellen.  Die folgenden Schritte veranschaulichen die Vorgehensweise. Wir haben zudem die Beispieldatei [Telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) für Testzwecke bereitgestellt.

1. Wählen Sie Ihre Event Hub-Eingabe aus, und klicken Sie am unteren Seitenrand auf **BEISPIELDATEN**.
2. Geben Sie im daraufhin angezeigten Dialogfeld eine **STARTZEIT** für das Sammeln der Daten und die **DAUER** ein, während der zusätzliche Daten verarbeitet werden sollen.
3. Klicken Sie auf die Schaltfläche **PRÜFEN**, um mit dem Erfassen der Eingabedaten zu beginnen.  Das Erstellen der Datendatei kann ein oder zwei Minuten dauern.  Wenn der Vorgang abgeschlossen ist, klicken Sie auf **DETAILS**, laden Sie die generierte JSON-Datei herunter, und speichern Sie diese.

   ![Laden Sie die verarbeiteten Daten herunter, und speichern Sie diese in einer JSON-Datei.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-download-save-json-file.png)

#### <a name="pass-through-query"></a>Pass-Through-Abfrage
Wenn Sie alle Ereignisse archivieren möchten, können Sie eine Pass-Through-Abfrage verwenden, um alle Felder der Ereignis- oder Nachrichtennutzlast abzufragen. Beginnen Sie mit einer einfachen Pass-Through-Abfrage, die alle Felder in einem Ereignis projiziert.

1. Klicken Sie auf **ABFRAGE** oben auf der Seite des Stream Analytics-Auftrags.
2. Fügen Sie dem Code-Editor Folgendes hinzu:

     SELECT * FROM CallStream

   > [!IMPORTANT]
   > Stellen Sie sicher, dass der Name der Eingabequelle dem Namen der Eingabe entspricht, die Sie zuvor angegeben haben.
   >
   > 
3. Klicken Sie im Abfrage-Editor auf **Test** .
4. Geben Sie eine Testdatei an. Hierbei kann es sich um die im Rahmen der obigen Schritte von Ihnen erstellte Datei oder um die Datei [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/SampleDataFiles/Telco.json) handeln.
5. Klicken Sie auf die Schaltfläche **PRÜFEN**. Die Ergebnisse werden unterhalb der Abfragedefinition angezeigt.

   ![Ergebnisse der Abfragedefinition](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sim-fraud-output.png)

### <a name="column-projection"></a>Spaltenprojektion
Nun reduzieren wir die zurückgegebenen Felder auf eine kleinere Menge.

1. Ändern Sie die Abfrage im Code-Editor in:

     SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum   FROM CallStream
2. Klicken Sie im Abfrage-Editor auf **Erneut ausführen** , um die Ergebnisse der Abfrage anzuzeigen.

   ![Ausgabe im Abfrage-Editor.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Anzahl der eingehenden Aufrufe pro Region: Rollierendes Fenster mit Aggregation
Um die Anzahl der eingehenden Anrufe pro Region zu vergleichen, rufen wir alle fünf Sekunden die Anzahl der eingehenden Anrufe im [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) gruppiert nach **SwitchNum** ab.

1. Ändern Sie die Abfrage im Code-Editor in:

     SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount   FROM CallStream TIMESTAMP BY CallRecTime   GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

   Diese Abfrage gibt mit dem Schlüsselwort **Timestamp By** ein Zeitstempelfeld für die Nutzlast an, das für die zeitliche Berechnung verwendet wird. Wird dieses Feld nicht angegeben, dann würde der Vorgang die Zeit verwenden, zu der jedes Ereignis beim Event Hub eingeht. Weitere Informationen erhalten Sie unter ["Arrival Time Vs Application Time" in der Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)(in englischer Sprache).

   Beachten Sie, dass Sie mit der Eigenschaft **System.Timestamp** jeweils auf einen Zeitstempel für das Ende der einzelnen Fenster zugreifen können.
2. Klicken Sie im Abfrage-Editor auf **Erneut ausführen** , um die Ergebnisse der Abfrage anzuzeigen.

   ![Abfrageergebnisse für "Timestamp By"](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>SIM-Betrugserkennung mit einem Self-Join
Um eine potenziell betrügerische Verwendung erkennen zu können, betrachten wir Anrufe, die in weniger als fünf Sekunden von einem Benutzer von unterschiedlichen Standorten aus eingegangen sind.  Wir [verknüpfen](https://msdn.microsoft.com/library/azure/dn835026.aspx) den Strom der Anrufereignisse mit sich selbst, um diese Fälle zu überprüfen.

1. Ändern Sie die Abfrage im Code-Editor in:

     SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,   CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2   FROM CallStream CS1 TIMESTAMP BY CallRecTime   JOIN CallStream CS2 TIMESTAMP BY CallRecTime   ON CS1.CallingIMSI = CS2.CallingIMSI   AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5   WHERE CS1.SwitchNum != CS2.SwitchNum
2. Klicken Sie im Abfrage-Editor auf **Erneut ausführen** , um die Ergebnisse der Abfrage anzuzeigen.

   ![Abfrageergebnisse einer Verknüpfung](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Erstellen der Ausgabetabelle
Nun, da wir einen Datenstrom, eine Event Hub-Eingabe zum Erfassen von Ereignissen und eine Abfrage zur Ausführung einer Umwandlung über den Stream definiert haben, ist der letzte Schritt die Definition einer Ausgabetabelle für den Auftrag. Die Ereignisse im Zusammenhang mit betrügerischem Verhalten werden in Azure Blob Storage geschrieben.

Befolgen Sie die nachstehenden Schritte zum Erstellen eines Containers für Blob Storage, wenn Sie noch keinen erstellt haben.

1. Verwenden Sie ein vorhandenes Speicherkonto, oder erstellen Sie ein neues, indem Sie auf **NEU > DATENDIENSTE > SPEICHER > SCHNELLERFASSUNG** klicken und die Anweisungen befolgen.
2. Wählen Sie das Speicherkonto aus, klicken Sie oben auf der Seite auf **CONTAINER**, und klicken Sie dann auf **HINZUFÜGEN**.
3. Geben Sie einen **NAMEN** für den Container an und legen Sie seinen **ZUGRIFF** auf **Öffentlicher BLOB** fest.

## <a name="specify-job-output"></a>Festlegen der Auftragsausgabe
1. Klicken Sie in Ihrem Stream Analytics-Auftrag am oberen Rand der Seite auf **AUSGABE**, und klicken Sie dann auf **AUSGABE HINZUFÜGEN**. Das aufgerufene Dialogfeld führt Sie durch eine Reihe von Schritten, um Ihre Ausgabe einzurichten.
2. Wählen Sie **BLOB STORAGE**, und klicken Sie dann mit der rechten Maustaste.
3. Geben Sie die folgenden Werte auf der dritten Seite ein, oder wählen Sie sie aus:

   * **AUSGABEALIAS**: Geben Sie einen Anzeigenamen für diese Auftragsausgabe ein.
   * **ABONNEMENT**: Befindet sich der Blob Storage, den Sie erstellt haben, im gleichen Abonnement wie der Stream Analytics-Auftrag, wählen Sie **Speicherkonto aus dem aktuellen Abonnement verwenden** aus. Wenn sich der Speicher in einem anderen Abonnement befindet, wählen Sie **Speicherkonto von einem anderen Abonnement verwenden** aus, und geben Sie manuell die Informationen für **SPEICHERKONTO**, **SPEICHERKONTOSCHLÜSSEL** und **CONTAINER** ein.
   * **SPEICHERKONTO**: Wählen Sie den Namen des Speicherkontos aus.
   * **CONTAINER**: Wählen Sie den Namen des Containers aus.
   * **DATEINAMENPRÄFIX**: Geben Sie ein Präfix ein, das beim Schreiben von BLOB-Ausgaben verwendet wird.
4. Klicken Sie auf die rechte Taste.
5. Geben Sie die folgenden Werte an:

   * **EVENT SERIALIZER FORMAT**: JSON
   * **CODIERUNG**: UTF8
6. Klicken Sie auf die Schaltfläche **PRÜFEN**, um diese Quelle hinzuzufügen und zu überprüfen, ob Stream Analytics erfolgreich mit dem Speicherkonto verbunden werden kann.

## <a name="start-job-for-real-time-processing"></a>Starten des Auftrags für Echtzeitverarbeitung
Da wir Eingabe, Abfrage und Ausgabe für den Auftrag angegeben haben, können wir den Stream Analytics-Auftrag für die Betrugserkennung in Echtzeit starten.

1. Klicken Sie im **DASHBOARD** des Auftrags am unteren Rand der Seite auf **STARTEN**.
2. Wählen Sie im daraufhin angezeigten Dialogfeld **AUFTRAGSSTARTZEIT** aus, und klicken Sie dann auf das **Häkchen** am unteren Rand des Dialogfelds. Der Status ändert sich in **Wird gestartet** und wechselt nach kurzer Zeit zu **Wird ausgeführt**.

## <a name="view-fraud-detection-output"></a>Anzeigen der Ausgabe zur Betrugserkennung
Verwenden Sie ein Tool wie [Azure Storage Explorer](http://storageexplorer.com/) oder [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction), um betrügerische Ereignisse beim Schreiben in die Ausgabe in Echtzeit anzuzeigen.  

![Betrugserkennung: Echtzeitanzeige von betrügerischen Ereignissen](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Support
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)




<!--HONumber=Nov16_HO3-->


