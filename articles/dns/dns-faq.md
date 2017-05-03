---
title: "Häufig gestellte Fragen zu Azure DNS | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu Azure DNS"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: jonatul
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: f365574a12047f6952209dc3883af32a2e9ecd1e
ms.lasthandoff: 04/22/2017

---

# <a name="azure-dns-faq"></a>Häufig gestellte Fragen zu Azure DNS

## <a name="about-azure-dns"></a>Informationen zu Azure DNS

### <a name="what-is-azure-dns"></a>Was ist Azure DNS?

Das Domain Name System (DNS) ist für die Übersetzung (oder Auflösung) eines Website- oder Dienstnamens in die IP-Adresse verantwortlich. Azure DNS ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe der Microsoft Azure-Infrastruktur durchführt. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten.

DNS-Domänen in Azure DNS werden im globalen Azure-Netzwerk von DNS-Servern gehostet. Wir verwenden Anycast-Netzwerke, sodass jede DNS-Abfrage vom nächsten verfügbaren DNS-Server beantwortet wird. Damit wird eine schnelle Leistung und hohe Verfügbarkeit für Ihre Domäne sichergestellt.

Der Azure DNS-Dienst basiert auf Azure Resource Manager. Daher profitiert er von Resource Manager-Funktionen, wie z.B. die rollenbasierte Zugriffskontrolle, Überwachungsprotokolle und Ressourcensperren. Ihren Domänen und Einträge können über das Azure-Portal, Azure PowerShell-Cmdlets und die plattformübergreifende Azure-Befehlszeilenschnittstelle verwaltet werden. Anwendungen, die eine automatische DNS-Verwaltung erfordern, können über die REST-API und SDKs mit dem Dienst zusammenarbeiten.

### <a name="how-much-does-azure-dns-cost"></a>Wie viel kostet Azure DNS?

Das Abrechnungsmodell von Azure DNS basiert auf der Anzahl von DNS-Zonen, die in Azure DNS gehostet werden, und der Anzahl der von ihnen empfangenen DNS-Abfragen. Rabatte werden basierend auf der Nutzung angeboten.

