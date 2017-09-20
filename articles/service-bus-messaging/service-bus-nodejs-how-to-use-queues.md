---
title: Verwenden von Service Bus-Warteschlangen in Node.js | Microsoft Docs
description: "Erfahren Sie mehr über die Verwendung von Service Bus-Warteschlangen in Azure aus einer Node.js-App."
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a87a00f9-9aba-4c49-a0df-f900a8b67b3f
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: f539e11096cbfb305d3724a2942a5487b65e5c7e
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---
# <a name="how-to-use-service-bus-queues-with-nodejs"></a>Verwenden von Service Bus-Warteschlangen mit Node.js

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In diesem Artikel wird beschrieben, wie Sie Service Bus-Warteschlangen mit Node.js verwenden. Die Beispiele sind in JavaScript geschrieben und verwenden das Azure-Modul Node.js. Die Szenarios behandeln die Themen **Erstellen von Warteschlangen**, **Senden und Empfangen von Nachrichten** und **Löschen von Warteschlangen**. Weitere Informationen zu Warteschlangen finden Sie im Abschnitt [Nächste Schritte](#next-steps).

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-nodejs-application"></a>Erstellen einer Node.js-Anwendung
Erstellen Sie eine leere Node.js-Anwendung. Anweisungen zum Erstellen von Node.js-Anwendungen finden Sie unter [Erstellen und Bereitstellen einer Node.js-Anwendung auf einer Azure-Website][Create and deploy a Node.js application to an Azure Website] und [Node.js-Clouddienst][Node.js Cloud Service] (mithilfe von Windows PowerShell).

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren Ihrer Anwendung für die Verwendung von Service Bus
Damit Azure Service Bus verwendet werden kann, laden Sie das Node.js-Azure-Paket herunter und verwenden es. Dieses Paket enthält eine Reihe von Bibliotheken, die mit den Service Bus-REST-Diensten kommunizieren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Verwenden von Node Package Manager (NPM) zum Beziehen des Pakets
1. Verwenden Sie das Befehlsfenster von **Windows PowerShell für Node.js**, um zum Ordner **c:\\node\\sbqueues\\WebRole1** zu navigieren, in dem Sie Ihre Beispielanwendung erstellt haben.
2. Geben Sie **npm install azure** im Befehlsfenster ein. Die Ausgabe sollte in etwa wie folgt aussehen:

    ```
    azure@0.7.5 node_modules\azure
        ├── dateformat@1.0.2-1.2.3
        ├── xmlbuilder@0.4.2
        ├── node-uuid@1.2.0
        ├── mime@1.2.9
        ├── underscore@1.4.4
        ├── validator@1.1.1
        ├── tunnel@0.0.2
        ├── wns@0.5.3
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```
3. Sie können den Befehl **ls** manuell ausführen, um zu überprüfen, ob der Ordner **node_modules** erstellt wurde. In diesem Ordner finden Sie das **azure**-Paket, das die für den Zugriff auf Service Bus-Warteschlangen benötigten Bibliotheken enthält.

### <a name="import-the-module"></a>Importieren des Moduls
Verwenden Sie Editor oder einen anderen Texteditor, um die folgende Zeile am Anfang der Datei **server.js** der Anwendung einzufügen:

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Einrichten einer Azure Service Bus-Verbindung
Das Azure-Modul liest die Umgebungsvariable `AZURE_SERVICEBUS_CONNECTION_STRING`, um Informationen zu erhalten, die für eine Verbindung mit Service Bus benötigt werden. Wenn diese Umgebungsvariable nicht festgelegt wurde, müssen Sie beim Aufrufen von `createServiceBusService` die Kontoinformationen angeben.

Ein Beispiel zum Festlegen der Umgebungsvariablen in einer Konfigurationsdatei für einen Azure-Clouddienst finden Sie unter [Node.js-Clouddienst mit Speicher][Node.js Cloud Service with Storage].

Ein Beispiel zum Festlegen der Umgebungsvariablen im [Azure-Portal][Azure portal] für eine Azure-Website finden Sie unter [Node.js-Webanwendung mit Storage][Node.js Web Application with Storage].

## <a name="create-a-queue"></a>Erstellen einer Warteschlange
Das **ServiceBusService**-Objekt ermöglicht Ihnen, mit Service Bus-Warteschlangen zu arbeiten. Der folgende Code erstellt ein **ServiceBusService**-Objekt. Fügen Sie ihn am Anfang der Datei **server.js** hinzu, nach der Anweisung zum Importieren des Azure-Moduls:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Durch Aufrufen von `createQueueIfNotExists` für das Objekt **ServiceBusService** wird die angegebene Warteschlange zurückgegeben (sofern vorhanden), oder es wird eine neue Warteschlange mit dem angegebenen Namen erstellt. Der folgende Code verwendet `createQueueIfNotExists`, um die Warteschlange `myqueue` zu erstellen oder eine Verbindung mit dieser Warteschlange herzustellen:

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

Die Methode `createServiceBusService` unterstützt zudem weitere Optionen, mit denen Sie Standardeinstellungen für die Warteschlange überschreiben können. Hierzu zählen beispielsweise die Gültigkeitsdauer von Nachrichten und die maximale Warteschlangengröße. Das folgende Beispiel legt die maximale Warteschlangengröße auf 5 GB bei einer Gültigkeitsdauer (Time-to-live, TTL) von 1 Minute fest:

```javascript
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Filter
Mit **ServiceBusService** können Sie optionale Filteroperationen auf Operationen ausführen. Filtervorgänge können Protokollierung, automatische Wiederholung usw. umfassen. Filter sind Objekte, die eine Methode mit der folgenden Signatur implementieren:

```javascript
function handle (requestOptions, next)
```

Nachdem die Vorverarbeitung der Anforderungsoptionen abgeschlossen ist, muss die Methode `next` aufrufen und hierbei eine Rückruffunktion mit der folgenden Signatur übergeben:

```javascript
function (returnObject, finalCallback, next)
```

Nach der Verarbeitung von `returnObject` (die Antwort auf die an den Server gesendete Anforderung) muss der Rückruf entweder `next` aufrufen (sofern vorhanden), um weitere Filter zu verarbeiten, oder er muss einfach `finalCallback` aufrufen, um den Dienstaufruf zu beenden.

Das Azure SDK für Node.js enthält zwei Filter zur Implementierung von Wiederholungslogik: `ExponentialRetryPolicyFilter` und `LinearRetryPolicyFilter`. Der folgende Code erstellt ein Objekt vom Typ `ServiceBusService`, das `ExponentialRetryPolicyFilter` verwendet:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a>Senden von Nachrichten an eine Warteschlange
Um eine Nachricht an eine Service Bus-Warteschlange zu senden, ruft Ihre Anwendung die Methode `sendQueueMessage` für das Objekt **ServiceBusService** auf. Nachrichten, die an die Service Bus-Warteschlangen gesendet werden (und von diesen empfangen werden), sind **BrokeredMessage**-Objekte und verfügen über einen Satz von Standardeigenschaften (z.B. **Label** und **TimeToLive**), ein Wörterbuch, in dem benutzerdefinierte anwendungsspezifische Eigenschaften enthalten sind, sowie einen Bestand beliebiger Anwendungsdaten. Eine Anwendung kann den Text der Nachricht festlegen, indem eine Zeichenfolge als Nachricht übergeben wird. Alle erforderlichen Standardeigenschaften werden mit Standardwerten aufgefüllt.

Das folgende Beispiel zeigt, wie mithilfe von `sendQueueMessage` eine Testnachricht an die Warteschlange `myqueue` gesendet wird:

```javascript
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Service Bus-Warteschlangen unterstützen eine maximale Nachrichtengröße von 256 KB für den [Standard-Tarif](service-bus-premium-messaging.md) und 1 MB für den [Premium-Tarif](service-bus-premium-messaging.md). Der Header, der die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Bei der Anzahl der Nachrichten, die in einer Warteschlange aufgenommen werden können, besteht keine Beschränkung. Allerdings gilt eine Deckelung bei der Gesamtgröße der in einer Warteschlange aufzunehmenden Nachrichten. Die Warteschlangengröße wird bei der Erstellung definiert. Die Obergrenze beträgt 5 GB. Weitere Informationen zu Kontingenten finden Sie unter [Service Bus-Kontingente][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Empfangen von Nachrichten aus einer Warteschlange
Zum Empfangen von Nachrichten aus einer Warteschlange wird die Methode `receiveQueueMessage` für das Objekt **ServiceBusService** verwendet. Nachrichten werden standardmäßig aus der Warteschlange gelöscht, wenn sie gelesen wurden. Sie können jedoch Nachrichten lesen (sprich: einen kurzen Blick darauf werfen) und sperren, ohne sie aus der Warteschlange zu löschen, indem Sie den optionalen Parameter `isPeekLock` auf **true** festlegen.

Das Standardverhalten für das Lesen und Löschen der Nachricht als Teil des Empfangsvorgangs ist das einfachste Modell. Es wird am besten für Szenarios eingesetzt, bei denen es eine Anwendung tolerieren kann, wenn eine Nachricht bei Auftreten eines Fehlers nicht verarbeitet wird. Um dieses Verfahren zu verstehen, stellen Sie sich ein Szenario vor, in dem der Consumer die Empfangsanforderung ausstellt und dann abstürzt, bevor diese verarbeitet wird. Da Service Bus die Nachricht als verwendet markiert hat, wird er jene Nachricht auslassen, die vor dem Absturz verwendet wurde, wenn die Anwendung neu startet und erneut mit der Verwendung von Nachrichten beginnt.

Wenn der Parameter `isPeekLock` auf **true** festgelegt ist, wird der Empfangsvorgang zu einem zweistufigen Vorgang. Dadurch können Anwendungen ohne Toleranz für fehlende Nachrichten unterstützt werden. Wenn Service Bus eine Anfrage erhält, ermittelt der Dienst die nächste zu verarbeitende Nachricht, sperrt diese, um zu verhindern, dass andere Consumer sie erhalten, und sendet sie dann zurück an die Anwendung. Nachdem die Anwendung die Verarbeitung der Nachricht abgeschlossen hat (oder sie zwecks zukünftiger Verarbeitung zuverlässig gespeichert hat), führt sie die zweite Phase des Empfangsprozesses durch: In dieser Phase wird die Methode `deleteMessage` aufgerufen und die zu löschende Nachricht als Parameter angegeben. Die Methode `deleteMessage` markiert die Nachricht als verarbeitet und entfernt sie aus der Warteschlange.

Das folgende Beispiel zeigt, wie Nachrichten mit `receiveQueueMessage` empfangen und verarbeitet werden können. In dem Beispiel wird zunächst eine Nachricht empfangen und gelöscht. Anschließend wird eine Nachricht empfangen, während `isPeekLock` auf **true** festgelegt ist, und die Nachricht anschließend mithilfe von `deleteMessage` gelöscht:

```javascript
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandeln von Anwendungsabstürzen und nicht lesbaren Nachrichten
Service Bus stellt Funktionen zur Verfügung, die Sie bei der ordnungsgemäßen Behandlung von Fehlern in der Anwendung oder bei Problemen beim Verarbeiten einer Nachricht unterstützen. Wenn eine Empfängeranwendung die Nachricht aus einem bestimmten Grund nicht verarbeiten kann, kann sie die Methode `unlockMessage` für das Objekt **ServiceBusService** aufrufen. Dies führt dazu, dass Service Bus die Nachricht innerhalb der Warteschlange entsperrt und verfügbar macht, damit sie erneut empfangen werden kann, und zwar entweder durch dieselbe verarbeitende Anwendung oder durch eine andere verarbeitende Anwendung.

Zudem wird einer in der Warteschlange gesperrten Anwendung ein Zeitlimit zugeordnet. Wenn die Anwendung die Nachricht vor Ablauf des Sperrzeitlimits nicht verarbeiten kann (zum Beispiel, wenn die Anwendung abstürzt), entsperrt Service Bus die Nachricht automatisch und macht sie verfügbar, um erneut empfangen zu werden.

Falls die Anwendung nach der Verarbeitung der Nachricht, aber vor dem Aufruf der Methode `deleteMessage` abstürzt, wird die Nachricht wieder an die Anwendung zugestellt, wenn diese neu gestartet wird. Dies wird häufig als *At Least Once Processing* (Mindestens einmal verarbeiten) bezeichnet und bedeutet, dass jede Nachricht mindestens einmal verarbeitet wird, wobei dieselbe Nachricht in bestimmten Situationen möglicherweise erneut zugestellt wird. Wenn eine doppelte Verarbeitung im betreffenden Szenario nicht geeignet ist, sollten Anwendungsentwickler ihrer Anwendung zusätzliche Logik für den Umgang mit der Übermittlung doppelter Nachrichten hinzufügen. Dies wird häufig durch die Verwendung der **MessageId**-Eigenschaft der Nachricht erzielt, die über mehrere Zustellungsversuche hinweg konstant bleibt.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Warteschlangen finden Sie in den folgenden Ressourcen:

* [Warteschlangen, Themen und Abonnements][Queues, topics, and subscriptions]
* [Azure SDK für Node][Azure SDK for Node]-Repository auf GitHub
* [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application to an Azure Website]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md

