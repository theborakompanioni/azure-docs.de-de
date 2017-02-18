---
title: Twitter-Stimmungsanalyse in Echtzeit mit Stream Analytics | Microsoft Docs
description: "Erfahren Sie, wie Sie Stream Analytics für Twitter-Stimmungsanalysen in Echtzeit verwenden. Schrittweise Anleitung von der Ereignisgenerierung bis hin zu Daten im Live-Dashboard."
keywords: "Twitter-Trendanalyse in Echtzeit, Stimmungsanalyse, Analyse sozialer Medien, Beispiel für Trendanalysen"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 42068691-074b-4c3b-a527-acafa484fda2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/03/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 110bf7df8753ec83a5a8b4219891700b462d4eb1
ms.openlocfilehash: 339301772b1ee3bf22e543d4d4183adda5b54c2e

---

# <a name="social-media-analysis-real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Soziale Medienanalyse: Twitter-Stimmungsanalyse in Echtzeit in Azure Stream Analytics
Erfahren Sie, wie Sie eine Lösung für Standpunktanalysen zur Analyse sozialer Medien durch die Einbindung von Twitter-Echtzeitereignissen in Azure Event Hubs erstellen können. Sie werden eine Azure Stream Analytics-Abfrage schreiben, um die Daten zu analysieren. Anschließend werden Sie die Ergebnisse entweder zur späteren Durchsicht speichern oder ein Dashboard und [Power BI](https://powerbi.com/) verwenden, um verwertbare Daten in Echtzeit bereitzustellen.

Analysetools für soziale Medien können Unternehmen helfen, Trendthemen zu verstehen, d.h. Themen und Einstellungen mit einer hohen Anzahl von Beiträgen in sozialen Medien. Für die Standpunktanalyse – auch als *Opinion Mining* bezeichnet – werden Analysetools für soziale Medien verwendet, um Einstellungen zu einem Produkt, Meinungen usw. zu bestimmen. Die Twitter-Trendanalyse in Echtzeit ist ein gutes Beispiel, da das Hashtag-Abonnementmodell es Ihnen ermöglicht, bestimmte Schlüsselwörter zu berücksichtigen und eine Standpunktanalyse für den Feed zu entwickeln.

## <a name="scenario-sentiment-analysis-in-real-time"></a>Szenario: Standpunktanalyse in Echtzeit
Ein Unternehmen, das über eine Nachrichtenwebsite verfügt, ist daran interessiert, sich durch Seiteninhalte mit direkter Relevanz für die Leser gegenüber der Konkurrenz einen Vorteil zu verschaffen. Das Unternehmen verwendet eine Analyse sozialer Medien für Themen mit Relevanz für Leser und führt dazu Standpunktanalysen von Twitter-Daten in Echtzeit durch. Insbesondere benötigt das Unternehmen Echtzeitanalysen des Tweet-Umfangs und der Stimmung im Hinblick auf wichtige Themen, um zu erkennen, welche Themen sich auf Twitter in Echtzeit zu Trendthemen entwickeln. Im Wesentlichen wird also ein Analysemodul für Standpunktanalysen basierend auf diesem Feed in sozialen Medien benötigt.

## <a name="prerequisites"></a>Voraussetzungen
* Twitter-Konto und [OAuth-Zugriffstoken](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)
* [TwitterClient.zip](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip) aus dem Microsoft Download Center
* Optional: Quellcode für Twitter-Client von [GitHub](https://aka.ms/azure-stream-analytics-twitterclient)

## <a name="create-an-event-hub-input-and-a-consumer-group"></a>Erstellen einer Event Hub-Eingabe und einer Consumergruppe
Die Beispielanwendung generiert Ereignisse und überträgt sie mithilfe von Push an eine Event Hub-Instanz (kurz ein Event Hub). Service Bus-Event Hubs sind die bevorzugte Methode der Ereigniserfassung für Stream Analytics. Weitere Informationen finden Sie unter „Event Hubs“ in der [Azure Dokumentation zu Service Bus](/azure/service-bus/).

Führen Sie die folgenden Schritte aus, um einen Event Hub zu erstellen.

1. Klicken Sie im Azure-Portal auf **NEU** > **APP SERVICES** > **SERVICE BUS** > **EVENT HUB** > **SCHNELLERFASSUNG**, und geben Sie einen Namen, eine Region und einen neuen oder vorhandenen Namespace an.  
2. Als bewährte Methode sollte jeder Stream Analytics-Auftrag aus einer einzelnen Event Hubs-Consumergruppe lesen. Wir führen Sie später durch den Prozess des Erstellens einer Consumergruppe. Erfahren Sie mehr über Verbrauchergruppen in der [Übersicht über Event Hubs](https://msdn.microsoft.com/library/azure/dn836025.aspx). Um eine Consumergruppe zu erstellen, wechseln Sie zu dem neu erstellten Event Hub, klicken Sie auf die Registerkarte **CONSUMERGRUPPEN** und anschließend auf **ERSTELLEN** am unteren Rand der Seite. Geben Sie anschließend einen Namen für die Consumergruppe ein.
3. Um Zugriff auf den Event Hub zu gewähren, müssen wir eine Richtlinie für gemeinsamen Zugriff erstellen. Klicken Sie auf die Registerkarte **KONFIGURIEREN** Ihres Event Hubs.
4. Erstellen Sie unter **RICHTLINIEN FÜR GEMEINSAMEN ZUGRIFF** eine neue Richtlinie mit Berechtigungen zum **VERWALTEN**.

   ![Unter "Gemeinsame Zugriffsrichtlinien" können Sie eine neue Richtlinie mit Verwaltungsberechtigungen erstellen.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-ananlytics-shared-access-policies.png)

5. Klicken Sie unten auf der Seite auf **Speichern** .
6. Wechseln Sie zum **DASHBOARD**, klicken Sie unten auf der Seite auf **VERBINDUNSINFORMATIONEN**, und kopieren und speichern Sie anschließend die Verbindungsinformationen. (Verwenden Sie das Symbol „Kopieren“, das unter dem Suchsymbol angezeigt wird.)

## <a name="configure-and-start-the-twitter-client-application"></a>Konfigurieren und Starten der Twitter-Clientanwendung
Wir haben eine Clientanwendung bereitgestellt, die über die [Streaming-APIs von Twitter](https://dev.twitter.com/streaming/overview) eine Verbindung mit Twitter-Daten herstellt, um Tweet-Ereignisse zu einer parametrisierten Reihe von Themen zu sammeln. Das Open Source-Tool [Sentiment140](http://help.sentiment140.com/) wird dazu verwendet, jedem Tweet einen Stimmungswert zuzuweisen.

* 0 = negativ
* 2 = neutral
* 4 = positiv

Anschließend werden Tweet-Ereignisse mithilfe von Push an den Event Hub übertragen.  

Gehen Sie folgendermaßen vor, um die Anwendung einzurichten:

1. [Laden Sie die TwitterClient-Lösung herunter](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip).
2. Öffnen Sie „TwitterClient.exe.config“, und ersetzen Sie „oauth_consumer_key“, „oauth_consumer_secret“, „oauth_token“ und „oauth_token_secret“ durch Twitter-Token mit Ihren Werten.  

   [Schritte zum Generieren eines OAuth-Zugriffstokens](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)  

   Beachten Sie, dass Sie zum Generieren eines Tokens eine leere Anwendung erstellen müssen.  

3. Ersetzen Sie die Werte EventHubConnectionString und EventHubName in „TwitterClient.exe.config“ durch die Verbindungszeichenfolge und den Namen Ihres Event Hubs. Die zuvor kopierte Verbindungszeichenfolge enthält sowohl die Verbindungszeichenfolge als auch den Namen Ihres Event Hubs. Trennen Sie die beiden Angaben also, und geben Sie sie in die richtigen Felder ein. Ziehen Sie beispielsweise die folgende Verbindungszeichenfolge in Betracht:  
   
   `Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey;EntityPath=yourhub`
   
   Die Datei „TwitterClient.exe.config“ sollte Ihre Einstellungen wie im folgenden Beispiel enthalten:
   
   ```
     add key="EventHubConnectionString" value="Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey"
     add key="EventHubName" value="yourhub"
   ```
   
   Beachten Sie unbedingt, dass der Text „EntityPath=“ **nicht** im EventHubName-Wert enthalten ist.
   
4. *Optional:* Passen Sie die Schlüsselwörter für die Suche an.  Standardmäßig sucht diese Anwendung nach „Azure,Skype,XBox,Microsoft,Seattle“.  Falls gewünscht, können Sie die Werte für **twitter_keywords** in „TwitterClient.exe.config“ anpassen.
5. Führen Sie „TwitterClient.exe“ aus, um Ihre Anwendung zu starten. Sie sehen nun, wie Ereignisse mit den **CreatedAt**-, **Topic**- und **SentimentScore**-Werten an Ihren Event Hub gesendet werden.
   
   ![Stimmungsanalyse: SentimentScore-Werte, die an einen Event Hub gesendet werden.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-sentiment-output-to-event-hub.png)

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags
Nun, da wir einen Datenstrom von Tweet-Ereignissen haben, können wir einen Stream Analytics-Auftrag einrichten, um diese Ereignisse in Echtzeit zu analysieren.

### <a name="provision-a-stream-analytics-job"></a>Bereitstellen eines Stream Analytics-Auftrags
1. Klicken Sie im [Azure-Portal](https://manage.windowsazure.com/) auf **NEU** > **DATA SERVICES** (DATENDIENSTE) > **STREAM ANALYTICS** > **SCHNELLSTART**.
2. Geben Sie die folgenden Werte an, und klicken Sie dann auf **STREAM ANALYTICS-AUFTRAG ERSTELLEN**:

   * **AUFTRAGSNAME**: Geben Sie einen Auftragsnamen ein.
   * **REGION**: Wählen Sie die Region aus, in der der Auftrag ausgeführt werden soll. Ziehen Sie es in Betracht, den Auftrag und das Event Hub in derselben Region zu platzieren, um für eine bessere Leistung zu sorgen und sicherzustellen, dass Sie nicht für die Übertragung von Daten zwischen Regionen bezahlen.
   * **SPEICHERKONTO**: Wählen Sie das Azure-Speicherkonto aus, das Sie zum Speichern von Überwachungsdaten für alle Stream Analytics-Aufträge verwenden möchten, die innerhalb dieser Region ausgeführt werden. Sie haben die Möglichkeit, ein vorhandenes Speicherkonto auszuwählen oder ein neues zu erstellen.   

3. Klicken Sie im linken Bereich auf **STREAM ANALYTICS**, um die Stream Analytics-Aufträge aufzulisten.  
   
   ![Symbol des Stream Analytics-Diensts](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-service-icon.png)
   
   Der neue Auftrag wird mit dem Status **ERSTELLT**aufgeführt. Beachten Sie, dass die Schaltfläche **START** am unteren Seitenrand deaktiviert ist. Sie müssen die Auftragseingabe, -ausgabe und -abfrage konfigurieren, bevor Sie den Auftrag starten können.


### <a name="specify-job-input"></a>Festlegen der Auftragseingabe

1. Klicken Sie in Ihrem Stream Analytics-Auftrag auf **EINGABEN** am oberen Rand der Seite, und klicken Sie anschließend auf **EINGABE HINZUFÜGEN**. Das aufgerufene Dialogfeld führt Sie durch eine Reihe von Schritten, um Ihre Eingabe einzurichten.
2. Klicken Sie auf **DATENSTROM** und anschließend auf die rechte Schaltfläche.
3. Klicken Sie auf **EVENT HUB**, und dann mit der rechten Maustaste.
4. Geben Sie die folgenden Werte auf der dritten Seite ein, oder wählen Sie sie aus:  
   
   * **EINGABEALIAS**: Geben Sie einen Anzeigenamen für diese Auftragseingabe ein, wie z.B. *TwitterStream*. Beachten Sie, dass Sie diesen Namen später in der Abfrage verwenden werden.
   * **EVENT HUB**: Wenn sich der Event Hub, den Sie erstellt haben, im gleichen Abonnement wie der Stream Analytics-Auftrag befindet, wählen Sie den Namespace aus, in dem sich der Event Hub befindet.
      * Wenn sich Ihr Event Hub in einem anderen Abonnement befindet, klicken Sie auf **Use Event Hub from Another Subscription** (Event Hub aus einem anderen Abonnement verwenden) aus, und geben Sie manuell die Informationen für **SERVICE BUS-NAMESPACE**, **EVENT HUB-NAME**, **NAME DER EVENT HUB-RICHTLINIE**, **SCHLÜSSEL DER EVENT HUB-RICHTLINIE** und **EVENT HUB-PARTITIONSANZAHL** ein.
   * **EVENT HUB-NAME**: Wählen Sie den Namen des Event Hubs aus.
   * **NAME DER EVENT HUB-RICHTLINIE**: Wählen Sie die Event Hub-Richtlinie aus, die Sie zuvor in diesem Lernprogramm erstellt haben.
   * **EVENT HUB-CONSUMERGRUPPE**: Geben Sie den Namen der Consumergruppe ein, die Sie zuvor in diesem Tutorial erstellt haben.
   
5. Klicken Sie auf die rechte Taste.
6. Geben Sie die folgenden Werte an:  
   
   * **EVENT SERIALIZER FORMAT**: JSON
   * **CODIERUNG**: UTF8
  
7. Klicken Sie auf das **Kontrollkästchen**, um diese Quelle hinzuzufügen und um zu überprüfen, ob Stream Analytics erfolgreich mit dem Event Hub verbunden werden kann.

### <a name="specify-job-query"></a>Festlegen der Auftragsabfrage
Stream Analytics unterstützt ein einfaches, deklaratives Abfragemodell, das Transformationen beschreibt. Weitere Informationen zur Sprache finden Sie in der [Azure Stream Analytics-Abfragesprachreferenz](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Dieses Lernprogramm hilft Ihnen beim Erstellen und Testen mehrerer Abfragen über Twitter-Daten.

#### <a name="sample-data-input"></a>Beispiel-Dateneingabe
Um Ihre Abfrage mit den tatsächlichen Auftragsdaten zu überprüfen, können Sie die Funktion **BEISPIELDATEN** verwenden, um Ereignisse aus Ihrem Datenstrom zu extrahieren und eine JSON-Datei der Ereignisse zum Testen zu erstellen.

1. Wählen Sie Ihre Event Hub-Eingabe aus, und klicken Sie am unteren Seitenrand auf **BEISPIELDATEN**.
2. Geben Sie im daraufhin angezeigten Dialogfeld eine **STARTZEIT** für das Sammeln der Daten und die **DAUER** ein, während der zusätzliche Daten verarbeitet werden sollen.
3. Klicken Sie auf die Schaltfläche **DETAILS** und dann auf den Link **Klicken Sie hier**, um die generierte JSON-Datei herunterzuladen und zu speichern.

#### <a name="pass-through-query"></a>Pass-Through-Abfrage
Wir beginnen mit einer einfachen Pass-Through-Abfrage, die alle Felder in einem Ereignis projiziert.

1. Klicken Sie auf **ABFRAGE** oben auf der Seite des Stream Analytics-Auftrags.
2. Ersetzen Sie im Code-Editor die ursprüngliche Abfragevorlage mit Folgendem:  
   
   `SELECT * FROM TwitterStream`
   
   Stellen Sie sicher, dass der Name der Eingabequelle dem Namen der Eingabe entspricht, die Sie zuvor angegeben haben.
   
3. Klicken Sie im Abfrage-Editor auf **Test** .
4. Navigieren Sie zu Ihrer JSON-Beispieldatei.
5. Klicken Sie auf die Schaltfläche **PRÜFEN**. Die Ergebnisse werden unterhalb der Abfragedefinition angezeigt.
   
   ![Unterhalb der Abfragedefinition angezeigte Ergebnisse](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sentiment-by-topic.png)
   
#### <a name="count-of-tweets-by-topic-tumbling-window-with-aggregation"></a>Anzahl der Tweets nach Thema: rollierendes Fenster mit Aggregation
Um die Anzahl der Erwähnungen verschiedener Themen zu vergleichen, verwenden wir ein [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx), um alle fünf Sekunden die Anzahl der Erwähnungen jedes Themas abzurufen.

1. Ändern Sie die Abfrage im Code-Editor in:  
   
   ```
     SELECT System.Timestamp as Time, Topic, COUNT(*)
     FROM TwitterStream TIMESTAMP BY CreatedAt
     GROUP BY TUMBLINGWINDOW(s, 5), Topic
   ```
   
   Diese Abfrage gibt mit dem Schlüsselwort **TIMESTAMP BY** ein Zeitstempelfeld für die Nutzlast an, das für die zeitliche Berechnung verwendet wird. Wird dieses Feld nicht angegeben, dann würde der Vorgang die Zeit verwenden, zu der jedes Ereignis beim Event Hub eingeht.  Weitere Informationen erhalten Sie im Abschnitt „Arrival Time Vs Application Time“ (Eingangszeit im Vgl. zu Anwendungszeit) in der [Stream Analytics Query Language Reference (Stream Analytics-Abfragereferenz)](https://msdn.microsoft.com/library/azure/dn834998.aspx).
   
   Diese Abfrage greift zudem mithilfe der Eigenschaft **System.Timestamp** jeweils auf einen Zeitstempel für das Ende der einzelnen Fenster zu.
   
2. Klicken Sie im Abfrage-Editor auf **Erneut ausführen** , um die Ergebnisse der Abfrage anzuzeigen.

#### <a name="identify-trending-topics-sliding-window"></a>Identifizieren von Trendthemen: gleitendes Fenster
Zum Identifizieren von Trendthemen suchen wir nach Themen, die einen Schwellenwert für Erwähnungen in einem bestimmten Zeitraum überschreiten. Für die Zwecke dieses Tutorials suchen wir mithilfe eines [SlidingWindow](https://msdn.microsoft.com/library/azure/dn835051.aspx)nach Themen, die in den letzten fünf Sekunden mehr als 20-mal erwähnt werden.

1. Ändern Sie die Abfrage im Code-Editor in:  
   
   ```
     SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
     FROM TwitterStream TIMESTAMP BY CreatedAt
     GROUP BY SLIDINGWINDOW(s, 5), topic
     HAVING COUNT(*) > 20
   ```
   
2. Klicken Sie im Abfrage-Editor auf **Erneut ausführen** , um die Ergebnisse der Abfrage anzuzeigen.  
   
   ![Gleitendes Fenster – Abfrageausgabe](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-query-output.png)
   
#### <a name="count-of-mentions-and-sentiment-tumbling-window-with-aggregation"></a>Anzahl der Erwähnungen und Stimmung: Rollierendes Fenster mit Aggregation
Die letzte Abfrage, die wir testen, verwendet **TumblingWindow**, um alle fünf Sekunden für jedes Thema die Anzahl von Erwähnungen sowie die Durchschnitts-, Mindest-, Maximal- und Standardabweichung von der Bewertung der Stimmung abzurufen.

1. Ändern Sie die Abfrage im Code-Editor in:  
   
   ```
     SELECT System.Timestamp as Time, Topic, COUNT(*), AVG(SentimentScore), MIN(SentimentScore),
     Max(SentimentScore), STDEV(SentimentScore)
     FROM TwitterStream TIMESTAMP BY CreatedAt
     GROUP BY TUMBLINGWINDOW(s, 5), Topic
   ```     

2. Klicken Sie im Abfrage-Editor auf **Erneut ausführen** , um die Ergebnisse der Abfrage anzuzeigen.
3. Dies ist die Abfrage, die wir für unser Dashboard verwenden.  Klicken Sie unten auf der Seite auf **Speichern** .

## <a name="create-output-sink"></a>Erstellen der Ausgabetabelle
Nun, da wir einen Datenstrom, eine Event Hub-Eingabe zum Erfassen von Ereignissen und eine Abfrage zur Ausführung einer Umwandlung über den Stream definiert haben, ist der letzte Schritt die Definition einer Ausgabetabelle für den Auftrag.  Die aggregierten Tweet-Ereignisse aus unserer Auftragsabfrage schreiben wir in Azure Blob Storage.  Sie können die Ergebnisse je nach den spezifischen Anforderungen Ihrer Anwendung auch mithilfe von Push in Azure SQL-Datenbank, Azure Table Storage oder Event Hubs übertragen.

Befolgen Sie die nachstehenden Schritte zum Erstellen eines Containers für Blob Storage, wenn Sie noch keinen erstellt haben:

1. Verwenden Sie ein vorhandenes Speicherkonto, oder erstellen Sie ein neues, indem Sie auf **NEU** > **DATA SERVICES** (DATENDIENSTE) > **SPEICHER** > **SCHNELLERFASSUNG** klicken und anschließend die Anweisungen auf dem Bildschirm befolgen.
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

## <a name="start-job"></a>Auftrag starten
Da wir Eingabe, Abfrage und Ausgabe für den Auftrag angegeben haben, können wir den Stream Analytics-Auftrag nun starten.

1. Klicken Sie im **DASHBOARD** des Auftrags am unteren Rand der Seite auf **STARTEN**.
2. Wählen Sie im daraufhin angezeigten Dialogfeld **AUFTRAGSSTARTZEIT** aus, und klicken Sie dann auf das **Häkchen** am unteren Rand des Dialogfelds. Der Status ändert sich in **Wird gestartet** und wechselt nach kurzer Zeit zu **Wird ausgeführt**.

## <a name="view-output-for-sentiment-analysis"></a>Anzeigen der Ausgabe für die Stimmungsanalyse
Nachdem Ihr Auftrag ausgeführt wird und den Twitter-Datenstrom in Echtzeit verarbeitet, wählen Sie aus, wie die Ausgabe für die Standpunktanalyse angezeigt werden soll. Verwenden Sie ein Tool wie [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/) oder [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction), um die Auftragsausgabe in Echtzeit anzuzeigen. Ab hier können Sie [Power BI](https://powerbi.com/) dazu verwenden, Ihre Anwendung zu erweitern, um ein benutzerdefiniertes Dashboard wie das im nachstehenden Screenshot einzuschließen.

![Soziale Medienanalyse: Ausgabe der Stimmungsanalyse (Opinion Mining) mit Stream Analytics in einem Power BI-Dashboard.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-output-power-bi.png)

## <a name="get-support"></a>Support
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)




<!--HONumber=Feb17_HO1-->


