---
title: Twitter-Standpunktanalyse in Echtzeit mit Azure Stream Analytics | Microsoft-Dokumentation
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
ms.date: 03/28/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 7f8b63c22a3f5a6916264acd22a80649ac7cd12f
ms.openlocfilehash: d4fae06cb240c1687b059ae991b8d09f94e2ffc3
ms.lasthandoff: 05/01/2017

---

# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Twitter-Standpunktanalyse in Echtzeit in Azure Stream Analytics

Erfahren Sie, wie Sie eine Lösung für Standpunktanalysen zur Analyse sozialer Medien durch die Einbindung von Twitter-Echtzeitereignissen in Azure Event Hubs erstellen können. In diesem Szenario schreiben Sie eine Azure Stream Analytics-Abfrage, um die Daten zu analysieren. Anschließend speichern Sie die Ergebnisse entweder zur späteren Verwendung oder verwenden ein Dashboard und [Power BI](https://powerbi.com/), um verwertbare Daten in Echtzeit bereitzustellen.

Analysetools für soziale Medien können Unternehmen helfen, Trendthemen zu verstehen. Bei Trendthemen handelt es sich um Themen und Einstellungen mit einer hohen Anzahl von Beiträgen in sozialen Medien. Für die Standpunktanalyse – auch als *Opinion Mining* bezeichnet – werden Analysetools für soziale Medien verwendet, um Einstellungen zu einem Produkt, Meinungen usw. zu bestimmen.

Die Twitter-Trendanalyse in Echtzeit ist ein gutes Beispiel für ein Analysetool, da das Hashtag-Abonnementmodell es Ihnen ermöglicht, bestimmte Schlüsselwörter zu berücksichtigen und eine Standpunktanalyse für den Feed zu entwickeln.

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a>Szenario: Standpunktanalyse für soziale Medien in Echtzeit

Ein Unternehmen, das über eine Nachrichtenwebsite verfügt, ist daran interessiert, sich durch Seiteninhalte mit direkter Relevanz für die Leser gegenüber der Konkurrenz einen Vorteil zu verschaffen. Das Unternehmen verwendet eine Analyse sozialer Medien für Themen mit Relevanz für Leser und führt dazu Standpunktanalysen von Twitter-Daten in Echtzeit durch.

Insbesondere benötigt das Unternehmen Echtzeitanalysen des Tweet-Umfangs und der Stimmung im Hinblick auf wichtige Themen, um zu erkennen, welche Themen sich auf Twitter in Echtzeit zu Trendthemen entwickeln. Anders ausgedrückt: Es wird also ein Analysemodul für Standpunktanalysen basierend auf diesem Feed in sozialen Medien benötigt.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement
* Ein Twitter-Konto und [OAuth-Zugriffstoken](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)
* Die [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip)-Datei von GitHub
* Optional: Quellcode für den Twitter-Client von [GitHub](https://aka.ms/azure-stream-analytics-twitterclient)

## <a name="create-an-event-hub-input"></a>Erstellen einer Event Hub-Eingabe

Die Beispielanwendung generiert Ereignisse und überträgt sie per Push an eine Event Hub-Instanz (kurz: einen Event Hub). Azure Service Bus Event Hubs sind die bevorzugte Methode der Ereigniserfassung für Stream Analytics. Weitere Informationen finden Sie in der Event Hub-Dokumentation unter [Azure-Dokumentation zu Service Bus](/azure/service-bus/).

### <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

Führen Sie die folgenden Schritte aus, um einen Event Hub zu erstellen:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **NEU**, geben Sie **Event Hubs** ein, und wählen Sie dann aus den Suchergebnissen **Event Hubs** aus. Klicken Sie anschließend auf **Erstellen**.

2. Geben Sie einen Namen für den Event Hub an, und erstellen Sie dann eine Ressourcengruppe. Wir haben `socialtwitter-eh` bzw. `socialtwitter-rg` angegeben. Wählen Sie die Option, um das Konto auf dem Dashboard anzuheften, und klicken Sie dann auf die Schaltfläche **Erstellen**.

3. Wählen Sie nach Abschluss der Bereitstellung den Event Hub aus. Wählen Sie anschließend unter **Entitäten** die Option **Event Hubs**.

4. Wählen Sie zum Erstellen des Event Hubs die Schaltfläche **+ Event Hub**. Geben Sie Ihren Namen erneut ein (unser lautete `socialtwitter-eh`), und wählen Sie dann **Erstellen**.

5. Um Zugriff auf den Event Hub zu gewähren, müssen wir eine Richtlinie für gemeinsamen Zugriff erstellen. Wählen Sie den Event Hub und dann unter **Einstellungen** die Option **Richtlinien für gemeinsamen Zugriff**.

6. Erstellen Sie unter **Richtlinien für gemeinsamen Zugriff** eine Richtlinie mit Berechtigungen zum **VERWALTEN**, indem Sie **+ Hinzufügen** wählen. Benennen Sie die Richtlinie, aktivieren Sie **VERWALTEN**, und wählen Sie dann **Erstellen**.

7. Klicken Sie auf die neue Richtlinie, nachdem sie erstellt wurde, und wählen Sie dann die Schaltfläche „Kopieren“ für die Entität **VERBINDUNGSZEICHENFOLGE – PRIMÄRSCHLÜSSEL**. Wir benötigen sie später in der Übung. Kehren Sie dann zum Dashboard zurück.

![Endpunkte der SAS-Richtlinie](./media/stream-analytics-twitter-sentiment-analysis-trends/keysandendpoints.png)

## <a name="configure-and-start-the-twitter-client-application"></a>Konfigurieren und Starten der Twitter-Clientanwendung

Wir haben eine Clientanwendung erstellt, die über die [Streaming-APIs von Twitter](https://dev.twitter.com/streaming/overview) eine Verbindung mit Twitter-Daten herstellt, um Tweet-Ereignisse zu einer parametrisierten Reihe von Themen zu sammeln. Das Open Source-Tool [Sentiment140](http://help.sentiment140.com/) weist wie folgt jedem Tweet einen Stimmungswert zu:

* 0 = negativ
* 2 = neutral
* 4 = positiv

Anschließend werden Tweet-Ereignisse mithilfe von Push an den Event Hub übertragen.  

### <a name="set-up-the-application"></a>Einrichten der Anwendung
 Gehen Sie folgendermaßen vor, um die Anwendung einzurichten:

1. [Laden Sie die Datei „TwitterWPFClient.zip“ herunter](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip), und entzippen Sie sie.

2. Führen Sie die Anwendung `TwitterWPFClient.exe` aus. Geben Sie Ihre Daten für den Twitter-API-Schlüssel und das Geheimnis, das Twitter-Zugriffstoken und -Geheimnis sowie die Event Hub-Informationen ein. Definieren Sie abschließend, für welche Schlüsselwörtern Sie die Stimmung bestimmen wollen.

### <a name="generate-an-oauth-access-token"></a>Generieren eines OAuth-Zugriffstokens
Weitere Informationen finden Sie in den [Schritten zum Generieren eines OAuth-Zugriffstokens](https://dev.twitter.com/oauth/overview/application-owner-access-tokens).  

 Zum Generieren eines Tokens müssen Sie eine leere Anwendung erstellen.  

1. Ersetzen Sie die Werte EventHubConnectionString und EventHubName in „TwitterWpfClient.exe.config“ durch die Verbindungszeichenfolge und den Namen Ihres Event Hubs. Die zuvor kopierte Verbindungszeichenfolge enthält sowohl die Verbindungszeichenfolge als auch den Namen Ihres Event Hubs. Trennen Sie die beiden Angaben also, und geben Sie sie in die richtigen Felder ein. Ziehen Sie beispielsweise die folgende Verbindungszeichenfolge in Betracht:

 ```
    Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey;EntityPath=yourhub
 ```  

   Die Datei „TwitterWpfClient.exe.config“ sollte Ihre Einstellungen wie im folgenden Beispiel enthalten:

 ```
      add key="EventHubConnectionString" value="Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey"
      add key="EventHubName" value="yourhub"
   ```
   > [!NOTE]
   > Der Text „EntityPath=“ ist **nicht** im EventHubName-Wert enthalten.

    Sie können die Werte für Ihre Twitter- und Azure-Verbindungsinformationen auch direkt in den Client eingeben. Die gleiche Logik gilt, wenn „EntityPath =“ nicht verwendet wird.

   ![wpfclient](./media/stream-analytics-twitter-sentiment-analysis-trends/wpfclientlines.png)

2. **Optional:** Passen Sie die Schlüsselwörter für die Suche an. In der Standardeinstellung sucht diese Anwendung nach Schlüsselwörtern für Spiele.  Falls gewünscht, können Sie die Werte für **twitter_keywords** in „TwitterWpfClient.exe.config“ anpassen.

3. Führen Sie „TwitterWpfClient.exe“ aus. Wählen Sie dann die grüne Startschaltfläche, um Stimmungen aus sozialen Medien zu sammeln. Sie sehen nun, wie Tweet-Ereignisse mit den **CreatedAt**-, **Topic**- und **SentimentScore**-Werten an Ihren Event Hub gesendet werden.

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags

Nun, da wir einen Datenstrom von Tweet-Ereignissen haben, können wir einen Stream Analytics-Auftrag einrichten, um diese Ereignisse in Echtzeit zu analysieren.

### <a name="provision-a-stream-analytics-job"></a>Bereitstellen eines Stream Analytics-Auftrags

Führen Sie zum Bereitstellen eines Stream Analytics-Auftrags die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Option **NEU**, geben Sie **STREAM ANALYTICS** ein, und wählen Sie dann das Ergebnis der Stream Analytics-Kachel.

2. Geben Sie die folgenden Werte an, und wählen Sie dann **ERSTELLEN**.

   * **AUFTRAGSNAME**: Geben Sie einen Auftragsnamen ein.

   * **Ressourcengruppe**: Wählen Sie die Ressourcengruppe, die zuvor in dieser Übung erstellt wurde, über die Option „use existing“ („vorhandene verwenden“) aus.

   * **SPEICHERKONTO**: Wählen Sie das Azure-Speicherkonto aus, das Sie zum Speichern von Überwachungsdaten für alle Stream Analytics-Aufträge verwenden möchten, die innerhalb dieser Region ausgeführt werden. Sie haben die Möglichkeit, ein vorhandenes Speicherkonto auszuwählen oder ein neues zu erstellen.   

![Neuer Auftrag](./media/stream-analytics-twitter-sentiment-analysis-trends/newjob.png)

Nachdem der Auftrag erstellt wurde, wird er im Azure-Portal geöffnet.

## <a name="specify-the-job-input"></a>Festlegen der Auftragseingabe
Führen Sie zum Festlegen der Auftragseingabe die folgenden Schritte aus:

1. Wählen Sie in Ihrem Stream Analytics-Auftrag unter **Auftragstopologie** in der Mitte des Auftragsbereichs **EINGABEN**. Wählen Sie dann **HINZUFÜGEN**.

    Anschließend werden Sie im Portal zur Eingabe der folgenden Informationen aufgefordert. Der Großteil der hier definierten Standardwerte kann verwendet werden:

   * **ALIAS EINGEBEN**: Geben Sie einen Anzeigenamen für diesen Auftrag ein wie z.B. `TwitterStream`. Diesen Namen verwenden Sie später in der Abfrage.  

   * **EVENT HUB-NAME**: Wählen Sie den Namen des Event Hubs aus.

   * **NAME DER EVENT HUB-RICHTLINIE**: Wählen Sie die Event Hub-Richtlinie aus, die Sie zuvor in diesem Lernprogramm erstellt haben.

2. Wählen Sie die Schaltfläche **Erstellen**.

## <a name="specify-the-job-query"></a>Festlegen der Auftragsabfrage

Stream Analytics unterstützt ein einfaches, deklaratives Abfragemodell, das Transformationen beschreibt. Weitere Informationen zur Sprache finden Sie in der [Azure Stream Analytics-Abfragesprachreferenz](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Dieses Tutorial hilft Ihnen beim Erstellen und Testen mehrerer Abfragen über Twitter-Daten.

Um die Anzahl der Erwähnungen verschiedener Themen zu vergleichen, verwenden wir ein [rollierendes Fenster](https://msdn.microsoft.com/library/azure/dn835055.aspx), um alle fünf Sekunden die Anzahl der Erwähnungen jedes Themas abzurufen.

Ändern Sie die Abfrage im Code-Editor in den folgenden Code, und wählen Sie dann **Speichern**:

```
SELECT System.Timestamp as Time, Topic, COUNT(*)
FROM TwitterStream TIMESTAMP BY CreatedAt
GROUP BY TUMBLINGWINDOW(s, 5), Topic
```   

Diese Abfrage gibt mit dem Schlüsselwort **TIMESTAMP BY** ein Zeitstempelfeld für die Nutzlast an, das für die zeitliche Berechnung verwendet wird. Wird dieses Feld nicht angegeben, verwendet der Vorgang die Zeit, zu der jedes Ereignis beim Event Hub eingeht. Weitere Informationen erhalten Sie im Abschnitt „Arrival Time vs Application Time“ („Eingangszeit im Vgl. zu Anwendungszeit“) in der [Stream Analytics Query Reference (Stream Analytics-Abfragereferenz)](https://msdn.microsoft.com/library/azure/dn834998.aspx).

Diese Abfrage greift zudem mithilfe der Eigenschaft **System.Timestamp** jeweils auf einen Zeitstempel für das Ende der einzelnen Fenster zu.

![Feld „Abfrage“](./media/stream-analytics-twitter-sentiment-analysis-trends/querybox.png)

## <a name="create-an-output-sink"></a>Erstellen einer Ausgabesenke

Nun, da wir einen Datenstrom, eine Event Hub-Eingabe zum Erfassen von Ereignissen und eine Abfrage zur Ausführung einer Umwandlung über den Stream definiert haben, ist der letzte Schritt die Definition einer Ausgabetabelle für den Auftrag.  

Die aggregierten Tweet-Ereignisse aus unserer Auftragsabfrage schreiben wir in Azure Blob Storage.  Sie können die Ergebnisse je nach den spezifischen Anforderungen Ihrer Anwendung auch mithilfe von Push in Azure SQL-Datenbank, Azure Table Storage oder Event Hubs übertragen.

## <a name="specify-the-job-output"></a>Festlegen der Auftragsausgabe
Führen Sie zum Festlegen der Auftragsausgabe die folgenden Schritte aus:

1. Wählen Sie in Ihrem Stream Analytics-Auftrag unter **Auftragstopologie** die Option **AUSGABE**, und wählen Sie dann **HINZUFÜGEN**.

2. Geben Sie in dem Bereich die folgenden Werte ein, oder wählen Sie sie aus:

   * **AUSGABEALIAS**: Geben Sie einen Anzeigenamen für diese Auftragsausgabe ein. In dieser Demo wird `Output` verwendet.

   * **Senke**: Wählen Sie **Blobspeicher** aus.

   * **SPEICHERKONTO**: Wählen Sie **Neues Speicherkonto erstellen** aus.

    * **SPEICHERKONTO**: Benennen Sie das neue Speicherkonto (`socialtwitter` in diesem Beispiel).

    * **CONTAINER**: Benennen Sie den neuen Container (`socialtwitter` in diesem Beispiel).

3. Behalten Sie die übrigen Einträge als Standardwerte bei. Wählen Sie abschließend **Erstellen**.

## <a name="start-the-job"></a>Starten des Auftrags

Da wir Eingabe, Abfrage und Ausgabe für den Auftrag angegeben haben, können wir den Stream Analytics-Auftrag nun starten.

Führen Sie zum Starten des Auftrags die folgenden Schritte aus:

1. Wählen Sie im Auftragsübersichtsbereich oben auf der Seite **STARTEN**.

2. Wählen Sie im daraufhin angezeigten Dialogfeld **AUFTRAGSSTARTZEIT** und dann auf die Schaltfläche zum **ÜBERPRÜFEN** am unteren Rand des Dialogfelds. Der Status des Auftrags ändert sich zunächst in **Wird gestartet** und anschließend in **Wird ausgeführt**.

![Auftrag wird ausgeführt](./media/stream-analytics-twitter-sentiment-analysis-trends/jobrunning.png)

## <a name="view-output-for-sentiment-analysis"></a>Anzeigen der Ausgabe für die Stimmungsanalyse

Nachdem die Ausführung Ihres Auftrags gestartet wurde und der Auftrag den Twitter-Datenstrom in Echtzeit verarbeitet, wählen Sie aus, wie die Ausgabe für die Standpunktanalyse angezeigt werden soll.

Verwenden Sie ein Tool wie [Azure Storage Explorer](https://http://storageexplorer.com/) oder [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction), um die Auftragsausgabe in Echtzeit anzuzeigen. Ab hier können Sie [Power BI](https://powerbi.com/) dazu verwenden, Ihre Anwendung zu erweitern, um ein benutzerdefiniertes Dashboard wie das im nachstehenden Screenshot einzuschließen:

![Power BI](./media/stream-analytics-twitter-sentiment-analysis-trends/power-bi.png)

## <a name="create-another-query-to-identify-trending-topics"></a>Erstellen einer weiteren Abfrage zum Identifizieren von Trendthemen

Eine andere Beispielabfrage, die wir für dieses Szenario erstellt haben, basiert auf einem [Schiebefenster](https://msdn.microsoft.com/library/azure/dn835051.aspx). Zum Identifizieren von Trendthemen suchen wir nach Themen, die einen Schwellenwert für Erwähnungen in einem bestimmten Zeitraum überschreiten.

Für die Zwecke dieses Tutorials suchen wir nach Themen, die in den letzten fünf Sekunden mehr als 20-mal erwähnt werden.

```
SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
FROM TwitterStream TIMESTAMP BY CreatedAt
GROUP BY SLIDINGWINDOW(s, 5), topic
HAVING COUNT(*) > 20
```

## <a name="use-field-headers"></a>Verwenden von Feldheadern

Die Feldbezeichnungen, die Sie in dieser Übung verwenden können, werden in dieser Tabelle aufgeführt. Sie können im Abfrage-Editor experimentieren.

JSON-Eigenschaft | Definition
--- | ---
CreatedAt | Zeitpunkt der Erstellung des Tweets
Thema | Thema, das dem angegebenen Schlüsselwort entspricht
SentimentScore | Stimmungswert aus Sentiment140
Autor | Twitter-Handle, das den Tweet gesendet hat
Text | Vollständiger Text des Tweets


## <a name="get-support"></a>Support
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

