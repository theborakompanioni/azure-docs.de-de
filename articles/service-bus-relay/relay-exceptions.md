---
title: Azure Relay-Ausnahmen und Tipps zur ihrer Behebung | Microsoft-Dokumentation
description: "Hier erhalten Sie eine Liste mit Azure Relay-Ausnahmen und empfohlenen Maßnahmen zu deren Behebung."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 5f9dd02c-cce0-43b3-8eb8-744f0c27f38c
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 83ff97b59e428e7b617a7f5d1011ca5ddf3060b6
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="azure-relay-exceptions"></a>Azure Relay-Ausnahmen

In diesem Artikel werden einige Ausnahmen aufgelistet, die von den Azure Relay-APIs generiert werden können. Diese Referenz kann geändert werden. Prüfen Sie darum bei Bedarf, ob Aktualisierungen vorgenommen wurden.

## <a name="exception-categories"></a>Ausnahmekategorien

Die Relay-APIs generieren Ausnahmen, die in folgende Kategorien fallen können. Empfohlene Maßnahmen, die Sie durchführen können, um die Ausnahmen zu beheben, sind ebenfalls aufgeführt.

*   **Codierfehler des Benutzers**: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). 

    **Allgemeine Maßnahme**: Korrigieren Sie den Code, bevor Sie fortfahren.
*   **Setup-/Konfigurationsfehler**: [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). 

    **Allgemeine Maßnahme**: Überprüfen Sie die Konfiguration. Ändern Sie die Konfiguration bei Bedarf.
*   **Vorübergehende Ausnahmen**: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). 

    **Allgemeine Maßnahme**: Wiederholen Sie den Vorgang, oder benachrichtigen Sie die Benutzer.
*   **Andere Ausnahmen**: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx). 

    **Allgemeine Maßnahme**: Richtet sich nach dem Ausnahmetyp. Informationen hierzu finden Sie in der Tabelle im nächsten Abschnitt. 

## <a name="exception-types"></a>Ausnahmetypen

In der folgenden Tabelle werden die Typen von Messagingausnahmen und ihre Ursachen aufgelistet. Empfohlene Maßnahmen, die Sie durchführen können, um die Ausnahmen zu beheben, sind ebenfalls aufgeführt.

