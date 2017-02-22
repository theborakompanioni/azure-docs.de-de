---
title: Power BI-Dashboard auf Stream Analytics | Microsoft Docs
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
ms.date: 02/01/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: c60d620d9fc91cfef2340b645e15ed19d3d1f4e1
ms.openlocfilehash: 39b2852c39e8d3eb4687cbcfd9116e1d66fc9d9d


---
# <a name="stream-analytics--power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Stream Analytics und Power BI: Ein Dashboard mit Echtzeitanalyse von Streamingdaten
Azure Stream Analytics ermöglicht Ihnen die Nutzung von Microsoft Power BI, eines der führenden Business Intelligence-Tools. Erfahren Sie, wie Sie Azure Stream Analytics verwenden, um hohe Volumen von Streamingdaten zu analysieren und diese in einem Power BI-Dashboard mit Echtzeitanalyse darzustellen.

Verwenden Sie [Microsoft Power BI](https://powerbi.com/) , um ohne großen Zeitaufwand ein Live-Dashboard zu erstellen. [Betrachten Sie ein Video zur Veranschaulichung des Szenarios](https://www.youtube.com/watch?v=SGUpT-a99MA).

In diesem Artikel erfahren Sie, wie Sie eigene benutzerdefinierte Business Intelligence-Tools erstellen können, indem Sie Power BI als Ausgabe für Ihre Aufträge in Azure Stream Analytics verwenden und dazu ein Echtzeitdashboard nutzen.

## <a name="prerequisites"></a>Voraussetzungen
* Microsoft Azure-Konto
* Geschäfts- oder Schulkonto für Power BI
* Schließen Sie das Erste-Schritte-Szenario [Betrugserkennung in Echtzeit](stream-analytics-real-time-fraud-detection.md). Dieser Artikel baut auf diesem Workflow auf und fügt eine Power BI-Streaming-Dataset-Ausgabe hinzu.

## <a name="add-power-bi-output"></a>Power BI-Ausgabe hinzufügen
Da nun eine Eingabe für den Auftrag vorhanden ist, kann eine Ausgabe zu Power BI definiert werden. Aktivieren Sie das Kästchen in der Mitte des Dashboardauftrags **Ausgaben**. Klicken Sie dann auf die bekannte **+ Hinzufügen**-Schaltfläche, und erstellen Sie Ihre Ausgabe.

![Ausgabe hinzufügen](./media/stream-analytics-power-bi-dashboard/create-pbi-output.png)

Stellen Sie den **Ausgabealias** bereit – Sie können jeden Ausgabealias verwenden, auf den Sie auf einfache Weise verweisen können. Dieser Ausgabealias ist besonders hilfreich, wenn Sie sich mehrere Ausgaben für den Auftrag benötigen. In diesem Fall müssen Sie in der Abfrage auf diesen Alias verweisen. Verwenden Sie beispielsweise den Aliasausgabewert „StreamAnalyticsRealTimeFraudPBI“.

Klicken Sie auf die Schaltfläche **Autorisieren**.

![Autorisierung hinzufügen](./media/stream-analytics-power-bi-dashboard/pbi-authorize.png)

Daraufhin wird ein Fenster angezeigt, in dem Sie Ihre Azure-Anmeldeinformationen (Geschäfts- oder Schulkonto) eingeben können, und dort erhalten Sie Azure-Auftragszugriff auf Ihren Power BI-Bereich.

![Autorisierungsfelder](./media/stream-analytics-power-bi-dashboard/authorize-area.png)

Das Fenster für die Autorisierung wird nicht mehr angezeigt, nachdem es ausgefüllt wurde, und der neue Ausgabebereich enthält Felder für den Dataset- und Tabellennamen.

![PBI-Arbeitsbereich](./media/stream-analytics-power-bi-dashboard/pbi-workspace.png)

Definieren Sie sie wie folgt:
* **Gruppenarbeitsbereich** – wählen Sie einen Arbeitsbereich in Ihrem Power BI-Mandanten aus, unter dem das Dataset erstellt wird.
* **Datasetname** – Stellen Sie einen Datasetnamen für die Power BI-Ausgabe bereit. Verwenden Sie beispielsweise „StreamAnalyticsRealTimeFraudPBI“.
* **Tabellenname** – Geben Sie einen Tabellennamen unter dem Dataset der Power BI-Ausgabe ein. Nehmen wir einfach „StreamAnalyticsRealTimeFraudPBI“. Derzeit darf die Power BI-Ausgabe von Stream Analytics-Aufträgen nur eine Tabelle pro Dataset aufweisen.

Klicken Sie auf **Erstellen**, und damit ist Ihre Ausgabekonfiguration abgeschlossen.

> [!WARNING]
> Wenn Power BI bereits über ein Dataset und eine Tabelle mit dem gleichen Namen verfügt, den Sie in diesem Stream Analytics-Auftrag angegeben haben, beachten Sie bitte, dass die vorhandenen Daten überschrieben werden!
> Sie sollten dieses Dataset und diese Tabelle auch nicht explizit in Ihrem Power BI-Konto erstellen. Sie werden automatisch erstellt, wenn Sie Ihren Stream Analytics-Auftrag starten und der Auftrag damit beginnt, Power BI mit Ausgaben zu füttern. Wenn die Auftragsabfrage keine Ergebnisse generiert, werden die Datasets und die Tabelle nicht erstellt.
> 
> 

Das Dataset wird mit den folgenden Einstellungen erstellt:
* defaultRetentionPolicy: BasicFIFO – Daten sind FIFO, maximale Zeilenanzahl 200k
* defaultMode: pushStreaming: unterstützt sowohl Streamingkacheln als auch herkömmliche berichtsbasierte Visualisierungen (auch „Push“ genannt)
* Erstellen von Datasets mit anderen Flags wird zu diesem Zeitpunkt nicht unterstützt

Weitere Informationen zu Power BI-Datasets finden Sie in der [Power BI-REST-API](https://msdn.microsoft.com/library/mt203562.aspx)-Referenz.


## <a name="write-query"></a>Schreiben von Abfragen
Gehen Sie zur Registerkarte **Abfrage** des Auftrags. Schreiben Sie die Abfrage, deren Ausgabe Sie in Power BI sehen möchten. Es kann z.B. die folgende SQL-Abfrage zum Abfangen von SIM-Betrug in der Telekommunikationsbranche sein:


```
/* Our criteria for fraud:
 Calls made from the same caller to two phone switches in different locations (e.g. Australia and Europe) within 5 seconds) */

 SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
 INTO "StreamAnalyticsRealTimeFraudPBI"
 FROM "StreamAnalyticsRealTimeFraudInput" CS1 TIMESTAMP BY CallRecTime
 JOIN "StreamAnalyticsRealTimeFraudInput" CS2 TIMESTAMP BY CallRecTime
   
/* Where the caller is the same, as indicated by IMSI (International Mobile Subscriber Identity) */
 ON CS1.CallingIMSI = CS2.CallingIMSI

/* ...and date between CS1 and CS2 is between 1 and 5 seconds */
 AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

/* Where the switch location is different */
 WHERE CS1.SwitchNum != CS2.SwitchNum
 GROUP BY TumblingWindow(Duration(second, 1))
```

## <a name="create-the-dashboard-in-power-bi"></a>Erstellen des Dashboards in Power BI

Wechseln Sie zu [Powerbi.com](https://powerbi.com) , und melden Sie sich mit Ihrem Geschäfts- oder Schulkonto an. Wenn die Stream Analytics-Auftragsabfrage Ergebnisse ausgibt, werden Sie feststellen, dass das Dataset bereits erstellt wurde:

![Streamingdataset](./media/stream-analytics-power-bi-dashboard/streaming-dataset.png)

Klicken Sie auf „Kachel hinzufügen“, und wählen Sie Ihre benutzerdefinierten Streamingdaten.

![Benutzerdefiniertes Streamingdataset](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

Wählen Sie dann Ihr Dataset aus der Liste:

![Ihr Streamingdataset](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

Nun erstellen Sie eine Visualisierungskarte, und wählen Sie das Feld „fraudulentcalls“ aus.

![Betrug hinzufügen](./media/stream-analytics-power-bi-dashboard/add-fraud.png)

Jetzt haben wir einen Betrugszähler!

![Betrugszähler](./media/stream-analytics-power-bi-dashboard/fraud-counter.png)

Fügen Sie in der Übung eine weitere Kachel hinzu, aber wählen Sie nun ein Liniendiagramm. Fügen Sie „fraudulentcalls“ als Wert und die Achse als Fensterende hinzu. Wir haben die letzten 10 Minuten ausgewählt:

![Betrugsanrufe](./media/stream-analytics-power-bi-dashboard/fraud-calls.png)


Beachten Sie, dass in diesem Tutorial nur die Erstellung einer Art von Diagramm für ein Dataset gezeigt wurde. Mithilfe von Power BI können Sie weitere kundenspezifische Business Intelligence Tools für Ihr Unternehmen erstellen. Ein weiteres Beispiel für ein Power BI-Dashboard sehen Sie im Video [Erste Schritte mit Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) .

Weitere Informationen zum Konfigurieren einer Power BI-Ausgabe und zur Verwendung von Power BI-Gruppen finden Sie im Abschnitt [Power BI](stream-analytics-define-outputs.md#power-bi) unter [Stream Analytics-Ausgaben: Optionen für Speicher, Analyse](stream-analytics-define-outputs.md "Understanding Stream Analytics outputs"). Eine weitere nützliche Ressource, um mehr über das Erstellen von Dashboards mit Power BI zu erfahren, sind die [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="limitations-and-best-practices"></a>Einschränkungen und bewährte Methoden
In Power BI werden Parallelitäts- und Durchsatzeinschränkungen genutzt. Eine Beschreibung finden Sie hier: [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing "Power BI Pricing").

Derzeit können Sie Power BI etwa einmal pro Sekunde aufrufen. Das Streaming von visuellen Elementen unterstützt Pakete bis zu einer Größe von 15KB. Darüber hinaus tritt beim Streaming von visuellen Elementen ein Fehler auf (aber Push ist weiterhin funktionsfähig).

Aufgrund dieser Einschränkungen eignet sich Power BI perfekt für Anwendungsfälle, bei denen Azure Stream Analytics eine erhebliche Datenlastverringerung ermöglicht.
Es wird die Verwendung von TumblingWindow oder HoppingWindow empfohlen, um sicherzustellen, dass beim Datenpush maximal 1 Push pro Sekunde erfolgt und dass die Abfrage innerhalb der Durchsatzanforderungen liegt. Mit der folgenden Gleichung können Sie den Wert für das Fenster in Sekunden berechnen:

![Gleichung1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Beispiel: Wenn 1.000 Geräte jede Sekunde Daten senden und Sie das Power BI Pro-SKU verwenden, das 1.000.000 Zeilen pro Stunde unterstützt, und die durchschnittlichen Daten pro Gerät in Power BI berechnen möchten, können Sie maximal alle 4 Sekunden einen Push pro Gerät durchführen (wie im Folgenden gezeigt):  

![Gleichung2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Das bedeutet, dass die ursprüngliche Abfrage wie folgt geändert wird:

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
Sie müssen Ihr Power BI-Konto erneut authentifizieren, wenn das Kennwort seit der Erstellung oder letzten Authentifizierung Ihres Auftrags geändert wurde. Wenn Multi-Factor Authentication (MFA) auf Ihrem Azure Active Directory (AAD)-Mandanten konfiguriert ist, müssen Sie auch Power BI-Autorisierung alle 2 Wochen erneuern. Dieses Problem zeigt sich daran, dass keine Auftragsausgabe erfolgt und in den Vorgangsprotokollen ein Benutzerauthentifizierungsfehler angezeigt wird.

Wenn ein Auftrag zu starten versucht, während das Token abgelaufen ist, wird eine Fehlermeldung angezeigt und das Starten des Auftrags schlägt fehl. Um dieses Problem zu beheben, halten Sie den laufenden Auftrag an, und wechseln Sie zur Power BI-Ausgabe.  Klicken Sie auf den Link "Autorisierung erneuern", und starten Sie den Auftrag ab dem letzten Anhaltepunkt neu, um Datenverlust zu vermeiden. Sobald die Autorisierung mit Power BI aktualisiert wurde, wird im Autorisierungsbereich eine grüne Warnung angezeigt, um wiederzugeben, dass das Problem gelöst ist.

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)




<!--HONumber=Feb17_HO1-->


