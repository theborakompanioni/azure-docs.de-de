<properties 
    pageTitle="AMQP 1.0 in Azure Service Bus und Event Hubs – Protokollleitfaden | Microsoft Azure" 
    description="Enthält einen Protokollleitfaden für Ausdrücke und eine Beschreibung von AMQP 1.0 in Azure Service Bus und Event Hubs." 
    services="service-bus,event-hubs" 
    documentationCenter=".net" 
    authors="clemensv" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="07/01/2016"
    ms.author="clemensv;jotaub;hillaryc;sethm"/>

# AMQP 1.0 in Azure Service Bus und Event Hubs – Protokollleitfaden

Das Advanced Message Queueing Protocol 1.0 ist ein standardisiertes Framing- und Übertragungsprotokoll für das asynchrone, sichere und zuverlässige Übertragen von Nachrichten zwischen zwei Parteien. Es ist das primäre Protokoll von Azure Service Bus Messaging und Azure Event Hubs. Beide Dienste unterstützen auch HTTPS. Das proprietäre SBMP-Protokoll, das ebenfalls unterstützt wird, wird zugunsten von AMQP nach und nach abgeschafft.

AMQP 1.0 ist das Ergebnis einer breiten Zusammenarbeit, bei der Anbieter von Middleware, z.B. Microsoft und Red Hat, mit vielen Nutzern von Messaging-Middleware, z.B. JP Morgan Chase als Vertreter der Finanzdienstleistungsbranche, kooperiert haben. Das Forum für die technische Standardisierung für das AMQP-Protokoll und Erweiterungsspezifikationen ist OASIS, das als internationaler Standard formell anerkannt ist (ISO/IEC 19494).

## Ziele

In diesem Artikel sind die Kernkonzepte der AMQP 1.0-Messagingspezifikation und einige im Entwurfsstadium befindliche Erweiterungsspezifikationen zusammengefasst, die derzeit vom technischen Komitee für OASIS AMQP abschließend festgelegt werden. Außerdem wird beschrieben, wie diese Spezifikationen von Azure Service Bus implementiert und genutzt werden.

Das Ziel besteht darin, dass alle Entwickler, die einen vorhandenen AMQP 1.0-Clientstapel auf einer beliebigen Plattform verwenden, mit Azure Service Bus über AMQP 1.0 interagieren können.

In gängige allgemeine AMQP 1.0-Stapel, wie Apache Proton oder AMQP.NET Lite, sind alle wichtigen AMQP 1.0-Gesten bereits implementiert. Diese grundlegenden Gesten werden manchmal von einer übergeordneten API umschlossen. Bei Apache Proton sind dies sogar zwei, nämlich die imperative Messenger-API und die reaktive Reactor-API.

In der folgenden Beschreibung wird davon ausgegangen, dass die Verwaltung von AMQP-Verbindungen, -Sitzungen und -Links und die Behandlung von Frameübertragungen und der Flusssteuerung vom jeweiligen Stapel übernommen wird (z.B. Apache Proton-C) und von Anwendungsentwicklern nur sehr wenig oder gar keine Aufmerksamkeit benötigt wird. Wir setzen auf abstrakte Weise das Vorhandensein einiger API-Grundlagen voraus, z.B. die Verbindungsherstellung und die Erstellung von *sender*- und *receiver*-Abstraktionsobjekten, die dann jeweils über `send()`- und `receive()`-Vorgänge verfügen.

Beim Beschreiben von erweiterten Funktionen von Azure Service Bus, z.B. Durchsuchen von Nachrichten oder Verwalten von Sitzungen, werden nicht nur AMQP-Begriffe verwendet, sondern zusätzlich zu dieser vorausgesetzten API-Abstraktion wird auch eine mehrstufige Pseudoimplementierung genutzt.

## Was ist AMQP?

AMQP ist ein Framing- und Übertragungsprotokoll. „Framing“ bedeutet, dass eine Struktur für binäre Datenströme bereitgestellt wird, die in beide Richtungen einer Netzwerkverbindung fließen. Die Struktur ermöglicht, dass einzelne Datenblöcke – Frames – zwischen den verbundenen Parteien ausgetauscht werden. Mit den Übertragungsfunktionen wird sichergestellt, dass beide kommunizierenden Parteien ein gemeinsames Verständnis entwickeln können, wann Frames übertragen und Übertragungen als abgeschlossen angesehen werden sollen.

Im Gegensatz zu früheren abgelaufenen Entwürfen, die von der AMQP-Arbeitsgruppe produziert wurden und von einigen Nachrichtenbrokern noch verwendet werden, wird im letzten und standardisierten AMQP 1.0-Protokoll nicht das Vorhandensein eines Nachrichtenbrokers oder einer bestimmten Topologie für Entitäten in einem Nachrichtenbroker vorgeschrieben.

Das Protokoll kann für die symmetrische Peer-to-Peer-Kommunikation verwendet werden, um die Interaktion mit Nachrichtenbrokern zu ermöglichen, die Warteschlangen unterstützen und Entitäten veröffentlichen/abonnieren, wie dies bei Azure Service Bus der Fall ist. Außerdem kann das Protokoll für die Interaktion mit der Messaginginfrastruktur verwendet werden, bei der sich die Interaktionsmuster von regulären Warteschlangen unterscheiden, wie dies bei Azure Event Hubs der Fall ist. Ein Event Hub verhält sich wie eine Warteschlange, wenn Ereignisse an den Event Hub gesendet werden, aber eher wie ein serieller Speicherdienst, wenn Ereignisse daraus gelesen werden. Dieses Verhalten ähnelt in gewisser Weise einem Bandlaufwerk. Der Client wählt ein Offset in den verfügbaren Datenstrom und erhält dann alle Ereignisse dieses Offsets bis zum aktuellsten Ereignis.

