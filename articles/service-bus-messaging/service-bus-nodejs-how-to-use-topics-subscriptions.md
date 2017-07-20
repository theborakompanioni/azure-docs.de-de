---
title: Verwenden von Azure Service Bus-Themen und -Abonnements mit Node.js | Microsoft Docs
description: Erfahren Sie mehr zur Verwendung von Service Bus-Themen und -Abonnements in Azure aus einer Node.js-App.
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 04/27/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 8f987d079b8658d591994ce678f4a09239270181
ms.openlocfilehash: ef14dbaecf5675fa92c91cb5df58b65f59764fa7
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Verwenden von Service Bus-Themen und -Abonnements
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In diesem Leitfaden erfahren Sie, wie Sie Service Bus-Themen und -Abonnements über Node.js-Anwendungen verwenden. Die behandelten Szenarios umfassen **das Erstellen von Themen und Abonnements**, **das Erstellen von Abonnementfiltern**, **das Senden von Nachrichten an ein Thema**, **das Empfangen von Nachrichten von einem Abonnement** und **das Löschen von Themen und Abonnements**. Weitere Informationen zu Themen und Abonnements finden Sie im Abschnitt [Nächste Schritte](#next-steps).

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Erstellen einer Node.js-Anwendung
Erstellen Sie eine leere Node.js-Anwendung. Anweisungen zum Erstellen von Node.js-Anwendungen finden Sie unter [Erstellen und Bereitstellen einer Node.js-Anwendung auf einer Azure-Website] zur Verwendung des [Node.js-Clouddiensts][Node.js Cloud Service] mit Windows PowerShell oder einer Website mit WebMatrix.

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren Ihrer Anwendung für die Verwendung von Service Bus
Um Service Bus zu verwenden, laden Sie das Node.js-Azure-Paket herunter. Dieses Paket enthält eine Reihe von Bibliotheken, die mit den Service Bus-REST-Diensten kommunizieren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Verwenden von Node Package Manager (NPM) zum Beziehen des Pakets
1. Verwenden Sie eine Befehlszeilenschnittstelle, z.B.**PowerShell** (Windows,) **Terminal** (Mac) oder **Bash** (Unix), und navigieren Sie zu dem Ordner, in dem Sie die Beispielanwendung erstellt haben.
2. Geben Sie **npm install azure** im Befehlsfenster ein. Die Ausgabe dieses Befehls sollte wie folgt aussehen:

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
3. Sie können den Befehl **ls** manuell ausführen, um zu überprüfen, ob der Ordner **node\_modules** erstellt wurde. In diesem Ordner finden Sie das Paket **azure**, das die für den Zugriff auf Service Bus-Themen benötigten Bibliotheken enthält.

### <a name="import-the-module"></a>Importieren des Moduls
Verwenden Sie Editor oder einen anderen Text-Editor, um die folgende Zeile am Anfang der Datei **server.js** der Anwendung einzufügen:

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Einrichten einer Service Bus-Verbindung
Das Azure-Modul liest die Umgebungsvariablen AZURE\_SERVICEBUS\_NAMESPACE und AZURE\_SERVICEBUS\_ACCESS\_KEY nach Informationen aus, die für eine Verbindung mit Service Bus benötigt werden. Wenn diese Umgebungsvariablen nicht festgelegt wurden, müssen Sie beim Aufruf von **createServiceBusService** die Kontoinformationen angeben.

Ein Beispiel zum Festlegen der Umgebungsvariablen für einen Azure-Clouddienst finden Sie unter [Node.js-Clouddienst mit Speicher][Node.js Cloud Service with Storage].

Ein Beispiel zum Festlegen der Umgebungsvariablen für eine Azure-Website finden Sie unter [Node.js-Webanwendung mit Speicher][Node.js Web Application with Storage].

## <a name="create-a-topic"></a>Erstellen eines Themas
Das **ServiceBusService**-Objekt ermöglicht es Ihnen, mit Themen zu arbeiten. Der folgende Code erstellt ein **ServiceBusService**-Objekt. Fügen Sie ihn am Anfang der Datei **server.js** hinzu, nach der Anweisung zum Importieren des Azure-Moduls:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Durch das Aufrufen von **createTopicIfNotExists** im **ServiceBusService**-Objekt wird das angegebene Thema zurückgegeben (falls vorhanden), oder es wird ein neues Thema mit dem angegebenen Namen erstellt. Folgende Codes verwenden **createTopicIfNotExists**, um ein Thema namens „MyTopic“ zu erstellen oder eine Verbindung mit ihm herzustellen:

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

**createServiceBusService** unterstützt zudem weitere Optionen, mit denen Sie die Standardthemeneinstellungen überschreiben können, wie zum Beispiel die Nachrichtenlebensdauer oder die maximale Themengröße. Das folgende Beispiel legt die maximale Themengröße auf 5 GB bei einer Gültigkeitsdauer (Time-to-live, TTL) von 1 Minute fest:

```javascript
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filter
Mit **ServiceBusService** können Sie optionale Filteroperationen auf Operationen ausführen. Filtervorgänge können Protokollierung, automatische Wiederholung usw. umfassen. Filter sind Objekte, die eine Methode mit der Signatur implementieren:

```javascript
function handle (requestOptions, next)
```

Nachdem die Vorverarbeitung der Anforderungsoptionen abgeschlossen ist, ruft die Methode `next` auf und übergibt dabei eine Rückruffunktion mit der folgenden Signatur:

```javascript
function (returnObject, finalCallback, next)
```

Nachdem das **returnObject**-Objekt (die Antwort auf die an den Server gesendete Anforderung) verarbeitet wurde, muss in dieser Rückruffunktion entweder „next“ aufgerufen werden(falls vorhanden), um weitere Filter zu verarbeiten, oder andernfalls einfach **finalCallback**, um den Dienstaufruf zu beenden.

Zwei Filter, die eine Wiederholungslogik implementieren, sind im Azure SDK für Node.js enthalten: **ExponentialRetryPolicyFilter** und **LinearRetryPolicyFilter**. Der folgende Code erstellt ein **ServiceBusService**-Objekt, das den **ExponentialRetryPolicyFilter** verwendet:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a>Erstellen von Abonnements
Themenabonnements werden ebenfalls mit dem **ServiceBusService**-Objekt erstellt. Abonnements werden benannt und können einen optionalen Filter aufweisen, der die Nachrichten einschränkt, die an die virtuelle Warteschlange des Abonnements übermittelt werden.

> [!NOTE]
> Abonnements sind persistent und bleiben erhalten, bis sie selbst oder die mit ihnen verbundenen Themen gelöscht werden. Wenn Ihre Anwendung Logik zum Erstellen eines Abonnements beinhaltet, sollte sie zuerst mithilfe der **getSubscription**-Methode überprüfen, ob das Abonnement bereits vorhanden ist.
>
>

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen eines Abonnements mit dem Standardfilter (MatchAll)
**MatchAll** ist der Standardfilter, der verwendet wird, wenn beim Erstellen eines neuen Abonnements kein Filter angegeben wird. Wenn der Filter **MatchAll** verwendet wird, werden alle für das Thema veröffentlichten Nachrichten in die virtuelle Warteschlange des Abonnements gestellt. Mit dem folgenden Beispiel wird ein Abonnement namens „AllMessages“ erstellt, für das der Standardfilter **MatchAll** verwendet wird.

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Erstellen von Abonnements mit Filtern
Sie können auch Filter erstellen, durch die Sie angeben können, welche an ein Thema gesendeten Nachrichten in einem bestimmten Themenabonnement angezeigt werden sollen.

Der von Abonnements unterstützte flexibelste Filtertyp ist **SqlFilter**, der eine Teilmenge von SQL92 implementiert. SQL-Filter werden auf die Eigenschaften der Nachrichten angewendet, die für das Thema veröffentlicht werden. Weitere Informationen zu den Ausdrücken, die mit einem SQL-Filter verwendet werden können, finden Sie in der Syntax von [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

Sie können mithilfe der **createRule**-Methode des **ServiceBusService**-Objekts einem Abonnement Filter hinzufügen. Durch diese Methode können Sie neue Filter zu einem vorhandenen Abonnement hinzufügen.

> [!NOTE]
> Da der Standardfilter automatisch auf alle neuen Abonnements angewendet wird, müssen Sie zuerst den Standardfilter entfernen. Andernfalls überschreibt **MatchAll** alle Filter, die Sie angeben. Sie können die Standardregel mithilfe der **deleteRule**-Methode des **ServiceBusService**-Objekts entfernen.
>
>

Mit dem folgenden Beispiel wird ein Abonnement namens `HighMessages` mit einem **SqlFilter** erstellt, der nur Nachrichten auswählt, deren benutzerdefinierte **messagenumber**-Eigenschaft größer ist als 3:

```javascript
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'HighMessages',
            'HighMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Ebenso erstellt das folgende Beispiel ein Abonnement namens `LowMessages` mit einem **SqlFilter**-Element, das nur Nachrichten auswählt, deren benutzerdefinierte **messagenumber**-Eigenschaft kleiner oder gleich 3 ist:

```javascript
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'LowMessages',
            'LowMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Wenn jetzt eine Nachricht an `MyTopic` gesendet wird, wird diese stets an das `AllMessages`-Themenabonnement zugestellt. Sie wird selektiv an die Empfänger der Abonnements `HighMessages` und `LowMessages` zugestellt (je nach Inhalt der Nachricht).

## <a name="how-to-send-messages-to-a-topic"></a>Senden von Nachrichten an ein Thema
Um eine Nachricht an ein Service Bus-Thema zu senden, muss die Anwendung die **sendTopicMessage**-Methode des **ServiceBusService**-Objekts verwenden.
Nachrichten, die an Service Bus-Themen gesendet werden, sind **BrokeredMessage**-Objekte.
**BrokeredMessage**-Objekte weisen eine Reihe von Standardeigenschaften (wie etwa **Label** und **TimeToLive**), ein Wörterbuch zur Aufnahme benutzerdefinierter anwendungsspezifischer Eigenschaften und einen Haupttext mit Zeichenfolgendaten auf. Eine Anwendung kann den Nachrichtentext festlegen, indem sie einen Zeichenfolgenwert an **sendTopicMessage** weitergibt. Erforderliche Standardeigenschaften werden mit den Standardwerten ausgefüllt.

Das folgende Beispiel zeigt, wie Sie fünf Testnachrichten an "MyTopic" senden. Beachten Sie, dass der Eigenschaftswert **MessageNumber** jeder Nachricht gemäß der Iteration der Schleife variiert (dadurch wird bestimmt, welche Abonnements die Nachricht erhalten):

```javascript
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Service Bus-Themen unterstützen eine maximale Nachrichtengröße von 256 KB im [Standard-Tarif](service-bus-premium-messaging.md) und 1 MB im [Premium-Tarif](service-bus-premium-messaging.md). Der Header, der die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung für die Anzahl der Nachrichten, die ein Thema enthält. Es gibt jedoch eine Obergrenze für die Gesamtgröße der Nachrichten eines Themas. Die Themengröße wird bei der Erstellung definiert. Die Obergrenze beträgt 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten aus einem Abonnement
Nachrichten werden von einem Abonnement über die **receiveSubscriptionMessage**-Methode auf dem **ServiceBusService**-Objekt empfangen. Standardmäßig werden Nachrichten aus dem Abonnement gelöscht, wenn sie gelesen wurden. Sie können Nachrichten jedoch lesen (einen Blick darauf werfen) und sperren, ohne sie aus dem Abonnement zu löschen, indem Sie den optionalen Parameter **isPeekLock** auf **true** festlegen.

Das Standardverhalten für das Lesen und Löschen der Nachricht als Teil des Empfangsvorgangs ist das einfachste Modell. Es wird am besten für Szenarios eingesetzt, bei denen es eine Anwendung tolerieren kann, wenn eine Nachricht bei Auftreten eines Fehlers nicht verarbeitet wird. Um dieses Verfahren zu verstehen, stellen Sie sich ein Szenario vor, in dem der Consumer die Empfangsanforderung ausstellt und dann abstürzt, bevor diese verarbeitet wird. Da Service Bus die Nachricht als verwendet markiert hat, wird er jene Nachricht auslassen, die vor dem Absturz verwendet wurde, wenn die Anwendung neu startet und erneut mit der Verwendung von Nachrichten beginnt.

Wenn der Parameter **isPeekLock** auf **TRUE** festgelegt ist, wird der Empfangsvorgang zu einem zweistufigen Vorgang. Dadurch können Anwendungen unterstützt werden, die das Auslassen bzw. Fehlen von Nachrichten nicht zulassen können. Wenn Service Bus eine Anfrage erhält, ermittelt der Dienst die nächste zu verarbeitende Nachricht, sperrt diese, um zu verhindern, dass andere Consumer sie erhalten, und sendet sie dann zurück an die Anwendung.
Nachdem die Anwendung die Verarbeitung der Nachricht abgeschlossen hat (oder sie zwecks zukünftiger Verarbeitung zuverlässig gespeichert hat), führt sie die zweite Phase des Empfangsprozesses durch Aufruf der **deleteMessage**-Methode und Bereitstellen der Nachricht durch, die als Parameter gelöscht wird. Die **deleteMessage**-Methode markiert die Nachricht als in Verarbeitung und entfernt sie aus dem Abonnement.

Das folgende Beispiel zeigt, wie Nachrichten mithilfe von **receiveSubscriptionMessage** empfangen und verarbeitet werden können. In diesem Beispiel wird zuerst eine Nachricht des Abonnements „LowMessages“ empfangen und gelöscht und anschließend eine Nachricht des Abonnements „HighMessages“ empfangen, wobei **isPeekLock** auf „true“ festgelegt ist. Anschließend wird die Nachricht über **deleteMessage** gelöscht:

```javascript
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandeln von Anwendungsabstürzen und nicht lesbaren Nachrichten
Service Bus stellt Funktionen zur Verfügung, die Sie bei der ordnungsgemäßen Behandlung von Fehlern in der Anwendung oder bei Problemen beim Verarbeiten einer Nachricht unterstützen. Wenn eine Empfängeranwendung die Nachricht aus einem bestimmten Grund nicht verarbeiten kann, kann sie die **unlockMessage**-Methode im **ServiceBusService**-Objekt aufrufen. Dies führt dazu, dass Service Bus die Nachricht innerhalb des Abonnements entsperrt und zur Verfügung stellt, damit sie erneut empfangen werden kann – durch die gleiche verarbeitende Anwendung oder durch eine andere verarbeitenden Anwendung.

Zudem wird einer im Abonnement gesperrten Anwendung ein Zeitlimit zugeordnet. Wenn die Anwendung die Nachricht vor Ablauf des Sperrzeitlimits nicht verarbeiten kann (zum Beispiel wenn die Anwendung abstürzt) entsperrt Service Bus die Nachricht automatisch und macht sie verfügbar, um erneut empfangen zu werden.

Falls die Anwendung nach der Verarbeitung der Nachricht, aber vor Abrufen der **deleteMessage**-Methode abstürzt, wird die Nachricht wieder an die Anwendung zugestellt, wenn diese neu gestartet wird. Dies wird häufig als *At Least Once Processing* (Mindestens einmal verarbeiten) bezeichnet und bedeutet, dass jede Nachricht mindestens einmal verarbeitet wird, wobei dieselbe Nachricht in bestimmten Situationen möglicherweise erneut zugestellt wird. Wenn eine doppelte Verarbeitung im betreffenden Szenario nicht geeignet ist, sollten Anwendungsentwickler ihrer Anwendung zusätzliche Logik für den Umgang mit der Übermittlung doppelter Nachrichten hinzufügen. Dies wird häufig durch die Verwendung der **MessageId**-Eigenschaft der Nachricht erzielt, die über mehrere Zustellungsversuche hinweg konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Löschen von Themen und Abonnements
Themen und Abonnements sind persistent und müssen über das [Azure-Portal][Azure portal] oder programmgesteuert explizit gelöscht werden.
Das folgende Beispiel zeigt das Löschen des Themas `MyTopic`:

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

Durch das Löschen eines Themas werden auch alle Abonnements gelöscht, die mit dem Thema registriert sind. Abonnements können auch unabhängig gelöscht werden. Das folgende Beispiel zeigt, wie ein Abonnement namens `HighMessages` aus dem Thema `MyTopic` gelöscht wird:

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun mit den Grundlagen der Servicebus-Themen vertraut sind, finden Sie unter den folgenden Links weitere Informationen.

* Siehe [Warteschlangen, Themen und Abonnements][Queues, topics, and subscriptions].
* API-Referenz für [SqlFilter][SqlFilter]
* Besuchen Sie das [Azure SDK for Node][Azure SDK for Node]-Repository auf GitHub.

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Erstellen und Bereitstellen einer Node.js-Anwendung auf einer Azure-Website]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Node.js Web Application with Storage]: ../storage/storage-nodejs-use-table-storage-cloud-service-app.md

