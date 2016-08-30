<properties
	pageTitle="Erste Schritte mit Event Hubs in Java mit Apache Storm | Microsoft Azure"
	description="Befolgen Sie dieses Lernprogramm für die ersten Schritte bei der Verwendung von Azure Event Hubs für das Senden von Ereignissen mit Java und das Empfangen in einem Apache Storm-Cluster."
	services="event-hubs"
	documentationCenter=""
	authors="fsautomata"
	manager="timlt"
	editor=""/>

<tags
	ms.service="event-hubs"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/16/2016"
	ms.author="sethm"/>

# Erste Schritte mit Event Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## Einführung

Event Hubs sind ein hochgradig skalierbares Aufnahmesystem, das Millionen von Ereignissen pro Sekunde aufnehmen kann, wodurch eine Anwendung die Möglichkeit erhält, die von Ihren verbundenen Geräten und Anwendungen erzeugten immensen Datenmengen zu verarbeiten und zu analysieren. Nach der Erfassung in Event Hubs können Sie Daten über einen beliebigen Echtzeit-Analyseanbieter oder ein Speichercluster transformieren und speichern.

Weitere Informationen finden Sie unter [Übersicht über Event Hubs][].

In diesem Lernprogramm erfahren Sie, wie Nachrichten an einen Event Hub mithilfe einer Konsolenanwendung in Java erfasst und gleichzeitig mit Apache Storm abgerufen werden können.

Für dieses Tutorial benötigen Sie Folgendes:

+ Eine Java-Entwicklungsumgebung, die zum Ausführen von [Maven](http://maven.apache.org/) konfiguriert ist. In diesem Lernprogramm wird von [Eclipse](https://www.eclipse.org/) ausgegangen.

+ Ein aktives Azure-Konto. Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## Ausführen der Anwendungen

Sie können jetzt die Anwendung ausführen.

1.	Führen Sie die **LogTopology**-Klasse aus Eclipse aus, und warten Sie dann, bis sie die Empfänger für alle Partitionen gestartet hat.

2.	Führen Sie das Programm **Sender** aus, drücken Sie im Konsolenfenster die **Eingabetaste**, und sehen Sie sich die im Empfängerfenster angezeigten Ereignisse an.

   	![][22]

> [AZURE.NOTE] Verwenden Sie nur in diesem Lernprogramm Storm im lokalen Modus zu Entwicklungszwecken. Weitere Informationen zu Storm-Bereitstellungen und -Mustern finden Sie in der [Übersicht zu HDInsight Storm][] und in der offiziellen Dokumentation zu [Apache Storm][].

## Nächste Schritte

Die folgenden Ressourcen sind für die Entwicklung von Anwendungen verfügbar, die Event Hubs und Storm integrieren.

- [Analysieren von Sensordaten mit Storm und HDInsight] ist ein Lernprogramm mit einem umfassenden Szenario, das Event Hubs, Storm und HBase zum Erfassen von Sensordaten in einen Hadoop-Cluster verwendet.
- [Entwickeln von Anwendungen zur Verarbeitung von Streamingdaten mit SCP.NET und C# mit Storm und HDInsight][] ist ein Lernprogramm zum Schreiben von Storm-Pipelines mithilfe von C#.

<!-- Images. -->
[22]: ./media/event-hubs-java-storm-getstarted/receive-storm2.png

<!-- Links -->
[klassischen Azure-Portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Übersicht über Event Hubs]: event-hubs-overview.md

[Apache Storm]: https://storm.incubator.apache.org
[Übersicht zu HDInsight Storm]: ../hdinsight/hdinsight-storm-overview.md
[Analysieren von Sensordaten mit Storm und HDInsight]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Entwickeln von Anwendungen zur Verarbeitung von Streamingdaten mit SCP.NET und C# mit Storm und HDInsight]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
 

<!---HONumber=AcomDC_0817_2016-->