---
title: "Service Bus – Häufig gestellte Fragen | Microsoft Docs"
description: "Enthält Antworten auf einige häufig gestellte Fragen zu Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: cc75786d-3448-4f79-9fec-eef56c0027ba
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/04/2016
ms.author: sethm;juconway
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: b350bac36f1d46c97da37a2807ead8c9c732d69a


---
# <a name="service-bus-faq"></a>Service Bus – Häufig gestellte Fragen
In diesem Artikel werden einige häufig gestellte Fragen zu Microsoft Azure Service Bus beantwortet. Sie können auch [Häufig gestellte Fragen zum Azure-Support](http://go.microsoft.com/fwlink/?LinkID=185083) aufrufen, wenn Sie nach allgemeinen Informationen zu Azure-Preisen und zum Support suchen. Die folgenden Themen werden behandelt:

* [Allgemeine Fragen zu Azure Service Bus Messaging](#general-questions-about-azure-service-bus-messaging)
* [Service Bus – Bewährte Methoden](#service-bus-best-practices)
* [Service Bus Preise](#service-bus-pricing)
* [Service Bus-Kontingente](#service-bus-quotas)
* [Abonnement- und Namespace-Verwaltung](#subscription-and-namespace-management)
* [Problembehandlung](#service-bus-troubleshooting)

## <a name="general-questions-about-azure-service-bus-messaging"></a>Allgemeine Fragen zu Azure Service Bus Messaging
### <a name="what-is-azure-service-bus-messaging"></a>Was ist Azure Service Bus Messaging?
[Azure Service Bus Messaging](service-bus-messaging-overview.md) ist eine asynchrone Cloudplattform für Messaging, mit der Sie Daten zwischen entkoppelten Systemen senden können. Microsoft stellt dieses Feature als Dienst bereit. Dies bedeutet, dass Sie Ihre Hardware nicht selbst hosten müssen, um sie verwenden zu können.

### <a name="what-is-a-service-bus-namespace"></a>Was ist ein Service Bus-Namespace?
Ein [Namespace](service-bus-create-namespace-portal.md) ist ein Bereichscontainer für die Adressierung von Service Bus-Ressourcen innerhalb Ihrer Anwendung. Die Namespace-Erstellung ist für die Verwendung von Service Bus erforderlich und ist einer der ersten Schritte.

### <a name="what-is-an-azure-service-bus-queue"></a>Was ist eine Azure Service Bus-Warteschlange?
Eine [Service Bus-Warteschlange](service-bus-queues-topics-subscriptions.md) ist eine Entität, in der Nachrichten gespeichert werden. Warteschlangen sind besonders nützlich, wenn Sie über mehrere Anwendungen oder mehrere Teile einer verteilten Anwendung verfügen, die miteinander kommunizieren müssen. Die Warteschlange ähnelt einem Verteilzentrum, in dem mehrere Produkte (Nachrichten) eingehen und von diesem Standort aus dann weiterverschickt werden.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Was sind Azure Service Bus-Themen und -Abonnements?
Eine Thema kann als Warteschlange visualisiert werden, und bei Verwendung mehrerer Abonnements ergibt sich ein umfassenderes Messagingmodell. Im Wesentlichen handelt es sich um ein Tool für die 1:n-Kommunikation. Bei diesem Modell vom Typ „Veröffentlichen/Abonnieren“ (*Pub/Sub*) kann für eine Anwendung, die eine Nachricht an ein Thema mit mehreren Abonnements sendet, erreicht werden, dass die Nachricht von mehreren Anwendungen empfangen wird.

### <a name="what-is-a-partitioned-entity"></a>Was ist eine partitionierte Entität?
Eine herkömmliche Warteschlange oder ein Thema werden von einem einzelnen Nachrichtenbroker verarbeitet und in einem Nachrichtenspeicher gespeichert. Eine [partitionierte Warteschlange bzw. ein Thema](service-bus-partitioning.md) wird mit mehreren Nachrichtenbrokern verarbeitet und in mehreren Nachrichtenspeichern gespeichert. Dies bedeutet, dass der Gesamtdurchsatz einer partitionierten Warteschlange oder eines Themas nicht mehr durch die Leistung eines einzelnen Nachrichtenbrokers oder Nachrichtenspeichers beschränkt wird. Außerdem führt ein vorübergehender Ausfall eines Nachrichtenspeichers nicht dazu, dass eine partitionierte Warteschlange oder ein Thema nicht verfügbar ist.

Beachten Sie, dass die Sortierung bei der Verwendung von partitionierten Entitäten nicht sichergestellt ist. Wenn eine Partition nicht verfügbar ist, ist es trotzdem möglich, Nachrichten zu senden und von anderen Partitionen zu empfangen.

## <a name="service-bus-best-practices"></a>Service Bus – Bewährte Methoden
### <a name="what-are-some-azure-service-bus-best-practices"></a>Gibt es Beispiele für bewährte Azure Service Bus-Methoden?
* [Bewährte Methoden für Leistungsoptimierungen mithilfe von Service Bus-Brokermessaging][Bewährte Methoden für Leistungsoptimierungen mithilfe von Service Bus-Brokermessaging]: In diesem Artikel wird beschrieben, wie Sie die Leistung beim Austauschen von im Broker gespeicherten Nachrichten optimieren.

### <a name="what-should-i-know-before-creating-messaging-entities"></a>Was muss ich vor der Erstellung von Nachrichtenentitäten wissen?
Die folgenden Eigenschaften einer Warteschlange und eines Themas sind unveränderlich. Berücksichtigen Sie dies beim Bereitstellen Ihrer Entitäten, da eine Änderung nicht möglich ist, ohne eine neue Ersatzentität zu erstellen.

* Größe
* Partitionierung
* Sitzungen
* Duplikaterkennung
* Express-Entität

## <a name="service-bus-pricing"></a>Service Bus – Preise
In diesem Abschnitt werden einige häufig gestellte Fragen zur Service Bus-Preisstruktur beantwortet. Sie können auch die [Azure-Support-FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) aufrufen, wenn Sie nach allgemeinen Microsoft Azure-Preisinformationen suchen. Vollständige Informationen zu Service Bus-Preisen finden Sie unter [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="how-do-you-charge-for-service-bus"></a>Wie werden die Kosten für Service Bus berechnet?
Umfassende Informationen zu Service Bus-Preisen finden Sie unter [Service Bus Preise][Preisübersicht]. Neben den aufgeführten Preisen werden Ihnen die damit verbundenen aus dem Datencenter ausgehenden Datenübertragungen berechnet, mit denen Ihre Anwendung bereitgestellt wird.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>Bei welcher Nutzung von Service Bus werden Daten übertragen? Bei welcher nicht?
Jede Datenübertragung innerhalb der jeweiligen Azure-Region ist kostenfrei. Bei ausgehenden Datenübertragungen außerhalb einer Region fallen Kosten zu einem Tarif von $0,15 pro GB von den Regionen Nordamerika und Europa an sowie $0,20 pro GB von der Region Asien-Pazifik. Sämtliche eingehenden Datenübertragungen sind kostenfrei.

### <a name="does-service-bus-charge-for-storage"></a>Fallen für Service Bus Speicherkosten an?
Nein, für Service Bus fallen keine Speicherkosten an. Es gibt allerdings ein Kontingent, das die Höchstmenge an Daten begrenzt, die pro Warteschlange/Thema beibehalten werden kann. Weitere Informationen finden Sie im nächsten Abschnitt.

## <a name="service-bus-quotas"></a>Service Bus-Kontingente
Eine Liste mit Service Bus-Grenzwerten und -Kontingenten finden Sie unter [Übersicht über Kontingente][Übersicht über Kontingente].

### <a name="does-service-bus-have-any-usage-quotas"></a>Gibt es für Service Bus Nutzungskontingente?
Microsoft legt für jeden Clouddienst standardmäßig ein aggregiertes monatliches Nutzungskontingent fest, das abonnementübergreifend für einen Kunden berechnet wird. Uns ist bewusst, dass Sie möglicherweise mehr benötigen als diese Grenzwerte. Sie können sich gerne jederzeit an unseren Kundendienst wenden und uns Ihre Anforderungen mitteilen, damit wir die Grenzwerte entsprechend anpassen können. Für Service Bus gelten die folgenden aggregierten Nutzungskontingente:

* 5 Milliarden Nachrichten

Wir behalten uns das Recht vor, ein Kundenkonto zu deaktivieren, das die Nutzungskontingente in einem Monat überschritten hat. Wir informieren allerdings per E-Mail darüber und unternehmen mehrere Versuche, den Kunden zu kontaktieren, ehe wir diese Maßnahme ergreifen. Kunden, die diese Kontingente überschreiten, haben die Kosten für die Überschreitung zu tragen.

Genau wie andere Dienste in Azure setzt Service Bus eine Reihe spezifischer Kontingente durch, die eine faire Nutzung der Ressourcen sicherstellen. Im Folgenden finden Sie die Nutzungskontingente, die vom Dienst durchgesetzt werden:

#### <a name="queuetopic-size"></a>Warteschlangen-/Themengröße
Bei der Erstellung einer Warteschlange oder eines Themas legen Sie jeweils die maximale Größe fest. Dieses Kontingent kann einen Wert von 1, 2, 3, 4 oder 5 GB aufweisen. Wenn die festgelegte Größe erreicht ist, werden zusätzlich eingehende Nachrichten abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen.

#### <a name="naming-restrictions"></a>Benennungseinschränkungen
Die Namen für einen Service Bus-Namespace können nur zwischen 6 und 50 Zeichen lang sein. Die Zeichenanzahl für Warteschlangen, Themen oder Abonnements liegt zwischen 1 und 50 Zeichen.

#### <a name="number-of-concurrent-connections"></a>Anzahl gleichzeitiger Verbindungen
Warteschlange/Thema/Abonnement: Die Anzahl der gleichzeitigen TCP-Verbindungen für eine Warteschlange, ein Thema oder ein Abonnement ist auf 100 begrenzt. Wenn dieses Kontingent erreicht wird, werden nachfolgende Anforderungen für zusätzliche Verbindungen abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen. Für jede Messagingfactory unterhält Service Bus eine TCP-Verbindung, wenn einer der von ihr erstellten Clients einen ausstehenden aktiven Vorgang aufweist oder ein Vorgang vor weniger als 60 Sekunden abgeschlossen wurde. REST-Vorgänge werden nicht zu den gleichzeitigen TCP-Verbindungen hinzugezählt.

#### <a name="number-of-topicsqueues-per-service-namespace"></a>Anzahl von Themen/Warteschlangen pro Dienstnamespace
Die maximale Anzahl von Themen/Warteschlangen (permanente, durch Speicher gesicherte Entitäten) in einem Dienstnamespace ist auf 10.000 begrenzt. Wenn dieses Kontingent erreicht wird, werden nachfolgende Anforderungen zum Erstellen eines neuen Themas bzw. einer neuen Warteschlange für den Dienstnamespace zurückgewiesen. In diesem Fall zeigt das klassische Azure-Portal eine Fehlermeldung an, oder der anfordernde Clientcode empfängt eine Ausnahme. Dies hängt davon ab, ob der Erstellungsversuch über das Portal oder über den Clientcode erfolgt ist.

### <a name="message-size-quotas"></a>Kontingente für die Nachrichtengröße
#### <a name="queuetopicsubscription"></a>Warteschlange/Thema/Abonnement
**Nachrichtengröße** : Jede Nachricht ist auf eine Gesamtgröße von 256 KB einschließlich Nachrichtenheader begrenzt.

**Nachrichtenheadergröße** : Jeder Nachrichtenheader ist auf 64 KB begrenzt.

Nachrichten, die diese Größenkontingente überschreiten, werden abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen.

**Anzahl von Abonnements pro Thema** : Die maximale Anzahl von Abonnements pro Thema ist auf 2.000 beschränkt. Wenn dieses Kontingent erreicht ist, werden nachfolgende Anforderungen zum Erstellen weiterer Abonnements für das Thema zurückgewiesen. In diesem Fall zeigt das klassische Azure-Portal eine Fehlermeldung an, oder der anfordernde Clientcode empfängt eine Ausnahme. Dies hängt davon ab, ob der Erstellungsversuch über das Portal oder über den Clientcode erfolgt ist.

**Anzahl von SQL-Filtern pro Thema** : Die maximale Anzahl von SQL-Filtern pro Thema ist auf 2.000 beschränkt. Wenn dieses Kontingent erreicht ist, werden nachfolgende Anforderungen für die Erstellung weiterer Filter für das Thema zurückgewiesen, und der aufrufende Code empfängt eine Ausnahme.

**Anzahl von Korrelationsfiltern pro Thema** : Die maximale Anzahl von Korrelationsfiltern pro Thema ist auf 100.000 beschränkt. Wenn dieses Kontingent erreicht ist, werden nachfolgende Anforderungen für die Erstellung weiterer Filter für das Thema zurückgewiesen, und der aufrufende Code empfängt eine Ausnahme.

## <a name="subscription-and-namespace-management"></a>Abonnement- und Namespace-Verwaltung
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Wie migriere ich einen Namespace zu einem anderen Azure-Abonnement?
Sie können PowerShell-Befehle verwenden (siehe [hier][hier]), um einen Namespace von einem Azure-Abonnement in ein anderes zu verschieben. Der Namespace muss bereits aktiv sein, um diesen Vorgang durchführen zu können. Außerdem muss der Benutzer, der die Befehle ausführt, sowohl im Quellabonnement als auch im Zielabonnement ein Administrator sein.

## <a name="service-bus-troubleshooting"></a>Problembehandlung in Service Bus
[Übersicht über Ausnahmen][Übersicht über Ausnahmen]

### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-messaging-apis-and-their-suggested-actions"></a>Welche Beispiele gibt es für Ausnahmen, die von Azure Service Bus-Messaging-APIs generiert werden, und die vorgeschlagenen Aktionen?
Die Ausnahmen, die von Messaging-APIs generiert werden können, können in die folgenden Kategorien fallen:

* Codierfehler des Benutzers
* Einrichtungs-/Konfigurationsfehler
* Vorübergehende Ausnahmen
* Andere Ausnahmen

Im Artikel [Service Bus-Messagingausnahmen][Übersicht über Ausnahmen] werden einige Ausnahmen mit Vorschlägen für Aktionen beschrieben.

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Was ist eine Shared Access Signature, und welche Sprachen unterstützen die Generierung einer Signatur?
SAS (Shared Access Signatures) sind ein Authentifizierungsmechanismus, der auf sicheren Hashes (SHA-256) oder URIs basiert. Informationen dazu, wie Sie Ihre eigenen Signaturen in Node, PHP, Java und C\# generieren, finden Sie im Artikel [Shared Access Signatures][Shared Access Signatures].

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Service Bus Messaging finden Sie in folgenden Themen.

* [Einführung in Azure Service Bus Premium-Messaging (Blogbeitrag)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Einführung in Azure Service Bus Premium-Messaging (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Übersicht über Service Bus-Messaging](service-bus-messaging-overview.md)
* [Übersicht über die Architektur von Azure Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)

[Bewährte Methoden für Leistungsoptimierungen mithilfe von Service Bus-Brokermessaging]: service-bus-performance-improvements.md
[Bewährte Methoden zum Schützen von Anwendungen vor Service Bus-Ausfällen und Notfällen]: service-bus-outages-disasters.md
[Preisübersicht]: https://azure.microsoft.com/pricing/details/service-bus/
[Übersicht über Kontingente]: service-bus-quotas.md
[hier]: service-bus-powershell-how-to-provision.md#migrate-a-namespace-to-another-azure-subscription
[Übersicht über Ausnahmen]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas-overview.md



<!--HONumber=Nov16_HO3-->


