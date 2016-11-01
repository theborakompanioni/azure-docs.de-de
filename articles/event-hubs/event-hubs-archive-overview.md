<properties
	pageTitle="Azure Event Hubs Archive | Microsoft Azure"
	description="Übersicht über das Azure Event Hubs Archive-Feature"
	services="event-hubs"
	documentationCenter=""
	authors="djrosanova"
	manager="timlt"
	editor=""/>

<tags
	ms.service="event-hubs"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/13/2016"
	ms.author="darosa;sethm"/>

# Azure Event Hubs Archive

Azure Event Hubs Archive ermöglicht Ihnen das automatische Streaming von Daten in Ihren Event Hubs an ein Blobspeicherkonto Ihrer Wahl. Für mehr Flexibilität ist dabei die Angabe eines beliebigen Zeit- oder Größenintervalls möglich. Das Einrichten von Archive geht schnell, für das Ausführen fallen keine Verwaltungskosten an, und die Skalierung erfolgt automatisch mit den [Durchsatzeinheiten](event-hubs-overview.md#capacity-and-security) Ihrer Event Hubs. Event Hubs Archive bietet die einfachste Möglichkeit zum Laden von Streamingdaten in Azure und ermöglicht Ihnen, sich auf die Datenverarbeitung anstatt die Datenerfassung zu konzentrieren.

Azure Event Hubs Archive ermöglicht Ihnen das Verarbeiten von Echtzeit-Pipelines und batchbasierten Pipelines für den gleichen Stream. Dadurch können Sie Lösungen erstellen, die sich im Laufe der Zeit parallel zu Ihren Anforderungen erweitern lassen. Ob Sie derzeit batchbasierte Systeme mit Blick auf zukünftige Echtzeitverarbeitung erstellen oder einer vorhandenen Echtzeitlösung effiziente kalte Daten hinzufügen möchten – Event Hubs Archive macht das Arbeiten mit Streamingdaten einfacher.

## Azure Event Hubs Archive – Funktionsweise

Event Hubs ist ein beständiger Puffer mit zeitbasierter Speicherung für Telemetrieeingänge, vergleichbar mit einem verteilten Protokoll. Der Schlüssel zur Skalierung in Event Hubs ist das [partitionierte Consumermodell](event-hubs-overview.md#partition-key). Jede Partition ist ein unabhängiges Datensegment und wird unabhängig genutzt. Mit der Zeit werden diese Daten basierend auf der konfigurierbaren Beibehaltungsdauer ersetzt. Daher kann ein Event Hub nie „zu voll“ werden.

Mit Event Hubs Archive können Sie Ihr eigenes Azure Blob-Speicherkonto und Container festlegen, die zum Speichern der archivierten Daten verwendet werden sollen. Dieses Konto kann sich in der gleichen Region wie Ihr Event Hub oder in einer anderen Region befinden, was zur Flexibilität des Event Hubs Archive-Features beiträgt.

Archivierte Daten werden im [Apache Avro][]-Format geschrieben; dabei handelt es sich um ein kompaktes, schnelles, binäres Format, das umfangreiche Datenstrukturen mit Inlineschema bietet. Dieses Format wird im Hadoop-Ökosystem sowie von Stream Analytics und Azure Data Factory häufig verwendet. Weitere Informationen zum Arbeiten mit Avro finden Sie weiter unten in diesem Artikel.

### Archiv-Windowing

Event Hubs Archive ermöglicht Ihnen das Einrichten eines Fensters zur Steuerung der Archivierung. Dieses Fenster ist eine Konfiguration mit Mindestgröße und -zeit, für die das FIFO-Prinzip gilt, d. h . der erste Auslöser, der auftritt, führt zu einem Archivvorgang. Wenn Sie ein Archivfenster mit 15 Minuten/100 MB haben und 1 MB/s senden, das Größenfenstergröße vor dem Zeitfenster ausgelöst. Jede Partition wird unabhängig archiviert und schreibt zum Archivierungszeitpunkt einen abgeschlossenen Blockblob. Dieser wird nach dem Zeitpunkt benannt, zu dem das Archivintervall aufgetreten ist. Die Namenskonvention lautet wie folgt:

```
<Namespace>/<EventHub>/<Partition>/<YYYY>/<MM>/<DD>/<HH>/<mm>/<ss>
```

### Skalierung auf Durchsatzeinheiten

Der Datenverkehr von Event Hubs wird von [Durchsatzeinheiten](event-hubs-overview.md#capacity-and-security) gesteuert. Eine einzelne Durchsatzeinheit lässt eingehenden Datenverkehr von 1 MB/s oder 1000 Ereignisse pro Sekunde und die doppelte Menge an ausgehendem Datenverkehr zu. Standard-Event Hubs können mit 1 bis 20 Durchsatzeinheiten konfiguriert werden, und über eine [Supportanfrage][] für eine Kontingenterhöhung können weitere Einheiten erworben werden . Bei Überschreitung der erworbenen Durchsatzeinheiten wird die Nutzung gedrosselt. Event Hubs Archive kopiert Daten direkt aus dem internen Event Hubs-Speicher. Dabei werden Durchsatzeinheitkontingente für ausgehenden Datenverkehr umgangen und stattdessen für andere Verarbeitungsreader wie Stream Analytics oder Spark verwendet.

Nach der Konfiguration wird Event Hubs Archive automatisch ausgeführt, sobald Sie das erste Ereignis senden. Die Ausführung wird ununterbrochen fortgesetzt. Damit Ihre Downstreamverarbeitung leichter erkennt, dass der Prozess ausgeführt wird, schreibt Event Hubs leere Dateien, wenn keine Daten vorhanden sind. Dies sorgt für einen vorhersagbaren Rhythmus und Marker, die als Feed für Ihre Batchprozessoren fungieren.

## Einrichten von Event Hubs Archive

Event Hubs Archive kann zum Zeitpunkt der Erstellung von Event Hub über das Portal oder Azure Resource Manager konfiguriert werden. Sie aktivieren Archive einfach durch Klicken auf die Schaltfläche **Ein**. Konfigurieren Sie ein Speicherkonto und Container durch Klicken auf den Abschnitt **Container** des Blatts. Da Event Hubs Archive die Dienst-zu-Dienst-Authentifizierung mit Speicher verwendet, müssen Sie keine Speicherverbindungszeichenfolge angeben. Die Ressourcenauswahl wählt automatisch den Ressourcen-URI für Ihr Speicherkonto. Wenn Sie Azure Resource Manager verwenden, müssen Sie diesen URI explizit als Zeichenfolge angeben.

Das Standrdzeitfenster beträgt fünf Minuten. Der Mindestwert ist 1, der Höchstwert 15. Für das **Größenfenster** gilt ein Bereich von 10 bis 500 MB.

![][1]

## Hinzufügen von Archive zu einem vorhandenen Event Hub

Archive können auf vorhandenen Event Hubs konfiguriert werden, die sich in einem Event Hubs-Namespace befinden. Das Feature ist für ältere Messaging-Namespaces oder Namespaces vom Typ „Gemischt“ nicht verfügbar. Zum Aktiveren von Archive auf einem vorhandenen Event Hub oder zum Ändern der Archive-Einstellungen klicken Sie auf Ihren Namespace, um das Blatt **Zusammenfassung** zu laden. Anschließend klicken Sie auf den Event Hub, den Sie aktivieren bzw. dessen Archive-Einstellung Sie ändern möchten. Klicken Sie abschließend auf den Abschnitt **Eigenschaften** des geöffneten Blatts, wie in der folgenden Abbildung dargestellt.

![][2]

Sie können Event Hubs Archive auch über Azure Resource Manager-Vorlagen konfigurieren. Weitere Informationen dazu finden Sie in [diesem Artikel](event-hubs-resource-manager-namespace-event-hub-enable-archive.md).

## Erkunden des Archivs und Arbeiten mit Avro

Nach der Konfiguration erstellt Event Hubs Archive im Azure-Speicherkonto Dateien und Container, die im konfigurierten Zeitfenster bereitgestellt werden. Sie können diese Dateien mit einem beliebigen Tool wie beispielsweise [Azure-Speicher-Explorer][] anzeigen. Sie können die Dateien zur Bearbeitung lokal herunterladen.

Die von Event Hubs Archive erzeugten Dateien weisen das folgende Avro-Schema auf:

![][3]

Eine einfache Möglichkeit zum Untersuchen von Avro-Dateien ist die Verwendung der [Avro Tools][]-JAR-Datei von Apache. Nach dem Herunterladen dieser JAR-Datei können Sie das Schema einer bestimmten Avro-Datei anzeigen, indem Sie folgenden Befehl ausführen:

```
java -jar avro-tools-1.8.1.jar getschema <name of archive file>
```

Dieser Befehl gibt Folgendes zurück:

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

Sie können die Datei auch mithilfe von Avro-Tools in das JSON-Format konvertieren und anders verarbeiten.

Für eine erweiterte Verarbeitung laden Sie Avro herunter und installieren es für eine Plattform Ihrer Wahl. Zum Zeitpunkt der Erstellung dieses Dokuments sind Implementierungen für C, C++, C#, Java, NodeJS, Perl, PHP, Python und Ruby verfügbar.

Apache Avro bietet vollständige Anleitungen für die ersten Schritte mit [Java][] und [Python][]. Lesen Sie auch den Artikel zu den [ersten Schritten mit Event Hubs Archive](event-hubs-archive-python.md).

## Event Hubs Archive – Abrechnung

Die Gebühren für Event Hubs Archive werden ähnlich wie für Durchsatzeinheiten auf Stundenbasis erhoben. Die Gebühr ist direkt proportional zur Anzahl der Durchsatzeinheiten, die für den Namespace erworben werden. Event Hubs Archive wird parallel zu Durchsatzeinheiten erhöht und verringert, um eine übereinstimmende Leistung zu bieten. Die Messwerte werden zusammen ermittelt. Die Gebühren für Event Hubs Archive betragen 0,10 US-Dollar pro Stunde und Durchsatzeinheit. Während des Vorschauzeitraums wird ein Rabatt von 50 % angeboten.

Event Hubs Archive ist wirklich die einfachste Möglichkeit zum Laden von Daten in Azure. Mithilfe von Azure Data Lake, Azure Data Factory und Azure HDInsight können Sie Batchverarbeitung und andere Analysen Ihrer Wahl mit vertrauten Tools und Plattformen jeder Größenordnung durchführen.

## Nächste Schritte

Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

- Eine vollständige [Beispielanwendung mit Verwendung von Ereignis-Hubs][].
- Das Beispiel [Horizontales Hochskalieren der Ereignisverarbeitung mit Event Hubs][]
- [Übersicht über Event Hubs][]

[Apache Avro]: http://avro.apache.org/
[Supportanfrage]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[1]: ./media/event-hubs-archive-overview/event-hubs-archive1.png
[2]: media/event-hubs-archive-overview/event-hubs-archive2.png
[Azure-Speicher-Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-archive-overview/event-hubs-archive3.png
[Avro Tools]: http://www-us.apache.org/dist/avro/avro-1.8.1/java/avro-tools-1.8.1.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Übersicht über Event Hubs]: event-hubs-overview.md
[Beispielanwendung mit Verwendung von Ereignis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Horizontales Hochskalieren der Ereignisverarbeitung mit Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3

<!---HONumber=AcomDC_0914_2016-->