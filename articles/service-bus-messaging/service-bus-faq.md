<properties 
    pageTitle="Service Bus – Häufig gestellte Fragen | Microsoft Azure"
    description="Enthält Antworten auf einige häufig gestellte Fragen zu Azure Service Bus."
    services="service-bus-messaging"
    documentationCenter="na"
    authors="justinconway"
    manager=""
    editor="" />
<tags 
    ms.service="service-bus-messaging"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="06/09/2016"
    ms.author="sethm;juconway" />

# Service Bus – Häufig gestellte Fragen

In diesem Artikel werden einige häufig gestellte Fragen zu Microsoft Azure Service Bus beantwortet. Sie können auch [Häufig gestellte Fragen zum Azure-Support](http://go.microsoft.com/fwlink/?LinkID=185083) aufrufen, wenn Sie nach allgemeinen Informationen zu Azure-Preisen und zum Support suchen. Die folgenden Themen werden behandelt:

- [Allgemeine Fragen zu Azure Service Bus Messaging](#general-questions-about-azure-service-bus-messaging)
- [Service Bus – Bewährte Methoden](#service-bus-best-practices)
- [Service Bus – Preise](#service-bus-pricing)
- [Service Bus-Kontingente](#service-bus-quotas)
- [Abonnement- und Namespace-Verwaltung](#subscription-and-namespace-management)
- [Problembehandlung](#service-bus-troubleshooting)

## Allgemeine Fragen zu Azure Service Bus Messaging

### Was ist Azure Service Bus Messaging?

[Azure Service Bus Messaging](service-bus-messaging-overview.md) ist eine asynchrone Cloudplattform für Messaging, mit der Sie Daten zwischen entkoppelten Systemen senden können. Microsoft stellt dieses Feature als Dienst bereit. Dies bedeutet, dass Sie Ihre Hardware nicht selbst hosten müssen, um sie verwenden zu können.

### Was ist ein Service Bus-Namespace?

Ein Namespace ist ein Bereichscontainer für die Adressierung von Service Bus-Ressourcen innerhalb Ihrer Anwendung. Die Namespace-Erstellung ist für die Verwendung von Service Bus erforderlich und ist einer der ersten Schritte.

### Was ist eine Azure Service Bus-Warteschlange?

Eine [Service Bus-Warteschlange](service-bus-queues-topics-subscriptions.md) ist eine Entität, in der Nachrichten gespeichert werden. Warteschlangen sind besonders nützlich, wenn Sie über mehrere Anwendungen oder mehrere Teile einer verteilten Anwendung verfügen, die miteinander kommunizieren müssen. Die Warteschlange ähnelt einem Verteilzentrum, in dem mehrere Produkte (Nachrichten) eingehen und von diesem Standort aus dann weiterverschickt werden.

### Was sind Azure Service Bus-Themen und -Abonnements?

Eine Thema kann als Warteschlange visualisiert werden, und bei Verwendung mehrerer Abonnements ergibt sich ein umfassenderes Messagingmodell. Im Wesentlichen handelt es sich um ein Tool für die 1:n-Kommunikation. Bei diesem Modell vom Typ „Veröffentlichen/Abonnieren“ (*Pub/Sub*) kann für eine Anwendung, die eine Nachricht an ein Thema mit mehreren Abonnements sendet, erreicht werden, dass die Nachricht von mehreren Anwendungen empfangen wird.

### Was ist der Azure Service Bus Relay-Dienst?

Mit dem [Relay-Dienst](../service-bus-relay/service-bus-relay-overview.md) haben Sie die Möglichkeit, WCF-Dienste transparent zu hosten und von jedem Ort aus darauf zuzugreifen. Anders ausgedrückt: Sie können für Hybridanwendungen erreichen, dass sie sowohl in einem Azure-Rechenzentrum als auch in einer lokalen Unternehmensumgebung ausgeführt werden.

### Was ist eine partitionierte Entität?

Eine herkömmliche Warteschlange oder ein Thema werden von einem einzelnen Nachrichtenbroker verarbeitet und in einem Nachrichtenspeicher gespeichert. Eine [partitionierte Warteschlange bzw. ein Thema](service-bus-partitioning.md) wird mit mehreren Nachrichtenbrokern verarbeitet und in mehreren Nachrichtenspeichern gespeichert. Dies bedeutet, dass der Gesamtdurchsatz einer partitionierten Warteschlange oder eines Themas nicht mehr durch die Leistung eines einzelnen Nachrichtenbrokers oder Nachrichtenspeichers beschränkt wird. Außerdem führt ein vorübergehender Ausfall eines Nachrichtenspeichers nicht dazu, dass eine partitionierte Warteschlange oder ein Thema nicht verfügbar ist.

Beachten Sie, dass die Sortierung bei der Verwendung von partitionierten Entitäten nicht sichergestellt ist. Wenn eine Partition nicht verfügbar ist, ist es trotzdem möglich, Nachrichten zu senden und von anderen Partitionen zu empfangen.

## Service Bus – Bewährte Methoden

### Gibt es Beispiele für bewährte Azure Service Bus-Methoden?

-   [Bewährte Methoden für Leistungsoptimierungen mithilfe von Service Bus-Brokermessaging][]\: In diesem Artikel wird beschrieben, wie Sie die Leistung beim Austauschen von im Broker gespeicherten Nachrichten optimieren.

-   [Bewährte Methoden zum Schützen von Anwendungen vor Service Bus-Ausfällen und Notfällen][]\: In diesem Artikel wird beschrieben, wie Sie Relayendpunkte, Warteschlangen und Themen und die aktive und passive Replikation zwischen Ausfällen von Datencentern am besten schützen.

###Was muss ich vor der Erstellung von Nachrichtenentitäten wissen?   

Die folgenden Eigenschaften einer Warteschlange und eines Themas sind unveränderlich. Berücksichtigen Sie dies beim Bereitstellen Ihrer Entitäten, da eine Änderung nicht möglich ist, ohne eine neue Ersatzentität zu erstellen.

-   Größe

-   Partitionierung

-   Sitzungen

-   Duplikaterkennung

-   Express-Entität

## Service Bus – Preise

In diesem Abschnitt werden einige häufig gestellte Fragen zur Service Bus-Preisstruktur beantwortet. Sie können auch die [Azure-Support-FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) aufrufen, wenn Sie nach allgemeinen Microsoft Azure-Preisinformationen suchen. Vollständige Informationen zu Service Bus-Preisen finden Sie unter [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/).

### Wie werden die Kosten für Service Bus berechnet?

Umfassende Informationen zu Service Bus-Preisen finden Sie unter [Service Bus – Preise][Pricing overview]. Neben den aufgeführten Preisen werden Ihnen die damit verbundenen aus dem Datencenter ausgehenden Datenübertragungen berechnet, mit denen Ihre Anwendung bereitgestellt wird.

### Bei welcher Nutzung von Service Bus werden Daten übertragen? Bei welcher nicht?

Jede Datenübertragung innerhalb der jeweiligen Azure-Region ist kostenfrei. Bei ausgehenden Datenübertragungen außerhalb einer Region fallen Kosten zu einem Tarif von $0,15 pro GB von den Regionen Nordamerika und Europa an sowie $0,20 pro GB von der Region Asien-Pazifik. Sämtliche eingehenden Datenübertragungen sind kostenfrei.

### Wie wird die Verbrauchseinheit "Relaystunden" berechnet?

Relaystunden werden für den kumulativen Zeitraum in Rechnung gestellt, in dem jedes Service Bus Relay während eines bestimmten Abrechnungszeitraums „geöffnet“ ist. Ein Relay wird an einer bestimmten Service Bus-Adresse implizit instanziiert und geöffnet (Dienstnamespace-URL), sobald ein relayfähiger WCF-Dienst oder „Relaylistener“ zum ersten Mal eine Verbindung mit dieser Adresse herstellt. Das Relay wird erst geschlossen, wenn der letzte Listener die Verbindung mit dieser Adresse trennt. Zu Abrechnungszwecken wird ein Relay daher ab der Verbindungsherstellung durch den ersten Relaylistener bis zur Trennung der Verbindung mit der Service Bus-Adresse des Relays durch den letzten Relaylistener als „geöffnet“ betrachtet. Anders ausgedrückt: Ein Relay gilt als geöffnet, sobald mindestens ein Relaylistener mit seiner Service Bus-Adresse verbunden ist.

### Was geschieht, wenn mehrere Listener mit einem bestimmten Relay verbunden sind?

In einigen Fällen können mit einem einzelnen Relay in Service Bus mehrere Listener verbunden sein. Das kann bei Diensten mit Lastenausgleich vorkommen, die die WCF-Bindungen „netTCPRelay“ oder „HttpRelay“ verwenden, oder bei Sendungsereignislistenern, die die WCF-Bindung „netEventRelay“ verwenden. Ein Relay in Service Bus wird als "geöffnet" betrachtet, wenn mindestens ein Relaylistener mit ihm verbunden ist. Das Hinzufügen weiterer Listener zu einem offenen Relay verändert nicht dessen Status im Hinblick auf die Abrechnung. Die Anzahl von Relaysendern (Clients, die Nachrichten von Relays aufrufen oder an diese senden), die mit dem Relay verbunden sind, hat ebenfalls keine Auswirkung auf die Berechnung der Relaystunden.

### Wie wird die Verbrauchseinheit "Nachrichten" für Relays berechnet?

Im Allgemeinen werden abrechenbare Nachrichten für Relays mit derselben Methode berechnet, die oben für Brokerentitäten (Warteschlangen, Themen und Abonnements) beschrieben wird. Es gibt jedoch einige wichtige Ausnahmen:

Das Senden einer Nachricht an ein Service Bus Relay wird als „ganz durchgehendes“ Senden an den Relaylistener behandelt, der die Nachricht erhält, und nicht als ein Senden an das Service Bus Relay, gefolgt von einer Zustellung an den Relaylistener. Aus diesem Grund führt ein Dienstaufruf im Anforderungs-/Antwortstil (von bis zu 64 KB) für einen Relaylistener zu zwei abrechenbaren Nachrichten: Eine Nachricht ist für die Anforderung und eine Nachricht für die Antwort abrechenbar (unter der Voraussetzung, dass die Antwort ebenfalls <= 64 KB ist). Dies unterscheidet sich vom Einsatz einer Warteschlange für die Vermittlung zwischen einem Client und einem Dienst. In letzterem Fall würde dasselbe Anforderungs-/Antwort-Muster erfordern, dass eine Anfrage an die Warteschlange gesandt wird, gefolgt von einem Entfernen aus der Warteschlange/einer Zustellung an den Dienst, wiederum gefolgt von einer Antwortsendung an eine andere Warteschlange und einem Entfernen aus der Warteschlange/einer Zustellung an den Client. Mit denselben Annahmen für die Größe (<= 64 KB) würde dieses vermittelte Warteschlangenmuster darum zu vier abrechenbaren Nachrichten führen, also der doppelten Menge im Vergleich zur Anwendung desselben Musters mithilfe eines Relays. Natürlich gibt es Vorteile beim Einsatz von Warteschlangen, wenn man dieses Muster verwenden möchte, wie etwa Dauerhaftigkeit und Lastenausgleich. Diese Vorteile können die zusätzlichen Kosten rechtfertigen.

Relays, die anhand der WCF-Bindung „netTCPRelay“ geöffnet werden, behandeln Nachrichten nicht als einzelne Nachrichten, sondern als Datenstrom, der durch das System fließt. Anders ausgedrückt: Nur der Absender und der Listener besitzen Einblick in das Framing der einzelnen Nachrichten, die mit dieser Bindung gesendet/empfangen werden. Zum Zweck der Berechnung abrechenbarer Nachrichten werden deshalb für Relays, die die Bindung „netTCPRelay“ verwenden, alle Daten als ein Datenstrom behandelt. In diesem Fall berechnet Service Bus die Gesamtmenge der über die einzelnen Relays gesendeten oder empfangen Daten auf 5-Minuten-Basis und teilt diese Menge durch 64 KB, um so die Anzahl abrechenbarer Nachrichten für das fragliche Relay während dieses Zeitraums zu ermitteln.

### Fallen für Service Bus Speicherkosten an?

Nein, für Service Bus fallen keine Speicherkosten an. Es gibt allerdings ein Kontingent, das die Höchstmenge an Daten begrenzt, die pro Warteschlange/Thema beibehalten werden kann. Weitere Informationen finden Sie im nächsten Abschnitt.

## Service Bus-Kontingente

Eine Liste mit Service Bus-Grenzwerten und -Kontingenten finden Sie unter [Übersicht über Kontingente][].

### Gibt es für Service Bus Nutzungskontingente?

Microsoft legt für jeden Clouddienst standardmäßig ein aggregiertes monatliches Nutzungskontingent fest, das abonnementübergreifend für einen Kunden berechnet wird. Uns ist bewusst, dass Sie möglicherweise mehr benötigen als diese Grenzwerte. Sie können sich gerne jederzeit an unseren Kundendienst wenden und uns Ihre Anforderungen mitteilen, damit wir die Grenzwerte entsprechend anpassen können. Für Service Bus gelten die folgenden aggregierten Nutzungskontingente:

- 5 Milliarden Nachrichten
- 2 Millionen Relaystunden

Wir behalten uns das Recht vor, ein Kundenkonto zu deaktivieren, das die Nutzungskontingente in einem Monat überschritten hat. Wir informieren allerdings per E-Mail darüber und unternehmen mehrere Versuche, den Kunden zu kontaktieren, ehe wir diese Maßnahme ergreifen. Kunden, die diese Kontingente überschreiten, haben die Kosten für die Überschreitung zu tragen.

Genau wie andere Dienste in Azure setzt Service Bus eine Reihe spezifischer Kontingente durch, die eine faire Nutzung der Ressourcen sicherstellen. Im Folgenden finden Sie die Nutzungskontingente, die vom Dienst durchgesetzt werden:

#### Warteschlangen-/Themengröße

Bei der Erstellung einer Warteschlange oder eines Themas legen Sie jeweils die maximale Größe fest. Dieses Kontingent kann einen Wert von 1, 2, 3, 4 oder 5 GB aufweisen. Wenn die festgelegte Größe erreicht ist, werden zusätzlich eingehende Nachrichten abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen.

#### Anzahl gleichzeitiger Verbindungen

Warteschlange/Thema/Abonnement: Die Anzahl der gleichzeitigen TCP-Verbindungen für eine Warteschlange, ein Thema oder ein Abonnement ist auf 100 begrenzt. Wenn dieses Kontingent erreicht wird, werden nachfolgende Anforderungen für zusätzliche Verbindungen abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen. Für jede Messagingfactory unterhält Service Bus eine TCP-Verbindung, wenn einer der von ihr erstellten Clients einen ausstehenden aktiven Vorgang aufweist oder ein Vorgang vor weniger als 60 Sekunden abgeschlossen wurde. REST-Vorgänge werden nicht zu den gleichzeitigen TCP-Verbindungen hinzugezählt.

#### Anzahl der gleichzeitigen Listener für ein Relay

Die Anzahl gleichzeitiger **netTcpRelay**- und **netHttpRelay**-Listener auf einem Relay ist auf 25 begrenzt (1 für ein **NetOneway**-Relay).

#### Anzahl von gleichzeitigen Relaylistenern pro Namespace

Service Bus setzt einen Grenzwert von 2.000 gleichzeitigen Relaylistenern pro Dienstnamespace durch. Wenn dieses Kontingent erreicht wird, werden nachfolgende Anforderungen für das Öffnen zusätzlicher Relaylistener abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen.

#### Anzahl von Themen/Warteschlangen pro Dienstnamespace

Die maximale Anzahl von Themen/Warteschlangen (permanente, durch Speicher gesicherte Entitäten) in einem Dienstnamespace ist auf 10.000 begrenzt. Wenn dieses Kontingent erreicht wird, werden nachfolgende Anforderungen zum Erstellen eines neuen Themas bzw. einer neuen Warteschlange für den Dienstnamespace zurückgewiesen. In diesem Fall zeigt das klassische Azure-Portal eine Fehlermeldung an, oder der anfordernde Clientcode empfängt eine Ausnahme. Dies hängt davon ab, ob der Erstellungsversuch über das Portal oder über den Clientcode erfolgt ist.

### Kontingente für die Nachrichtengröße

#### Warteschlange/Thema/Abonnement

**Nachrichtengröße**: Jede Nachricht ist auf eine Gesamtgröße von 256 KB einschließlich Nachrichtenheader begrenzt.

**Nachrichtenheadergröße**: Jeder Nachrichtenheader ist auf 64 KB begrenzt.

**NetOneway- und NetEvent-Relays**: Jede Nachricht ist auf eine Gesamtgröße von 64 KB einschließlich Nachrichtenheader begrenzt.

**Http- und NetTcp-Relays**: Service Bus setzt keine obere Grenze hinsichtlich der Größe von Nachrichten durch.

Nachrichten, die diese Größenkontingente überschreiten, werden abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen.

**Anzahl von Abonnements pro Thema**: Die maximale Anzahl von Abonnements pro Thema ist auf 2.000 beschränkt. Wenn dieses Kontingent erreicht ist, werden nachfolgende Anforderungen zum Erstellen weiterer Abonnements für das Thema zurückgewiesen. In diesem Fall zeigt das klassische Azure-Portal eine Fehlermeldung an, oder der anfordernde Clientcode empfängt eine Ausnahme. Dies hängt davon ab, ob der Erstellungsversuch über das Portal oder über den Clientcode erfolgt ist.

**Anzahl von SQL-Filtern pro Thema**: Die maximale Anzahl von SQL-Filtern pro Thema ist auf 2.000 beschränkt. Wenn dieses Kontingent erreicht ist, werden nachfolgende Anforderungen für die Erstellung weiterer Filter für das Thema zurückgewiesen, und der aufrufende Code empfängt eine Ausnahme.

**Anzahl von Korrelationsfiltern pro Thema**: Die maximale Anzahl von Korrelationsfiltern pro Thema ist auf 100.000 beschränkt. Wenn dieses Kontingent erreicht ist, werden nachfolgende Anforderungen für die Erstellung weiterer Filter für das Thema zurückgewiesen, und der aufrufende Code empfängt eine Ausnahme.

## Abonnement- und Namespace-Verwaltung

### Wie migriere ich einen Namespace zu einem anderen Azure-Abonnement?

Sie können PowerShell-Befehle verwenden (siehe [diesen Artikel][]), um einen Namespace von einem Azure-Abonnement in ein anderes zu verschieben. Der Namespace muss bereits aktiv sein, um diesen Vorgang durchführen zu können. Außerdem muss der Benutzer, der die Befehle ausführt, sowohl im Quellabonnement als auch im Zielabonnement ein Administrator sein.

## Problembehandlung in Service Bus

[Übersicht über Ausnahmen][]

### Welche Beispiele gibt es für Ausnahmen, die von Azure Service Bus-Messaging-APIs generiert werden, und die vorgeschlagenen Aktionen?

Die Ausnahmen, die von Messaging-APIs generiert werden können, können in die folgenden Kategorien fallen:

-   Codierfehler des Benutzers

-   Einrichtungs-/Konfigurationsfehler

-   Vorübergehende Ausnahmen

-   Andere Ausnahmen

Im Artikel [Service Bus-Messagingausnahmen][Exceptions overview] werden einige Ausnahmen mit Vorschlägen für Aktionen beschrieben.

### Was ist eine Shared Access Signature, und welche Sprachen unterstützen die Generierung einer Signatur?

SAS (Shared Access Signatures) sind ein Authentifizierungsmechanismus, der auf sicheren Hashes (SHA-256) oder URIs basiert. Informationen dazu, wie Sie Ihre eigenen Signaturen in Node, PHP, Java und C# generieren, finden Sie im Artikel [Shared Access Signatures][].

## Nächste Schritte

Weitere Informationen zu Service Bus Messaging finden Sie in folgenden Themen.

- [Einführung in Azure Service Bus Premium-Messaging (Blogbeitrag)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Einführung in Azure Service Bus Premium-Messaging (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Übersicht über Service Bus-Messaging](service-bus-messaging-overview.md)
- [Übersicht über die Architektur von Azure Service Bus](../service-bus/service-bus-fundamentals-hybrid-solutions.md)
- [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)

[Bewährte Methoden für Leistungsoptimierungen mithilfe von Service Bus-Brokermessaging]: service-bus-performance-improvements.md
[Bewährte Methoden zum Schützen von Anwendungen vor Service Bus-Ausfällen und Notfällen]: ../service-bus/service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Übersicht über Kontingente]: ../service-bus/service-bus-quotas.md
[diesen Artikel]: ../service-bus/service-bus-powershell-how-to-provision.md#migrate-a-namespace-to-another-azure-subscription
[Exceptions overview]: ../service-bus/service-bus-messaging-exceptions.md
[Übersicht über Ausnahmen]: ../service-bus/service-bus-messaging-exceptions.md
[Shared Access Signatures]: ../service-bus/service-bus-sas-overview.md

<!---HONumber=AcomDC_0928_2016-->