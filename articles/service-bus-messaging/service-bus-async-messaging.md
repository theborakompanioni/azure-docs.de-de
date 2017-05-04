---
title: Asynchrones Messaging mit Service Bus | Microsoft Docs
description: Beschreibung des asynchronen Messaging mit Azure Service Bus.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f1435549-e1f2-40cb-a280-64ea07b39fc7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: e0c999b2bf1dd38d8a0c99c6cdd4976cc896dd99
ms.openlocfilehash: 95d6f295ba145a55fe4ed3fc7c6f627c9d419a3c
ms.lasthandoff: 04/20/2017


---
# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asynchrone Nachrichtenmuster und hohe Verfügbarkeit

Asynchrones Messaging kann auf verschiedenen Arten implementiert werden. Mit Warteschlangen, Themen und Abonnements unterstützt Azure Service Bus Asynchronität über einen Speicher- und Weiterleitungsmechanismus. Im normalen (synchronen) Betrieb senden Sie Nachrichten an Warteschlangen und Themen und erhalten Nachrichten von Warteschlangen und Abonnements. Anwendungen, die Sie schreiben, hängen davon ab, dass diese Entitäten immer verfügbar sind. Wenn sich der Zustand der Entität aufgrund von verschiedenen Umständen ändert, benötigen Sie eine Möglichkeit, eine Entität mit geringerer Funktionalität bereitzustellen, die die meisten Anforderungen erfüllen kann.

Anwendungen verwenden typischerweise asynchrone Nachrichtenmuster, um eine Reihe von Kommunikationsszenarien zu ermöglichen. Sie können Anwendungen erstellen, in denen Clients Nachrichten an Dienste senden können, selbst wenn der Dienst nicht ausgeführt wird. Für Anwendungen mit Kommunikationsspitzen kann eine Warteschlange dabei helfen, die Last durch Bereitstellen eines Puffers für die Kommunikation auszugleichen. Schließlich erhalten Sie einen einfachen und effektiven Lastenausgleich, um Nachrichten auf mehrere Computer zu verteilen.

Um die Verfügbarkeit dieser Entitäten zu gewährleisten, sollten Sie eine Reihe von Möglichkeiten in Betracht ziehen, in denen diese Entitäten für ein langlebiges Messagingsystem als nicht verfügbar erscheinen können. Im Allgemeinen ist die Entität für Anwendungen, die wir schreiben, auf folgende Arten nicht mehr verfügbar:

* Nachrichten können nicht gesendet werden.
* Nachrichten können nicht empfangen werden.
* Entitäten können nicht verwaltet werden (Erstellen, Abrufen, Aktualisieren oder Löschen von Entitäten).
* Mit dem Dienst kann keine Verbindung hergestellt werden.

Für jeden dieser Fehler sind verschiedene Fehlermodi vorhanden, durch die eine Anwendung mit geringerer Funktionalität Arbeiten weiterhin erledigen kann. Beispiel: Ein System, das Nachrichten senden, jedoch nicht empfangen kann, kann weiterhin Aufträge von Kunden erhalten, diese Aufträge aber nicht verarbeiten. In diesem Thema werden mögliche Probleme behandelt, die auftreten können, und wie diese Probleme gemindert werden. Mit Service Bus wurde eine Reihe von Lösungen eingeführt, die Sie abonnieren müssen, und in diesem Thema werden auch die Regeln erläutert, die die Verwendung dieser abonnierten Lösungen steuern.

## <a name="reliability-in-service-bus"></a>Zuverlässigkeit in Service Bus
Es gibt mehrere Arten, Probleme mit Nachrichten und Entitäten zu behandeln, und Richtlinien regeln die entsprechende Verwendung dieser Lösungen. Um die Richtlinien zu verstehen, müssen Sie zuerst verstehen, welche Fehler in Service Bus auftreten können. Aufgrund des Entwurfs von Azure-Systemen sind diese Probleme häufig kurzlebig. Auf allgemeiner Ebene treten die folgenden Gründe für Nichtverfügbarkeit auf:

