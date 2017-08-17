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
ms.date: 07/03/2017
ms.author: sethm;clemensv
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: 6b76403ba5fc4d00a625057549c85db59a473898
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# Azure Relay-Hybridverbindungsprotokoll
Azure Relay ist eine der Säulen der Schlüsselfunktionen der Azure Service Bus-Plattform Die neue Funktion *Hybridverbindungen* von Relay ist eine sichere Entwicklung mit offenem Protokoll, die auf HTTP und WebSockets basiert. Sie ersetzt die ehemalige, gleichnamige Funktion *BizTalk Services*, die auf einer proprietären Protokollbasis erstellt wurde. Die Integration von Hybridverbindungen in Azure App Services funktioniert weiterhin ohne weiteren Aufwand.

Mit Hybridverbindungen können Sie eine bidirektionale, binäre Datenstromkommunikation zwischen zwei vernetzten Anwendungen einrichten, bei der sich eine oder beide hinter NAT-Funktionen oder Firewalls befinden können. Dieser Artikel beschreibt die clientseitigen Interaktionen mit dem Hybridverbindungsrelay für die Verbindung von Clients in Listener- und Absenderrollen und erläutert, wie Listener neue Verbindungen akzeptieren.

## Interaktionsmodell
Das Hybridverbindungs-Relay verbindet zwei Parteien durch einen Rendezvouspunkt in der Azure-Cloud, den beide Parteien erkennen können und mit dem sie eine Verbindung von ihrer eigenen Netzwerkperspektive aus herstellen können. In dieser Dokumentation und anderen Dokumentationen heißt der Rendezvouspunkt „Hybridverbindung“ – in den APIs und auch im Azure-Portal. Der Endpunkt dieses Hybridverbindungsdiensts wird im weiteren Verlauf dieses Artikels als „Dienst“ bezeichnet. Für das Interaktionsmodell werden Bezeichnungen verwendet, die auch für viele andere Netzwerk-APIs gängig sind.

Es gibt einen Listener, der zuerst die Bereitschaft anzeigt, eingehende Verbindungen zu verarbeiten und akzeptiert diese anschließend, wenn sie ankommen. Andererseits gibt es einen verbindenden Client, der die Verbindung mit dem Listener herstellt und erwartet, dass diese Verbindung für die Erstellung eines bidirektionalen Kommunikationspfads akzeptiert wird.
„Verbinden“ (Connect), „Lauschen“ (Listen) und „Akzeptieren“ (Accept) sind die Begriffe, die Sie in den meisten Socket-APIs finden.

In jedem Relaykommunikationsmodell stellt eine Partei ausgehende Verbindungen mit einem Dienstendpunkt her, wodurch der „Listener“ umgangssprachlich zu einem „Client“ wird und auch weitere Überschneidungen in der Terminologie verursacht werden. Daher verwenden wir bei Hybridverbindungen die folgende präzise Terminologie:

Die Programme auf beiden Seiten der Verbindung werden als „Clients“ bezeichnet, da sie für den Dienst Clients darstellen. Der Client, der auf Verbindungen wartet und diese akzeptiert, ist der „Listener“ oder befindet sich in der „Listenerrolle“. Der Client, der über den Dienst eine neue Verbindung mit einem Listener herstellt, heißt „Absender“ bzw. befindet sich in der „Absenderrolle“.

### Listener-Interaktionen
Der Listener verfügt über vier Interaktionen mit dem Dienst. Alle Transportdetails werden später im Abschnitt „Referenz“ diesem Artikel erläutert.

#### Lauschen
Um die Bereitschaft an einen Dienst zu übermitteln, dass ein Listener bereit zum Akzeptieren von Verbindungen ist, erstellt er eine ausgehende WebSocket-Verbindung. Der Verbindungshandshake enthält den Namen einer Hybridverbindung, die im Relaynamespace konfiguriert wurde, sowie ein Sicherheitstoken, das die Berechtigung „Lauschen“ direkt innerhalb dieses Namens überträgt.
Wenn der WebSocket vom Dienst akzeptiert wird, ist die Registrierung abgeschlossen, und der erstellte WebSocket wird als „Steuerungskanal“ für die Aktivierung aller nachfolgenden Interaktionen aufrechterhalten. Der Dienst erlaubt bis zu 25 gleichzeitige Listener für eine Hybridverbindung. Wenn zwei oder mehr aktive Listener vorhanden sind, werden eingehende Verbindungen in zufälliger Reihenfolge darauf aufgeteilt. Eine faire Aufteilung kann nicht garantiert werden.

