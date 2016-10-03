<properties
	pageTitle="Ausgabe von Daten aus Azure Stream Analytics in einem Azure Redis Cache mithilfe von Azure Functions | Microsoft Azure"
	description="Erfahren Sie, wie Sie eine mit einer Service Bus-Warteschlange verbundene Azure Function verwenden, um einen Azure Redis Cache mit der Ausgabe eines Stream Analytics-Auftrags aufzufüllen."
	keywords="Datenstrom, Redis Cache, Service Bus-Warteschlange"
	services="stream-analytics"
	authors="ryancrawcour"
	manager="jhubbard"
    documentationCenter=""
	/>

<tags
	ms.service="stream-analytics"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="na"
	ms.date="08/09/2016"
	ms.author="ryancraw"/>

# Speichern von Daten aus Azure Stream Analytics in einem Azure Redis Cache mithilfe von Azure Functions

Mit Azure Stream Analytics (ASA) können Sie schnell kostengünstige Analyselösungen entwickeln und bereitstellen, mit denen Sie in Echtzeit basierend auf Daten aus Geräten, Sensoren, Infrastrukturen und Anwendungen oder beliebigen anderen Datenströmen wichtige Einblicke erhalten. Sie können verschiedene Anwendungsfälle umsetzen, wie z.B. Verwaltung, Überwachung, Befehlsausführung und Steuerung, Betrugserkennung, vernetzte Fahrzeuge und vieles mehr – alles in Echtzeit. In vielen Szenarien müssen Sie Daten, die von Azure Stream Analytics ausgegeben werden, in einem verteilten Datenspeicher wie z.B. einem Azure Redis Cache speichern.

Beispiel: Sie arbeiten in einem Telekommunikationsunternehmen. Sie versuchen, einen Betrugsversuch mit einer SIM-Karte aufzudecken, bei dem viele Anrufe von der gleichen Identität und zur gleichen Zeit, aber an unterschiedlichen geografischen Standorten getätigt werden. Ihre Aufgabe ist es, alle potenziell betrügerischen Telefongespräche in einem Azure Redis Cache zu speichern. In diesem Blog bieten wir Ihnen eine Anleitung, wie Sie diese Aufgabe problemlos erfüllen können.

## Voraussetzungen
Arbeiten Sie die exemplarische Vorgehensweise [Betrugserkennung in Echtzeit][fraud-detection] für ASA durch.

## Übersicht über die Architektur
![Screenshot der Architektur](./media/stream-analytics-functions-redis/architecture-overview.png)

Wie in der obigen Abbildung gezeigt, lassen sich Eingabedatenströme mit Stream Analytics abfragen und an eine Ausgabe senden. Basierend auf der Ausgabe kann Azure Functions dann ein Ereignis auslösen.

In diesem Blog geht es um den Teil der Pipeline, der über Azure Functions ausgeführt wird – genauer gesagt um das Auslösen eines Ereignisses, das Betrugsdaten im Cache speichert. Nach Abschluss des Tutorials [Betrugserkennung in Echtzeit][fraud-detection] verfügen Sie über eine Eingabe (einen Event Hub), eine Abfrage und eine Ausgabe (Blobspeicher), die bereits konfiguriert sind und ausgeführt werden. In diesem Blog ändern wir die Ausgabe so, dass stattdessen eine Service Bus-Warteschlange verwendet wird. Anschließend verbinden wir eine Azure Function mit dieser Warteschlange.

## Erstellen einer Ausgabe in eine Service Bus-Warteschlange und Herstellen einer Verbindung
Um eine Service Bus-Warteschlange zu erstellen, führen Sie Schritt 1 und 2 des .NET-Abschnitts in [Erste Schritte mit Service Bus-Warteschlangen][servicebus-getstarted] durch. Nun verbinden wir die Warteschlange mit dem Stream Analytics-Auftrag, der in der oben erwähnten exemplarischen Vorgehensweise zur Betrugserkennung erstellt wurde.



1. Wechseln Sie im Azure-Portal zum Blatt **Ausgaben** Ihres Auftrags, und wählen Sie oben auf der Seite die Option **Hinzufügen** aus.

	![Hinzufügen von Ausgaben](./media/stream-analytics-functions-redis/adding-outputs.png)

2. Wählen Sie **Service Bus-Warteschlange** als **Senke**, und folgen Sie den Anweisungen auf dem Bildschirm. Stellen Sie sicher, dass Sie den Namespace der Service Bus-Warteschlange auswählen, die Sie in [Erste Schritte mit Service Bus-Warteschlangen][servicebus-getstarted] erstellt haben. Klicken Sie anschließend auf die Schaltfläche zur Bestätigung.
3. Geben Sie die folgenden Werte an:
	- **Event Serializer Format**: JSON
	- **Codierung**: UTF8
	- **FORMAT**: Separate Zeile

