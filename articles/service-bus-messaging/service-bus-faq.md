---
title: "Häufig gestellte Fragen (FAQ) zu Azure Service Bus | Microsoft-Dokumentation"
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
ms.date: 08/07/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: 1403184d96388cb03b2c767c4da342ec1c6fe236
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="service-bus-faq"></a>Service Bus – Häufig gestellte Fragen
In diesem Artikel werden einige häufig gestellte Fragen zu Microsoft Azure Service Bus beantwortet. Sie können auch [Häufig gestellte Fragen zum Azure-Support](http://go.microsoft.com/fwlink/?LinkID=185083) aufrufen, wenn Sie nach allgemeinen Informationen zu Azure-Preisen und zum Support suchen.

## <a name="general-questions-about-azure-service-bus"></a>Allgemeine Fragen zu Azure Service Bus
### <a name="what-is-azure-service-bus"></a>Was ist Azure Service Bus?
[Azure Service Bus](service-bus-messaging-overview.md) ist eine asynchrone Cloudplattform für Messaging, mit der Sie Daten zwischen entkoppelten Systemen senden können. Microsoft stellt dieses Feature als Dienst bereit. Dies bedeutet, dass Sie Ihre Hardware nicht selbst hosten müssen, um sie verwenden zu können.

### <a name="what-is-a-service-bus-namespace"></a>Was ist ein Service Bus-Namespace?
Ein [Namespace](service-bus-create-namespace-portal.md) ist ein Bereichscontainer für die Adressierung von Service Bus-Ressourcen innerhalb Ihrer Anwendung. Die Namespace-Erstellung ist für die Verwendung von Service Bus erforderlich und ist einer der ersten Schritte.

### <a name="what-is-an-azure-service-bus-queue"></a>Was ist eine Azure Service Bus-Warteschlange?
Eine [Service Bus-Warteschlange](service-bus-queues-topics-subscriptions.md) ist eine Entität, in der Nachrichten gespeichert werden. Warteschlangen sind besonders nützlich, wenn Sie über mehrere Anwendungen oder mehrere Teile einer verteilten Anwendung verfügen, die miteinander kommunizieren müssen. Die Warteschlange ähnelt einem Verteilzentrum, in dem mehrere Produkte (Nachrichten) eingehen und von diesem Standort aus dann weiterverschickt werden.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Was sind Azure Service Bus-Themen und -Abonnements?
Eine Thema kann als Warteschlange visualisiert werden, und bei Verwendung mehrerer Abonnements ergibt sich ein umfassenderes Messagingmodell. Im Wesentlichen handelt es sich um ein Tool für die 1:n-Kommunikation. Bei diesem Modell vom Typ „Veröffentlichen/Abonnieren“ (*Pub/Sub*) kann für eine Anwendung, die eine Nachricht an ein Thema mit mehreren Abonnements sendet, erreicht werden, dass die Nachricht von mehreren Anwendungen empfangen wird.

### <a name="what-is-a-partitioned-entity"></a>Was ist eine partitionierte Entität?
Eine herkömmliche Warteschlange oder ein Thema werden von einem einzelnen Nachrichtenbroker verarbeitet und in einem Nachrichtenspeicher gespeichert. Eine [partitionierte Warteschlange bzw. ein Thema](service-bus-partitioning.md) wird mit mehreren Nachrichtenbrokern verarbeitet und in mehreren Nachrichtenspeichern gespeichert. Dies bedeutet, dass der Gesamtdurchsatz einer partitionierten Warteschlange oder eines Themas nicht mehr durch die Leistung eines einzelnen Nachrichtenbrokers oder Nachrichtenspeichers beschränkt wird. Außerdem führt ein vorübergehender Ausfall eines Nachrichtenspeichers nicht dazu, dass eine partitionierte Warteschlange oder ein Thema nicht verfügbar ist.

Beachten Sie, dass die Sortierung bei der Verwendung von partitionierten Entitäten nicht sichergestellt ist. Wenn eine Partition nicht verfügbar ist, ist es trotzdem möglich, Nachrichten zu senden und von anderen Partitionen zu empfangen.

## <a name="best-practices"></a>Bewährte Methoden
### <a name="what-are-some-azure-service-bus-best-practices"></a>Gibt es Beispiele für bewährte Azure Service Bus-Methoden?
* [Bewährte Methoden für Leistungsoptimierungen mithilfe von Service Bus][Best practices for performance improvements using Service Bus]: In diesem Artikel wird beschrieben, wie Sie die Leistung beim Austauschen von Nachrichten optimieren können.

### <a name="what-should-i-know-before-creating-entities"></a>Was muss ich vor der Erstellung von Entitäten wissen?
Die folgenden Eigenschaften einer Warteschlange und eines Themas sind unveränderlich. Berücksichtigen Sie dies beim Bereitstellen Ihrer Entitäten, da eine Änderung nicht möglich ist, ohne eine neue Ersatzentität zu erstellen.

* Größe
* Partitionierung
* Sitzungen
* Duplikaterkennung
* Express-Entität

## <a name="pricing"></a>Preise
In diesem Abschnitt werden einige häufig gestellte Fragen zur Service Bus-Preisstruktur beantwortet.

Der Artikel [Service Bus – Preise und Abrechnung](service-bus-pricing-billing.md) erläutert die Verbrauchseinheiten für die Abrechnung in Service Bus. Informationen zu den Optionen für Service Bus-Preise finden Sie unter [Service Bus – Preisdetails](https://azure.microsoft.com/pricing/details/service-bus/).

Sie können auch [Häufig gestellte Fragen zum Azure-Support](http://go.microsoft.com/fwlink/?LinkID=185083) aufrufen, wenn Sie nach allgemeinen Azure-Preisinformationen suchen. 

### <a name="how-do-you-charge-for-service-bus"></a>Wie werden die Kosten für Service Bus berechnet?
Umfassende Informationen zu Service Bus-Preisen finden Sie unter [Service Bus – Preise][Pricing overview]. Neben den aufgeführten Preisen werden Ihnen die damit verbundenen aus dem Rechenzentrum ausgehenden Datenübertragungen berechnet, mit denen Ihre Anwendung bereitgestellt wird.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>Bei welcher Nutzung von Service Bus werden Daten übertragen? Bei welcher nicht?
Jede Datenübertragung innerhalb der jeweiligen Azure-Region ist ebenso wie jede eingehende Datenübertragung kostenfrei. Für die Datenübertragung außerhalb einer Region gelten Gebühren für ausgehenden Datenverkehr, die [hier](https://azure.microsoft.com/pricing/details/bandwidth/) aufgeführt sind.

### <a name="does-service-bus-charge-for-storage"></a>Fallen für Service Bus Speicherkosten an?
Nein, für Service Bus fallen keine Speicherkosten an. Es gibt allerdings ein Kontingent, das die Höchstmenge an Daten begrenzt, die pro Warteschlange/Thema beibehalten werden kann. Weitere Informationen finden Sie im nächsten Abschnitt.

## <a name="quotas"></a>Kontingente

Eine Liste mit Service Bus-Grenzwerten und -Kontingenten finden Sie unter [Übersicht über Service Bus-Kontingente][Quotas overview].

### <a name="does-service-bus-have-any-usage-quotas"></a>Gibt es für Service Bus Nutzungskontingente?
Microsoft legt für jeden Clouddienst standardmäßig ein aggregiertes monatliches Nutzungskontingent fest, das abonnementübergreifend für einen Kunden berechnet wird. Uns ist bewusst, dass Sie möglicherweise mehr benötigen als diese Grenzwerte. Sie können sich gerne jederzeit an unseren Kundendienst wenden und uns Ihre Anforderungen mitteilen, damit wir die Grenzwerte entsprechend anpassen können. Für Service Bus liegt das aggregierte Nutzungskontingent bei 5 Milliarden Nachrichten pro Monat.

Wir behalten uns das Recht vor, ein Kundenkonto zu deaktivieren, das die Nutzungskontingente in einem Monat überschritten hat. Wir informieren allerdings per E-Mail darüber und unternehmen mehrere Versuche, den Kunden zu kontaktieren, ehe wir diese Maßnahme ergreifen. Kunden, die diese Kontingente überschreiten, haben die Kosten für die Überschreitung zu tragen.

Genau wie andere Dienste in Azure setzt Service Bus eine Reihe spezifischer Kontingente durch, die eine faire Nutzung der Ressourcen sicherstellen. Ausführlichere Informationen zu diesen Kontingenten finden Sie in der [Übersicht über Service Bus-Kontingente][Quotas overview].

## <a name="troubleshooting"></a>Problembehandlung
### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Welche Beispiele gibt es für Ausnahmen, die von Azure Service Bus-APIs generiert werden, und die vorgeschlagenen Aktionen?
Eine Liste der möglichen Service Bus-Ausnahmen finden Sie in der [Übersicht über Ausnahmen][Exceptions overview].

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Was ist eine Shared Access Signature, und welche Sprachen unterstützen die Generierung einer Signatur?
SAS (Shared Access Signatures) sind ein Authentifizierungsmechanismus, der auf sicheren Hashes (SHA-256) oder URIs basiert. Informationen dazu, wie Sie Ihre eigenen Signaturen in Node, PHP, Java und C\# generieren, finden Sie im Artikel [Shared Access Signatures][Shared Access Signatures].

## <a name="subscription-and-namespace-management"></a>Abonnement- und Namespace-Verwaltung
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Wie migriere ich einen Namespace zu einem anderen Azure-Abonnement?

Sie können einen Namespace entweder über das [Azure-Portal](https://portal.azure.com) oder unter Verwendung von PowerShell-Befehlen von einem Azure-Abonnement in ein anderes verschieben. Der Namespace muss bereits aktiv sein, um diesen Vorgang durchführen zu können. Der Benutzer, der die Befehle ausführt, muss sowohl im Quellabonnement als auch im Zielabonnement Administrator sein.

#### <a name="portal"></a>Portal

Wenn Sie Service Bus-Namespaces mithilfe des Azure-Portals auf ein anderes Abonnement übertragen möchten, folgen Sie den [hier](../azure-resource-manager/resource-group-move-resources.md#use-portal) aufgeführten Anweisungen. 

#### <a name="powershell"></a>PowerShell

Mit der folgenden PowerShell-Befehlssequenz wird ein Namespace aus einem Azure-Abonnement in ein anderes verschoben. Um diesen Vorgang auszuführen, muss der Namespace bereits aktiv sein und der Benutzer, der die PowerShell-Befehle ausführt, muss sowohl im Quell- als auch im Zielabonnement Administratorrechte besitzen.

```powershell
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Service Bus finden Sie in den folgenden Themen:

* [Einführung in Azure Service Bus Premium (Blogbeitrag)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Einführung in Azure Service Bus Premium (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Übersicht über Service Bus](service-bus-messaging-overview.md)
* [Übersicht über die Architektur von Azure Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md