#### Akzeptieren
Wenn ein Absender eine neue Verbindung im Dienst öffnet, wählt und benachrichtigt der Dienst einen der aktiven Listener in der Hybridverbindung. Diese Benachrichtigung wird an den Listener als JSON- Nachricht über den offenen Steuerkanal übermittelt. Sie enthält die URL des WebSocket-Endpunkts, mit dem der Listener eine Verbindung herstellen muss, damit die Verbindung akzeptiert werden kann.

Die URL kann bzw. muss vom Listener ohne zusätzlichen Aufwand direkt verwendet werden.
Die codierten Informationen sind nur für kurze Zeit gültig. Dies ist im Grunde genommen der Zeitraum, der vom Absender toleriert wird, bis die End-to-End-Verbindung hergestellt wurde. Der Höchstwert beträgt aber maximal 30 Sekunden. Die URL kann nur für einen erfolgreichen Verbindungsversuch verwendet werden. Sobald die WebSocket-Verbindung mit der Rendezvous-URL hergestellt wurde, wird jede weitere Aktivität in diesem WebSocket ohne Eingriff oder Interpretation des Diensts an den und von dem Absender übermittelt.

#### Erneuern
Das Sicherheitstoken, das verwendet werden muss, um den Listener zu registrieren und den Steuerungskanal zu verwalten, läuft möglicherweise ab, währen der Listener aktiv ist. Der Ablauf des Tokens beeinträchtigt keine laufenden Verbindungen, aber der Steuerungskanal wird dadurch während oder kurz nach dem Moment des Ablaufs vom Dienst entfernt. Der „renew“-Vorgang zum Erneuern ist eine JSON-Nachricht, die der Listener senden kann, um das Token zu ersetzen, das dem Steuerungskanal zugeordnet ist. Dadurch kann der Steuerungskanal über einen längeren Zeitraum beibehalten werden.

#### Pingen
Wenn der Steuerungskanal für lange Zeit im Leerlauf bleibt, wird die TCP-Verbindung möglicherweise von geplanten Vermittlern, z.B. vom Lastenausgleich oder von NATs, gelöscht. Der „ping“-Vorgang vermeidet dies, indem eine kleine Datenmenge durch den Kanal gesendet wird, die alle beteiligten Elemente auf der Netzwerkroute daran erinnert, dass die Verbindung aufrechterhalten werden soll. Der Vorgang dient auch als Aktivitätstest für den Listener. Wenn der Ping-Befehl fehlschlägt, sollte der Steuerungskanal als nicht verwendbar angesehen werden und der Listener sollte eine neue Verbindung herstellen.

### Absenderinteraktion
Der Absender führt nur eine einzige Interaktion mit dem Dienst durch: Er stellt eine Verbindung her.

#### Verbinden
Der „connect“-Vorgang zum Verbinden öffnet einen WebSocket im Dienst, der den Namen der Hybridverbindung sowie ein (optionales, aber standardmäßig erforderliches) Sicherheitstoken bereitstellt, das Berechtigungen zum Senden in der Abfragezeichenfolge überträgt. Der Dienst interagiert dann mit dem Listener, wie oben beschrieben. Der Listener erstellt eine Rendezvousverbindung, die mit diesem WebSocket verknüpft wird. Nachdem der WebSocket akzeptiert wurde, werden alle weiteren Interaktionen für diesen WebSocket mit dem verbundenen Listener durchgeführt.