* Drosselung durch ein externes System, von dem Service Bus abhängt. Die Drosselung ergibt sich durch Interaktionen mit Speicher- und Computeressourcen.
* Problem für ein System, von dem Service Bus abhängt. Beispielsweise können in einem bestimmten Teil des Speichers Probleme auftreten.
* Ausfall von Service Bus in einem einzelnen Subsystem. In diesem Fall kann auf dem Computeknoten ein inkonsistenter Zustand auftreten, und er muss neu gestartet werden, wodurch für alle Entitäten, die von dem Knoten verarbeitet werden, ein Lastenausgleich auf andere Knoten ausgeführt wird. Dies kann wiederum für einen kurzen Zeitraum zu einer langsamen Nachrichtenverarbeitung führen.
* Ausfall von Service Bus in einem Azure-Rechenzentrum. Dies ist ein „schwerwiegender Fehler“, durch den das System für viele Minuten oder einige Stunden nicht erreichbar ist.

> [!NOTE]
> Mit dem Begriff **Speicher** kann sowohl Azure Storage als auch SQL Azure gemeint sein.
> 
> 

Service Bus enthält eine Reihe von Entschärfungen für diese Probleme. In den folgenden Abschnitten werden die einzelnen Probleme und die entsprechenden Lösungen beschrieben.

### <a name="throttling"></a>Drosselung
Mit Service Bus ermöglicht die Drosselung eine kooperative Nachrichtenratenverwaltung. Jeder einzelne Service Bus-Knoten enthält viele Entitäten. Jede dieser Entitäten stellt die Anforderungen an das System in Bezug auf CPU, Arbeitsspeicher, Speicher und andere Facets. Wenn für diese Facets eine Nutzung ermittelt wird, die definierte Schwellenwerte überschreitet, kann Service Bus eine bestimmte Anforderung verweigern. Der Aufrufer empfängt [ServerBusyException][ServerBusyException] und führt nach 10 Sekunden einen Wiederholungsversuch durch.

Als Lösung muss der Code den Fehler lesen und alle Wiederholungsversuche für die Nachricht für mindestens 10 Sekunden anhalten. Da der Fehler auf Teilen der Kundenanwendung auftreten kann, wird erwartet, dass die Wiederholungslogik auf allen Teilen unabhängig ausgeführt wird. Der Code kann die Wahrscheinlichkeit der Drosselung durch Aktivierung der Partitionierung für eine Warteschlange oder ein Thema reduzieren.

### <a name="issue-for-an-azure-dependency"></a>Problem für eine Azure-Abhängigkeit
Bei anderen Komponenten in Azure können gelegentlich Dienstprobleme auftreten. Beispielsweise kann bei der Aktualisierung eines Systems, das Service Bus verwendet, das System vorübergehend eine geringere Funktionalität aufweisen. Um derartige Probleme zu umgehen, untersucht und implementiert Service Bus regelmäßig Lösungen. Bei diesen Lösungen treten Nebeneffekte auf. Beispielsweise implementiert Service Bus zur Behandlung vorübergehender Speicherprobleme ein System, mit dem Nachrichtensendevorgänge einheitlich arbeiten können. Aufgrund der Natur der Lösung kann es bis zu 15 Minuten dauern, bis eine gesendete Nachricht in der betroffenen Warteschlange oder im Abonnement angezeigt wird und für einen Empfangsvorgang bereit ist. Im Allgemeinen tritt dieses Problem bei den meisten Entitäten nicht auf. Allerdings ist angesichts der Anzahl von Entitäten in Service Bus in Azure diese Lösung manchmal für eine kleine Teilmenge der Service Bus-Kunden erforderlich.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Ausfall von Service Bus in einem einzelnen Subsystem
Bei jeder Anwendung können Umstände dazu führen, dass eine interne Komponente von Service Bus nicht mehr konsistent ist. Wenn Service Bus dies erkennt, werden Daten von der Anwendung erfasst, um die Diagnose des Problems zu unterstützen. Nachdem die Daten erfasst wurden, wird die Anwendung neu gestartet, um zu versuchen, einen konsistenten Status wiederherzustellen. Dieser Prozess läuft ziemlich schnell ab und führt dazu, dass eine Entität für einige Minuten nicht verfügbar zu sein scheint, obwohl die normalen Ausfallzeiten viel kürzer sind.

