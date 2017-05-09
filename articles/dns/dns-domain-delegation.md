---
title: "Übersicht über die Azure DNS-Delegierung | Microsoft-Dokumentation"
description: "Grundlegendes zum Ändern der Domänendelegierung und zum Verwenden von DNS-Namenserver zum Hosten von Domänen."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: ab6dd0e1e5975770bec741ed4a06b6eb4745e174
ms.contentlocale: de-de
ms.lasthandoff: 04/20/2017

---

# <a name="delegation-of-dns-zones-with-azure-dns"></a>Delegieren von DNS-Zonen mit Azure DNS

Azure DNS ermöglicht es Ihnen, eine DNS-Zone zu hosten und die DNS-Einträge für eine Domäne in Azure zu verwalten. Damit DNS-Abfragen für eine Domäne Azure DNS erreichen, muss die Domäne aus der übergeordneten Domäne an Azure DNS delegiert werden. Denken Sie daran, dass Azure DNS keine Domänenregistrierungsstelle ist. In diesem Artikel wird die Funktionsweise der Domänendelegierung und die Domänendelegierung an Azure DNS beschrieben.

## <a name="how-dns-delegation-works"></a>Funktionsweise der DNS-Delegierung

### <a name="domains-and-zones"></a>Domänen und Zonen

Das Domain Name System ist eine Hierarchie von Domänen. Die Hierarchie beginnt mit der Stammdomäne, deren Name einfach „**.**“ lautet.  Darunter befinden sich Domänen der obersten Ebene, z.B. „com“, „net“, „org“, „uk“ oder „jp“.  Unter diesen Domänen der obersten Ebene befinden sich die Domänen der zweiten Ebene (beispielsweise „org.uk“ oder „co.jp“).  Und so weiter. Die Domänen in der DNS-Hierarchie werden mithilfe von separaten DNS-Zonen gehostet. Diese Zonen sind global verteilt und werden von DNS-Namenservern in der ganzen Welt gehostet.

**DNS-Zone:** Eine Domäne ist ein eindeutiger Name im Domain Name System (beispielsweise „contoso.com“). Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Beispiel: Die Domäne „contoso.com“ kann eine Reihe von DNS-Einträgen wie „mail.contoso.com“ (für einen E-Mail-Server) und „www.contoso.com“ (für eine Website) enthalten.

**Domänenregistrierungsstelle:** Eine Domänenregistrierungsstelle ist ein Unternehmen, das Internetdomänennamen bereitstellen kann. Das Unternehmen überprüft, ob die Internetdomäne, die Sie verwenden möchten, noch verfügbar ist, und ermöglicht es Ihnen, sie zu erwerben. Sobald der Domänenname registriert wurde, sind Sie der rechtmäßige Besitzer des Domänennamens. Wenn Sie bereits über eine Internetdomäne verfügen, verwenden Sie für die Delegierung an Azure DNS die aktuelle Domänenregistrierungsstelle.