| **Ausnahmetyp** | **Beschreibung** | **Vorgeschlagene Maßnahme** | **Hinweis zur automatischen oder sofortigen Wiederholung** |
| --- | --- | --- | --- |
| [Timeout](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Der Server hat nicht innerhalb der von [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout)vorgegebenen Zeit auf den angeforderten Vorgang reagiert. Möglicherweise hat der Server den angeforderten Vorgang abgeschlossen. Diese Ausnahme kann aufgrund von Verzögerungen im Netzwerk oder in der Infrastruktur auftreten. |Überprüfen Sie den Systemzustand auf Konsistenz, und wiederholen Sie den Vorgang bei Bedarf. Siehe [TimeoutException](#timeoutexception). |In einigen Fällen kann eine Wiederholung helfen. Fügen Sie dem Code eine Wiederholungslogik hinzu. |
| [Ungültiger Vorgang](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |Der angeforderte Benutzervorgang ist auf dem Server oder innerhalb des Diensts nicht zulässig. Sehen Sie sich die Details in der Ausnahmemeldung an. |Überprüfen Sie den Code und die Dokumentation. Stellen Sie sicher, dass der angeforderte Vorgang gültig ist. |Eine Wiederholung hilft nicht. |
| [Vorgang abgebrochen](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Es wurde versucht, einen Vorgang für ein Objekt aufzurufen, das bereits geschlossen, abgebrochen oder verworfen wurde. In seltenen Fällen wurde die Ambient-Transaktion bereits verworfen. |Überprüfen Sie den Code, und stellen Sie sicher, dass keine Vorgänge für verworfene Objekte aufgerufen werden. |Eine Wiederholung hilft nicht. |
| [Nicht autorisierter Zugriff](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |Das [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) -Objekt konnte kein Token abrufen, das Token ist ungültig, oder das Token enthält nicht die Ansprüche, die zum Ausführen des Vorgangs erforderlich sind. |Stellen Sie sicher, dass der Tokenanbieter mit den richtigen Werten erstellt wird. Überprüfen Sie die Konfiguration für den Access Control Service. |In einigen Fällen kann eine Wiederholung helfen. Fügen Sie dem Code eine Wiederholungslogik hinzu. |
| [Argumentausnahme](https://msdn.microsoft.com/library/system.argumentexception.aspx),<br /> [Argument NULL](https://msdn.microsoft.com/library/system.argumentnullexception.aspx),<br />[Argument außerhalb des gültigen Bereichs](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Mindestens einer der folgenden Fehler ist aufgetreten:<br />Mindestens eines der für die Methode bereitgestellten Argumente ist ungültig.<br /> Der für [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) oder [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) bereitgestellte URI enthält mindestens ein Pfadsegment.<br />Das für [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) oder für [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) bereitgestellte URI-Schema ist ungültig. <br />Der Eigenschaftswert ist größer als 32 KB. |Überprüfen Sie den aufrufenden Code, und stellen Sie sicher, dass die Argumente richtig sind. |Eine Wiederholung hilft nicht. |
| [Server ausgelastet](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |Der Dienst kann die Anforderung derzeit nicht verarbeiten. |Der Client kann eine gewisse Zeit warten und dann den Vorgang wiederholen. |Der Client kann versuchen, den Vorgang nach einem bestimmten Zeitintervall zu wiederholen. Wenn die Wiederholung zu einer anderen Ausnahme führt, überprüfen Sie das Wiederholungsverhalten dieser Ausnahme. |
| [Kontingent überschritten](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |Die Messagingentität hat die maximal zulässige Größe erreicht. |Schaffen Sie Platz in der Entität, indem Sie Nachrichten aus der Entität oder ihren Unterwarteschlangen empfangen. Siehe [QuotaExceededException](#quotaexceededexception). |Eine Wiederholung kann helfen, wenn in der Zwischenzeit Nachrichten entfernt wurden. |
| [Nachrichtengröße überschritten](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Eine Nachrichtennutzlast überschreitet den Grenzwert von 256 KB. Beachten Sie, dass es sich beim Grenzwert von 256-KB um die Gesamtgröße der Nachricht handelt. Zur Gesamtgröße der Nachricht gehören auch Systemeigenschaften und Microsoft .NET-Mehraufwand. |Reduzieren Sie die Größe der Nachrichtennutzlast, und wiederholen Sie den Vorgang. |Eine Wiederholung hilft nicht. |

## <a name="quotaexceededexception"></a>QuotaExceededException

[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) gibt an, dass das Kontingent für eine bestimmte Entität überschritten wurde.

Bei Relay umschließt diese Ausnahme die [System.ServiceModel.QuotaExceededException](https://msdn.microsoft.com/library/system.servicemodel.quotaexceededexception.aspx), die angibt, dass die maximale Anzahl von Listenern für diesen Endpunkt überschritten wurde. Dies wird durch den **MaximumListenersPerEndpoint** -Wert der Ausnahmemeldung angegeben.

## <a name="timeoutexception"></a>TimeoutException
Eine [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) zeigt an, dass ein von einem Benutzer initiierter Vorgang länger als das Timeout des Vorgangs dauert. 

Überprüfen Sie den Wert der Eigenschaft [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit). Durch Erreichen dieses Grenzwerts kann auch eine [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) ausgelöst werden.

Bei Relay können Timeoutausnahmen auftreten, wenn Sie erstmals eine Verbindung zwischen Relay und Sender öffnen. Für diese Ausnahme gibt es zwei häufige Ursachen:

*   Der [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx)-Wert ist möglicherweise zu klein (möglicherweise nur um einen Sekundenbruchteil).
* Ein lokaler Relaylistener reagiert möglicherweise nicht (oder Probleme mit Firewallregeln verhindern, dass Listener neue Clientverbindungen akzeptieren), und der [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx)-Wert ist kleiner als ungefähr 20 Sekunden.

Beispiel:

```
'System.TimeoutException’: The operation did not complete within the allotted timeout of 00:00:10.
The time allotted to this operation may have been a portion of a longer timeout.
```

### <a name="common-causes"></a>Häufige Ursachen
Für diesen Fehler gibt es zwei häufige Ursachen:

*   **Falsche Konfiguration**
    
    Das Zeitlimit für den Vorgang ist möglicherweise zu gering für die Betriebsbedingung. Der Standardwert für das Zeitlimit für den Vorgang im Client-SDK beträgt 60 Sekunden. Überprüfen Sie, ob der Wert in Ihrem Code auf einen zu geringen Wert festgelegt wurde. Beachten Sie, dass CPU-Auslastung und Netzwerkbedingungen sich auf die Zeit auswirken können, die für den Abschluss eines Vorgangs benötigt wird. Es empfiehlt sich, das Timeout für den Vorgang nicht auf einen sehr kleinen Wert festzulegen.
*   **Vorübergehender Dienstfehler**

    In einigen Fällen können bei der Verarbeitung von Anforderungen durch den Relay-Dienst Verzögerungen auftreten. Dies kann beispielsweise in Zeiträumen mit hohem Datenverkehrsaufkommen passieren. Wiederholen Sie in solchen Fällen den Vorgang nach einer Verzögerung so lange, bis der Vorgang erfolgreich ist. Wenn der gleiche Vorgang auch nach mehreren Versuchen nicht erfolgreich ist, überprüfen Sie auf der [Website mit dem Azure-Status](https://azure.microsoft.com/status/), ob Dienstausfälle bekannt sind.

## <a name="next-steps"></a>Nächste Schritte
* [Azure Relay – häufig gestellte Fragen](relay-faq.md)
* [Erstellen eines Relaynamespace](relay-create-namespace-portal.md)
* [Erste Schritte mit Azure Relay und NET](relay-hybrid-connections-dotnet-get-started.md)
* [Erste Schritte mit Azure Relay und Node](relay-hybrid-connections-node-get-started.md)