### Zusammenfassung der Interaktion
Das Ergebnis dieses Interaktionsmodells ist, dass der Absenderclient den Handshake mit einem „bereinigten“ WebSocket verlässt, der mit einem Listener verbunden ist und keine weitere Präambel oder Vorbereitung benötigt. Dieses Modell ermöglicht, dass praktisch alle vorhandenen Implementierungen des WebSocket-Clients den Hybridverbindungsdienst nutzen, indem eine richtig strukturierte URL für die WebSocket-Clientebene angegeben wird.

Der WebSocket der Rendezvousverbindung, den der Listener durch die akzeptierten Interaktionen abruft, ist ebenfalls bereinigt und kann mit minimaler zusätzlicher Abstraktion an jede vorhandene WebSocket-Serverimplementierung übergeben werden. Die Abstraktion unterscheidet zwischen „accept“-Vorgängen in den lokalen Netzwerklistenern des Frameworks und den „accept“-Remotevorgängen der Hybridverbindungen.

## Protokollreferenz

In diesem Abschnitt werden die Details der oben erwähnten Protokollinteraktionen beschrieben.

Alle WebSocket-Verbindungen werden an Port 443 als Upgrade von HTTPS 1.1 durchgeführt, das häufig durch ein WebSocket-Framework oder eine API abstrahiert wird. Die Beschreibung hier hält die Implementierung neutral und ohne ein bestimmtes Framework vorzuschlagen.

### Listener-Protokoll
Das Listener-Protokoll besteht aus zwei Verbindungsgesten und drei Nachrichtenvorgängen.