Weitere Informationen dazu, wer der Besitzer eines bestimmten Domänennamens ist, oder Informationen zum Kauf einer Domäne finden Sie im Thema zur [Verwaltung von Internetdomänen in Azure AD](https://msdn.microsoft.com/library/azure/hh969248.aspx).

### <a name="resolution-and-delegation"></a>Auflösung und Delegierung

Es gibt zwei Arten von DNS-Server:

* Ein *autoritativer* DNS-Server hostet DNS-Zonen. Er antwortet nur auf DNS-Abfragen nach Einträgen für diese Zonen.
* Ein *rekursiver* DNS-Server hostet keine DNS-Zonen. Er reagiert auf alle DNS-Abfragen durch Aufrufen der autoritativen DNS-Server, um die benötigten Daten zu erfassen.

Azure DNS stellt einen autoritativen DNS-Dienst bereit.  Ein rekursiver DNS-Dienst wird nicht bereitgestellt. Cloud Services und virtuelle Computer in Azure werden automatisch für die Verwendung eines rekursiven DNS-Diensts konfiguriert, der separat als Teil der Infrastruktur von Azure bereitgestellt wird. Informationen zum Ändern dieser DNS-Einstellungen finden Sie unter [Namensauflösung mithilfe eines eigenen DNS-Servers](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

DNS-Clients in PCs und mobilen Geräten rufen in der Regel einen rekursiven DNS-Server auf, um DNS-Abfragen auszuführen, die die Clientanwendungen benötigen.

Wenn ein rekursiver DNS-Server eine Abfrage für einen DNS-Eintrag erhält, z.B. „www.contoso.com“, muss er zuerst nach dem Namenserver suchen, der die Zone für die Domäne „contoso.com“ hostet. Um den Namensserver zu finden, beginnt er mit den Stammnamenservern und sucht von dort aus die Namenserver, die die Zone „com“ hosten. Anschließend fragt er die com-Namenserver ab, um den Namenserver zu suchen, der die Zone „contoso.com“ hostet.  Anschließend kann er diese Namenserver nach „www.contoso.com“ abfragen.

Dieses Verfahren wird als Auflösen des DNS-Namens bezeichnet. Streng genommen umfasst die DNS-Auflösung zusätzliche Schritte wie das Verfolgen von CNAMEs, aber für die Funktionsweise der DNS-Delegierung ist dies nicht relevant.

Wie verweist eine übergeordnete Zone auf die Namenserver für eine untergeordnete Zone? Hierfür wird eine besondere Art von DNS-Eintrag verwendet: ein so genannter NS-Eintrag. (NS steht hier für Namenserver.) Die Stammzone enthält beispielsweise NS-Einträge für „com“ und zeigt die Namenserver für die Zone „com“ an. Die Zone „com“ enthält im Gegenzug NS-Einträge für „contoso.com“ und zeigt die Namenserver für die Zone „contoso.com“ an. Das Einrichten der NS-Einträge für eine untergeordnete Zone in einer übergeordneten Zone wird Delegieren der Domäne genannt.

Die folgende Abbildung zeigt eine DNS-Beispielabfrage. „contoso.net“ und „partners.contoso.net“ sind Azure DNS-Zonen.

![Dns-nameserver](./media/dns-domain-delegation/image1.png)

1. Der Client fordert `www.partners.contoso.net` von seinem lokalen DNS-Server an.
1. Da der lokale DNS-Server nicht über den Eintrag verfügt, sendet er eine Anforderung an seinen Stammnamenserver.
1. Der Stammnamenserver verfügt zwar nicht über den Eintrag, kennt aber die Adresse des Namenservers für `.net` und gibt sie an den DNS-Server weiter.
1. Der DNS-Server sendet die Anforderung an den Namenserver für `.net`. Dieser verfügt zwar nicht über den Eintrag, kennt jedoch die Adresse des Namenservers für „contoso.net“. In diesem Fall handelt es sich um eine in Azure DNS gehostete DNS-Zone.
1. Die Zone `contoso.net` verfügt nicht über den Eintrag, kennt aber den Namenserver für `partners.contoso.net` und gibt diesen zurück. In diesem Fall handelt es sich um eine in Azure DNS gehostete DNS-Zone.
1. Der DNS-Server fordert die IP-Adresse von `partners.contoso.net` aus der Zone `partners.contoso.net` an. Sie enthält den A-Eintrag und antwortet mit der IP-Adresse.
1. Der DNS-Server gibt die IP-Adresse an den Client weiter.
1. Der Client stellt eine Verbindung mit der Website `www.partners.contoso.net` her.

Jede Delegierung umfasst eigentlich zwei Kopien der NS-Einträge: eine in der übergeordneten Zone, die auf die untergeordnete Zone verweist, und eine in der untergeordneten Zone selbst. Die Zone „contoso.net“ enthält die NS-Einträge für „contoso.net“ (neben den NS-Einträgen in „net“). Diese Einträge werden als autoritative NS-Einträge bezeichnet und befinden sich an der Spitze der untergeordneten Zone.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Ihre Domäne an Azure DNS delegieren](dns-delegate-domain-azure-dns.md).