In diesen Fällen generiert die Clientanwendung eine Ausnahme des Typs [System.TimeoutException][System.TimeoutException] oder [MessagingException][MessagingException]. Service Bus enthält eine Lösung zur Behebung dieses Problems in Form einer automatisierten Clientwiederholungslogik. Wenn der Wiederholungszeitraum beendet ist und die Nachricht nicht zugestellt wurde, können Sie die Untersuchung mit anderen Features wie [gekoppelten Namespaces][paired namespaces] durchführen. Für gekoppelte Namespace gelten andere Einschränkungen, die in diesem Artikel erläutert werden.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Ausfall von Service Bus in einem Azure-Rechenzentrum
Der wahrscheinlichste Grund für einen Ausfall in einem Azure-Rechenzentrum ist eine fehlgeschlagene Aktualisierungsbereitstellung von Service Bus oder eines abhängigen Systems. Mit zunehmender Reife der Plattform verringert sich die Wahrscheinlichkeit für diese Art von Ausfall. Ein Rechenzentrumsausfall kann auch aus den folgenden Gründen auftreten:

* Stromausfall (Stromversorgung und Stromerzeugung sind nicht mehr vorhanden).
* Konnektivität (Unterbrechung der Internetverbindung zwischen den Clients und Azure).

In beiden Fällen verursacht ein vom Menschen gemachter Notfall das Problem. Um dieses Problem zu umgehen und sicherzustellen, dass Sie weiterhin Nachrichten senden können, können Sie mithilfe [gekoppelter Namespaces][paired namespaces] Nachrichten an einen zweiten Standort senden, während der primäre Standort wieder in einen fehlerfreien Zustand versetzt wird. Weitere Informationen finden Sie unter [Best Practices zum Schützen von Anwendungen vor Service Bus-Ausfällen und Notfällen][Best practices for insulating applications against Service Bus outages and disasters].

## <a name="paired-namespaces"></a>Gekoppelte Namespaces
Das Feature für [gekoppelte Namespaces][paired namespaces] unterstützt Szenarien, in denen eine Service Bus-Entität oder -Bereitstellung in einem Rechenzentrum nicht mehr zur Verfügung steht. Zwar tritt dieses Ereignis nur selten auf, verteilte Systeme müssen jedoch immer vorbereitet sein, den ungünstigsten Fall zu bewältigen. In der Regel tritt dieses Ereignis auf, weil sich bei einem Element, von dem Service Bus abhängt, ein vorübergehendes Problem ergeben hat. Um die Verfügbarkeit der Anwendung bei einem Ausfall zu gewährleisten, können Service Bus-Benutzer zwei separate Namespaces verwenden, vorzugsweise in getrennten Rechenzentren, die ihre Messagingentitäten hosten. Im restlichen Teil dieses Abschnitts wird die folgende Terminologie verwendet:

* Primärer Namespace: Der Namespace, mit dem die Anwendung für Sende- und Empfangsvorgänge interagiert.
* Sekundärer Namespace: Der Namespace, der als Ersatz für den primären Namespace fungiert. Die Anwendungslogik interagiert mit diesem Namespace nicht.
* Failoverintervall: Die Zeitspanne, für die normale Fehler akzeptiert werden, bevor die Anwendung vom primären Namespace zum sekundären Namespace wechselt.

Gekoppelte Namespaces unterstützen *Sendeverfügbarkeit*. Die Sendeverfügbarkeit sorgt dafür, dass die Möglichkeit zum Senden von Nachrichten erhalten bleibt. Um die Sendeverfügbarkeit nutzen zu können, muss Ihre Anwendung die folgenden Anforderungen erfüllen:

