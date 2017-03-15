---
title: Leitfaden zum Azure Relay-Hybridverbindungsprotokoll | Microsoft-Dokumentation
description: Leitfaden zum Azure Relay-Hybridverbindungsprotokoll.
services: service-bus-relay
documentationcenter: na
author: clemensv
manager: timlt
editor: 
ms.assetid: 149f980c-3702-4805-8069-5321275bc3e8
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2017
ms.author: sethm;clemensv
translationtype: Human Translation
ms.sourcegitcommit: 4a972b9b8b52a90f27afda98d8bdc661016d1fe1
ms.openlocfilehash: f5fd4c6c0b8db3fe91d8b57a68fe33dcff353a59
ms.lasthandoff: 02/28/2017


---
# <a name="azure-relay-hybrid-connections-protocol"></a>Azure Relay-Hybridverbindungsprotokoll
Azure Relay ist eine der Säulen der Schlüsselfunktionen der Azure Service Bus-Plattform Die neue Funktion „Hybridverbindungen“ des Relays ist eine sichere Entwicklung mit offenem Protokoll, die auf HTTP und WebSockets basiert. Sie ersetzt die ehemalige, gleichnamige Funktion „BizTalk Services“, die auf einer proprietären Protokollbasis erstellt wurde. Die Integration von Hybridverbindungen in Azure App Services funktioniert weiterhin ohne weiteren Aufwand.

Mit Hybridverbindungen können Sie eine bidirektionale, binäre Streamkommunikation zwischen zwei vernetzten Anwendungen einrichten, bei der sich eine oder beide hinter NAT-Funktionen oder Firewalls befinden können. Dieser Artikel beschreibt die clientseitigen Interaktionen mit dem Hybridverbindungsrelay für die Verbindung von Clients in Listener- und Absenderrollen und erläutert, wie Listener neue Verbindungen akzeptieren.

## <a name="interaction-model"></a>Interaktionsmodell
Das Hybridverbindungs-Relay verbindet zwei Parteien durch einen Rendezvouspunkt in der Azure-Cloud, den beide Parteien erkennen können und mit dem sie eine Verbindung von ihrer eigenen Netzwerkperspektive aus herstellen können. In dieser Dokumentation und anderen Dokumentationen heißt der Rendezvouspunkt „Hybridverbindung“ – in den APIs und auch im Azure-Portal. Der Endpunkt dieses Hybridverbindungsdiensts wird im weiteren Dokumentverlauf als „Dienst“ bezeichnet. Das Interaktionsmodell profitiert von der Bezeichnung, die von vielen anderen Netzwerk-APIs erstellt wird.

Es gibt einen Listener, der zuerst die Bereitschaft anzeigt, eingehende Verbindungen zu verarbeiten und akzeptiert diese anschließend, wenn sie ankommen. Andererseits gibt es einen verbindenden Client, der die Verbindung mit dem Listener herstellt und erwartet, dass diese Verbindung für die Erstellung eines bidirektionalen Kommunikationspfads akzeptiert wird.
„Verbinden“ (Connect), „Lauschen“ (Listen) und „Akzeptieren“ (Accept) sind die Begriffe, die Sie in den meisten Socket-APIs finden.

In jedem Relaykommunikationsmodell stellt eine Partei ausgehende Verbindungen mit einem Dienstendpunkt her, wodurch der „Listener“ umgangssprachlich zu einem „Client“ wird und auch weitere Überschneidungen in der Terminologie verursacht werden. Daher verwenden wir bei Hybridverbindungen folgende Begriffe:

Die Programme auf beiden Seiten der Verbindung heißen „Client“, da sie Clients für den Dienst darstellen. Der Client, der auf Verbindungen wartet und diese akzeptiert, ist der „Listener“ oder befindet sich in der „Listenerrolle“. Der Client, der über den Dienst eine neue Verbindung mit einem Listener herstellt, heißt „Absender“ bzw. befindet sich in der „Absenderrolle“.

### <a name="listener-interactions"></a>Listener-Interaktionen
Der Listener verfügt über vier Interaktionen mit dem Dienst. Alle Transportdetails werden später im Abschnitt „Referenz“ diesem Artikel erläutert.

