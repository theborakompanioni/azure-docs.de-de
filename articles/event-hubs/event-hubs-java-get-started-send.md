---
title: Senden von Ereignissen an Azure Event Hubs mithilfe von Java | Microsoft-Dokumentation
description: Erste Schritte zum Senden an Event Hubs mit Java
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c4d5e161c9f7af33609be53e7b82f156bb0e33f
ms.openlocfilehash: 45776b0920f65ae9749b00978656bcefa2bf01a8
ms.contentlocale: de-de
ms.lasthandoff: 05/04/2017


---

# <a name="send-events-to-azure-event-hubs-using-java"></a>Senden von Ereignissen an Azure Event Hubs mithilfe von Java

## <a name="introduction"></a>Einführung
Event Hubs sind ein hochgradig skalierbares Aufnahmesystem, das Millionen von Ereignissen pro Sekunde aufnehmen kann, wodurch eine Anwendung die Möglichkeit erhält, die von Ihren verbundenen Geräten und Anwendungen erzeugten immensen Datenmengen zu verarbeiten und zu analysieren. Nach der Erfassung in Event Hubs können Sie Daten über einen beliebigen Echtzeitanalyseanbieter oder ein Speichercluster transformieren und speichern.

Weitere Informationen finden Sie unter [Übersicht über Event Hubs][Event Hubs overview].

In diesem Tutorial wird gezeigt, wie Sie mithilfe einer Konsolenanwendung in Java Ereignisse an einen Event Hub senden. Informationen zum Empfangen von Ereignissen mithilfe der Java-Bibliothek des Ereignisprozessorhosts finden Sie in [diesem Artikel](event-hubs-java-get-started-receive-eph.md). Alternativ können Sie auch links im Inhaltsverzeichnis auf die entsprechende Empfangssprache klicken.

Zur Ausführung dieses Lernprogramms benötigen Sie Folgendes:

* Eine Java-Entwicklungsumgebung. In diesem Lernprogramm wird von [Eclipse](https://www.eclipse.org/)ausgegangen.
* Ein aktives Azure-Konto. <br/>Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Konto erstellen. Einzelheiten finden Sie unter <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Kostenlose Azure-Testversion</a>.

## <a name="send-messages-to-event-hubs"></a>Senden von Nachrichten an Ereignis-Hubs
Die Java-Clientbibliothek für Event Hubs steht für die Verwendung in Maven-Projekten im [zentralen Maven-Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) zur Verfügung. Sie können darauf verweisen, indem Sie in der Maven-Projektdatei die folgende Abhängigkeitsdeklaration verwenden:    

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

Für unterschiedliche Arten von Buildumgebungen können Sie explizit die zuletzt veröffentlichten JAR-Dateien aus dem [zentralen Maven-Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) oder vom [Versionsverteilungspunkt auf GitHub](https://github.com/Azure/azure-event-hubs/releases) abrufen.  

Um einen einfachen Ereignisherausgeber zu erhalten, importieren Sie das Paket *com.microsoft.azure.eventhubs* für die Event Hubs-Clientklassen und das Paket *com.microsoft.azure.servicebus* für Hilfsklassen, z.B. häufige Ausnahmen für den Azure Service Bus-Messagingclient. 

Erstellen Sie für das folgende Beispiel zuerst ein neues Maven-Projekt für eine Konsolen-/Shellanwendung in Ihrer bevorzugten Java-Entwicklungsumgebung. Die Klasse hat den Namen `Send`.     

```Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Ersetzen Sie den Namespace und den Event Hub-Namen durch die Werte, die Sie beim Erstellen des Event Hubs verwendet haben.

```Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Anschließend erstellen Sie ein einzelnes Ereignis, indem Sie eine Zeichenfolge in die entsprechende UTF-8-Bytecodierung umwandeln. Als Nächstes erstellen wir aus der Verbindungszeichenfolge dann eine neue Event Hubs-Clientinstanz und senden die Nachricht ab.   

```Java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Receive events using the Event Processor Host](event-hubs-java-get-started-receive-eph.md) (Empfangen von Ereignissen mithilfe des Ereignisprozessorhosts)
* [Übersicht über Event Hubs][Event Hubs overview]
* [Erstellen eines Event Hubs](event-hubs-create.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