Das AMQP 1.0-Protokoll ist auf Erweiterbarkeit ausgelegt, damit die Funktionen durch weitere Spezifikationen ausgebaut werden können. Dies wird in den drei Erweiterungsspezifikationen deutlich, die in diesem Dokument beschrieben werden. Für die Kommunikation über die vorhandene HTTPS/WebSockets-Infrastruktur, bei der die Konfiguration der nativen AMQP-TCP-Ports schwierig sein kann, wird mit einer bindenden Spezifikation definiert, wie AMQP auf WebSockets verteilt werden kann. Für die Interaktion mit der Messaginginfrastruktur nach dem Schema „Anforderung/Antwort“ für Verwaltungszwecke oder zur Bereitstellung erweiterter Funktionen sind in der AMQP-Verwaltungsspezifikation die erforderlichen Grundlagen für die Interaktion definiert. Für die Verbundautorisierungsmodell-Integration definiert die anspruchsbasierte AMQP-Sicherheitsspezifikation, wie Sie Autorisierungstoken, die Links zugeordnet sind, zuordnen und erneuern.

## Grundlegende AMQP-Szenarien

In diesem Abschnitt wird die grundlegende Nutzung von AMQP 1.0 mit Azure Service Bus beschrieben. Dies umfasst das Erstellen von Verbindungen, Sitzungen und Links und das Übertragen von Nachrichten an und von Service Bus-Entitäten wie Warteschlangen, Themen und Abonnements.

Die beste Quelle für Informationen zur Funktionsweise von AMQP ist die AMQP 1.0-Spezifikation. Sie wurde aber als genaue Anleitung für die Implementierung und nicht als Leitfaden zum Protokoll geschrieben. In diesem Abschnitt geht es um die Einführung in die Terminologie, die benötigt wird, um die Nutzung von AMQP 1.0 durch Service Bus zu beschreiben. Eine umfassendere Einführung in AMQP und eine ausführlichere Beschreibung von AMQP 1.0 erhalten Sie in diesem [Videokurs][].

### Verbindungen und Sitzungen

![][1]

In AMQP werden die kommunizierenden Programme als *Container* bezeichnet. Die Container enthalten *Knoten*, bei denen es sich um die kommunizierenden Entitäten in den Containern handelt. Eine Warteschlange kann ein Knoten dieser Art sein. AMQP ermöglicht Multiplexing, sodass eine Verbindung für viele Kommunikationspfade zwischen Knoten verwendet werden kann. Beispielsweise kann ein Anwendungsclient über eine Netzwerkverbindung gleichzeitig Daten von einer Warteschlange erhalten und Daten an eine andere Warteschlange senden.

Die Netzwerkverbindung ist also im Container verankert. Sie wird vom Container in der Clientrolle initiiert. Es wird eine ausgehende TCP-Socketverbindung mit einem Container in der Empfängerrolle hergestellt, über die das Abhören und Akzeptieren von eingehenden TCP-Verbindungen durchgeführt wird. Der Verbindungshandshake umfasst die Aushandlung der Protokollversion, die Deklaration oder Aushandlung der Nutzung von Transport Level Security (TLS/SSL) und einen Handshake für die Authentifizierung/Autorisierung für den Verbindungsbereich, der auf SASL basiert.

Für Azure Service Bus ist jederzeit die Verwendung von TLS erforderlich. Es werden Verbindungen über TCP-Port 5671 unterstützt, wobei die TCP-Verbindung zuerst mit TLS überlagert wird, bevor der AMQP-Protokollhandshake aktiviert wird. Außerdem werden Verbindungen über TCP-Port 5672 unterstützt, wobei der Server sofort ein obligatorisches Upgrade der Verbindung auf TLS anbietet, indem das von AMQP vorgeschriebene Modell verwendet wird. Die AMQP-WebSockets-Bindung erstellt einen Tunnel über TCP-Port 443, der dann AMQP 5671-Verbindungen entspricht.

Nach dem Einrichten der Verbindung und von TLS bietet Service Bus zwei SASL-Mechanismusoptionen an:

-   SASL PLAIN wird häufig zum Übergeben von Benutzername- und Kennwort-Anmeldeinformationen an einen Server verwendet. Service Bus verfügt nicht über Konten, sondern über benannte [SAS-Regeln](service-bus-shared-access-signature-authentication.md), mit denen Rechte gewährt werden und die einem Schlüssel zugeordnet sind. Der Name einer Regel wird als Benutzername und der Schlüssel (Base64-codierter Text) wird als Kennwort verwendet. Die Rechte, die der ausgewählten Regel zugeordnet sind, regeln die für die Verbindung zulässigen Vorgänge.

-   SASL ANONYMOUS wird zum Umgehen der SASL-Autorisierung verwendet, wenn der Client das Modell „Anspruchsbasierte Sicherheit“ (Claims-Based Security, CBS) verwenden möchte. Dieses Modell wird weiter unten beschrieben. Mit dieser Option kann eine Clientverbindung für kurze Zeit anonym eingerichtet werden. Während dieses Zeitraums kann der Client nur mit dem CBS-Endpunkt interagieren, und der CBS-Handshake muss abgeschlossen sein.

Nachdem die Transportverbindung hergestellt wurde, deklarieren die Container jeweils die maximale Framegröße, die sie verarbeiten können. Außerdem wird deklariert, nach welcher Leerlaufdauer die Verbindung einseitig getrennt wird, wenn sie keine Aktivität aufweist.

Ebenso wird deklariert, wie viele gleichzeitige Kanäle unterstützt werden. Ein Kanal ist ein unidirektionaler, ausgehender, virtueller Übertragungspfad zusätzlich zur Verbindung. Eine Sitzung nutzt jeweils einen Kanal aus miteinander verbundenen Containern, um einen bidirektionalen Kommunikationspfad zu bilden.

Sitzungen verfügen über ein fensterbasiertes Flusssteuerungsmodell. Wenn eine Sitzung erstellt wird, deklariert jede Partei, wie viele Frames im Empfängerfenster akzeptiert werden können. Wenn die Parteien Frames austauschen, wird das Fenster mit Frames gefüllt. Die Übertragungen werden beendet, wenn das Fenster voll ist, bis es zurückgesetzt oder mit der *flow-Performative* erweitert wird (*Performative* ist der AMQP-Begriff für Gesten auf Protokollebene, die zwischen zwei Parteien ausgetauscht werden).