1. Nachrichten werden nur vom primären Namespace empfangen.
2. An eine bestimmte Warteschlange oder ein bestimmtes Thema gesendete Nachrichten können ungeordnet ankommen.
3. Nachrichten in einer Sitzung können ungeordnet ankommen. Dies entspricht nicht der normalen Funktionalität von Sitzungen. Es bedeutet, dass Ihre Anwendung Sitzungen verwendet, um Nachrichten logisch zu gruppieren.
4. Der Sitzungszustand wird nur für den primären Namespace verwaltet.
5. Die primäre Warteschlange kann online geschaltet werden und damit beginnen, Nachrichten zu akzeptieren, bevor die sekundäre Warteschlange alle Nachrichten an die primäre Warteschlange übermittelt.

In den folgenden Abschnitten werden die APIs und deren Implementierung beschrieben, und Beispielcode wird gezeigt, in dem das Feature verwendet wird. Beachten Sie, dass dieses Feature Auswirkungen auf die Abrechnung hat.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>MessagingFactory.PairNamespaceAsync-API
Das Feature für gekoppelte Namespaces enthält die [PairNamespaceAsync][PairNamespaceAsync]-Methode für die Klasse [Microsoft.ServiceBus.Messaging.MessagingFactory][Microsoft.ServiceBus.Messaging.MessagingFactory]:

```csharp
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Beim Abschluss der Aufgabe ist die Namespacekopplung ebenfalls beendet und kann für beliebige [MessageReceiver][MessageReceiver]-, [QueueClient][QueueClient]- oder [TopicClient][TopicClient]-Elemente verwendet werden, die mit der [MessagingFactory][MessagingFactory]-Instanz erstellt werden. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] ist die Basisklasse für die unterschiedlichen Typen von Kopplung, die mit einem [MessagingFactory][MessagingFactory]-Objekt verfügbar sind. Die einzige abgeleitete Klasse ist derzeit [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions], die Anforderungen an die Sendeverfügbarkeit implementiert. [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions] verfügt über einen Satz von Konstruktoren, die aufeinander aufbauen. Wenn Sie den Konstruktor mit den meisten Parametern betrachten, können Sie das Verhalten der anderen Konstruktoren verstehen.

```csharp
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Diese Parameter haben folgende Bedeutung:

* *secondaryNamespaceManager*: Eine initialisierte [NamespaceManager][NamespaceManager]-Instanz für den sekundären Namespace, die die [PairNamespaceAsync][PairNamespaceAsync]-Methode zum Einrichten von sekundären Namespaces verwenden kann. Der Namespace-Manager wird verwendet, um die Liste der Warteschlangen im Namespace abzurufen und sicherzustellen, dass die erforderlichen Backlogwarteschlangen vorhanden sind. Wenn diese Warteschlangen nicht vorhanden sind, werden sie erstellt. [NamespaceManager][NamespaceManager] muss in der Lage sein, ein Token mit dem Anspruch **Verwalten** zu erstellen.
* *messagingFactory:* Die [MessagingFactory][MessagingFactory]-Instanz für den sekundären Namespace. Das [MessagingFactory][MessagingFactory]-Objekt dient zum Senden und, falls die [EnableSyphon][EnableSyphon]-Eigenschaft auf **TRUE** festgelegt ist, zum Empfangen von Nachrichten aus den Backlogwarteschlangen.
* *backlogQueueCount*: Die Anzahl der zu erstellenden Backlogwarteschlangen. Dieser Wert muss mindestens 1 sein. Beim Senden von Nachrichten an den Backlog wird eine dieser Warteschlangen zufällig ausgewählt. Wenn Sie den Wert auf 1 festlegen, kann immer nur eine einzige Warteschlange verwendet werden. Wenn dies geschieht und die einzige Backlogwarteschlange einen Fehler generiert, kann der Client keine andere Backlogwarteschlange verwenden und Ihre Nachricht möglicherweise nicht senden. Es wird empfohlen, diesen Wert auf einen größeren Wert und standardmäßig auf 10 festzulegen. Sie können diesen Wert in einen höheren oder niedrigeren Wert ändern, abhängig von der Datenmenge, die Ihre Anwendung pro Tag sendet. Jede Backlogwarteschlange kann bis zu 5 GB an Nachrichten enthalten.
* *failoverInterval*: Die Zeitspanne, während der Sie Fehler im primären Namespace akzeptieren, bevor eine einzelne Entität zum sekundären Namespace wechselt. Failover treten pro Entität auf. Entitäten in einem einzelnen Namespace befinden sich häufig auf verschiedenen Knoten in Service Bus. Ein Fehler in einer Entität impliziert keinen Fehler in einer anderen. Legen Sie diesen Wert auf [System.TimeSpan.Zero][System.TimeSpan.Zero] fest, damit ein Failover auf den sekundären Namespace unmittelbar nach dem ersten dauerhaften Fehler erfolgt. Fehler, die den Failovertimer auslösen, sind alle [MessagingException][MessagingException]-Elemente, bei denen die [IsTransient][IsTransient]-Eigenschaft FALSE ist, oder [System.TimeoutException][System.TimeoutException]. Andere Ausnahmen wie [UnauthorizedAccessException][UnauthorizedAccessException] verursachen kein Failover, da sie angeben, dass der Client nicht ordnungsgemäß konfiguriert ist. [ServerBusyException][ServerBusyException] verursacht kein Failover, da das richtige Muster ist, 10 Sekunden zu warten und die Nachricht dann erneut zu senden.
* *enableSyphon*: Gibt an, dass diese bestimmte Kopplung auch Nachrichten vom sekundären Namespace zurück an den primären Namespace übertragen soll. In der Regel sollte für Anwendungen, die Nachrichten senden, dieser Wert auf **FALSE** festgelegt werden. Für Anwendungen, die Nachrichten empfangen, sollte dieser Wert auf **TRUE** festgelegt werden. Der Grund dafür ist, dass häufig weniger Nachrichtenempfänger als Nachrichtenabsender vorhanden sind. Abhängig von der Anzahl der Empfänger können Sie eine einzelne Anwendungsinstanz für die Übertragung verwenden. Die Verwendung vieler Empfänger hat Auswirkungen auf die Abrechnung für jede Backlogwarteschlange.

