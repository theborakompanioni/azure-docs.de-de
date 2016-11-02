<properties
    pageTitle="Erste Schritte mit Event Hubs in Java | Microsoft Azure"
    description="Befolgen Sie dieses Tutorial für die ersten Schritte bei der Verwendung von Azure Event Hubs und für das Senden von Ereignissen mit Java und das Empfangen mit EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="core"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>


# <a name="get-started-with-event-hubs"></a>Erste Schritte mit Event Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Einführung

Event Hubs sind ein hochgradig skalierbares Aufnahmesystem, das Millionen von Ereignissen pro Sekunde aufnehmen kann, wodurch eine Anwendung die Möglichkeit erhält, die von Ihren verbundenen Geräten und Anwendungen erzeugten immensen Datenmengen zu verarbeiten und zu analysieren. Nach der Erfassung in Event Hubs können Sie Daten über einen beliebigen Echtzeitanalyseanbieter oder ein Speichercluster transformieren und speichern.

Weitere Informationen finden Sie unter [Übersicht über Event Hubs][].

In diesem Tutorial erfahren Sie, wie Nachrichten mithilfe einer Konsolenanwendung in Java in einem Event Hub erfasst werden und wie Sie diese gleichzeitig mit der Java-Bibliothek des Ereignisprozessorhosts abrufen.

Zur Ausführung dieses Lernprogramms benötigen Sie Folgendes:

+ Eine Java-Entwicklungsumgebung. In diesem Lernprogramm wird von [Eclipse](https://www.eclipse.org/)ausgegangen.

+ Ein aktives Azure-Konto. <br/>Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Konto erstellen. Einzelheiten finden Sie unter <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Kostenlose Azure-Testversion</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Ausführen der Anwendungen

Sie können jetzt die Anwendung ausführen.

1.  Führen Sie das Projekt **Receiver** aus.

    ![][21]

2.  Führen Sie das Projekt **Sender** aus.

    ![][22]

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie eine funktionierende Anwendung erstellt haben, die einen Ereignis-Hub erstellt und Daten sendet und empfängt, können Sie mit den folgenden Szenarios fortfahren:

- Eine vollständige [Beispielanwendung mit Verwendung von Ereignis-Hubs][].
- Das Beispiel [Horizontales Hochskalieren der Ereignisverarbeitung mit Event Hubs][]

Weitere Informationen finden Sie im [Java Developer Center](/develop/java/).

<!-- Images. -->
[21]: ./media/event-hubs-java-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-java-ephjava-getstarted/java-send.png

<!-- Links -->
[klassischen Azure-Portal]: https://manage.windowsazure.com/
[Übersicht über Event Hubs]: event-hubs-overview.md
[Beispielanwendung mit Verwendung von Ereignis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Horizontales Hochskalieren der Ereignisverarbeitung mit Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 


<!--HONumber=Oct16_HO2-->