#### <a name="listen"></a>Empfangen
Um Bereitschaft an einen Dienst zu übermitteln, dass ein Listener bereit ist, Verbindungen zu akzeptieren, erstellt er eine ausgehende WebSocket-Verbindung. Der Verbindungshandshake trägt den Namen einer Hybridverbindung, die im Relaynamespace konfiguriert wurde, sowie ein Sicherheitstoken, das die Berechtigung „Lauschen“ direkt in diesem Namen überträgt.
Wenn der Websocket vom Dienst akzeptiert wird, ist die Registrierung abgeschlossen und der erstellte Websocket wird als „Steuerungskanal“ für die Aktivierung aller nachfolgenden Interaktionen aufrechterhalten. Der Dienst erlaubt bis zu 25 gleichzeitige Listener für eine Hybridverbindung. Wenn zwei oder mehr aktive Listener existieren, werden eingehende Verbindungen auf diese in zufälliger Reihenfolge aufgeteilt. Eine faire Aufteilung kann nicht garantiert werden.

#### <a name="accept"></a>Accept
Wenn ein Absender eine neue Verbindung im Dienst öffnet, wählt und benachrichtigt der Dienst einen der aktiven Listener in der Hybridverbindung. Die Benachrichtigung wird an den Listener als JSON- Nachricht über den offenen Steuerkanal übermittelt. Sie enthält die URL des WebSocket-Endpunkts, mit dem der Listener eine Verbindung herstellen muss, damit die Verbindung akzeptiert werden kann.

Die URL kann und muss direkt vom Listener ohne zusätzlichen Aufwand verwendet werden. Die codierte Information ist nur für kurze Zeit gültig, im Grunde genommen für den Zeitraum, in dem der Absender warten möchte, bis die End-to-End-Verbindung hergestellt wurde, jedoch nur maximal 30 Sekunden. Die URL kann nur für einen erfolgreichen Verbindungsversuch verwendet werden. Sobald die Websocketverbindung mit der Rendezvous-URL hergestellt wurde, wird jede weitere Aktivität in diesem Websocket ohne Eingriff oder Interpretation des Diensts an den und von dem Absender übermittelt.

#### <a name="renew"></a>Erneuern
Das Sicherheitstoken, das verwendet werden muss, um den Listener zu registrieren und den Steuerungskanal zu verwalten, läuft möglicherweise ab, währen der Listener aktiv ist. Der Ablauf des Tokens beeinträchtigt keine laufenden Verbindungen, jedoch wird der Steuerungskanal dadurch während oder kurz nach dem Moment des Ablaufs vom Dienst entfernt. Der „renew“-Vorgang zum Erneuern ist eine JSON-Nachricht, die der Listener senden kann, um das Token zu ersetzen, das dem Steuerungskanal zugeordnet ist. Dadurch kann der Steuerungskanal über einen längeren Zeitraum beibehalten werden.

#### <a name="ping"></a>Pingen
Wenn der Steuerungskanal für lange Zeit im Leerlauf bleibt, wird die TCP-Verbindung möglicherweise von geplanten Vermittlern, z.B. vom Lastenausgleich oder von NATs, gelöscht. Der „ping“-Vorgang vermeidet dies, indem eine kleine Datenmenge durch den Kanal gesendet wird, die alle beteiligten Elemente im Netzwerk daran erinnert, dass die Verbindung aufrechterhalten werden soll. Der Vorgang dient auch als Aktivitätstest für den Listener. Wenn der Ping-Befehl fehlschlägt, sollte der Steuerungskanal als nicht verwendbar angesehen werden und der Listener sollte eine neue Verbindung herstellen.

### <a name="sender-interaction"></a>Absenderinteraktion
Der Absender hat nur eine einzige Interaktion mit dem Dienst: Er stellt eine Verbindung her.

#### <a name="connect"></a>Verbinden
Der connect-Vorgang zum Verbinden öffnet einen Websocket im Dienst, der den Namen der Hybridverbindung sowie ein (optionales, aber standardmäßig erforderliches) Sicherheitstoken bereitstellt, das Berechtigungen zum Senden in der Abfragezeichenfolge überträgt. Der Dienst interagiert dann mit dem Listener, wie oben beschrieben. Der Listener erstellt eine Rendezvousverbindung, die mit diesem Websocket verknüpft wird. Nachdem der Websocket akzeptiert wurde, werden alle weiteren Interaktionen in diesem Websocket mit einem verbundenen Listener durchgeführt.