4. Klicken Sie auf die Schaltfläche **Erstellen**, um diese Quelle hinzuzufügen und zu überprüfen, ob Stream Analytics erfolgreich mit dem Speicherkonto verbunden werden kann.

5. Ersetzen Sie auf der Registerkarte **Abfrage** die aktuelle Abfrage durch die folgende. Ersetzen Sie *[YOUR SERVICE BUS NAME]* durch den Namen der in Schritt 3 erstellten Ausgabe.

    ```    

	    SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2

        INTO [YOUR SERVICE BUS NAME]
    
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
	    JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
    
        WHERE CS1.SwitchNum != CS2.SwitchNum
    
    ```

## Erstellen eines Azure Redis Cache
Erstellen Sie einen Azure Redis Cache, indem Sie die Anweisungen im .NET-Abschnitt des Artikels [Verwenden von Azure Redis Cache][use-rediscache] bis zum Abschnitt ***Konfigurieren der Cacheclients*** befolgen. Wenn Sie diese Schritte durchgeführt haben, verfügen Sie über einen neuen Redis Cache. Wählen Sie unter **Alle Einstellungen** die Option **Zugriffsschlüssel** aus, und notieren Sie sich die ***Primäre Verbindungszeichenfolge***.

![Screenshot der Architektur](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## Erstellen einer Azure Function
Befolgen Sie das Tutorial [Erstellen Sie Ihre erste Funktion in Azure Functions][functions-getstarted], um mit Azure Functions zu beginnen. Wenn Sie bereits über eine Azure Function verfügen, die Sie verwenden möchten, fahren Sie mit [Schreiben in den Redis Cache](#Writing-to-Redis-Cache) fort.

1. Wählen Sie im Portal im Navigationsbereich auf der linken Seite die Option „App Services“ aus, und klicken Sie auf den Namen Ihrer Azure Functions-App, um zur entsprechenden Website zu wechseln. ![Screenshot der App Services-Funktionsliste](./media/stream-analytics-functions-redis/app-services-function-list.png)

2. Klicken Sie auf **Neue Funktion > ServiceBusQueueTrigger – C#**. Geben Sie in den folgenden Feldern die nachstehenden Informationen ein:
	- **Warteschlangenname**: Der Name, den Sie beim Erstellen der Warteschlange unter [Erste Schritte mit Service Bus-Warteschlangen][servicebus-getstarted] angegeben haben (nicht der Service Bus-Name). Stellen Sie sicher, dass Sie die Warteschlange verwenden, die mit der Stream Analytics-Ausgabe verbunden ist.
	- **Service Bus-Verbindung**: Wählen Sie **Verbindungszeichenfolge hinzufügen** aus. Um die Verbindungszeichenfolge zu ermitteln, wechseln Sie zum klassischen Portal, und wählen Sie **Service Bus** – der von Ihnen erstellte Service Bus – und am unteren Bildschirmrand **VERBINDUNGSINFORMATIONEN** aus. Stellen Sie sicher, dass Sie sich auf dem Hauptbildschirm dieser Seite befinden. Kopieren Sie die Verbindungszeichenfolge, und fügen Sie sie ein. Sie können auf Wunsch auch einen Namen für die Verbindung eingeben.
	
		![Screenshot der Service Bus-Verbindung](./media/stream-analytics-functions-redis/servicebus-connection.png)
	- **AccessRights**: Wählen Sie **Verwalten**.


3. Klicken Sie auf **Erstellen**.

## Schreiben in den Redis Cache
Wir haben jetzt eine Azure Function erstellt, die aus einer Service Bus-Warteschlange liest. Nun müssen wir diese Funktion nur noch verwenden, um diese Daten in den Redis Cache schreibt.

1. Wählen Sie den neu erstellten **ServiceBusQueueTrigger** aus, und klicken Sie in der oberen rechten Ecke auf **Funktionen-App-Einstellungen**. Wählen Sie **Zu App Service-Einstellungen wechseln > Einstellungen > Anwendungseinstellungen** aus.

2. Geben Sie unter „Verbindungszeichenfolgen“ im Feld **Name** einen Namen ein. Fügen Sie die primäre Verbindungszeichenfolge aus dem Schritt **Erstellen eines Redis Cache** in das Feld **Wert** ein. Wählen Sie statt des Eintrags **SQL-Datenbank** die Option **Benutzerdefiniert** aus.

3. Klicken Sie oben auf **Speichern**.

	![Screenshot der Service Bus-Verbindung](./media/stream-analytics-functions-redis/function-connection-string.png)

4. Wechseln Sie zurück zu den App Service-Einstellungen, und wählen Sie **Extras > App Service-Editor (Vorschau) > Ein > Los**.

	![Screenshot der Service Bus-Verbindung](./media/stream-analytics-functions-redis/app-service-editor.png)

5. Erstellen Sie in einem Editor Ihrer Wahl eine JSON-Datei namens **project.json** mit dem folgenden Inhalt, und speichern Sie die Datei auf Ihrer lokalen Festplatte.

        {
            "frameworks": {
		        "net46": {
		            "dependencies": {
				        "StackExchange.Redis":"1.1.603",
				        "Newtonsoft.Json": "9.0.1"
			        }
		        }
	        }
        }

6. Laden Sie diese Datei in das Stammverzeichnis Ihrer Funktion hoch (nicht WWWROOT). Es wird automatisch eine Datei namens **project.lock.json** angezeigt, wodurch bestätigt wird, dass die NuGet-Pakete „StackExchange.Redis“ und „Newtonsoft.Json“ importiert wurden.

7. Ersetzen Sie in der Datei **run.csx** den vorab generierten Code durch den folgenden Code. Ersetzen Sie in der lazyConnection-Funktion „CONN NAME“ durch den Namen, den Sie in Schritt 2 von **Speichern von Daten im Redis Cache** erstellt haben.

````

	using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;
    
    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");
    
        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
		new Lazy<ConnectionMultiplexer>(() =>
    		{
				var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
        		return ConnectionMultiplexer.Connect();
    		});
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## Starten des Stream Analytics-Auftrags

1. Starten Sie die Anwendung „telcodatagen.exe“. Die Nutzung ist wie folgt: ````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````

2. Klicken Sie im Portal auf dem Blatt des Stream Analytics-Auftrags oben auf der Seite auf **Starten**.

	![Screenshot des Auftragsstarts](./media/stream-analytics-functions-redis/starting-job.png)

3. Wählen Sie auf dem angezeigten Blatt **Auftrag starten** die Option **Jetzt** aus, und klicken Sie dann am unteren Bildschirmrand auf die Schaltfläche **Starten**. Der Auftragsstatus ändert sich zu „Wird gestartet“ und nach einiger Zeit zu „Wird ausgeführt“.
 
	![Screenshot der Uhrzeitauswahl für den Auftragsstart](./media/stream-analytics-functions-redis/start-job-time.png)

## Ausführen der Lösung und Prüfen der Ergebnisse
Wenn Sie zurück zur Seite **ServiceBusQueueTrigger** wechseln, sehen Sie Protokolleinträge. Diese Protokolle zeigen, dass Daten aus der Service Bus-Warteschlange gesendet, in die Datenbank gespeichert und anhand der Uhrzeit als Schlüssel abgerufen wurden.

Um zu überprüfen, ob sich die Daten in Ihrem Redis Cache befinden, wechseln Sie im neuen Portal auf die Seite Ihres Redis Cache (wie im vorherigen Schritt [Erstellen eines Azure Redis Cache](#Create-an-Azure-Redis-Cache) gezeigt), und wählen Sie „Konsole“ aus.

Jetzt können Sie Redis-Befehle schreiben, um zu bestätigen, dass sich tatsächlich Daten im Cache befinden.

![Screenshot der Redis-Konsole](./media/stream-analytics-functions-redis/redis-console.png)

## Nächste Schritte
Wir finden es sehr spannend, was sich mit der Kombination aus Azure Functions und Stream Analytics erzielen lässt, und hoffen, dass Sie dadurch neue Möglichkeiten entdecken! Wenn Sie Vorschläge zu neuen Funktionen haben, teilen Sie uns diese auf der Seite [Azure UserVoice](https://feedback.azure.com/forums/270577-stream-analytics) mit.

Wenn Sie mit Microsoft Azure noch nicht vertraut sind, laden wir Sie herzlich ein, sich für ein [kostenloses Azure-Testkonto](https://azure.microsoft.com/pricing/free-trial/) zu registrieren. Wenn Sie mit Stream Analytics noch nicht vertraut sind, legen Sie einfach los, indem Sie [Ihren ersten Stream Analytics-Auftrag erstellen](stream-analytics-create-a-job.md).

Wenn Sie Hilfe benötigen, posten Sie Ihre Fragen im Forum für [MSDN](https://social.msdn.microsoft.com/Forums/de-DE/home?forum=AzureStreamAnalytics) oder [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics).

Weitere Informationen finden Sie auch in den folgenden Ressourcen:

- [Entwicklerreferenz zu Azure Functions](../azure-functions/functions-reference.md)
- [C#-Entwicklerreferenz zu Azure Functions](../azure-functions/functions-reference-csharp.md)
- [F#-Entwicklerreferenz zu Azure Functions](../azure-functions/functions-reference-fsharp.md)
- [NodeJS-Entwicklerreferenz zu Azure Functions](../azure-functions/functions-reference.md)
- [Trigger und Bindungen in Azure Functions](../azure-functions/functions-triggers-bindings.md)
- [Überwachen von Azure Redis Cache](../redis-cache/cache-how-to-monitor.md)

Um bei den neuesten Features auf dem Laufenden zu bleiben, folgen Sie [@AzureStreaming](https://twitter.com/AzureStreaming) auf Twitter.


[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md

<!---HONumber=AcomDC_0921_2016-->