Weitere Informationen finden Sie auf der [Seite mit den Preisen von Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Wie lautet die Vereinbarung zum Servicelevel (SLA) für Azure DNS?

Für gültige DNS-Anforderungen garantieren wir, dass in 99,99 % der Fälle eine Antwort von mindestens einem Azure DNS-Namenserver empfangen wird.

Weitere Informationen finden Sie auf der [Seite mit der SLA für Azure DNS](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>Was ist eine „DNS-Zone“? Ist das dasselbe wie eine DNS-Domäne? 

Eine Domäne ist ein eindeutiger Name im Domain Name System, z.B. „contoso.com“.

Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Beispiel: Die Domäne „contoso.com“ kann eine Reihe von DNS-Einträgen wie „mail.contoso.com“ (für einen E-Mail-Server) und „www.contoso.com“ (für eine Website) enthalten. Diese würden in der DNS-Zone „contoso.com“ gehostet werden.

Ein Domänenname ist *nur ein Name*, während eine DNS-Zone eine Datenressource ist, die die DNS-Einträge für einen Domänennamen enthält. Azure DNS ermöglicht es Ihnen, eine DNS-Zone zu hosten und die DNS-Einträge für eine Domäne in Azure zu verwalten. Darüber hinaus stellt der Dienst DNS-Namenserver bereit, um DNS-Abfragen aus dem Internet zu beantworten.

### <a name="do-i-need-to-purchase-a-dns-domain-name-to-use-azure-dns"></a>Muss ich einen DNS-Domänennamen erwerben, um Azure DNS nutzen zu können? 

Nicht unbedingt.

Sie müssen keine Domäne erwerben, um eine DNS-Zone in Azure DNS zu hosten. Sie können jederzeit eine DNS-Zone erstellen, ohne einen Domänennamen zu besitzen. DNS-Abfragen für diese Zone werden nur aufgelöst, wenn sie an den Azure DNS-Namenserver weitergeleitet werden, der der Zone zugewiesen ist.

Sie müssen einen Domänennamen erwerben, wenn Sie Ihre DNS-Zone in der globalen DNS-Hierarchie verknüpfen möchten. Durch diese Verknüpfung ist Ihre DNS-Zone für DNS-Abfragen auf der ganzen Welt auffindbar, und die Abfragen können mit Ihren DNS-Einträgen beantwortet werden.

## <a name="azure-dns-features"></a>Azure DNS-Features

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Unterstützt Azure DNS die Weiterleitung von Datenverkehr oder Endpunktfailover auf Basis von DNS?

Die Weiterleitung von Datenverkehr und Endpunktfailover auf Basis von DNS werden von Azure Traffic Manager bereitgestellt. Dies ist ein separater Azure-Dienst, der zusammen mit Azure DNS verwendet werden kann. Weitere Informationen finden Sie in der [Traffic Manager-Übersicht](../traffic-manager/traffic-manager-overview.md).

Azure DNS unterstützt nur das Hosten von „statischen“ DNS-Domänen, in denen jede DNS-Abfrage für einen bestimmten DNS-Eintrag immer die gleiche DNS-Antwort erhält.

### <a name="does-azure-dns-support-domain-name-registration"></a>Unterstützt Azure DNS die Registrierung von Domänennamen?

Nein. Azure DNS unterstützt derzeit nicht den Kauf von Domänennamen. Wenn Sie Domänen erwerben möchten, müssen Sie eine von einem Drittanbieter angebotene Registrierungsstelle für Domänennamen verwenden. Die Registrierungsstelle erhebt in der Regel eine geringe jährliche Gebühr. Die Domänen können dann in Azure DNS für die Verwaltung von DNS-Einträgen gehostet werden. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md) .

Dies ist ein Feature, das wir in unserem Backlog nachverfolgen. Sie können auf unserer Feedbackwebsite [Ihre Unterstützung für dieses Feature registrieren](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-private-domains"></a>Unterstützt Azure DNS „private“ Domänen?

Nein. Azure DNS unterstützt derzeit nur Domänen mit Internetzugriff.

Dies ist ein Feature, das wir in unserem Backlog nachverfolgen. Sie können auf unserer Feedbackwebsite [Ihre Unterstützung für dieses Feature registrieren](https://feedback.azure.com/forums/217313-networking/suggestions/10737696-enable-split-dns-for-providing-both-public-and-int).

Informationen zu internen DNS-Optionen in Azure finden Sie unter [Namensauflösung für virtuelle Computer und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="does-azure-dns-support-dnssec"></a>Unterstützt Azure DNS DNSSEC?

Nein. Azure DNS unterstützt derzeit DNSSEC nicht.

Dies ist ein Feature, das wir in unserem Backlog nachverfolgen. Sie können auf unserer Feedbackwebsite [Ihre Unterstützung für dieses Feature registrieren](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Unterstützt Azure DNS Zonenübertragungen (AXFR/IXFR)?

Nein. Azure DNS unterstützt derzeit Zonenübertragungen nicht. DNS-Zonen können [mithilfe der Azure CLI in Azure DNS importiert werden](dns-import-export.md). DNS-Einträge lassen sich dann über das [Azure DNS-Verwaltungsportal](dns-operations-recordsets-portal.md), unsere [REST-API](https://docs.microsoft.com/powershell/module/azurerm.dns), das [SDK](dns-sdk.md), [PowerShell-Cmdlets](dns-operations-recordsets.md) oder das [CLI-Tool](dns-operations-recordsets-cli.md) verwalten.

Dies ist ein Feature, das wir in unserem Backlog nachverfolgen. Sie können auf unserer Feedbackwebsite [Ihre Unterstützung für dieses Feature registrieren](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Unterstützt Azure DNS URL-Umleitungen?

Nein. Dienste für URL-Umleitungen sind eigentlich keine DNS-Dienste – sie arbeiten auf der HTTP-Ebene, statt auf der DNS-Ebene. Bei einigen DNS-Anbietern gehört ein Dienst für URL-Umleitungen zum Gesamtangebot. Dies wird derzeit von Azure DNS nicht unterstützt.

Dieses Feature wird in unseren Backlog nachverfolgt. Sie können auf unserer Feedbackwebsite [Ihre Unterstützung für dieses Feature registrieren](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

## <a name="using-azure-dns"></a>Verwenden von Azure DNS

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Kann ich eine Domäne mit Azure DNS und einem anderen DNS-Anbieter hosten?

Ja. Azure DNS unterstützt das gemeinsame Hosten von Domänen mit anderen DNS-Diensten.

Dazu müssen Sie die NS-Einträge für die Domäne (die steuern, welche Anbieter DNS-Abfragen für die Domäne erhalten) so ändern, dass sie auf die Namenserver für beide Anbieter verweisen. Diese NS-Einträge müssen an drei Stellen geändert werden: in Azure DNS, beim anderen Anbieter und in der übergeordneten Zone (wird in der Regel über die Domänennamen-Registrierungsstelle konfiguriert). Weitere Informationen zur DNS-Delegierung finden Sie unter [Delegieren von Domänen](dns-domain-delegation.md).

Darüber hinaus müssen Sie sicherstellen, dass die DNS-Einträge für die Domäne zwischen beiden DNS-Anbietern synchron sind. Azure DNS unterstützt derzeit DNS-Zonenübertragungen nicht. Sie müssen DNS-Einträge über das [Azure DNS-Verwaltungsportal](dns-operations-recordsets-portal.md), unsere [REST-API](https://docs.microsoft.com/powershell/module/azurerm.dns), das [SDK](dns-sdk.md), [PowerShell-Cmdlets](dns-operations-recordsets.md) oder das [CLI-Tool](dns-operations-recordsets-cli.md) synchronisieren.

### <a name="do-i-have-to-delegate-my-domain-to-all-4-azure-dns-name-servers"></a>Muss ich meine Domäne an alle vier Azure DNS-Namenserver delegieren?

Ja. Azure DNS weist jeder DNS-Zone vier Namenserver zu. Dies dient der Isolierung von Fehlern und einer verbesserten Stabilität. Zur Qualifikation für die Azure DNS-SLA müssen Sie Ihre Domäne an alle vier Namenserver delegieren.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Welche Nutzungsgrenzwerte gelten für Azure DNS?

Bei der Verwendung von Azure DNS gelten folgende Standardgrenzwerte:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Kann ich eine Azure DNS-Zone zwischen Ressourcengruppen oder Abonnements verschieben?

Ja. DNS-Zonen können zwischen Ressourcengruppen oder Abonnements verschoben werden.

Es hat keine Auswirkungen auf DNS-Abfragen, wenn eine DNS-Zone verschoben wird. Die Namenserver, die der Zone zugewiesen sind, bleiben unverändert, und DNS-Abfragen werden als normaler Durchsatz verarbeitet.

Weitere Informationen und Anweisungen zum Verschieben von DNS-Zonen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>Wie lange dauert es, bis DNS-Änderungen wirksam werden?

Neue DNS-Zonen und DNS-Einträge werden in der Regel schnell (innerhalb weniger Sekunden) auf den Azure DNS-Namenservern widergespiegelt.

Änderungen an vorhandenen DNS-Einträgen können etwas länger dauern, sollten aber dennoch innerhalb von 60 Sekunden auf den Azure DNS-Namenservern wiedergegeben werden. Dabei kann die DNS-Zwischenspeicherung außerhalb von Azure DNS (durch DNS-Clients und rekursive DNS-Auflösungsdienste) auch Einfluss auf die Zeit haben, bis eine DNS-Änderung wirksam wird. Diese Dauer der Zwischenspeicherung kann mithilfe der Eigenschaft für die Gültigkeitsdauer (Time To Live, TTL) der einzelnen Ressourceneintragssätze gesteuert werden.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>Wie kann ich meine DNS-Zonen vor versehentlichem Löschen schützen?

Azure DNS wird mit Azure Resource Manager verwaltet und profitiert von Features für die Zugriffssteuerung, die Azure Resource Manager bereitstellt. Mit der rollenbasierten Zugriffssteuerung kann gesteuert werden, welche Benutzer über Lese- oder Schreibzugriff auf DNS-Zonen und Ressourceneintragssätze verfügen. Ressourcensperren können angewendet werden, um eine versehentliche Änderung oder Löschung von DNS-Zonen und Ressourceneintragssätzen zu verhindern.

Weitere Informationen finden Sie unter [Schützen von DNS-Zonen und -Einträgen](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Wie richte ich SPF-Datensätze in Azure DNS ein?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Wie richte ich einen internationalen Domänennamen (IDN) in Azure DNS ein?

Internationale Domänennamen (IDNs) funktionieren durch die Codierung der einzelnen DNS-Namen mit [Punycode](https://en.wikipedia.org/wiki/Punycode). DNS-Abfragen erfolgen mithilfe dieser mit Punycode codierten Namen.

Sie können internationale Domänennamen (IDNs) in Azure DNS konfigurieren, indem Sie zunächst den Zonennamen oder den Namen des Ressourceneintragssatzes in Punycode konvertieren. Azure DNS unterstützt die integrierte Konvertierung in/aus Punycode gegenwärtig nicht.

## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen zu Azure DNS](dns-overview.md)
<br>
[Weitere Informationen zu DNS-Zonen und -Einträgen](dns-zones-records.md)
<br>
[Erste Schritte mit Azure DNS](dns-getstarted-portal.md)