Dieses fensterbasierte Modell entspricht im Wesentlichen dem TCP-Konzept der fensterbasierten Flusssteuerung, aber auf Sitzungsebene innerhalb des Sockets. Das Konzept des Protokolls, mehrere gleichzeitige Sitzungen zuzulassen, hat den Zweck, dass Datenverkehr mit hoher Priorität gedrosselten normalen Verkehr überholen kann – wie auf der linken Spur auf der Autobahn.

In Azure Service Bus wird für jede Verbindung derzeit genau eine Sitzung verwendet. Die maximale Service Bus-Framegröße beträgt 262.144 Byte (256 KB) für Service Bus-Standardhubs und Event Hubs. Für Service Bus Premium sind es 1.048.576 Byte (1 MB). Service Bus gibt keine bestimmten Fenster für die Drosselung auf Sitzungsebene vor, sondern setzt das Fenster im Rahmen der Flusssteuerung auf Linkebene regelmäßig zurück (siehe [nächster Abschnitt](#links)).

Verbindungen, Kanäle und Sitzungen sind flüchtig. Wenn die zugrunde liegende Verbindung getrennt wird, müssen Verbindungen, TLS-Tunnel, SASL-Autorisierungskontext und Sitzungen wiederhergestellt werden.

### Links

![][2]

Für AMQP werden Nachrichten über Verknüpfungen (Links) übertragen. Eine Verknüpfung ist ein Kommunikationspfad, der für eine Sitzung erstellt wird und mit dem Nachrichten in einer Richtung übertragen werden können. Die Aushandlung des Übertragungsstatus wird über die Verknüpfung und in bidirektionaler Form zwischen den verbundenen Parteien durchgeführt.

Verknüpfungen können jederzeit von jedem Container und über eine vorhandene Sitzung erstellt werden. Dies unterscheidet AMQP von vielen anderen Protokollen, z.B. HTTP und MQTT, bei denen die Initiierung von Übertragungen und der Übertragungspfad alleiniges Privileg der Partei sind, die die Socketverbindung erstellt.

Der Container, der die Verknüpfung initiiert hat, bittet den anderen Container, eine Verknüpfung zu akzeptieren, und wählt entweder die Absender- oder die Empfängerrolle. Daher können beide Container die Erstellung von unidirektionalen oder bidirektionalen Kommunikationspfaden initiieren, wobei Letzterer in Form eines Verknüpfungspaars modelliert ist.

Verknüpfungen werden benannt und Knoten zugeordnet. Wie am Anfang erwähnt, sind Knoten die kommunizierenden Entitäten in einem Container.

In Azure Service Bus entspricht ein Knoten direkt einer Warteschlange, einem Thema, einem Abonnement oder einer Unterwarteschlange für unzustellbare Nachrichten, die einer Warteschlange oder einem Abonnement zugeordnet ist. Der in AMQP verwendete Knotenname ist daher der relative Name der Entität im Service Bus-Namespace. Wenn eine Warteschlange den Namen **myqueue** hat, ist dies gleichzeitig ihr AMQP-Knotenname. Ein Themenabonnement basiert auf der HTTP-API-Konvention, indem es in der Ressourcensammlung „subscriptions“ angeordnet wird. Daher verfügt das Abonnement **sub** oder das Thema **mytopic** über den AMQP-Knotennamen **mytopic/subscriptions/sub**.

Der verbindende Client ist auch zum Verwenden eines lokalen Knotennamens zum Erstellen von Verknüpfungen erforderlich. Service Bus schreibt diese Knotennamen nicht vor und interpretiert sie auch nicht. AMQP 1.0-Clientstapel verwenden im Allgemeinen ein Schema, um sicherzustellen, dass diese flüchtigen Knotennamen im Bereich des Clients eindeutig sind.

### Übertragungen

![][3]

Nachdem eine Verknüpfung eingerichtet wurde, können Nachrichten über die Verknüpfung übertragen werden. In AMQP wird eine Übertragung mit einer expliziten Protokollgeste (der Performative *transfer*) übertragen, mit der eine Nachricht über eine Verknüpfung vom Absender zum Empfänger befördert wird. Eine Übertragung ist abgeschlossen, wenn eine „Übereinkunft“ (settled) erzielt wurde. Dies bedeutet, dass sich beide Parteien auf das Ergebnis der Übertragung geeinigt haben.

Im einfachsten Fall kann sich der Absender für das Senden von Nachrichten im Zustand vor der „Übereinkunft“ (pre-settled) entscheiden. Dies bedeutet, dass der Client am Ergebnis nicht interessiert ist und der Empfänger kein Feedback zum Ergebnis des Vorgangs liefert. Dieser Modus wird von Azure Service Bus auf AMQP-Protokollebene unterstützt, aber nicht in den Client-APIs verfügbar gemacht.

Der Normalfall ist, dass Nachrichten ohne Übereinkunft gesendet werden und der Empfänger mit der Performative *disposition* die Annahme oder Ablehnung angibt. Eine Ablehnung liegt vor, wenn der Empfänger die Nachricht aus einem bestimmten Grund nicht akzeptieren kann. Die Ablehnungsnachricht enthält Informationen zum Grund, und die Fehlerstruktur wird von AMQP definiert. Wenn Nachrichten aufgrund von internen Fehlern in Azure Service Bus abgelehnt werden, gibt der Dienst zusätzliche Informationen innerhalb dieser Struktur zurück. Diese Informationen können zum Bereitstellen von Diagnosehinweisen verwendet werden, um Mitarbeiter zu unterstützen, wenn Sie Supportanfragen erstellen. Weiter unten erhalten Sie weitere Informationen zu Fehlern.

Eine spezielle Form der Ablehnung ist der Status *Freigegeben* (released). Hiermit wird angegeben, dass der Empfänger keine technischen Einwände gegen die Übertragung erhebt, aber auch kein Interesse an der Erzielung einer Übereinkunft über die Übertragung hat. Ein Beispiel für diesen Fall ist: Eine Nachricht wird an einen Service Bus-Client zugestellt, und der Client entscheidet sich für das „Verwerfen“ der Nachricht, weil er die sich aus der Nachricht ergebenden Verarbeitungsschritte nicht ausführen kann, während die Nachrichtenzustellung selbst keinen Fehler aufweist. Eine Variante dieses Zustands ist der Zustand *Geändert* (modified), in dem Änderungen an der Nachricht zulässig sind, wenn sie freigegeben wird. Dieser Status wird von Service Bus derzeit nicht verwendet.

Die AMQP 1.0-Spezifikation definiert einen weiteren Dispositionsstatus, und zwar *Empfangen* (received), der speziell zur Unterstützung der Verknüpfungswiederherstellung dient. Die Verknüpfungswiederherstellung ermöglicht die Wiederherstellung des Zustands einer Verknüpfung und aller Zustellungen einer neuen Verbindung und Sitzung, wenn die vorherige Verbindung und Sitzung getrennt wurden.

Azure Service Bus unterstützt die Verknüpfungswiederherstellung nicht. Wenn die Verbindung des Clients mit Service Bus getrennt wird und noch eine ungeklärte Nachrichtenübertragung aussteht, geht diese Nachrichtenübertragung verloren. Der Client muss die Verbindung dann erneut herstellen, die Verknüpfung neu einrichten und versuchen, die Übertragung noch einmal durchzuführen.

Azure Service Bus und Event Hubs unterstützen daher Übertragungen der Art „At least once“ (mindestens einmal), bei denen der Absender sicher sein kann, dass die Nachricht gespeichert und akzeptiert wurde. Es werden aber keine Übertragungen der Art „Exactly once“ (genau einmal) auf AMQP-Ebene unterstützt, bei denen versucht wird, die Verknüpfung wiederherzustellen und mit der Aushandlung des Zustellungsstatus fortzufahren, um eine Duplizierung der Nachrichtenübertragung zu vermeiden.

Um das mögliche doppelte Senden zu kompensieren, unterstützt Azure Service Bus die Duplikaterkennung als optionales Feature für Warteschlangen und Themen. Bei der Duplikaterkennung werden die Nachrichten-IDs aller eingehenden Nachrichten während eines benutzerdefinierten Zeitfensters aufgezeichnet und im Hintergrund alle Nachrichten verworfen, die in diesem Zeitfenster mit den gleichen Nachrichten-IDs gesendet werden.

### Flusssteuerung

![][4]

Zusätzlich zum Modell der Flusssteuerung auf Sitzungsebene, das bereits erwähnt wurde, verfügt jede Verknüpfung über ein eigenes Modell für die Flusssteuerung. Die Flusssteuerung auf Sitzungsebene schützt den Container davor, zu viele Frames auf einmal verarbeiten zu müssen. Bei der Flusssteuerung auf Verknüpfungsebene kann die Anwendung entscheiden, wie viele Nachrichten einer Verknüpfung und zu welchem Zeitpunkt diese Nachrichten verarbeitet werden sollen.

Für eine Verknüpfung können Übertragungen nur durchgeführt werden, wenn der Absender über genügend „Link Credit“ (Verknüpfungsguthaben) verfügt. Das Verknüpfungsguthaben ist ein Indikator, der vom Empfänger mit der *flow*-Performative für den Bereich einer Verknüpfung festgelegt wird. Wenn dem Absender ein Verknüpfungsguthaben zugewiesen wird, versucht er, dieses Guthaben durch das Zustellen von Nachrichten aufzubrauchen. Mit jeder Nachrichtenzustellung wird das verbleibende Verknüpfungsguthaben um den Wert 1 verringert. Wenn das Verknüpfungsguthaben aufgebraucht ist, werden die Zustellungen beendet.

Falls Service Bus die Empfängerrolle innehat, stattet die Anwendung den Absender sofort mit ausreichend Verknüpfungsguthaben aus, damit sofort Nachrichten gesendet werden können. Beim Verwenden des Verknüpfungsguthabens sendet Service Bus gelegentlich eine *flow*-Performative an den Absender, um den Stand des Verknüpfungsguthabens zu aktualisieren.

In der Absenderrolle sendet Service Bus aktiv Nachrichten, um das vorhandene Verknüpfungsguthaben aufzubrauchen.

Der Aufruf von „receive“ auf der API-Ebene wird in eine *flow*-Performative übersetzt, die vom Client an Service Bus gesendet wird. Service Bus nutzt dieses Guthaben, indem die erste verfügbare nicht gesperrte Nachricht aus der Warteschlange gesperrt und übertragen wird. Falls keine Nachricht für die Zustellung verfügbar ist, bleibt das ausstehende Guthaben von Verknüpfungen mit der jeweiligen Entität in der Eingangsreihenfolge aufgezeichnet, und Nachrichten werden gesperrt und übertragen, sobald sie verfügbar sind, um ausstehendes Guthaben zu verbrauchen.

Die Sperre der Nachricht wird aufgehoben, wenn für die Übertragung einer der Endzustände *accepted*, *rejected* oder *released* (Akzeptiert, Abgelehnt oder Freigegeben) erreicht wird. Die Nachricht wird aus Service Bus entfernt, wenn der Endzustand *accepted* erreicht wird. Sie verbleibt in Service Bus und wird an den nächsten Empfänger zugestellt, wenn die Übertragung einen der anderen Zustände erreicht. Service Bus verschiebt die Nachricht automatisch in die Warteschlange der Entität für unzustellbare Nachrichten, wenn die maximal zulässige Zustellungsanzahl der Entität erreicht wird, weil wiederholt Ablehnungen oder Freigaben aufgetreten sind.

Für die offiziellen Service Bus-APIs wird eine Option dieser Art derzeit zwar nicht direkt verfügbar gemacht, aber ein AMQP-Protokollclient auf einer unteren Ebene kann das Verknüpfungsguthaben-Modell wie folgt nutzen: Umwandeln der „Pull“-Interaktion mit Ausgabe von einer Guthabeneinheit pro Empfangsanforderung in ein „Push“-Modell, indem eine sehr große Zahl von Verknüpfungsguthaben ausgestellt wird und Nachrichten dann ohne weitere Interaktion empfangen werden, wenn sie verfügbar sind. Dieser Pushvorgang wird von den Eigenschafteneinstellungen [MessagingFactory.PrefetchCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.prefetchcount.aspx) und [MessageReceiver.PrefetchCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.prefetchcount.aspx) unterstützt. Wenn sie nicht null sind, nutzt der AMQP-Client sie als Verknüpfungsguthaben.

In diesem Kontext ist es wichtig zu verstehen, dass die Zeitmessung für den Ablauf der Sperre einer Nachricht in der Entität beginnt, wenn die Nachricht aus der Entität entnommen wird, und nicht beim Start der Übermittlung einer Nachricht. Immer wenn der Client die Bereitschaft zum Empfangen von Nachrichten anzeigt, indem Verknüpfungsguthaben ausgegeben wird, wird daher erwartet, dass er aktiv Nachrichten aus dem gesamten Netzwerk per Pullvorgang empfangen und anschließend verarbeiten kann. Andernfalls läuft die Nachrichtensperrung ggf. ab, bevor die Nachricht zugestellt wurde. Die Verwendung der Flusssteuerung mit Verknüpfungsguthaben sollte direkt die sofortige Bereitschaft widerspiegeln, an den Empfänger gesendete verfügbare Nachrichten zu verarbeiten.

Als Zusammenfassung enthalten die folgenden Abschnitte eine schematische Übersicht über den Performativenfluss während der unterschiedlichen API-Interaktionen. In jedem Abschnitt wird ein anderer logischer Vorgang beschrieben. Einige dieser Interaktionen sind „lazy“ (faul). Dies bedeutet, dass sie ggf. erst ausgeführt werden, wenn sie benötigt werden. Die Erstellung eines Nachrichtenabsenders verursacht unter Umständen erst dann eine Netzwerkinteraktion, wenn die erste Nachricht gesendet oder angefordert wird.

Die Pfeile zeigen die Performativenflussrichtung an.

#### Erstellen eines Nachrichtenempfängers

| Client | Service Bus |
|---------------------------------------------------------------------------------------------------------------------------------------------------	|--------------------------------------------------------------------------------------------------------------------------------------------	|
| --> attach(<br/>name={Verknüpfungsname},<br/>handle={numerischer Handle},<br/>role=**receiver**,<br/>source={Entitätsname},<br/>target={Clientverknüpfungs-ID}<br/>) | Client wird Entität als Empfänger zugeordnet |
| Service Bus antwortet und ordnet sein Ende der Verknüpfung zu | <-- attach(<br/>name={Verknüpfungsname},<br/>handle={numerischer Handle},<br/>role=**sender**,<br/>source={Entitätsname},<br/>target={Clientverknüpfungs-ID}<br/>) |

#### Erstellen des Nachrichtenabsenders

| Client | Service Bus |
|------------------------------------------------------------------------------------------------------------------	|--------------------------------------------------------------------------------------------------------------------	|
| --> attach(<br/>name={Verknüpfungsname},<br/>handle={numerischer Handle},<br/>role=**sender**,<br/>source={Clientverknüpfungs-ID},<br/>target={Entitätsname}<br/>) | Keine Aktion |
| Keine Aktion | <-- attach(<br/>name={Verknüpfungsname},<br/>handle={numerischer Handle},<br/>role=**receiver**,<br/>source={Clientverknüpfungs-ID},<br/>target={Entitätsname}<br/>) |

#### Erstellen des Nachrichtenabsenders (Fehler)

| Client | Service Bus |
|------------------------------------------------------------------------------------------------------------------	|---------------------------------------------------------------------	|
| --> attach(<br/>name={Verknüpfungsname},<br/>handle={numerischer Handle},<br/>role=**sender**,<br/>source={Clientverknüpfungs-ID},<br/>target={Entitätsname}<br/>) | Keine Aktion |
| Keine Aktion | <-- attach(<br/>name={Verknüpfungsname},<br/>handle={numerischer Handle},<br/>role=**receiver**,<br/>source=null,<br/>target=null<br/>)<br/><br/><-- detach(<br/>handle={numerischer Handle},<br/>closed=**true**,<br/>error={Fehlerinformationen}<br/>) |

#### Schließen des Nachrichtenabsenders/-empfängers

| Client | Service Bus |
|-------------------------------------------------	|-------------------------------------------------	|
| --> detach(<br/>handle={numerischer Handle},<br/>closed=**true**<br/>) | Keine Aktion |
| Keine Aktion | <-- detach(<br/>handle={numerischer Handle},<br/>closed=**true**<br/>) |

#### Senden (Erfolg)

| Client | Service Bus |
|------------------------------------------------------------------------------------------------------------------------------	|------------------------------------------------------------------------------------------------------	|
| --> transfer(<br/>delivery-id={numerischer Handle},<br/>delivery-tag={binärer Handle},<br/>settled=**false**,,more=**false**,<br/>state=**null**,<br/>resume=**false**<br/>) | Keine Aktion |
| Keine Aktion | <-- disposition(<br/>role=receiver,<br/>first={Übermittlungs-ID},<br/>last={Übermittlungs-ID},<br/>settled=**true**,<br/>state=**accepted**<br/>) |

#### Senden (Fehler)

| Client | Service Bus |
|------------------------------------------------------------------------------------------------------------------------------	|-----------------------------------------------------------------------------------------------------------------------------	|
| --> transfer(<br/>delivery-id={numerischer Handle},<br/>delivery-tag={binärer Handle},<br/>settled=**false**,,more=**false**,<br/>state=**null**,<br/>resume=**false**<br/>) | Keine Aktion |
| Keine Aktion | <-- disposition(<br/>role=receiver,<br/>first={Übermittlungs-ID},<br/>last={Übermittlungs-ID},<br/>settled=**true**,<br/>state=**rejected**(<br/>error={Fehlerinformationen}<br/>)<br/>) |

#### Empfangen

| Client | Service Bus |
|------------------------------------------------------------------------------------------------------	|------------------------------------------------------------------------------------------------------------------------------	|
| --> flow(<br/>link-credit=1<br/>) | Keine Aktion |
| Keine Aktion | < transfer(<br/>delivery-id={numerischer Handle},<br/>delivery-tag={binärer Handle},<br/>settled=**false**,<br/>more=**false**,<br/>state=**null**,<br/>resume=**false**<br/>) |
| --> disposition(<br/>role=**receiver**,<br/>first={Übermittlungs-ID},<br/>last={Übermittlungs-ID},<br/>settled=**true**,<br/>state=**accepted**<br/>) | Keine Aktion |

#### Empfangen mehrerer Nachrichten

| Client | Service Bus |
|--------------------------------------------------------------------------------------------------------	|--------------------------------------------------------------------------------------------------------------------------------	|
| --> flow(<br/>link-credit=3<br/>) | Keine Aktion |
| Keine Aktion | < transfer(<br/>delivery-id={numerischer Handle},<br/>delivery-tag={binärer Handle},<br/>settled=**false**,<br/>more=**false**,<br/>state=**null**,<br/>resume=**false**<br/>) |
| Keine Aktion | < transfer(<br/>delivery-id={numerischer Handle+1},<br/>delivery-tag={binärer Handle},<br/>settled=**false**,<br/>more=**false**,<br/>state=**null**,<br/>resume=**false**<br/>) |
| Keine Aktion | < transfer(<br/>delivery-id={numerischer Handle+2},<br/>delivery-tag={binärer Handle},<br/>settled=**false**,<br/>more=**false**,<br/>state=**null**,<br/>resume=**false**<br/>) |
| --> disposition(<br/>role=receiver,<br/>first={Übermittlungs-ID},<br/>last={Übermittlungs-ID+2},<br/>settled=**true**,<br/>state=**accepted**<br/>) | Keine Aktion |

### Meldungen

In den folgenden Abschnitten wird erläutert, welche Eigenschaften aus den AMQP-Standardnachrichtenabschnitten von Service Bus verwendet werden und wie sie den offiziellen Service Bus-APIs zugeordnet werden.

#### Header

| Feldname | Verwendung | API-Name |
|----------------	|-------------------------------	|---------------	|
| durable | - | - |
| priority | - | - |
| ttl | Gültigkeitsdauer (Time To Live, TTL) für diese Nachricht | [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx) |
| first-acquirer | - | - |
| delivery-count | - | [DeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.deliverycount.aspx) |

#### Eigenschaften

| Feldname | Verwendung | API-Name |
|----------------------	|---------------------------------------------------------------------------------------------------------------------------------	|--------------------------------------------	|
| message-id | Anwendungsdefinierte Freiform-ID für diese Nachricht. Wird für die Duplikaterkennung verwendet. | [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) |
| user-id | Anwendungsdefinierte Benutzer-ID, von Service Bus nicht interpretiert. | Nicht über die Service Bus-API zugänglich. |
| to | Anwendungsdefinierte Ziel-ID, von Service Bus nicht interpretiert. | [To](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.to.aspx) |
| subject | Anwendungsdefinierte Nachrichtenzweck-ID, von Service Bus nicht interpretiert. | [Bezeichnung](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) |
| reply-to | Anwendungsdefinierter Antwortpfadindikator, von Service Bus nicht interpretiert. | [ReplyTo](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.replyto.aspx) |
| correlation-id | Anwendungsdefinierte Korrelations-ID, von Service Bus nicht interpretiert. | [CorrelationId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.correlationid.aspx) |
| Inhaltstyp | Anwendungsdefinierter Inhaltstypindikator für den Haupttext, von Service Bus nicht interpretiert. | [ContentType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) |
| content-encoding | Anwendungsdefinierter Inhaltscodierungsindikator für den Haupttext, von Service Bus nicht interpretiert. | Nicht über die Service Bus-API zugänglich. |
| absolute-expiry-time | Deklariert, zu welchem absolutem Zeitpunkt die Nachricht abläuft. Bei Eingabe ignoriert (Header-TTL wird beachtet), autoritativ bei Ausgabe. | [ExpiresAtUtc](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.expiresatutc.aspx) |
| creation-time | Deklariert, zu welchem Zeitpunkt die Nachricht erstellt wurde. Wird von Service Bus nicht verwendet. | Nicht über die Service Bus-API zugänglich. |
| group-id | Anwendungsdefinierte ID für eine verwandte Nachrichtengruppe. Wird für Service Bus-Sitzungen verwendet. | [SessionId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) |
| group-sequence | Indikator, der die relative Sequenznummer der Nachricht in einer Sitzung angibt. Wird von Service Bus ignoriert. | Nicht über die Service Bus-API zugänglich. |
| reply-to-group-id | - | [ReplyToSessionId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.replytosessionid.aspx) |

## Erweiterte Service Bus-Funktionen

In diesem Abschnitt werden die erweiterten Funktionen von Azure Service Bus behandelt. Sie basieren auf Erweiterungsentwürfen für AMQP, die derzeit vom OASIS Technical Committee für AMQP entwickelt werden. Azure Service Bus implementiert den aktuellen Status dieser Entwürfe und übernimmt die Änderungen, die eingeführt werden, wenn diese Entwürfe den Standardstatus erreichen.

> [AZURE.NOTE] Erweiterte Service Bus Messaging-Vorgänge werden über ein Muster vom Typ „Anforderung/Antwort“ unterstützt. Details zu diesen Vorgängen finden Sie im Dokument [AMQP 1.0 in Service Bus: request/response-based operations](https://msdn.microsoft.com/library/azure/mt727956.aspx) (AMQP 1.0 in Service Bus: Auf „Anforderung/Antwort“ basierende Vorgänge).

### AMQP-Verwaltung

Die AMQP-Verwaltungsspezifikation ist der erste der hier beschriebenen Erweiterungsentwürfe. Diese Spezifikation definiert eine Gruppe von Protokollgesten, die dem AMQP-Protokoll übergeordnet sind und Verwaltungsinteraktionen mit der Verwaltungsinfrastruktur über AMQP zulassen. Die Spezifikation definiert generische Vorgänge, z.B. *Erstellen*, *Lesen*, *Aktualisieren* und *Löschen*, um Entitäten in einer Messaginginfrastruktur und eine Gruppe von Abfragevorgängen zu verwalten.

Für all diese Gesten ist eine Anforderung/Antwort-Interaktion zwischen dem Client und der Messaginginfrastruktur erforderlich. Daher definiert die Spezifikation, wie dieses Interaktionsmuster zusätzlich zu AMQP modelliert wird: Der Client stellt eine Verbindung mit der Messaginginfrastruktur her, initiiert eine Sitzung und erstellt dann ein Verknüpfungspaar. Für die eine Verknüpfung fungiert der Client als Absender, und für die andere als Empfänger. Auf diese Weise wird ein Verknüpfungspaar erstellt, das als bidirektionaler Kanal dienen kann.

| Logischer Vorgang | Client | Service Bus |
|------------------------------|-----------------------------|-----------------------------|
| Anforderung/Antwort-Pfad erstellen | --> attach(<br/>name={*Verknüpfungsname*},<br/>handle={*numerischer Handle*},<br/>role=**sender**,<br/>source=**null**,<br/>target=”myentity/$management”<br/>) |Keine Aktion |
|Anforderung/Antwort-Pfad erstellen |Keine Aktion | <-- attach(<br/>name={*Verknüpfungsname*},<br/>handle={*numerischer Handle*},<br/>role=**receiver**,<br/>source=null,<br/>target=”myentity”<br/>) |
|Anforderung/Antwort-Pfad erstellen | --> attach(<br/>name={*Verknüpfungsname*},<br/>handle={*numerischer Handle*},<br/>role=**receiver**,<br/>source=”myentity/$management”,<br/>target=”myclient$id”<br/>) | |Keine Aktion
|Anforderung/Antwort-Pfad erstellen |Keine Aktion | <-- attach(<br/>name={*Verknüpfungsname*},<br/>handle={*numerischer Handle*},<br/>role=**sender**,<br/>source=”myentity”,<br/>target=”myclient$id”<br/>) |

Wenn dieses Verknüpfungspaar vorhanden ist, ist die Anforderung/Antwort-Implementierung einfach: Eine Anforderungsnachricht wird an eine Entität in der Messaginginfrastruktur gesendet, die dieses Muster versteht. In dieser Anforderungsnachricht wird das Feld *reply-to* im Abschnitt *properties* auf die ID *target* für die Verknüpfung festgelegt, für die die Antwort übermittelt wird. Die behandelnde Entität verarbeitet die Anforderung und übermittelt die Antwort dann über die Verknüpfung, deren ID *target* mit der angegebenen ID *reply-to* übereinstimmt.

Für dieses Muster ist es natürlich erforderlich, dass der Clientcontainer und die vom Client generierte ID für das Antwortziel für alle Clients eindeutig sind und aus Sicherheitsgründen schwer vorauszusagen sind.

Der Austausch von Nachrichten für das Verwaltungsprotokoll und alle anderen Protokolle, die das gleiche Muster verwenden, wird auf der Anwendungsebene durchgeführt. Es werden keine neuen Gesten auf AMQP-Protokollebene definiert. Dies ist beabsichtigt, damit Anwendungen diese Erweiterungen mit kompatiblen AMQP 1.0-Stapeln sofort nutzen können.

Azure Service Bus implementiert derzeit keine Kernfunktionen der Verwaltungsspezifikation. Das von der Verwaltungsspezifikation definierte Anforderung/Antwort-Muster stellt aber die Grundlage für die anspruchsbasierte Sicherheitsfunktion und fast alle erweiterten Funktionen dar, die wir in den folgenden Abschnitten beschreiben.

### Anspruchsbasierte Autorisierung

Der Entwurf der AMQP-Spezifikation für die anspruchsbasierte Autorisierung (Claims-Based Authorization Specification, CBS) baut auf dem Anforderung/Antwort-Muster der Verwaltungsspezifikation auf und beschreibt ein generalisiertes Modell für die Verwendung von Verbundsicherheitstoken mit AMQP.

Das Standardsicherheitsmodell von AMQP, das in dieser Einführung beschrieben wird, basiert auf SASL und ist in den AMQP-Verbindungshandshake integriert. Die Verwendung von SASL ist mit dem Vorteil verbunden, dass ein erweiterbares Modell bereitgestellt wird, für das eine Gruppe von Mechanismen definiert wurde. Hiervon können alle Protokolle profitieren, die formell an SASL angelehnt sind. Beispiele für diese Mechanismen sind „PLAIN“ für die Übertragung von Benutzernamen und Kennwörtern, „EXTERNAL“ für die Bindung an die Sicherheit auf TLS-Ebene, „ANONYMOUS“ zum Ausdrücken des Fehlens der expliziten Authentifizierung/Autorisierung und eine Vielzahl weiterer Mechanismen, die das Übergeben von Anmeldeinformationen oder Token für die Authentifizierung bzw. Autorisierung ermöglichen.

Die SASL-Integration von AMQP hat zwei Nachteile:

-   Alle Anmeldeinformationen und Token gelten für die Verbindung. Für eine Messaginginfrastruktur muss unter Umständen eine differenzierte Zugriffssteuerung pro Entität bereitgestellt werden. Beispielsweise kann einem Bearer eines Tokens das Senden an die Warteschlange A erlaubt werden, das Senden an Warteschlange B aber nicht. Wenn der Autorisierungskontext in der Verbindung verankert ist, ist es nicht möglich, eine einzelne Verbindung und unterschiedliche Zugriffstoken für Warteschlange A und Warteschlange B zu verwenden.

-   Zugriffstoken sind meist nur für einen bestimmten Zeitraum gültig. Der Benutzer muss daher regelmäßig Token neu beschaffen. Dies stellt für den Tokenaussteller eine Gelegenheit dar, das Ausstellen eines neuen Tokens zu verweigern, wenn sich die Zugriffsberechtigungen des Benutzers geändert haben. AMQP-Verbindungen verfügen ggf. über eine sehr lange Dauer. Das SASL-Modell bietet nur zur Verbindungszeit die Möglichkeit, ein Token festzulegen. Dies bedeutet, dass die Messaginginfrastruktur entweder die Verbindung des Clients trennen muss, wenn das Token abläuft, oder es muss das Risiko einer zugelassenen fortgesetzten Kommunikation mit einem Client eingegangen werden, dessen Zugriffsrechte in der Zwischenzeit unter Umständen widerrufen wurden.

Die von Azure Service Bus implementierte AMQP-CBS-Spezifikation ermöglicht für beide Probleme eine elegante Umgehung: Ein Client kann jedem Knoten Zugriffstoken zuordnen und diese Token aktualisieren, bevor sie ablaufen, ohne den Nachrichtenfluss zu unterbrechen.

CBS definiert einen virtuellen Verwaltungsknoten mit dem Namen *$cbs*, der von der Messaginginfrastruktur bereitgestellt wird. Der Verwaltungsknoten akzeptiert Knoten im Namen aller anderen Knoten in der Messaginginfrastruktur.

Die Protokollgeste ist ein Anforderung/Antwort-Austausch, der von der Verwaltungsspezifikation definiert wird. Dies bedeutet, dass der Client ein Verknüpfungspaar für den Knoten *$cbs* einrichtet und dann eine Anforderung an die ausgehende Verknüpfung übergibt. Anschließend wartet er auf die Antwort über die eingehende Verknüpfung.

Die Anforderungsnachricht weist die folgenden Anwendungseigenschaften auf:

| Schlüssel | Optional | Werttyp | Wertinhalt |
|------------|----------|------------|--------------------------------------------|
| operation | Nein | string | **put-token** |
| type | Nein | string | Der Typ des abgelegten Tokens. |
| name | Nein | string | Die Zielgruppe, für die das Token gilt. |
| expiration | Ja | timestamp | Der Ablaufzeitpunkt des Tokens. |

Die *name*-Eigenschaft identifiziert die Entität, der das Token zugeordnet werden soll. In Service Bus ist dies der Pfad zur Warteschlange oder zum Thema/Abonnement. Mit der *type*-Eigenschaft wird der Tokentyp identifiziert:

| Tokentyp | Tokenbeschreibung | Texttyp | Hinweise |
|---------------------------------|------------------------|---------------------|----------------------------------------------------------|
| amqp:jwt | JSON Web Token (JWT) | AMQP-Wert (Zeichenfolge) | Noch nicht verfügbar. |
| amqp:swt | Einfaches Webtoken (Simple Web Token, SWT) | AMQP-Wert (Zeichenfolge) | Wird nur für von AAD/ACS ausgestellte SWTs unterstützt. |
| servicebus.windows.net:sastoken | Service Bus-SAS-Token | AMQP-Wert (Zeichenfolge) | - |

Mit Token werden Rechte gewährt. Service Bus kennt drei grundlegende Rechte: „Senden“ (Send) für das Senden, „Abhören“ (Listen) für das Empfangen und „Verwalten“ (Manage) für das Ändern von Entitäten. Von AAD/ACS ausgestellte SWTs enthalten diese Rechte als Ansprüche. Service Bus-SAS-Token beziehen sich auf Regeln, die für den Namespace oder die Entität konfiguriert sind, und diese Regeln werden mit den Rechten konfiguriert. Das Signieren des Tokens mit dem Schlüssel, der der Regel zugeordnet ist, führt daher dazu, dass das Token die entsprechenden Rechte ausdrückt. Das mit *put-token* einer Entität zugeordnete Token ermöglicht es dem verbundenen Client, über die Tokenrechte mit der Entität zu interagieren. Für eine Verknüpfung, bei der der Client die Rolle des *Absenders* übernimmt, ist das Recht „Senden“ erforderlich. Für die Rolle des *Empfängers* wird das Recht „Abhören“ benötigt.

Die Antwortnachricht weist die folgenden *application-properties*-Werte auf:

| Schlüssel | Optional | Werttyp | Wertinhalt |
|--------------------|----------|------------|-----------------------------------|
| status-code | Nein | int | HTTP-Antwortcode **[RFC2616]** |
| status-description | Ja | string | Beschreibung des Status |

Der Client kann *put-token* mehrfach und für jede Entität in der Messaginginfrastruktur aufrufen. Die Token gelten für den aktuellen Client und sind in der aktuellen Verbindung verankert. Dies bedeutet, dass der Server alle beibehaltenen Token verwirft, wenn die Verbindung getrennt wird.

Für die derzeitige Service Bus-Implementierung ist CBS nur zusammen mit der SASL-Methode „ANONYMOUS“ zulässig. Vor dem SASL-Handshake muss immer eine SSL/TLS-Verbindung vorhanden sein.

Der ANONYMOUS-Mechanismus muss vom gewählten AMQP 1.0-Client also unterstützt werden. Anonymer Zugriff bedeutet, dass der erste Verbindungshandshake, einschließlich der Erstellung der ersten Sitzung, durchgeführt wird, ohne dass Service Bus weiß, von wem die Verbindung erstellt wird.

Nachdem die Verbindung und die Sitzung eingerichtet wurden, sind das Zuordnen der Verknüpfungen zum Knoten *$cbs* und das Senden der *put-token*-Anforderung die einzigen zulässigen Vorgänge. Ein gültiges Token muss mithilfe einer *put-token*-Anforderung für einen Entitätsknoten innerhalb von 20 Sekunden nach der Verbindungsherstellung erfolgreich festgelegt werden. Andernfalls wird die Verbindung von Service Bus einseitig verworfen.

Der Client ist anschließend dafür zuständig, den Tokenablauf nachzuverfolgen. Wenn ein Token abläuft, verwirft Service Bus sofort alle Verknüpfungen der Verbindung mit der jeweiligen Entität. Um dies zu verhindern, kann der Client das Token für den Knoten jederzeit durch ein neues Token ersetzen, indem er den virtuellen Verwaltungsknoten *$cbs* mit der gleichen *put-token*-Geste verwendet. Dabei wird der Nutzlast-Datenverkehr anderer Verknüpfungen nicht beeinträchtigt.

## Nächste Schritte

Weitere Informationen zu AMQP finden Sie unter den folgenden Links:

- [Übersicht über Service Bus AMQP]
- [AMQP 1.0-Unterstützung für partitionierte Warteschlangen und Themen von Service Bus]
- [AMQP in Service Bus für Windows Server]

[Videokurs]: https://www.youtube.com/playlist?list=PLmE4bZU0qx-wAP02i0I7PJWvDWoCytEjD
[1]: ./media/service-bus-amqp/amqp1.png
[2]: ./media/service-bus-amqp/amqp2.png
[3]: ./media/service-bus-amqp/amqp3.png
[4]: ./media/service-bus-amqp/amqp4.png

[Übersicht über Service Bus AMQP]: service-bus-amqp-overview.md
[AMQP 1.0-Unterstützung für partitionierte Warteschlangen und Themen von Service Bus]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP in Service Bus für Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx

<!---HONumber=AcomDC_0713_2016-->