### <a name="interaction-summary"></a>Zusammenfassung der Interaktion
Das Ergebnis dieses Interaktionsmodells ist, dass der Absenderclient den Handshake mit einem „bereinigten“ Websocket verlässt, der mit einem Listener verbunden ist und keine weitere Präambel oder Vorbereitung benötigt. Dadurch können praktisch alle vorhandenen Implementierungen des Websocketclients den Hybridverbindungsdienst nutzen, indem sie einfach eine richtig strukturierte URL in ihre Websocket-Clientebene angeben.

Der Websocket der Rendezvousverbindung, den der Listener durch die akzeptierten Interaktionen abruft, ist ebenso bereinigt und kann mit minimaler zusätzlicher Abstraktion an jede vorhandene Websocket-Serverimplementierung übergeben werden. Die Abstraktion unterscheidet zwischen „accept“-Vorgängen in den lokalen Netzwerklistenern des Frameworks und den „accept“-Remotevorgängen der Hybridverbindungen.

## <a name="protocol-reference"></a>Protokollreferenz

Dieser Abschnitt beschreibt die Details der oben beschriebenen Protokollinteraktionen.

Alle Websocketverbindungen werden an Port 443 als Upgrade von HTTPS 1.1 durchgeführt, das häufig durch ein Websocketframework oder eine API abstrahiert wird. Die Beschreibung hier hält die Implementierung neutral und ohne ein bestimmtes Framework vorzuschlagen.

### <a name="listener-protocol"></a>Listener-Protokoll
Das Listener-Protokoll besteht aus zwei Verbindungsgesten und drei Nachrichtenvorgängen.