Um den Code zu verwenden, erstellen Sie eine primäre [MessagingFactory][MessagingFactory]-Instanz, eine sekundäre [MessagingFactory][MessagingFactory]-Instanz, eine sekundäre [NamespaceManager][NamespaceManager]-Instanz und eine [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions]-Instanz. Der Aufruf kann einfach wie folgt aussehen:

```csharp
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Wenn die von der [PairNamespaceAsync][PairNamespaceAsync]-Methode zurückgegebene Aufgabe abgeschlossen wird, ist alles eingerichtet und einsatzbereit. Bevor die Aufgabe zurückgegeben wird, haben Sie möglicherweise nicht alle Hintergrundaufgaben abgeschlossen, die für das Funktionieren der Kopplung erforderlich sind. Daher sollten Sie nicht damit beginnen, Nachrichten zu senden, bis die Aufgabe zurückgegeben wird. Bei einem Fehler wie ungültigen Anmeldeinformationen oder wenn Backlogwarteschlangen nicht erstellt werden können, werden Ausnahmen ausgelöst, sobald die Aufgabe abgeschlossen ist. Sobald die Aufgabe zurückgegeben wird, überprüfen Sie, ob die Warteschlangen gefunden oder erstellt wurden, indem Sie die [BacklogQueueCount][BacklogQueueCount]-Eigenschaft Ihrer [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions]-Instanz untersuchen. Dieser Vorgang sieht für den obigen Code folgendermaßen aus:

```csharp
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Nächste Schritte
Da Sie nun mit den Grundlagen des asynchronen Messagings in Service Bus vertraut sind, können Sie sich ausführlicher mit [gekoppelten Namespaces][paired namespaces] beschäftigen.

[ServerBusyException]: /dotnet/api/microsoft.servicebus.messaging.serverbusyexception
[System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Microsoft.ServiceBus.Messaging.MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[MessageReceiver]: /dotnet/api/microsoft.servicebus.messaging.messagereceiver
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[EnableSyphon]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_EnableSyphon
[System.TimeSpan.Zero]: https://msdn.microsoft.com/library/system.timespan.zero.aspx
[IsTransient]: /dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient
[UnauthorizedAccessException]: https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx
[BacklogQueueCount]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_BacklogQueueCount
[paired namespaces]: service-bus-paired-namespaces.md

