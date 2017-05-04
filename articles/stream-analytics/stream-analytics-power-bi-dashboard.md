---
title: Power BI-Dashboard auf Azure Stream Analytics | Microsoft-Dokumentation
description: "Verwenden Sie ein Power BI-Dashboard für Echtzeit-Streaming, um Business Intelligence zu erfassen und hohe Volumen von Daten aus einem Stream Analytics-Auftrag zu analysieren."
keywords: Analysedashboard, Echtzeitdashboard
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: fe8db732-4397-4e58-9313-fec9537aa2ad
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 7f8b63c22a3f5a6916264acd22a80649ac7cd12f
ms.openlocfilehash: 278f73d9ee3f64727b34b3674cd38eaf5301e675
ms.lasthandoff: 05/01/2017


---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Stream Analytics und Power BI: Ein Dashboard mit Echtzeitanalyse von Streamingdaten
Azure Stream Analytics ermöglicht Ihnen die Nutzung von Microsoft Power BI, eines der führenden Business Intelligence-Tools. Erfahren Sie, wie Sie Azure Stream Analytics verwenden, um hohe Volumen von Streamingdaten zu analysieren und diese in einem Power BI-Analysedashboard in Echtzeit darzustellen.

Verwenden Sie [Microsoft Power BI](https://powerbi.com/) , um ohne großen Zeitaufwand ein Live-Dashboard zu erstellen. [Sehen Sie sich ein Video an, das dieses Szenario veranschaulicht](https://www.youtube.com/watch?v=SGUpT-a99MA).

In diesem Artikel erfahren Sie, wie Sie eigene benutzerdefinierte Business Intelligence-Tools erstellen können, indem Sie Power BI als Ausgabe für Ihre Aufträge in Azure Stream Analytics verwenden. Sie erfahren außerdem, wie Sie ein Echtzeitdashboard verwenden.

## <a name="prerequisites"></a>Voraussetzungen
* Microsoft Azure-Konto
* Geschäfts-, Schul- oder Unikonto für Power BI
* Abschluss des Szenarios zur [Betrugsermittlung in Echtzeit](stream-analytics-real-time-fraud-detection.md). Dieser Artikel baut auf dem Workflow auf, der unter [Betrugserkennung in Echtzeit](stream-analytics-real-time-fraud-detection.md) beschrieben ist, und fügt eine Power BI-Streaming-Dataset-Ausgabe hinzu.

## <a name="add-power-bi-output"></a>Power BI-Ausgabe hinzufügen
Da nun eine Eingabe für den Auftrag vorhanden ist, kann eine Ausgabe zu Power BI definiert werden.

1. Aktivieren Sie das Kästchen **Ausgaben** in der Mitte des Auftragsdashboards. Wählen Sie anschließend **+ Hinzufügen**, um Ihre Ausgabe zu erstellen.

    ![Ausgabe hinzufügen](./media/stream-analytics-power-bi-dashboard/create-pbi-output.png)

2. Geben Sie den **Ausgabealias** an. Sie können jeden Ausgabealias verwenden, auf den Sie auf einfache Weise verweisen können. Dieser Ausgabealias ist hilfreich, wenn Sie mehrere Ausgaben für den Auftrag benötigen. Verweisen Sie in diesem Fall in Ihrer Abfrage auf diese Ausgabe. Verwenden Sie beispielsweise den Aliasausgabewert „StreamAnalyticsRealTimeFraudPBI“.

3. Wählen Sie **Autorisieren**.

    ![Autorisierung hinzufügen](./media/stream-analytics-power-bi-dashboard/pbi-authorize.png)

4. Ein Fenster wird geöffnet, in dem Sie Ihre Azure-Anmeldeinformationen angeben können (für ein Geschäfts-, Schul- oder Unikonto). Es umfasst auch Ihren Azure-Auftrag mit Zugriff auf Ihren Power BI-Bereich.

    ![Autorisierungsfelder](./media/stream-analytics-power-bi-dashboard/authorize-area.png)

5. Die Autorisierung wird nicht mehr angezeigt, wenn Sie die erforderlichen Informationen angegeben haben. Der Bereich **Neue Ausgabe** beinhaltet die Felder **Datasetname** und **Tabellenname**.

    ![PBI-Arbeitsbereich](./media/stream-analytics-power-bi-dashboard/pbi-workspace.png)

6. Definieren Sie sie wie folgt:
    * **Gruppenarbeitsbereich**: Wählen Sie einen Arbeitsbereich in Ihrem Power BI-Mandanten aus, unter dem das Dataset erstellt wird.
    * **Datasetname**: Stellen Sie einen Datasetnamen für die Power BI-Ausgabe bereit. Verwenden Sie beispielsweise „StreamAnalyticsRealTimeFraudPBI“.
    * **Tabellenname**: Geben Sie einen Tabellennamen unter dem Dataset der Power BI-Ausgabe ein. Verwenden Sie beispielsweise „StreamAnalyticsRealTimeFraudPBI“. Derzeit darf die Power BI-Ausgabe von Stream Analytics-Aufträgen nur eine Tabelle pro Dataset aufweisen.

7. Klicken Sie auf **Erstellen**. Die Konfiguration der Ausgabe ist nun abgeschlossen.

> [!WARNING]
> Wenn Power BI bereits über ein Dataset und eine Tabelle mit demselben Namen verfügt, der in diesem Stream Analytics-Auftrag angegeben wurde, werden die vorhandenen Daten überschrieben.
> Außerdem empfiehlt es sich, dieses Dataset und diese Tabelle nicht explizit in Ihrem Power BI-Konto zu erstellen. Diese werden automatisch erstellt, wenn Sie Ihren Stream Analytics-Auftrag starten und der Auftrag damit beginnt, Power BI mit Ausgaben zu füttern. Wenn die Auftragsabfrage keine Ergebnisse generiert, werden die Datasets und die Tabelle nicht erstellt.
>
>

Das Dataset wird mit den folgenden Einstellungen erstellt:
* **defaultRetentionPolicy: BasicFIFO**: Daten sind FIFO mit maximal 200.000 Zeilen.
* **defaultMode: pushStreaming**: Unterstützt sowohl Streamingkacheln als auch herkömmliche berichtsbasierte Visualisierungen (auch „Push“ genannt).

Derzeit ist es nicht möglich, Datasets mit anderen Kennzeichen zu erstellen.

Weitere Informationen zu Power BI-Datasets finden Sie in der [Power BI-REST-API](https://msdn.microsoft.com/library/mt203562.aspx)-Referenz.


## <a name="write-query"></a>Schreiben von Abfragen
Gehen Sie zur Registerkarte **Abfrage** des Auftrags. Schreiben Sie die Abfrage, deren Ausgabe Sie in Power BI sehen möchten. Dies kann z.B. die folgende oder eine ähnliche SQL-Abfrage zum Abfangen von SIM-Betrug in der Telekommunikationsbranche sein:


```
/* Our criteria for fraud:
 Calls made from the same caller to two phone switches in different locations (for example, Australia and Europe) within five seconds */

 SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
 INTO "StreamAnalyticsRealTimeFraudPBI"
 FROM "StreamAnalyticsRealTimeFraudInput" CS1 TIMESTAMP BY CallRecTime
 JOIN "StreamAnalyticsRealTimeFraudInput" CS2 TIMESTAMP BY CallRecTime

/* Where the caller is the same, as indicated by IMSI (International Mobile Subscriber Identity) */
 ON CS1.CallingIMSI = CS2.CallingIMSI

/* ...and date between CS1 and CS2 is between one and five seconds */
 AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

/* Where the switch location is different */
 WHERE CS1.SwitchNum != CS2.SwitchNum
 GROUP BY TumblingWindow(Duration(second, 1))
```

## <a name="create-the-dashboard-in-power-bi"></a>Erstellen des Dashboards in Power BI

1. Wechseln Sie zu [Powerbi.com](https://powerbi.com) , und melden Sie sich mit Ihrem Geschäfts-, Schul- oder Unikonto an. Wenn die Stream Analytics-Auftragsabfrage Ergebnisse ausgibt, werden Sie feststellen, dass das Dataset wie hier gezeigt bereits erstellt wurde:

    ![Streamingdataset](./media/stream-analytics-power-bi-dashboard/streaming-dataset.png)

2. Wählen Sie **Kachel hinzufügen** und anschließend Ihre benutzerdefinierten Streamingdaten.

    ![Benutzerdefiniertes Streamingdataset](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

3. Wählen Sie dann Ihr Dataset aus der Liste aus.

    ![Ihr Streamingdataset](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

4. Erstellen Sie eine Visualisierungskarte. Wählen Sie dann das Feld **fraudulentcalls** aus.

    ![Betrug hinzufügen](./media/stream-analytics-power-bi-dashboard/add-fraud.png)

    Jetzt haben wir einen Betrugszähler!

    ![Betrugszähler](./media/stream-analytics-power-bi-dashboard/fraud-counter.png)

5. Fügen Sie in der Übung eine weitere Kachel hinzu. Wählen Sie dieses Mal jedoch das Liniendiagramm aus. Fügen Sie **fraudulentcalls** als Wert und **windowend** als Achse hinzu. Wie Sie im folgenden Screenshot sehen, haben wir die letzten 10 Minuten ausgewählt:

![Betrugsanrufe](./media/stream-analytics-power-bi-dashboard/fraud-calls.png)


In diesem Tutorial wird nur die Erstellung einer Art von Diagramm für ein Dataset gezeigt. Mithilfe von Power BI können Sie weitere kundenspezifische Business Intelligence Tools für Ihr Unternehmen erstellen. Ein weiteres Beispiel für ein Power BI-Dashboard sehen Sie im Video [Erste Schritte mit Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) .

Weitere Informationen zum Konfigurieren einer Power BI-Ausgabe und zur Verwendung von Power BI-Gruppen finden Sie im Abschnitt [Power BI](stream-analytics-define-outputs.md#power-bi) unter [Stream Analytics-Ausgaben: Optionen für Speicher, Analyse](stream-analytics-define-outputs.md "Stream Analytics-Ausgaben"). [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/) ist eine weitere hilfreiche Ressource.

## <a name="learn-about-limitations-and-best-practices"></a>Einschränkungen und bewährte Methoden
In Power BI werden Parallelitäts- und Durchsatzeinschränkungen genutzt. Eine Beschreibung hierzu finden Sie [auf dieser Seite zu Power BI](https://powerbi.microsoft.com/pricing "Power BI-Preise").

Derzeit können Sie Power BI etwa einmal pro Sekunde aufrufen. Das Streaming visueller Elemente unterstützt Pakete mit bis zu 15 KB. Darüber hinaus tritt beim Streaming visueller Elemente ein Fehler auf (Push funktioniert jedoch weiterhin).

Aufgrund dieser Einschränkungen eignet sich Power BI perfekt für Anwendungsfälle, bei denen Azure Stream Analytics eine erhebliche Datenlastverringerung ermöglicht.
Wir empfehlen die Verwendung rollierender oder springender Fenster, um sicherzustellen, dass der Datenpush nicht höher ist als ein Push pro Sekunde und Ihre Abfrage innerhalb der Durchsatzanforderungen liegt.

Mithilfe der folgenden Gleichung können Sie den Wert berechnen, um den Wert für Ihr Fenster in Sekunden zu berechnen:

![Gleichung1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Beispiel:
- Sie haben 1.000 Geräte, die in Intervallen von einer Sekunde Daten senden.
- Sie verwenden das Power BI Pro-SKU, das 1.000.000 Zeilen pro Stunde unterstützt.
- Sie möchten die Menge der durchschnittlichen Daten pro Gerät für Power BI veröffentlichen.

Daraus resultiert die folgende Gleichung:

![Gleichung2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Das heißt, dass wir die ursprüngliche Abfrage wie folgt ändern können:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl


### <a name="renew-authorization"></a>Erneuern der Autorisierung
Wenn das Kennwort seit der Erstellung oder letzten Authentifizierung Ihres Auftrags geändert wurde, müssen Sie Ihr Power BI-Konto erneut authentifizieren. Wenn Azure Multi-Factor Authentication auf Ihrem Azure Active Directory (Azure AD)-Mandanten konfiguriert ist, müssen Sie die Power BI-Autorisierung ebenfalls alle zwei Wochen erneuern. Wenn Sie diese nicht erneuern, erhalten Sie unter Umständen keine Auftragsausgabe, oder in den Vorgangsprotokollen tritt ein Benutzerauthentifizierungsfehler auf.

Wenn ein Auftrag zu starten versucht, nachdem das Token abgelaufen ist, wird eine Fehlermeldung angezeigt, und das Starten des Auftrags schlägt fehl. Um dieses Problem zu beheben, halten Sie den laufenden Auftrag an, und wechseln Sie zur Power BI-Ausgabe. Klicken Sie auf den Link **Autorisierung erneuern**, und starten Sie den Auftrag ab der **Letzten Beendigungszeit** neu, um Datenverlust zu vermeiden.

Nachdem die Autorisierung mit Power BI aktualisiert wurde, wird im Autorisierungsbereich eine grüne Benachrichtigung angezeigt, die darauf hinweist, dass das Problem gelöst ist.

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referenz zur Stream Analytics-Abfragesprache)
* [Azure Stream Analytics Management REST API reference](https://msdn.microsoft.com/library/azure/dn835031.aspx) (Referenz zur Azure Stream Analytics-Verwaltungs-REST-API)