#### <a name="listener-control-channel-connection"></a>Verbindung des Listener-Steuerungskanals
Der Steuerungskanal wird mit der Erstellung einer Websocketverbindung zu Folgendem geöffnet:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...
```

Die `namespace-address` ist der vollqualifizierte Domänenname des Azure Relaynamespace, der die Hybridverbindung hostet, typischerweise in der Form `{myname}.servicebus.windows.net`.

Der Abfragezeichenfolgenparameter verfügt über folgende Optionen.

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| sb-hc-action |Ja |Der Parameter muss für die Listener-Rolle **sb-hc-action=listen** sein |
| {path} |Ja |Der URL-codierte Namespace-Pfad der vorkonfigurierten Hybridverbindung auf die dieser Listener registriert wird. Dieser Ausdruck wird an den festen Pfadteil `$hc/` angehängt. |
| sb-hc-token |Ja\* |Der Listener muss ein gültiges URL-codiertes Service Bus Shared Access-Token für den Namespace bereitstellen, oder eine Hybridverbindung, die **Listen** (Lauschen) direkt verleiht. |
| sb-hc-id |Nein |Diese vom Client bereitgestellte optionale ID ermöglicht die End-to-End-Diagnoseablaufverfolgung. |

Wenn die Websocketverbindung nicht hergestellt werden kann, weil der Pfad der Hybridverbindung nicht registriert ist, ein Token ungültig ist oder fehlt oder ein anderer Fehler vorliegt, wird das Fehlerfeedback mithilfe des regulären HTTP 1.1-Statusinformationsmodells bereitgestellt. Die Statusbeschreibung enthält eine Fehlernachverfolgungs-ID, die an den Azure-Support übermittelt werden kann:

| Code | Error | Beschreibung |
| --- | --- | --- |
| 404 |Nicht gefunden |Der **Pfad** der Hybridverbindung ist ungültig oder die Basis-URL ist falsch formatiert. |
| 401 |Nicht autorisiert |Das Sicherheitstoken ist nicht vorhanden, falsch formatiert oder ungültig. |
| 403 |Verboten (403) |Das Sicherheitstoken ist für diesen Pfad für diese Aktion ungültig.. |
| 500 |Interner Fehler |Es ist ein Fehler im Dienst aufgetreten. |

Wenn die Websocketverbindung absichtlich durch den Dienst heruntergefahren wird, nachdem sie anfangs eingerichtet wurde, erfolgt die Angabe des Grunds dafür mithilfe eines entsprechenden Websocket-Protokollfehlercodes sowie mit einer beschreibenden Fehlermeldung, die auch eine Nachverfolgungs-ID enthält. Ohne eine erkennbare Fehlerbedingung fährt der Dienst den Steuerungskanal nicht herunter. Jedes saubere Herunterfahren wird durch den Client gesteuert.

| WS-Status | Beschreibung |
| --- | --- |
| 1001 |Der Hybridverbindungspfad wurde gelöscht oder deaktiviert. |
| 1008 |Das Sicherheitstoken ist abgelaufen und die Autorisierungsrichtlinie wurde deshalb verletzt. |
| 1011 |Es ist ein Fehler im Dienst aufgetreten. |

### <a name="accept-handshake"></a>Akzeptieren von Handshake
Die Benachrichtigung zum Akzeptieren wird vom Dienst als JSON-Nachricht in einem Websocket-Textrahmen über den zuvor erstellten Steuerungskanal an den Listener übermittelt. Es gibt keine Antwort auf diese Nachricht.

Die Nachricht enthält ein JSON-Objekt mit dem Namen „accept“, das zu diesem Zeitpunkt die folgenden Eigenschaften definiert:

* **address**: Die URL-Zeichenfolge, die zum Erstellen des Websockets für den Dienst verwendet wird, um eingehende Verbindungen zu akzeptieren.
* **id** – der eindeutige Bezeichner für diese Verbindung. Wenn die ID vom Absenderclient angegeben wurde, ist sie der Wert, der vom Absender angegeben wurde. Andernfalls ist sie ein systemgenerierter Wert.
* **connectHeaders** – alle HTTP-Header, die durch den Absender auf dem Relay-Endpunkt angegeben wurden, der ebenso das Sec-WebSocket-Protocol und die Sec-WebSocket-Extensions-Header enthält.

#### <a name="accept-message"></a>Akzeptieren der Nachricht
```json
{                                                           
    "accept" : {
        "address" : "wss://168.61.148.205:443/$hc/{path}?..."    
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736",  
        "connectHeaders" : {                                         
            "Host" : "...",                                                
            "Sec-WebSocket-Protocol" : "...",                              
            "Sec-WebSocket-Extensions" : "..."                             
        }                                                            
     }                                                         
}
```

Die in der JSON-Nachricht bereitgestellte Adress-URL wird vom Listener verwendet, um den Websocket zu erstellen, der den Absendersocket akzeptieren oder ablehnen soll.

#### <a name="accepting-the-socket"></a>Akzeptieren des Sockets
Zum Akzeptieren erstellt der Listener eine Websocketverbindung mit der bereitgestellten Adresse.

Beachten Sie, dass der Adress-URI in der Vorschauphase möglicherweise eine reine IP-Adresse verwendet und das vom Server angegebene TLS-Zertifikat die Überprüfung an dieser Adresse nicht besteht.
Dies wird während der Vorschau behoben werden.

Wenn die „accept“-Nachricht einen „Sec-WebSocket-Protocol“-Header enthält, wird erwartet, dass der Listener den Websocket nur akzeptiert, wenn er dieses Protokoll unterstützt, und dass er den Header festlegt, wenn der Websocket erstellt wird.

Das gleiche gilt für den Header „Sec-WebSocket-Extensions“. Wenn das Framework eine Erweiterung unterstützt, sollte es den Header auf die *serverseitige* Antwort des erforderlichen „Sec-WebSocket-Extension“-Handshakes für die Erweiterung festlegen.

Die URL muss unverändert für die Erstellung des accept-Sockets verwendet werden, doch enthält folgenden Parameter:

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| sb-hc-action |Ja |Zum Akzeptieren eines Sockets muss der Parameter `sb-hc-action=accept` lauten. |
| {path} |Ja |(Siehe folgenden Abschnitt.) |
| sb-hc-id |Nein |Siehe Beschreibung von **id** weiter oben. |

Der `{path}` ist der URL-codierte Namespacepfad der vorkonfigurierten Hybridverbindung, für die dieser Listener registriert werden soll. Dieser Ausdruck wird an den festen Pfadteil `$hc/` angehängt. 

Der Pfadausdruck KANN mit einem Suffix und einem Abfragezeichenfolgenausdruck erweitert werden, der nach einem trennenden Schrägstrich auf den registrierten Namen folgt. Damit kann der Absenderclient dispatch-Argumente an den akzeptierenden Listener übergeben, wenn das Einschließen von HTTP-Headern nicht möglich ist. Erwartet wird, dass das Listenerframework den festen Pfadteil und den registrierten Namen aus dem Pfad analysiert und den Rest – möglicherweise ohne Abfragezeichenfolgenargumente mit vorangestelltem „sb“ – für die Anwendung verfügbar macht, damit diese entscheiden kann, ob die Verbindung akzeptiert wird.

Weitere Informationen finden Sie im Abschnitt „Absenderprotokoll“ weiter unten.

Tritt ein Fehler auf, reagiert der Dienst wie Folgt:

| Code | Error | Beschreibung |
| --- | --- | --- |
| 403 |Verboten (403) |Die URL ist nicht gültig |
| 500 |Interner Fehler |Es ist ein Fehler im Dienst aufgetreten |

Nachdem die Verbindung hergestellt wurde, beendet der Server den Websocket, wenn der Absenderwebsocket beendet wird oder folgenden Status aufweist.

| WS-Status | Beschreibung |
| --- | --- |
| 1001 |Der Absenderclient beendet die Verbindung. |
| 1001 |Der Hybridverbindungspfad wurde gelöscht oder deaktiviert. |
| 1008 |Das Sicherheitstoken ist abgelaufen, und daher wurde die Autorisierungsrichtlinie verletzt. |
| 1011 |Es ist ein Fehler im Dienst aufgetreten. |

#### <a name="rejecting-the-socket"></a>Ablehnen des Sockets
Das Ablehnen des Sockets nach dem Untersuchen der „accept“-Nachricht erfordert einen ähnlichen Handshake, sodass der Statuscode und die Statusbeschreibung, die den Grund für die Ablehnung mitteilt, an den Absender zurückgegeben werden können.

Gemäß Protokollentwurf wird hier ein Websockethandshake (der per Entwurf in einem definierten Fehlerstatus endet) verwendet, sodass Implementierungen von Listenerclients weiterhin auf einem Websocketclient basieren können und keinen zusätzlichen reinen HTTP-Client verwenden müssen.

Zum Ablehnen des Sockets nimmt der Client den Adress-URI aus der accept-Nachricht und fügt zwei Abfragezeichenfolgenparameter daran an:

| Param | Erforderlich | Beschreibung |
| --- | --- | --- |
| statusCode |Ja |Numerischer HTTP-Statuscode. |
| statusDescription |Ja |Ein visuell lesbarer Grund für die Ablehnung. |

Der resultierende URI wird dann zum Herstellen einer Websocketverbindung verwendet. Beachten Sie (wie oben bereits erwähnt), dass das TLS-Zertifikat möglicherweise nicht mit der Adresse während der Vorschau übereinstimmt, die Überprüfung also womöglich deaktiviert werden muss.

Wenn der Vorgang erfolgreich abgeschlossen wurde, schlägt dieser Handshake absichtlich mit dem HTTP-Fehlercode 410 fehl, da kein Websocket erstellt wurde. Wenn ein Fehler auftritt, stehen diese Optionen zur Verfügung:

| Code | Error | Beschreibung |
| --- | --- | --- |
| 403 |Verboten (403) |Die URL ist nicht gültig |
| 500 |Interner Fehler |Es ist ein Fehler im Dienst aufgetreten. |

### <a name="listener-token-renewal"></a>Erneuerung des Listenertokens
Wenn das Listenertoken abläuft, kann es ersetzt werden, indem über den eingerichteten Steuerungskanal eine Textrahmennachricht an den Dienst gesendet wird. Die Nachricht enthält ein JSON-Objekt mit dem Namen „renewToken“, das zu diesem Zeitpunkt die folgende Eigenschaft definiert:

* **token** – ein gültiges URL-codiertes Service Bus Shared Access-Token für den Namespace bereitstellen, oder eine Hybridverbindung, das **Listen** direkt verleiht.

#### <a name="renewtoken-message"></a>renewToken-Nachricht
```json
{                                                                                                                                                                        
    "renewToken" : {                                                                                                                                                      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fhyco%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"  
    }                                                                                                                                                                     
}
```

Wenn bei der Tokenüberprüfung ein Fehler auftritt, wird der Zugriff verweigert, und der Clouddienst schließt den Websocket des Steuerungskanals mit einem Fehler. Ansonsten wird keine Antwort zurückgegeben.

| WS-Status | Beschreibung |
| --- | --- |
| 1008 |Das Sicherheitstoken ist abgelaufen und die Autorisierungsrichtlinie wurde deshalb verletzt. |

## <a name="sender-protocol"></a>Absenderprotokoll
Das Absenderprotokoll ist identisch mit der Methode, wie ein Listener erstellt wird.
Das Ziel ist die maximale Transparenz für den End-to-End-Websocket. Die Adresse für die Verbindung ist dieselbe wie für den Listener, aber die „action“ ist anders, und das Token benötigt eine andere Berechtigung:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

Die *namespace-address* ist der vollqualifizierte Domänenname des Azure Relaynamespace, der die Hybridverbindung hostet, typischerweise in der Form `{myname}.servicebus.windows.net`.

Die Anforderung kann beliebige zusätzliche HTTP-Header enthalten, einschließlich die anwendungsdefinierten. Alle angegebenen Header werden an den Listener weitergegeben und befinden sich im connectHeader-Objekt der accept-Steuerungsnachricht.

Der Optionen des Abfragezeichenfolgenparameters sind die folgenden

| Param | Erforderlich | Beschreibung |
| --- | --- | --- |
| sb-hc-action |Ja |Für die Absenderrolle muss der Parameter `action=connect` lauten. |
| {path} |Ja |(Siehe folgenden Abschnitt.) |
| sb-hc-token |Ja\* |Der Listener muss ein gültiges URL-codiertes Service Bus Shared Access-Token für den Namespace oder die Hybridverbindung bereitstellen, das die Berechtigung **Senden** direkt überträgt. |
| sb-hc-id |Nein |Eine optionale ID, mit der die End-to-End-Diagnoseablaufverfolgung möglich ist und die während des accept-Handshakes für den Listener verfügbar gemacht wird. |

Der {path} ist der URL-codierte Namespacepfad der vorkonfigurierten Hybridverbindung, für die dieser Listener registriert werden soll. Der Pfadausdruck KANN durch ein Suffix und einen Abfragezeichenfolgenausdruck für die weitere Kommunikation erweitert werden. Wenn die Hybridverbindung im Pfad „hyco“ registriert ist, kann der Pfadausdruck `hyco/suffix?param=value&...` lauten, gefolgt von den hier definierten Abfragezeichenfolgenparametern. Ein vollständiger Ausdruck kann dann folgendermaßen lauten:

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

Der Pfadausdruck wird an den Listener in dem Adress-URI übergeben, der in der accept-Steuerungsnachricht enthalten ist.

Wenn die Websocketverbindung nicht hergestellt werden kann, weil der Pfad der Hybridverbindung nicht registriert ist, ein Token ungültig ist oder fehlt oder ein anderer Fehler vorliegt, wird das Fehlerfeedback mithilfe des regulären HTTP 1.1-Statusinformationsmodells bereitgestellt. Die Statusbeschreibung enthält eine Fehlernachverfolgungs-ID, die an den Azure-Support übermittelt werden kann:

| Code | Error | Beschreibung |
| --- | --- | --- |
| 404 |Nicht gefunden |Der `path` der Hybridverbindung ist ungültig, oder die Basis-URL ist falsch formatiert. |
| 401 |Nicht autorisiert |Das Sicherheitstoken ist nicht vorhanden, falsch formatiert oder ungültig. |
| 403 |Verboten (403) |Das Sicherheitstoken ist für diesen Pfad für diese Aktion ungültig.. |
| 500 |Interner Fehler |Es ist ein Fehler im Dienst aufgetreten. |

Wenn die Websocketverbindung absichtlich durch den Dienst heruntergefahren wird, nachdem sie anfangs eingerichtet wurde, erfolgt die Angabe des Grunds dafür mithilfe eines entsprechenden Websocket-Protokollfehlercodes sowie mit einer beschreibenden Fehlermeldung, die auch eine Nachverfolgungs-ID enthält.

| WS-Status | Beschreibung |
| --- | --- |
| 1000 |Der Listener beendet den Socket. |
| 1001 |Der Hybridverbindungspfad wurde gelöscht oder deaktiviert. |
| 1008 |Das Sicherheitstoken ist abgelaufen, und daher wurde die Autorisierungsrichtlinie verletzt. |
| 1011 |Es ist ein Fehler im Dienst aufgetreten. |

## <a name="next-steps"></a>Nächste Schritte
* [Relay – häufig gestellte Fragen](relay-faq.md)
* [Erstellen eines Namespaces](relay-create-namespace-portal.md)
* [Erste Schritte mit .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Erste Schritte mit Node](relay-hybrid-connections-node-get-started.md)


