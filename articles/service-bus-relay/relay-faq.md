---
title: "Relay – häufig gestellte Fragen| Microsoft Docs"
description: "Enthält Antworten auf einige häufig gestellte Fragen zu Azure Relay."
services: service-bus-relay
documentationcenter: na
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 886d2c7f-838f-4938-bd23-466662fb1c8e
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2016
ms.author: jotaub,sethm
translationtype: Human Translation
ms.sourcegitcommit: 849d7995e9c74bc929c4f791ae9155ca18ddb77b
ms.openlocfilehash: 12702f585605a88ed6e753a0ff301095e8629e05


---
# <a name="relay-faq"></a>Relay – Häufig gestellte Fragen
In diesem Artikel werden einige häufig gestellte Fragen zu Microsoft Azure Relay beantwortet. Sie können auch [Häufig gestellte Fragen zum Azure-Support](http://go.microsoft.com/fwlink/?LinkID=185083) aufrufen, wenn Sie nach allgemeinen Informationen zu Azure-Preisen und zum Support suchen.

## <a name="general-questions"></a>Allgemeine Fragen
### <a name="what-is-azure-relay"></a>Was ist Azure Relay?
Der Azure-[Relaydienst](relay-what-is-it.md) unterstützt Ihre Hybridanwendungen, indem er Ihnen die Möglichkeit bietet, Dienste in einem Unternehmensnetzwerk sicher in der öffentlichen Cloud verfügbar zu machen, ohne dass eine Firewallverbindung geöffnet werden muss oder tiefgreifende Änderungen an der unternehmensinternen Netzwerkinfrastruktur erforderlich werden.

### <a name="what-is-a-relay-namespace"></a>Was ist ein Relay-Namespace?
Ein [Namespace](relay-create-namespace-portal.md) ist ein Bereichscontainer für die Adressierung von Relay-Ressourcen innerhalb Ihrer Anwendung. Die Namespace-Erstellung ist für die Verwendung von Relay erforderlich und ist einer der ersten Schritte.

### <a name="what-happened-to-the-previously-named-relay-service"></a>Was ist mit dem vorherigen Relaydienst passiert?
Der früher **Relay** genannte Dienst heißt jetzt *WCF-Relay*. Sie können den Dienst weiter wie gewohnt verwenden. Hybridverbindungen sind eine aktualisierte Version eines Diensts, der von BizTalk übernommen wurde. Sowohl WCF-Relay als auch Hybridverbindungen werden in Zukunft weiterhin unterstützt.

## <a name="pricing"></a>Preise
In diesem Abschnitt werden einige häufig gestellte Fragen zur Relay-Preisstruktur beantwortet. Sie können auch die [Azure-Support-FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) aufrufen, wenn Sie nach allgemeinen Microsoft Azure-Preisinformationen suchen. Vollständige Informationen zu Relay-Preisen finden Sie unter [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="how-do-you-charge-for-hybrid-connections-and-wcf-relay"></a>Wie werden Hybridverbindungen und WCF-Relay abgerechnet?
Umfassende Informationen zu Relay-Preisen finden Sie unter [Service Bus Preise][Preisübersicht]. Neben den aufgeführten Preisen werden Ihnen die damit verbundenen aus dem Rechenzentrum ausgehenden Datenübertragungen berechnet, mit denen Ihre Anwendung bereitgestellt wird.

### <a name="how-am-i-billed-for-hybrid-connections"></a>Wie werden Hybridverbindungen abgerechnet?
Im Folgenden finden Sie drei Beispielszenarien:

1. Wenn Sie einen einzelnen Listener installiert haben, z.B. eine Instanz des Hybridverbindungs-Managers, der den ganzen Monat kontinuierlich ausgeführt wird, und Sie im Laufe des Monats 3 GB Daten über diese Verbindung übertragen, belaufen sich Ihre Gesamtkosten auf 5 USD.
2. Wenn Sie einen einzelnen Listener installiert haben, z.B. eine Instanz des Hybridverbindungs-Managers, der den ganzen Monat kontinuierlich ausgeführt wird, und Sie im Laufe des Monats 10 GB Daten über diese Verbindung übertragen, belaufen sich Ihre Gesamtkosten auf 7,50 USD: 5 USD für die Verbindungen und die ersten 5 GB plus 2,50 USD für das zusätzliche Datenvolumen von 5 GB.
3. Wenn Sie zwei Instanzen A und B des Hybridverbindungs-Managers installiert haben, die den gesamten Monat kontinuierlich ausgeführt werden, und Sie 3 GB Daten über Verbindung A und 6 GB über Verbindung B übertragen, belaufen sich Ihre Gesamtkosten auf 10,50 USD: 5 USD für Verbindung A plus 5 USD für Verbindung B plus 0,50 USD (für 1 GB über das enthaltene Volumen von 5 GB bei Verbindung B hinaus).

Beachten Sie, dass die Preise in diesen Beispielen nur während der Vorschauphase gelten und sich bei allgemeiner Verfügbarkeit von Hybridverbindungen ändern können.

### <a name="how-are-wcf-relay-hours-calculated"></a>Wie werden WCF-Relaystunden berechnet?
Relaystunden werden für den kumulativen Zeitraum in Rechnung gestellt, in dem jedes Service Bus-Relay „geöffnet“ ist. Ein Relay wird an einer bestimmten Service Bus-Adresse (Dienstnamespace-URL) implizit instanziiert und geöffnet, sobald ein relayfähiger WCF-Dienst oder „Relaylistener“ zum ersten Mal eine Verbindung mit dieser Adresse herstellt. Es wird erst geschlossen, wenn der letzte Listener die Verbindung mit dieser Adresse trennt. Zu Abrechnungszwecken wird ein Relay daher ab der Verbindungsherstellung durch den ersten Relaylistener bis zur Trennung der Verbindung mit der Service Bus-Adresse des Relays durch den letzten Relaylistener als „geöffnet“ betrachtet.

### <a name="what-if-i-have-more-than-one-listener-connected-to-a-given-relay"></a>Was geschieht, wenn mehrere Listener mit einem bestimmten Relay verbunden sind?
In einigen Fällen können mit einem einzelnen Relay mehrere Listener verbunden sein. Ein Relay wird als „geöffnet“ betrachtet, wenn mindestens ein Relaylistener mit ihm verbunden ist. Das Hinzufügen von zusätzlichen Listenern zu einem offenen Relay führt zu zusätzlichen Relaystunden. Die Anzahl von Relaysendern (Clients, die Nachrichten von Relays aufrufen oder an diese senden), die mit dem Relay verbunden sind, hat ebenfalls keine Auswirkung auf die Berechnung der Relaystunden.

### <a name="how-is-the-messages-meter-calculated-for-wcf-relays"></a>Wie wird die Verbrauchseinheit „Nachrichten“ für WCF-Relays berechnet?
**Dies gilt nur für WCF-Relays, Hybridverbindungen werden nicht verrechnet**

Im Allgemeinen werden abrechenbare Nachrichten für Relays mit derselben Methode berechnet, die oben für Brokerentitäten (Warteschlangen, Themen und Abonnements) beschrieben wird. Es gibt jedoch einige wichtige Ausnahmen:

Das Senden einer Nachricht an ein Service Bus Relay wird als „ganz durchgehendes“ Senden an den Relaylistener behandelt, der die Nachricht erhält, und nicht als ein Senden an das Service Bus Relay, gefolgt von einer Zustellung an den Relaylistener. Aus diesem Grund führt ein Dienstaufruf im Anforderungs-/Antwortstil (von bis zu 64 KB) für einen Relaylistener zu zwei abrechenbaren Nachrichten: Eine Nachricht ist für die Anforderung und eine Nachricht für die Antwort abrechenbar (unter der Voraussetzung, dass die Antwort ebenfalls \<= 64 KB ist). Dies unterscheidet sich vom Einsatz einer Warteschlange für die Vermittlung zwischen einem Client und einem Dienst. In letzterem Fall würde dasselbe Anforderungs-/Antwort-Muster erfordern, dass eine Anfrage an die Warteschlange gesandt wird, gefolgt von einem Entfernen aus der Warteschlange/einer Zustellung an den Dienst, wiederum gefolgt von einer Antwortsendung an eine andere Warteschlange und einem Entfernen aus der Warteschlange/einer Zustellung an den Client. Mit denselben Annahmen für die Größe (\<= 64 KB) würde dieses vermittelte Warteschlangenmuster darum zu vier abrechenbaren Nachrichten führen, also der doppelten Menge im Vergleich zur Anwendung desselben Musters mithilfe eines Relays. Natürlich gibt es Vorteile beim Einsatz von Warteschlangen, wenn man dieses Muster verwenden möchte, wie etwa Dauerhaftigkeit und Lastenausgleich. Diese Vorteile können die zusätzlichen Kosten rechtfertigen.

Relays, die anhand der WCF-Bindung „netTCPRelay“ geöffnet werden, behandeln Nachrichten nicht als einzelne Nachrichten, sondern als Datenstrom, der durch das System fließt. Anders ausgedrückt: Nur der Absender und der Listener besitzen Einblick in das Framing der einzelnen Nachrichten, die mit dieser Bindung gesendet/empfangen werden. Zum Zweck der Berechnung abrechenbarer Nachrichten werden deshalb für Relays, die die netTCPRelay-Bindung verwenden, alle Daten als ein Datenstrom behandelt. In diesem Fall berechnet Service Bus die Gesamtmenge der über die einzelnen Relays gesendeten oder empfangen Daten auf 5-Minuten-Basis und teilt diese Menge durch 64 KB, um so die Anzahl abrechenbarer Nachrichten für das fragliche Relay während dieses Zeitraums zu ermitteln.

## <a name="quotas"></a>Kontingente
| Namen des Kontingents | Umfang | Typ | Verhalten beim Überschreiten | Wert |
| --- | --- | --- | --- | --- |
| Gleichzeitige Listener für ein Relay |Entität |Statisch |Nachfolgende Anforderungen für zusätzliche Verbindungen werden abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen. |25 |
| Gleichzeitige Relay-Listener |Systemweit |Statisch |Nachfolgende Anforderungen für zusätzliche Verbindungen werden abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen. |2.000 |
| Gleichzeitige Relayverbindungen für alle Relayendpunkte eines Dienstnamespace |Systemweit |Statisch |- |5.000 |
| Relayendpunkte pro Dienstnamespace |Systemweit |Statisch |- |10.000 |
| Nachrichtengröße für [NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx)- und [NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx)-Relays |Systemweit |Statisch |Eingehende Nachrichten, die diese Kontingente überschreiten, werden abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen. |64 KB |
| Nachrichtengröße für [HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx)- und [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx)-Relays |Systemweit |Statisch |- |Unbegrenzt |

### <a name="does-relay-have-any-usage-quotas"></a>Verfügt Relay über Verwendungskontingente?
Microsoft legt für jeden Clouddienst standardmäßig ein aggregiertes monatliches Nutzungskontingent fest, das abonnementübergreifend für einen Kunden berechnet wird. Uns ist bewusst, dass Sie möglicherweise mehr benötigen als diese Grenzwerte. Sie können sich gerne jederzeit an unseren Kundendienst wenden und uns Ihre Anforderungen mitteilen, damit wir die Grenzwerte entsprechend anpassen können. Für Service Bus gelten die folgenden aggregierten Nutzungskontingente:

* 5 Milliarden Nachrichten
* 2 Millionen Relaystunden

Wir behalten uns das Recht vor, ein Kundenkonto zu deaktivieren, das die Nutzungskontingente in einem Monat überschritten hat. Wir informieren allerdings per E-Mail darüber und unternehmen mehrere Versuche, den Kunden zu kontaktieren, ehe wir diese Maßnahme ergreifen. Kunden, die diese Kontingente überschreiten, haben die Kosten für die Überschreitung zu tragen.

### <a name="naming-restrictions"></a>Benennungseinschränkungen
Die Namen für einen Relay-Namespace können nur zwischen 6 und 50 Zeichen lang sein.

## <a name="subscription-and-namespace-management"></a>Abonnement- und Namespace-Verwaltung
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Wie migriere ich einen Namespace zu einem anderen Azure-Abonnement?
Sie können PowerShell-Befehle verwenden (siehe [diesen Artikel](../service-bus-messaging/service-bus-powershell-how-to-provision.md#migrate-a-namespace-to-another-azure-subscription)), um einen Namespace von einem Azure-Abonnement in ein anderes zu verschieben. Der Namespace muss bereits aktiv sein, um diesen Vorgang durchführen zu können. Außerdem muss der Benutzer, der die Befehle ausführt, sowohl im Quellabonnement als auch im Zielabonnement Administrator sein.

## <a name="troubleshooting"></a>Problembehandlung
### <a name="what-are-some-of-the-exceptions-generated-by-azure-relay-apis-and-their-suggested-actions"></a>Welche Beispiele gibt es für Ausnahmen, die von Azure Relay-APIs generiert werden, und die vorgeschlagenen Aktionen?
Im Artikel [Relay exceptions (Relay-Ausnahmen)][Relay exceptions (Relay-Ausnahmen)] (Relay-Ausnahmen) werden einige Ausnahmen mit Vorschlägen für Aktionen beschrieben.

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Was ist eine Shared Access Signature, und welche Sprachen unterstützen die Generierung einer Signatur?
SAS (Shared Access Signatures) sind ein Authentifizierungsmechanismus, der auf sicheren Hashes (SHA-256) oder URIs basiert. Informationen dazu, wie Sie Ihre eigenen Signaturen in Node, PHP, Java und C\# generieren, finden Sie im Artikel [Shared Access Signatures][Shared Access Signatures].

[Preisübersicht]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions (Relay-Ausnahmen)]: relay-exceptions.md
[Shared Access Signatures]: ../service-bus-messaging/service-bus-sas-overview.md

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines Namespaces](relay-create-namespace-portal.md)
* [Erste Schritte mit .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Erste Schritte mit Node](relay-hybrid-connections-node-get-started.md)



<!--HONumber=Nov16_HO3-->


