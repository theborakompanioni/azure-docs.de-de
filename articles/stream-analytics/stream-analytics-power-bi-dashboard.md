---
title: Power BI-Dashboard auf Azure Stream Analytics | Microsoft-Dokumentation
description: "Verwenden Sie ein Power BI-Dashboard für Echtzeit-Streaming, um Business Intelligence zu erfassen und hohe Volumen von Daten aus einem Stream Analytics-Auftrag zu analysieren."
keywords: Analysedashboard, Echtzeitdashboard
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: fe8db732-4397-4e58-9313-fec9537aa2ad
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: samacha
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: b446e2296f2747012849936b994c4a4a2044869e
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Stream Analytics und Power BI: Ein Dashboard mit Echtzeitanalyse von Streamingdaten
Azure Stream Analytics ermöglicht Ihnen die Nutzung von [Microsoft Power BI](https://powerbi.com/), einem der führenden Business Intelligence-Tools. In diesem Artikel erfahren Sie, wie Sie Business Intelligence-Tools erstellen können, indem Sie Power BI als Ausgabe für Ihre Azure Stream Analytics-Aufträge verwenden. Darüber hinaus erfahren Sie, wie Sie ein Echtzeitdashboard erstellen und verwenden.

Dieser Artikel ist eine Fortsetzung des Stream Analytics-Tutorials [Erste Schritte mit Azure Stream Analytics: Betrugserkennung in Echtzeit](stream-analytics-real-time-fraud-detection.md). Er baut auf dem in diesem Tutorial erstellten Workflow auf, und es wird eine Power BI-Ausgabe hinzugefügt, damit Sie betrügerische Telefonanrufe visualisieren können, die von einem Stream Analytics-Auftrag erkannt werden. 

Sie können sich ein [Video](https://www.youtube.com/watch?v=SGUpT-a99MA) ansehen, in dem dieses Szenario veranschaulicht wird.


## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass Sie über Folgendes verfügen:

* Ein Azure-Konto.
* Ein Konto für Power BI. Sie können ein Geschäfts-, Schul- oder Unikonto verwenden.
* Eine abgeschlossene Version des Tutorials [Erste Schritte mit Azure Stream Analytics: Betrugserkennung in Echtzeit](stream-analytics-real-time-fraud-detection.md). Das Tutorial enthält eine App, mit der fiktive Metadaten für Telefonanrufe generiert werden. Im Tutorial erstellen Sie einen Event Hub und senden die Streamingdaten der Telefonanrufe an ihn. Sie schreiben eine Abfrage, mit der betrügerische Anrufe erkannt werden (Anrufe unter derselben Nummer zur gleichen Zeit an unterschiedlichen Orten). 


## <a name="add-power-bi-output"></a>Hinzufügen der Power BI-Ausgabe
Im Tutorial zur Betrugsermittlung in Echtzeit wird die Ausgabe an Azure Blob Storage gesendet. In diesem Abschnitt fügen Sie eine Ausgabe hinzu, mit der Informationen an Power BI gesendet werden.

1. Öffnen Sie im Azure-Portal den Stream Analytics-Auftrag, den Sie zuvor erstellt haben. Wenn Sie den vorgeschlagenen Namen verwendet haben, hat der Auftrag den Namen `sa_frauddetection_job_demo`.

2. Aktivieren Sie das Kontrollkästchen **Ausgaben** in der Mitte des Auftragsdashboards, und wählen Sie anschließend **+ Hinzufügen**.

3. Geben Sie unter **Ausgabealias** den Text `CallStream-PowerBI` ein. Sie können auch einen anderen Namen verwenden. Wenn Sie dies tun, sollten Sie sich den Namen notieren, da Sie ihn später noch benötigen. 

4. Wählen Sie unter **Senke** die Option **Power BI**.

   ![Erstellen einer Ausgabe für Power BI](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut.png)

5. Klicken Sie auf **Autorisieren**.

    Es wird ein Fenster geöffnet, in dem Sie Ihre Azure-Anmeldeinformationen für ein Geschäfts-, Schul- oder Unikonto angeben können. 

    ![Eingeben der Anmeldeinformationen für den Zugriff auf Power BI](./media/stream-analytics-power-bi-dashboard/authorize-area.png)

6. Geben Sie Ihre Anmeldeinformationen ein. Bedenken Sie beim Eingeben Ihrer Anmeldeinformationen, dass Sie auch dem Stream Analytics-Auftrag die Berechtigung zum Zugreifen auf Ihren Power BI-Bereich gewähren.

7. Geben Sie die folgenden Informationen ein, wenn Sie zurück auf das Blatt **Neue Ausgabe** gelangen:

    * **Gruppenarbeitsbereich**: Wählen Sie einen Arbeitsbereich in Ihrem Power BI-Mandanten aus, in dem Sie das Dataset erstellen möchten.
    * **Datasetname**: Geben Sie `sa-dataset` ein. Sie können auch einen anderen Namen verwenden. Wenn Sie dies tun, sollten Sie sich den Namen zur späteren Verwendung notieren.
    * **Tabellenname**: Geben Sie `fraudulent-calls` ein. Derzeit darf die Power BI-Ausgabe von Stream Analytics-Aufträgen nur eine Tabelle pro Dataset aufweisen.

    ![PBI-Arbeitsbereich](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut-with-dataset-table.png)

    > [!WARNING]
    > Wenn Power BI über ein Dataset und eine Tabelle mit den gleichen Namen wie das Dataset und die Tabelle im Stream Analytics-Auftrag verfügt, werden die vorhandenen Elemente überschrieben.
    > Es wird empfohlen, dieses Dataset und diese Tabelle nicht explizit in Ihrem Power BI-Konto zu erstellen. Diese werden automatisch erstellt, wenn Sie Ihren Stream Analytics-Auftrag starten und der Auftrag damit beginnt, Power BI mit Ausgaben zu füttern. Wenn die Auftragsabfrage keine Ergebnisse generiert, werden das Dataset und die Tabelle nicht erstellt.
    >

8. Klicken Sie auf **Erstellen**.

Das Dataset wird mit den folgenden Einstellungen erstellt:

* **defaultRetentionPolicy: BasicFIFO**: Daten sind FIFO mit maximal 200.000 Zeilen.
* **defaultMode: pushStreaming**: Das Dataset unterstützt sowohl Streamingkacheln als auch herkömmliche berichtsbasierte Visualisierungen (auch als „Push“ bezeichnet).

Derzeit ist es nicht möglich, Datasets mit anderen Kennzeichen zu erstellen.

Weitere Informationen zu Power BI-Datasets finden Sie in der [Power BI-REST-API](https://msdn.microsoft.com/library/mt203562.aspx)-Referenz.


## <a name="write-the-query"></a>Schreiben der Abfrage

1. Schließen Sie das Blatt **Ausgaben**, und wechseln Sie zurück zum Auftragsblatt.

2. Klicken Sie in das Feld **Abfrage**. 

3. Geben Sie die folgende Abfrage ein. Diese Abfrage ähnelt der Selbstverknüpfungsabfrage, die Sie im Tutorial zur Betrugserkennung erstellt haben. Der Unterschied besteht darin, dass diese Abfrage Ergebnisse an die neue Ausgabe sendet, die Sie erstellt haben (`CallStream-PowerBI`). 

    >[!NOTE]
    >Wenn Sie der Eingabe im Tutorial zur Betrugserkennung nicht den Namen `CallStream` gegeben haben, müssen Sie für `CallStream` in den **FROM**- und **JOIN**-Klauseln der Abfrage Ihren Namen einfügen.

        /* Our criteria for fraud:
        Calls made from the same caller to two phone switches in different locations (for example, Australia and Europe) within five seconds */

        SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
        INTO "CallStream-PowerBI"
        FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
        JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime

        /* Where the caller is the same, as indicated by IMSI (International Mobile Subscriber Identity) */
        ON CS1.CallingIMSI = CS2.CallingIMSI

        /* ...and date between CS1 and CS2 is between one and five seconds */
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

        /* Where the switch location is different */
        WHERE CS1.SwitchNum != CS2.SwitchNum
        GROUP BY TumblingWindow(Duration(second, 1))

4. Klicken Sie auf **Speichern**.


## <a name="test-the-query"></a>Testen der Abfrage
Dieser Abschnitt ist optional, wird aber empfohlen. 

1. Wenn die TelcoStreaming-App derzeit nicht ausgeführt wird, können Sie sie mit den folgenden Schritten starten:

    * Öffnen Sie ein Befehlsfenster.
    * Navigieren Sie zu dem Ordner, in dem die Datei „telcogenerator.exe“ und die geänderte Datei „telcodatagen.exe.config“ enthalten sind.
    * Führen Sie den folgenden Befehl aus:

            telcodatagen.exe 1000 .2 2

2. Klicken Sie auf dem Blatt **Abfrage** auf die Punkte neben der `CallStream`-Eingabe, und wählen Sie anschließend **Beispieldaten aus Eingabe**.

3. Geben Sie an, dass Sie Daten für einen Zeitraum von drei Minuten benötigen, und klicken Sie auf **OK**. Warten Sie auf die Benachrichtigung, dass die Beispieldaten erfasst wurden.

4. Klicken Sie auf **Test**, und vergewissern Sie sich, dass Sie Ergebnisse erhalten.


## <a name="run-the-job"></a>Ausführen des Auftrags

1. Stellen Sie sicher, dass die TelcoStreaming-App ausgeführt wird.

2. Schließen Sie das Blatt **Abfrage**.

3. Klicken Sie auf dem Auftragsblatt auf **Start**.

    ![Starten des Stream Analytics-Auftrags](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

Der Stream Analytics-Auftrag beginnt mit der Suche nach betrügerischen Anrufen, die im eingehenden Datenstrom enthalten sind. Außerdem erstellt der Auftrag das Dataset und die Tabelle in Power BI und sendet Daten zu den betrügerischen Anrufen an das Dataset und die Tabelle.


## <a name="create-the-dashboard-in-power-bi"></a>Erstellen des Dashboards in Power BI

1. Navigieren Sie zu [Powerbi.com](https://powerbi.com), und melden Sie sich mit Ihrem Geschäfts-, Schul- oder Unikonto an. Sofern die Stream Analytics-Auftragsabfrage Ergebnisse ausgibt, werden Sie feststellen, dass das Dataset bereits erstellt wurde:

    ![Streamingdataset in Power BI](./media/stream-analytics-power-bi-dashboard/streaming-dataset.png)

2. Klicken Sie in Ihrem Arbeitsbereich auf **+&nbsp;Erstellen**.

    ![Schaltfläche „Erstellen“ im Power BI-Arbeitsbereich](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. Erstellen Sie ein neues Dashboard, und geben Sie ihm den Namen `Fraudulent Calls`.

    ![Erstellen und Benennen eines Dashboards im Power BI-Arbeitsbereich](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. Klicken Sie oben im Fenster auf **Kachel hinzufügen**, wählen Sie die Option **BENUTZERDEFINIERTE STREAMINGDATEN**, und klicken Sie dann auf **Weiter**.

    ![Benutzerdefiniertes Streamingdataset](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. Wählen Sie Ihr Dataset unter **IHRE DATASETS** aus, und klicken Sie anschließend auf **Weiter**.

    ![Ihr Streamingdataset](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. Wählen Sie unter **Visualisierungstyp** die Option **Karte** und in der Liste **Felder** dann den Eintrag **fraudulentcalls**.

    ![Visualisierungsdetails für die neue Kachel](./media/stream-analytics-power-bi-dashboard/add-fraud.png)

7. Klicken Sie auf **Weiter**.


8. Fügen Sie die Kacheldetails ein, z.B. Titel und Untertitel.

    ![Titel und Untertitel für neue Kachel](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. Klicken Sie auf **Anwenden**.

    Sie verfügen nun über einen Betrugszähler!

    ![Betrugszähler](./media/stream-analytics-power-bi-dashboard/fraud-counter.png)

8. Führen Sie die Schritte erneut aus, um eine Kachel hinzuzufügen (ab Schritt 4). Gehen Sie jetzt wie folgt vor:

    * Wählen Sie unter **Visualisierungstyp** die Option **Liniendiagramm**. 
    * Fügen Sie eine Achse hinzu, und wählen Sie **windowend** aus. 
    * Fügen Sie einen Wert hinzu, und wählen Sie **fraudulentcalls**.
    * Wählen Sie für **Das anzuzeigende Zeitfenster** die letzten zehn Minuten aus.

    ![Kachel zum Erstellen eines Liniendiagramms](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. Klicken Sie auf **Weiter**, fügen Sie einen Titel und einen Untertitel hinzu, und klicken Sie auf **Übernehmen**.

    Im Power BI-Dashboard werden jetzt zwei Ansichten mit Daten zu betrügerischen Anrufen angezeigt, die in den Streamingdaten erkannt wurden.

    ![Fertiges Power BI-Dashboard mit zwei Kacheln zu betrügerischen Anrufen](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a>Weitere Informationen zu Power BI

In diesem Tutorial wird veranschaulicht, wie Sie für ein Dataset nur einige Arten von Visualisierungen erstellen. Mithilfe von Power BI können Sie weitere kundenspezifische Business Intelligence Tools für Ihr Unternehmen erstellen. Weitere Ideen finden Sie unter den folgenden Ressourcen:

* Ein weiteres Beispiel für ein Power BI-Dashboard sehen Sie im Video [Erste Schritte mit Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) .
* Weitere Informationen zur Konfiguration der Ausgabe von Stream Analytics-Aufträgen für Power BI und zur Verwendung von Power BI-Gruppen finden Sie im Abschnitt [Power BI](stream-analytics-define-outputs.md#power-bi) des Artikels zu [Stream Analytics-Ausgaben](stream-analytics-define-outputs.md). 
* Allgemeine Informationen zur Verwendung von Power BI finden Sie unter [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).


## <a name="learn-about-limitations-and-best-practices"></a>Einschränkungen und bewährte Methoden
Derzeit können Sie Power BI etwa einmal pro Sekunde aufrufen. Das Streaming visueller Elemente unterstützt Pakete mit bis zu 15 KB. Darüber hinaus tritt beim Streaming visueller Elemente ein Fehler auf (Push funktioniert jedoch weiterhin). Aufgrund dieser Einschränkungen eignet sich Power BI perfekt für Anwendungsfälle, bei denen Azure Stream Analytics eine erhebliche Datenlastverringerung ermöglicht. Wir empfehlen die Verwendung eines rollierenden oder springenden Fensters, um sicherzustellen, dass der Datenpush nicht höher ist als ein Push pro Sekunde und Ihre Abfrage innerhalb der Durchsatzanforderungen liegt.

Mithilfe der folgenden Gleichung können Sie den Wert berechnen, um den Wert für Ihr Fenster in Sekunden zu berechnen:

![Gleichung1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Beispiel:

* Sie haben 1.000 Geräte, die in Intervallen von einer Sekunde Daten senden.
* Sie verwenden das Power BI Pro-SKU, das 1.000.000 Zeilen pro Stunde unterstützt.
* Sie möchten die Menge der durchschnittlichen Daten pro Gerät für Power BI veröffentlichen.

Daraus resultiert die folgende Gleichung:

![Gleichung2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Mit dieser Konfiguration können Sie die ursprüngliche Abfrage wie folgt ändern:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO "CallStream-PowerBI"
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl


### <a name="renew-authorization"></a>Erneuern der Autorisierung
Wenn das Kennwort seit der Erstellung oder letzten Authentifizierung Ihres Auftrags geändert wurde, müssen Sie Ihr Power BI-Konto erneut authentifizieren. Wenn Azure Multi-Factor Authentication auf Ihrem Azure Active Directory (Azure AD)-Mandanten konfiguriert ist, müssen Sie die Power BI-Autorisierung ebenfalls alle zwei Wochen erneuern. Wenn Sie sie nicht erneuern, erhalten Sie unter Umständen keine Auftragsausgabe, oder in den Vorgangsprotokollen tritt ein `Authenticate user error` (Benutzerauthentifizierungsfehler) auf.

Wenn ein Auftrag gestartet wird, nachdem das Token abgelaufen ist, wird eine Fehlermeldung angezeigt, und die Ausführung des Auftrags ist nicht erfolgreich. Um dieses Problem zu beheben, halten Sie den laufenden Auftrag an, und wechseln Sie zur Power BI-Ausgabe. Klicken Sie auf den Link **Autorisierung erneuern**, und starten Sie den Auftrag ab der **Letzten Beendigungszeit** neu, um Datenverlust zu vermeiden.

Nachdem die Autorisierung mit Power BI aktualisiert wurde, wird im Autorisierungsbereich eine grüne Benachrichtigung angezeigt, die darauf hinweist, dass das Problem gelöst ist.

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referenz zur Stream Analytics-Abfragesprache)
* [Azure Stream Analytics Management REST API reference](https://msdn.microsoft.com/library/azure/dn835031.aspx) (Referenz zur Azure Stream Analytics-Verwaltungs-REST-API)