#### Verbindung des Listener-Steuerungskanals
Der Steuerungskanal wird mit der Erstellung der folgenden WebSocket-Verbindung geöffnet:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...
```

Die `namespace-address` ist der vollqualifizierte Domänenname des Azure Relaynamespace, der die Hybridverbindung hostet, typischerweise in der Form `{myname}.servicebus.windows.net`.

Der Abfragezeichenfolgenparameter verfügt über folgende Optionen.

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| `sb-hc-action` |Ja |Der Parameter muss für die Listener-Rolle **sb-hc-action=listen** sein |
| `{path}` |Ja |Der URL-codierte Namespace-Pfad der vorkonfigurierten Hybridverbindung auf die dieser Listener registriert wird. Dieser Ausdruck wird an den festen Pfadteil `$hc/` angehängt. |
| `sb-hc-token` |Ja\* |Der Listener muss ein gültiges URL-codiertes Service Bus Shared Access-Token für den Namespace bereitstellen, oder eine Hybridverbindung, die **Listen** (Lauschen) direkt verleiht. |
| `sb-hc-id` |Nein |Diese vom Client bereitgestellte optionale ID ermöglicht die End-to-End-Diagnoseablaufverfolgung. |

Wenn die WebSocket-Verbindung nicht hergestellt werden kann, weil der Pfad der Hybridverbindung nicht registriert ist, ein Token ungültig ist oder fehlt oder ein anderer Fehler vorliegt, wird das Fehlerfeedback mithilfe des regulären HTTP 1.1-Statusinformationsmodells bereitgestellt. Die Statusbeschreibung enthält eine Fehlernachverfolgungs-ID, die an die Azure-Supportmitarbeiter übermittelt werden kann:

| Code | Error | Beschreibung |
| --- | --- | --- |
| 404 |Nicht gefunden |Der Pfad der Hybridverbindung ist ungültig, oder die Basis-URL ist falsch formatiert. |
| 401 |Nicht autorisiert |Das Sicherheitstoken ist nicht vorhanden, falsch formatiert oder ungültig. |
| 403 |Verboten (403) |Das Sicherheitstoken ist für diesen Pfad für diese Aktion ungültig. |
| 500 |Interner Fehler |Es ist ein Fehler im Dienst aufgetreten. |

Wenn die WebSocket-Verbindung absichtlich durch den Dienst heruntergefahren wird, nachdem sie anfangs eingerichtet wurde, erfolgt die Angabe des Grunds dafür mithilfe eines entsprechenden WebSocket-Protokollfehlercodes sowie mit einer beschreibenden Fehlermeldung, die auch eine Nachverfolgungs-ID enthält. Ohne eine erkennbare Fehlerbedingung fährt der Dienst den Steuerungskanal nicht herunter. Jedes saubere Herunterfahren wird durch den Client gesteuert.

| WS-Status | Beschreibung |
| --- | --- |
| 1001 |Der Hybridverbindungspfad wurde gelöscht oder deaktiviert. |
| 1008 |Das Sicherheitstoken ist abgelaufen, sodass die Autorisierungsrichtlinie verletzt wurde. |
| 1011 |Es ist ein Fehler im Dienst aufgetreten. |

### Akzeptieren von Handshake
Die Benachrichtigung zum Akzeptieren wird vom Dienst als JSON-Nachricht in einem WebSocket-Textrahmen über den zuvor erstellten Steuerungskanal an den Listener übermittelt. Es gibt keine Antwort auf diese Nachricht.

Die Nachricht enthält ein JSON-Objekt mit dem Namen „accept“, das zu diesem Zeitpunkt die folgenden Eigenschaften definiert:

* **address**: Die URL-Zeichenfolge, die zum Erstellen des WebSockets für den Dienst verwendet wird, um eingehende Verbindungen zu akzeptieren.
* **id**: Der eindeutige Bezeichner für diese Verbindung. Wenn die ID vom Absenderclient angegeben wurde, ist sie der Wert, der vom Absender angegeben wurde. Andernfalls ist sie ein systemgenerierter Wert.
* **connectHeaders**: Alle HTTP-Header, die durch den Absender auf dem Relay-Endpunkt angegeben wurden, der ebenso das Sec-WebSocket-Protocol und die Sec-WebSocket-Extensions-Header enthält.

#### Akzeptieren der Nachricht

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

Die in der JSON-Nachricht bereitgestellte Adress-URL wird vom Listener verwendet, um den WebSocket zu erstellen, der den Absendersocket akzeptieren oder ablehnen soll.

#### Akzeptieren des Sockets
Der Listener erstellt eine WebSocket-Verbindung zu der bereitgestellten Adresse, um zu akzeptieren.

Wenn die Benachrichtigung zum Akzeptieren einen `Sec-WebSocket-Protocol`-Header enthält, wird erwartet, dass der Listener den WebSocket nur dann akzeptiert, sofern er dieses Protokoll unterstützt. Außerdem wird der Header bei der WebSocket-Einrichtung festgelegt.

Dasselbe gilt für den `Sec-WebSocket-Extensions`-Header. Wenn das Framework eine Erweiterung unterstützt, sollte es den Header auf die serverseitige Antwort des erforderlichen `Sec-WebSocket-Extensions`-Handshakes für die Erweiterung festlegen.

Die URL muss unverändert für die Erstellung des accept-Sockets verwendet werden, doch enthält folgenden Parameter:

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| `sb-hc-action` |Ja |Zum Akzeptieren eines Sockets muss der Parameter `sb-hc-action=accept` lauten. |
| `{path}` |Ja |(Siehe folgenden Abschnitt.) |
| `sb-hc-id` |Nein |Siehe obige Beschreibung von **id**. |

`{path}` ist der URL-codierte Namespacepfad der vorkonfigurierten Hybridverbindung, für die dieser Listener registriert werden soll. Dieser Ausdruck wird an den festen Pfadteil `$hc/` angehängt. 

Der `path`-Ausdruck kann mit einem Suffix und einem Abfragezeichenfolgenausdruck erweitert werden, der nach einem trennenden Schrägstrich auf den registrierten Namen folgt. Hiermit kann der Absenderclient dispatch-Argumente an den akzeptierenden Listener übergeben, wenn das Einschließen von HTTP-Headern nicht möglich ist. Erwartet wird, dass das Listenerframework den festen Pfadteil und den registrierten Namen aus dem Pfad analysiert und den Rest – unter Umständen ohne Abfragezeichenfolgenargumente mit vorangestelltem `sb-` – für die Anwendung verfügbar macht, damit diese entscheiden kann, ob die Verbindung akzeptiert wird.

Weitere Informationen finden Sie im Abschnitt „Absenderprotokoll“ weiter unten.

Wenn ein Fehler auftritt, kann der Dienst wie folgt reagieren:

| Code | Error | Beschreibung |
| --- | --- | --- |
| 403 |Verboten (403) |Die URL ist nicht gültig |
| 500 |Interner Fehler |Es ist ein Fehler im Dienst aufgetreten |

Nachdem die Verbindung hergestellt wurde, beendet der Server den WebSocket, wenn der Absender-WebSocket beendet wird oder folgenden Status aufweist:

| WS-Status | Beschreibung |
| --- | --- |
| 1001 |Der Absenderclient beendet die Verbindung. |
| 1001 |Der Hybridverbindungspfad wurde gelöscht oder deaktiviert. |
| 1008 |Das Sicherheitstoken ist abgelaufen, sodass die Autorisierungsrichtlinie verletzt wurde. |
| 1011 |Es ist ein Fehler im Dienst aufgetreten. |

#### Ablehnen des Sockets
Das Ablehnen des Sockets nach dem Untersuchen der „accept“-Nachricht erfordert einen ähnlichen Handshake, sodass der Statuscode und die Statusbeschreibung, die den Grund für die Ablehnung mitteilt, an den Absender zurückgegeben werden können.

Gemäß Protokollentwurf wird hier ein WebSocket-Handshake verwendet (der per Entwurf in einem definierten Fehlerstatus endet), sodass Implementierungen von Listenerclients weiterhin auf einem WebSocket-Client basieren können und keinen zusätzlichen reinen HTTP-Client verwenden müssen.

Zum Ablehnen des Sockets verwendet der Client den Adress-URI aus der accept-Nachricht und fügt wie folgt zwei Abfragezeichenfolgenparameter an:

| Param | Erforderlich | Beschreibung |
| --- | --- | --- |
| statusCode |Ja |Numerischer HTTP-Statuscode. |
| statusDescription |Ja |Ein visuell lesbarer Grund für die Ablehnung. |

Der resultierende URI wird dann verwendet, um eine WebSocket-Verbindung herzustellen.

Wenn der Vorgang erfolgreich abgeschlossen ist, schlägt dieser Handshake absichtlich mit dem HTTP-Fehlercode 410 fehl, da kein WebSocket erstellt wurde. Im Fehlerfall werden Fehler mit den folgenden Codes beschrieben:

| Code | Error | Beschreibung |
| --- | --- | --- |
| 403 |Verboten (403) |Die URL ist nicht gültig |
| 500 |Interner Fehler |Es ist ein Fehler im Dienst aufgetreten. |

### Erneuerung des Listenertokens
Wenn das Listenertoken abläuft, kann es ersetzt werden, indem über den eingerichteten Steuerungskanal eine Textrahmennachricht an den Dienst gesendet wird. Die Nachricht enthält ein JSON-Objekt mit dem Namen `renewToken`, das derzeit die folgende Eigenschaft definiert:

* **token**: Ein gültiges URL-codiertes Service Bus Shared Access-Token für den Namespace oder eine Hybridverbindung zum direkten Gewähren des Rechts **Listen** (Lauschen).

#### renewToken-Nachricht

```json
{                                                                                                                                                                        
    "renewToken" : {                                                                                                                                                      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fhyco%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"  
    }                                                                                                                                                                     
}
```

Wenn die Tokenüberprüfung nicht erfolgreich ist, wird der Zugriff verweigert, und der Clouddienst schließt den WebSocket des Steuerkanals mit einem Fehler. Andernfalls erfolgt keine Antwort.

| WS-Status | Beschreibung |
| --- | --- |
| 1008 |Das Sicherheitstoken ist abgelaufen, sodass die Autorisierungsrichtlinie verletzt wurde. |

## Absenderprotokoll
Das Absenderprotokoll ist praktisch identisch mit der Erstellung eines Listeners.
Das Ziel ist die maximale Transparenz für den End-to-End-WebSocket. Die Adresse für die Verbindung ist dieselbe wie für den Listener, aber die „action“ ist anders, und das Token benötigt eine andere Berechtigung:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

Die *namespace-address* ist der vollqualifizierte Domänenname des Azure Relaynamespace, der die Hybridverbindung hostet, typischerweise in der Form `{myname}.servicebus.windows.net`.

Die Anforderung kann beliebige zusätzliche HTTP-Header enthalten, z.B. auch von der Anwendung definierte Header. Alle angegebenen Header werden an den Listener weitergegeben und befinden sich im `connectHeader`-Objekt der **accept**-Steuerungsnachricht.

Die Optionen des Abfragezeichenfolgenparameters lauten wie folgt:

| Parameter | Erforderlich? | Beschreibung |
| --- | --- | --- |
| `sb-hc-action` |Ja |Für die Absenderrolle muss der Parameter `action=connect` lauten. |
| `{path}` |Ja |(Siehe folgenden Abschnitt.) |
| `sb-hc-token` |Ja\* |Der Listener muss ein gültiges URL-codiertes Service Bus Shared Access-Token für den Namespace oder die Hybridverbindung bereitstellen, das die Berechtigung **Senden** direkt überträgt. |
| `sb-hc-id` |Nein |Eine optionale ID, mit der die End-to-End-Diagnoseablaufverfolgung möglich ist und die während des accept-Handshakes für den Listener verfügbar gemacht wird. |

Der `{path}` ist der URL-codierte Namespacepfad der vorkonfigurierten Hybridverbindung, für die dieser Listener registriert werden soll. Der `path`-Ausdruck kann mit einem Suffix und einem Abfragezeichenfolgenausdruck für die weitere Kommunikation erweitert werden. Wenn die Hybridverbindung unter dem Pfad `hyco` registriert ist, kann der `path`-Ausdruck `hyco/suffix?param=value&...` lauten, gefolgt von den hier definierten Abfragezeichenfolgenparametern. Ein vollständiger Ausdruck kann dann wie folgt lauten:

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

Der `path`-Ausdruck wird an den Listener in dem Adress-URI übergeben, der in der accept-Steuerungsnachricht enthalten ist.

Wenn die WebSocket-Verbindung nicht hergestellt werden kann, weil der Pfad der Hybridverbindung nicht registriert ist, ein Token ungültig ist oder fehlt oder ein anderer Fehler vorliegt, wird das Fehlerfeedback mithilfe des regulären HTTP 1.1-Statusinformationsmodells bereitgestellt. Die Statusbeschreibung enthält eine Fehlernachverfolgungs-ID, die an die Azure-Supportmitarbeiter übermittelt werden kann:

| Code | Error | Beschreibung |
| --- | --- | --- |
| 404 |Nicht gefunden |Der Pfad der Hybridverbindung ist ungültig, oder die Basis-URL ist falsch formatiert. |
| 401 |Nicht autorisiert |Das Sicherheitstoken ist nicht vorhanden, falsch formatiert oder ungültig. |
| 403 |Verboten (403) |Das Sicherheitstoken ist für diesen Pfad und für diese Aktion ungültig. |
| 500 |Interner Fehler |Es ist ein Fehler im Dienst aufgetreten. |

Wenn die WebSocket-Verbindung absichtlich durch den Dienst heruntergefahren wird, nachdem sie anfangs eingerichtet wurde, erfolgt die Angabe des Grunds dafür mithilfe eines entsprechenden WebSocket-Protokollfehlercodes sowie mit einer beschreibenden Fehlermeldung, die auch eine Nachverfolgungs-ID enthält.

| WS-Status | Beschreibung |
| --- | --- |
| 1000 |Der Listener beendet den Socket. |
| 1001 |Der Hybridverbindungspfad wurde gelöscht oder deaktiviert. |
| 1008 |Das Sicherheitstoken ist abgelaufen, sodass die Autorisierungsrichtlinie verletzt wurde. |
| 1011 |Es ist ein Fehler im Dienst aufgetreten. |

## Nächste Schritte
* [Relay – häufig gestellte Fragen](relay-faq.md)
* [Erstellen eines Namespaces](relay-create-namespace-portal.md)
* [Erste Schritte mit .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Erste Schritte mit Node](relay-hybrid-connections-node-get-started.md)


