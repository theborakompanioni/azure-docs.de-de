---
title: Erste Schritte mit Event Hubs mit C und Apache Storm | Microsoft Docs
description: "Befolgen Sie dieses Lernprogramm für die ersten Schritte bei der Verwendung von Azure Event Hubs für das Senden von Ereignissen in C und das Empfangen in einem Apache Storm-Cluster."
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 3b15825e-c53c-471b-870c-686bff46885a
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: java
ms.topic: article
ms.date: 08/16/2016
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: 63cf1a5476a205da2f804fb2f408f4d35860835f
ms.openlocfilehash: 1b4573db5c4e03aba8b98408765e7c2debfb9a30


---
# <a name="get-started-with-event-hubs"></a>Erste Schritte mit Event Hubs
[!INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Einführung
Event Hubs sind ein hochgradig skalierbares Aufnahmesystem, das Millionen von Ereignissen pro Sekunde aufnehmen kann, wodurch eine Anwendung die Möglichkeit erhält, die von Ihren verbundenen Geräten und Anwendungen erzeugten immensen Datenmengen zu verarbeiten und zu analysieren. Nach der Erfassung in Event Hubs können Sie Daten über einen beliebigen Echtzeit-Analyseanbieter oder ein Speichercluster transformieren und speichern.

Weitere Informationen finden Sie unter [Übersicht über Event Hubs].

In diesem Tutorial erfahren Sie, wie Nachrichten an einen Ereignis-Hub mithilfe einer Konsolenanwendung in C aufgenommen werden können und wie Sie diese parallel mit Apache Storm abrufen können.

Für dieses Tutorial benötigen Sie Folgendes:

* Eine C-Entwicklungsumgebung. In diesem Tutorial wird vom GCC-Stack auf einem [virtuellen Linux-Computer mit Azure](../virtual-machines/virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mit Ubuntu 14.04 ausgegangen. Anweisungen für andere Umgebungen werden in externen Links bereitgestellt.
* Eine Java-Entwicklungsumgebung, die zum Ausführen von [Maven](http://maven.apache.org/)konfiguriert ist. In diesem Tutorial wird von [Eclipse](https://www.eclipse.org/)ausgegangen.
* Ein aktives Azure-Konto. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Konto erstellen. Einzelheiten finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

[!INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[!INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[!INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Ausführen der Anwendungen
Sie können jetzt die Anwendung ausführen.

1. Führen Sie die **LogTopology** -Klasse aus Eclipse aus, und warten Sie dann, bis sie die Empfänger für alle Partitionen gestartet hat.
2. Führen Sie das Programm **sender** aus, und sehen Sie sich die im Empfängerfenster angezeigten Ereignisse an.
   
   ![][23]

> [!NOTE]
> Verwenden Sie nur in diesem Lernprogramm Storm im lokalen Modus zu Entwicklungszwecken. Weitere Informationen zu Storm-Bereitstellungen und -Mustern finden Sie in der [Übersicht zu HDInsight Storm] und in der offiziellen Dokumentation zu [Apache Storm].
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Die folgenden Ressourcen sind für die Entwicklung von Anwendungen verfügbar, die Event Hubs und Storm integrieren.

* [Analysieren von Sensordaten mit Storm und HDInsight][Analysieren von Sensordaten mit Storm und HDInsight] ist ein Tutorial mit einem umfassenden Szenario, das Event Hubs, Storm und HBase zum Erfassen von Sensordaten in einen Hadoop-Cluster verwendet.
* [Entwickeln von Anwendungen zur Verarbeitung von Streamingdaten mit SCP.NET und C# mit Storm und HDInsight][Entwickeln von Anwendungen zur Verarbeitung von Streamingdaten mit SCP.NET und C# mit Storm und HDInsight] ist ein Tutorial zum Schreiben von Storm-Pipelines mithilfe von C#.

<!-- Images. -->
[23]: ./media/event-hubs-c-storm-getstarted/receive-storm3.png

<!-- Links -->
[klassischen Azure-Portal]: https://manage.windowsazure.com/
[Ereignisprozessorhosts]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Übersicht über Event Hubs]: event-hubs-overview.md

[Apache Storm]: https://storm.incubator.apache.org
[Übersicht zu HDInsight Storm]: ../hdinsight/hdinsight-storm-overview.md
[Analysieren von Sensordaten mit Storm und HDInsight]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Entwickeln von Anwendungen zur Verarbeitung von Streamingdaten mit SCP.NET und C# mit Storm und HDInsight]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md



<!--HONumber=Nov16_HO3-->


