---
title: Azure Service Bus-Messagingausnahmen | Microsoft-Dokumentation
description: "Liste mit Service Bus-Messagingausnahmen und vorgeschlagenen Maßnahmen."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/03/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: 3b543b1c94122a037cdd3b16e25d60957add1cb7
ms.lasthandoff: 03/04/2017


---
# <a name="service-bus-messaging-exceptions"></a>Service Bus-Messagingausnahmen
In diesem Artikel werden einige Ausnahmen aufgelistet, die von den Microsoft Azure Service Bus-Messaging-APIs generiert werden. Diese Referenz kann geändert werden. Prüfen Sie darum bei Bedarf, ob Aktualisierungen vorgenommen wurden.

## <a name="exception-categories"></a>Ausnahmekategorien
Die von den Messaging-APIs generierten Ausnahmen können – zusammen mit den zugehörigen Korrekturmaßnahmen – zu den folgenden Kategorien gehören. Beachten Sie, dass die Bedeutung und die Ursachen einer Ausnahme je nach Art der Messagingentität (Warteschlangen/Themen, Event Hubs) variieren können:

1. Fehler der Benutzercodierung ([System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Allgemeine Maßnahme: Korrigieren Sie den Code, bevor Sie fortfahren.
2. Fehler bei der Einrichtung oder Konfiguration: ([Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Allgemeine Maßnahme: Überprüfen Sie die Konfiguration, und ändern Sie diese gegebenenfalls.
3. Vorübergehende Ausnahmen ([Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Allgemeine Maßnahme: Wiederholen Sie den Vorgang, oder benachrichtigen Sie die Benutzer.
4. Andere Ausnahmen ([System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception)). Allgemeine Aktion: spezifisch für den Typ der Ausnahme; zu finden in der Tabelle im folgenden Abschnitt. 

## <a name="exception-types"></a>Ausnahmetypen
In der folgenden Tabelle werden die Typen von Messagingausnahmen, ihre Ursachen und Vorschläge für Korrekturmaßnahmen aufgelistet.

| **Ausnahmetyp** | **Beschreibung/Ursache/Beispiele** | **Vorgeschlagene Maßnahme** | **Hinweis zur automatischen/sofortigen Wiederholung** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Der Server hat nicht innerhalb der von [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout)vorgegebenen Zeit auf den angeforderten Vorgang reagiert. Unter Umständen hat der Server den angeforderten Vorgang abgeschlossen. Diese Ausnahme kann aufgrund von Verzögerungen im Netzwerk oder in der Infrastruktur auftreten. |Überprüfen Sie den Systemzustand auf Konsistenz, und wiederholen Sie den Vorgang bei Bedarf. Siehe [Timeoutausnahmen](#timeoutexception). |In einigen Fällen kann eine Wiederholung helfen. Fügen Sie dem Code eine Wiederholungslogik hinzu. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |Der angeforderte Benutzervorgang ist auf dem Server oder innerhalb des Diensts nicht zulässig. Sehen Sie sich die Details in der Ausnahmemeldung an. Beispielsweise generiert [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) diese Ausnahme, wenn die Meldung im [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) -Modus empfangen wurde. |Überprüfen Sie den Code und die Dokumentation. Stellen Sie sicher, dass der angeforderte Vorgang gültig ist. |Eine Wiederholung hilft nicht. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Es wurde versucht, einen Vorgang für ein Objekt aufzurufen, das bereits geschlossen, abgebrochen oder verworfen wurde. In seltenen Fällen wurde die Ambient-Transaktion bereits verworfen. |Überprüfen Sie den Code, und stellen Sie sicher, dass keine Vorgänge für verworfene Objekte aufgerufen werden. |Eine Wiederholung hilft nicht. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |Das [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) -Objekt konnte kein Token abrufen, das Token ist ungültig, oder das Token enthält nicht die Ansprüche, die zum Ausführen des Vorgangs erforderlich sind. |Stellen Sie sicher, dass der Tokenanbieter mit den richtigen Werten erstellt wird. Überprüfen Sie die Konfiguration für den Access Control Service. |In einigen Fällen kann eine Wiederholung helfen. Fügen Sie dem Code eine Wiederholungslogik hinzu. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Mindestens eines der für die Methode bereitgestellten Argumente ist ungültig.<br /> Der für [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) oder [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) bereitgestellte URI enthält Pfadsegmente.<br /> Das für [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) oder für [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) bereitgestellte URI-Schema ist ungültig. <br />Der Eigenschaftswert ist größer als 32KB. |Überprüfen Sie den aufrufenden Code, und stellen Sie sicher, dass die Argumente richtig sind. |Eine Wiederholung hilft nicht. |
| [MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) |Die dem Vorgang zugeordnete Entität ist nicht vorhanden oder wurde gelöscht. |Stellen Sie sicher, dass die Entität vorhanden ist. |Eine Wiederholung hilft nicht. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Es wurde versucht, eine Nachricht mit einer bestimmten Sequenznummer zu empfangen. Diese Nachricht wurde nicht gefunden. |Stellen Sie sicher, dass die Nachricht nicht bereits empfangen wurde. Überprüfen Sie in der Warteschlange für unzustellbare Nachrichten, ob die Nachricht als unzustellbar gekennzeichnet wurde. |Eine Wiederholung hilft nicht. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |Der Client kann keine Verbindung mit Service Bus herstellen. |Stellen Sie sicher, dass der angegebene Hostname richtig und der Host erreichbar ist. |Eine Wiederholung kann helfen, wenn zeitweilige Verbindungsprobleme vorliegen. |
| [ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |Der Dienst kann die Anforderung derzeit nicht verarbeiten. |Der Client kann eine gewisse Zeit warten und dann den Vorgang wiederholen. |Der Client kann den Vorgang nach einer gewissen Zeitspanne wiederholen. Wenn die Wiederholung zu einer anderen Ausnahme führt, überprüfen Sie das Wiederholungsverhalten dieser Ausnahme. |
| [MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception) |Das der Nachricht zugeordnete Sperrtoken ist abgelaufen, oder das Sperrtoken wurde nicht gefunden. |Verwerfen Sie die Nachricht. |Eine Wiederholung hilft nicht. |
| [SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception) |Die dieser Sitzung zugeordnete Sperre ist verloren gegangen. |Brechen Sie das [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) -Objekt ab. |Eine Wiederholung hilft nicht. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Eine allgemeine Messagingausnahme, die in folgenden Fällen ausgelöst werden kann:<br /> Es wird versucht, ein [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient)-Element mit einem Namen oder einem Pfad zu erstellen, der zu einem anderen Entitätstyp gehört (z.B. zu einem Thema).<br />  Es wird versucht, eine Nachricht zu senden, die größer als 256 KB ist. Der Server oder der Dienst hat beim Verarbeiten der Anforderung einen Fehler festgestellt. Sehen Sie sich die Details in der Ausnahmemeldung an. Dies ist meist eine vorübergehende Ausnahme. |Überprüfen Sie den Code, und stellen Sie sicher, dass nur serialisierbare Objekte für den Nachrichtentext verwendet werden (oder verwenden Sie ein benutzerdefiniertes Serialisierungsprogramm). Überprüfen Sie die Dokumentation für die unterstützten Werttypen der Eigenschaften, und verwenden Sie nur unterstützte Typen. Überprüfen Sie die [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient) -Eigenschaft. Wenn sie den Wert **True**aufweist, können Sie versuchen, den Vorgang zu wiederholen. |Das Verhalten einer Wiederholung ist nicht definiert, u. U. hilft sie nicht. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Es wurde versucht, eine Entität mit einem Namen zu erstellen, der bereits von einer anderen Entität in diesem Dienstnamespace verwendet wird. |Löschen Sie die vorhandene Entität, oder wählen Sie einen anderen Namen für die zu erstellende Entität. |Eine Wiederholung hilft nicht. |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |Die Messagingentität hat die maximal zulässige Größe erreicht, oder die maximale Anzahl von Verbindungen zu einem Namespace wurde überschritten. |Schaffen Sie Platz in der Entität, indem Sie Nachrichten aus der Entität oder ihren Unterwarteschlangen empfangen. Siehe [QuotaExceededException](#quotaexceededexception). |Eine Wiederholung kann helfen, wenn in der Zwischenzeit Nachrichten entfernt wurden. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Service Bus gibt diese Ausnahme zurück, wenn Sie versuchen, eine ungültige Regelaktion zu erstellen. Service Bus fügt diese Ausnahme zu einer unzustellbaren Nachricht hinzu, wenn beim Verarbeiten der Regelaktion für diese Nachricht ein Fehler festgestellt wird. |Überprüfen Sie die Regelaktion auf ihre Richtigkeit. |Eine Wiederholung hilft nicht. |
| [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) |Service Bus gibt diese Ausnahme zurück, wenn Sie versuchen, einen ungültigen Filter zu erstellen. Service Bus fügt diese Ausnahme zu einer unzustellbaren Nachricht hinzu, wenn beim Verarbeiten des Filters für diese Nachricht ein Fehler festgestellt wird. |Überprüfen Sie den Filter auf Richtigkeit. |Eine Wiederholung hilft nicht. |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Es wurde versucht, eine Sitzung mit einer bestimmten Sitzungs-ID zu akzeptieren, aber die Sitzung wird derzeit durch einen anderen Client gesperrt. |Stellen Sie sicher, dass die Sperre für die Sitzung von den anderen Clients aufgehoben wird. |Eine Wiederholung kann helfen, wenn die Sitzung in der Zwischenzeit freigegeben wurde. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |Die Transaktion umfasst zu viele Vorgänge. |Reduzieren Sie die Anzahl der Vorgänge, die Teil dieser Transaktion sind. |Eine Wiederholung hilft nicht. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) |Es wurde ein Laufzeitvorgang für eine deaktivierte Entität angefordert. |Aktivieren Sie die Entität. |Eine Wiederholung kann helfen, wenn die Entität in der Zwischenzeit aktiviert wurde. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Service Bus gibt diese Ausnahme zurück, wenn Sie eine Nachricht zu einem Thema senden, für das eine Vorfilterung aktiviert wurde. |Stellen Sie sicher, dass mindestens ein Filter übereinstimmt. |Eine Wiederholung hilft nicht. |
| [MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Eine Nachrichtennutzlast überschreitet den Grenzwert von 256 KB. Beachten Sie, dass der Grenzwert von 256 KB für die Gesamtgröße der Nachricht gilt, zu der auch Systemeigenschaften und .NET-Mehraufwand gehören. |Reduzieren Sie die Größe der Nachrichtennutzlast, und wiederholen Sie den Vorgang. |Eine Wiederholung hilft nicht. |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |Die Ambient-Transaktion (*Transaction.Current*) ist ungültig. Sie wurde möglicherweise abgeschlossen oder abgebrochen. Unter Umständen enthält die innere Ausnahme weitere Informationen. | |Eine Wiederholung hilft nicht. |
| [TransactionInDoubtException](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |Es wurde versucht, einen Vorgang für eine unsichere Transaktion auszuführen, oder es wurde versucht, ein Commit für die Transaktion auszuführen, und die Transaktion wurde unsicher. |Die Anwendung muss diese Ausnahme (als Sonderfall) behandeln, da für die Transaktion u. U. bereits ein Commit ausgeführt wurde. |- |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) gibt an, dass das Kontingent für eine bestimmte Entität überschritten wurde.

### <a name="queues-and-topics"></a>Warteschlangen und Themen
Für Warteschlangen und Themen ist dies oft die Größe der Warteschlange. Die Fehlermeldungseigenschaft enthält weitere Details, wie im folgenden Beispiel gezeigt:

```
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: The maximum entity size has been reached or exceeded for Topic: ‘xxx-xxx-xxx’. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

Die Meldung besagt, dass das Thema seine maximale Größe überschritten hat, in diesem Fall 1GB (Standardgrenzwert). 

### <a name="namespaces"></a>Namespaces

Für Namespaces kann [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) angeben, dass eine Anwendung die maximale Anzahl der Verbindungen zu einem Namespace überschritten hat. Beispiel:

```
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

#### <a name="common-causes"></a>Häufige Ursachen
Es gibt zwei häufige Ursachen für diesen Fehler: die Warteschlange für unzustellbare Nachrichten und nicht funktionierende Nachrichtenempfänger.

1. **Warteschlange für unzustellbare Nachrichten**
    Ein Leser kann Nachrichten nicht abschließen, und die Nachrichten werden nach Ablauf der Sperre an die Warteschlange/das Thema zurückgegeben. Dies kann auftreten, wenn der Leser auf eine Ausnahme trifft, die den Aufruf von [BrokeredMessage.Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)verhindert. Nachdem eine Nachricht 10 Mal gelesen wurde, wird sie standardmäßig in die Warteschlange für unzustellbare Nachrichten verschoben. Dieses Verhalten wird von der Eigenschaft [QueueDescription.MaxDeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.maxdeliverycount.aspx) gesteuert, die standardmäßig den Wert 10 aufweist. Wenn sich Nachrichten in der Warteschlange für unzustellbare Nachrichten anhäufen, belegen sie Speicherplatz.
   
    Um das Problem zu beheben, lesen und schließen Sie die Nachrichten in der Warteschlange für unzustellbare Nachrichten ab – genau wie in jeder anderen Warteschlange. Die Klasse [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) enthält sogar eine Methode [FormatDeadLetterPath](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.formatdeadletterpath.aspx), die Sie beim Formatieren des Pfads für die Warteschlange für unzustellbare Nachrichten unterstützt.
2. **Empfänger beendet**
    Ein Empfänger hat den Empfang von Nachrichten aus einer Warteschlange oder einem Abonnement beendet. Dies können Sie anhand der Eigenschaft [QueueDescription.MessageCountDetails](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecountdetails.aspx) feststellen, die die vollständige Aufschlüsselung der Nachrichten zeigt. Wenn die Eigenschaft [ActiveMessageCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagecountdetails.activemessagecount.aspx) hoch ist oder anwächst, werden die Nachrichten nicht so schnell gelesen, wie sie geschrieben werden.

### <a name="event-hubs"></a>Event Hubs
Für Event Hubs gilt ein Limit von 20 Verbrauchergruppen pro Event Hub. Wenn Sie versuchen, weitere zu erstellen, erhalten Sie eine [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

## <a name="timeoutexception"></a>TimeoutException
Eine [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) zeigt an, dass ein von einem Benutzer initiierter Vorgang länger als das Timeout des Vorgangs dauert. 

Überprüfen Sie den Wert der [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit)-Eigenschaft, da das Erreichen dieses Limits auch eine [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) auslösen kann.

### <a name="queues-and-topics"></a>Warteschlangen und Themen
Für Warteschlangen und Themen wird das Zeitlimit entweder in der [MessagingFactorySettings.OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout)-Eigenschaft als Teil der Verbindungszeichenfolge oder über [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder) angegeben. Die Fehlermeldung selbst kann variieren, aber sie enthält immer den Timeoutwert für den aktuellen Vorgang. 

### <a name="event-hubs"></a>Event Hubs
Für Event Hubs wird das Zeitlimit entweder als Teil der Verbindungszeichenfolge oder über [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder)angegeben. Die Fehlermeldung selbst kann variieren, aber sie enthält immer den Timeoutwert für den aktuellen Vorgang. 

### <a name="common-causes"></a>Häufige Ursachen
Es gibt zwei häufige Ursachen für diesen Fehler: eine falsche Konfiguration oder ein vorübergehender Dienstfehler.

1. **Falsche Konfiguration**
    Das Zeitlimit für den Vorgang ist möglicherweise zu klein für die Betriebsbedingung. Der Standardwert für das Zeitlimit für den Vorgang im Client-SDK beträgt 60 Sekunden. Überprüfen Sie, ob der Wert in Ihrem Code auf einen zu geringen Wert festgelegt wurde. Beachten Sie, dass sich die Netzwerk- und CPU-Auslastung auf die Zeit auswirken kann, die zum Abschließen eines bestimmten Vorgangs erforderlich ist. Daher sollte das Zeitlimit für den Vorgang nicht auf einen sehr kleinen Wert festgelegt werden.
2. **Vorübergehender Dienstfehler**
    Gelegentlich kann es beim Service Bus-Dienst zu Verzögerungen bei der Verarbeitung von Anforderungen kommen, z.B. in Zeiten mit hohem Datenverkehr. In solchen Fällen können Sie den Vorgang nach einer kurzen Verzögerung so lange wiederholen, bis der Vorgang erfolgreich ist. Wenn der gleiche Vorgang auch nach mehreren Versuchen nicht erfolgreich ist, besuchen Sie die Website mit dem [Azure-Status](https://azure.microsoft.com/status/), um zu überprüfen, ob Dienstausfälle bekannt sind.

## <a name="next-steps"></a>Nächste Schritte
Die vollständige Referenz zu den .NET-APIs für Service Bus und Event Hubs finden Sie unter [Azure .NET API reference](/dotnet/api/) (Referenz zu den .NET-APIs für Azure).

Weitere Informationen zu [Service Bus](https://azure.microsoft.com/services/service-bus/)finden Sie in den folgenden Themen.

* [Übersicht über Service Bus-Messaging](service-bus-messaging-overview.md)
* [Service Bus – Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-Architektur](service-bus-architecture.md)


