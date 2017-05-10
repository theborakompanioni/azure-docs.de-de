---
title: Verwenden von Azure API Management mit virtuellen Netzwerken
description: Erfahren Sie, wie Sie eine Verbindung zu einem virtuellen Netzwerk in Azure API Management einrichten, und dadurch auf Webdienste zugreifen.
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.assetid: 64b58f7b-ca22-47dc-89c0-f6bb0af27a48
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 21cdfbbc457aad1cd3b1a5b20745eee4286a78bb
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="how-to-use-azure-api-management-with-virtual-networks"></a>Verwenden von Azure API Management mit virtuellen Netzwerken
Mit Azure Virtual Networks (VNets) können Sie alle Ihre Azure-Ressourcen in einem Netzwerk platzieren, das nicht über das Internet geroutet werden kann, und zu dem Sie den Zugang kontrollieren. Diese Netzwerke können dann mit verschiedenen VPN-Technologien mit Ihren lokalen Netzwerken verbunden werden. Beginnen Sie mit dem folgenden Thema, um weitere Informationen zu Azure Virtual Networks zu erhalten: [Virtuelle Netzwerke](../virtual-network/virtual-networks-overview.md).

Azure API Management kann im virtuellen Netzwerk (VNET) bereitgestellt werden, damit der Dienst auf Back-End-Dienste im Netzwerk zugreifen kann. Das Entwicklerportal und das API-Gateway können so konfiguriert werden, dass darauf entweder über das Internet oder nur vom virtuellen Netzwerk aus zugegriffen werden kann.

> [!NOTE]
> Azure API Management unterstützt sowohl klassische als auch Azure Resource Manager-VNets.
>
>

## <a name="enable-vpn"> </a>Aktivieren der VNet-Verbindung
> [!NOTE]
> VNet-Verbindungen stehen in den Tarifen **Premium** und **Developer** zur Verfügung. Um zwischen diesen Tarifen zu wechseln, öffnen Sie im Azure-Portal Ihren API Management-Dienst und dann die Registerkarte **Skalierung gesamt und Preise**. Wählen Sie im Abschnitt **Tarif** „Premium“ oder „Developer“ aus, und klicken Sie auf „Speichern“.
>

Um die VNet-Konnektivität zu aktivieren, öffnen Sie im Azure-Portal Ihren API Management-Dienst und dann die Seite **Virtuelles Netzwerk**.

![Menü Virtuelles Netzwerk in API Management][api-management-using-vnet-menu]

Wählen Sie den gewünschten Zugriffstyp aus:

* **Extern**: Auf das API Management-Gateway und Entwicklerportal kann vom öffentlichen Internet über einen externen Lastenausgleich zugegriffen werden. Das Gateway kann auf Ressourcen innerhalb des öffentlichen Netzwerks zugreifen.

![Öffentliches Peering][api-management-vnet-public]

* **Intern**: Auf das API Management-Gateway und Entwicklerportal kann nur aus dem virtuellen Netzwerk heraus über einen internen Lastenausgleich zugegriffen werden. Das Gateway kann auf Ressourcen innerhalb des öffentlichen Netzwerks zugreifen.

![Privates Peering][api-management-vnet-private]

Es wird daraufhin eine Liste aller Regionen angezeigt, in denen Ihr API Management-Dienst bereitgestellt wird. Wählen Sie ein VNet und Subnetz für jede Region aus. Die Liste der VNets enthält sowohl klassische als auch virtuelle Ressourcen-Manager-Netzwerke, die in Ihrem Azure-Abonnement für die Region zur Verfügung stehen, die Sie konfigurieren.

> [!NOTE]
> Der **Dienstendpunkt** in der obigen Abbildung schließt Gateway/Proxy, Herausgeberportal, Entwicklerportal, GIT und den direkten Verwaltungsendpunkt ein.
> Der **Verwaltungsendpunkt** in der obigen Abbildung ist der im Dienst gehostete Endpunkt zum Verwalten der Konfiguration über das Azure-Portal und PowerShell.
> Beachten Sie, dass im Diagramm zwar IP-Adressen für die verschiedenen Endpunkte aufgeführt sind, der API Management-Dienst jedoch **ausschließlich** auf die konfigurierten Hostnamen reagiert.

> [!IMPORTANT]
> Beim Bereitstellen einer Azure API Management-Instanz in einem Ressourcen-Manager-VNet muss sich der Dienst in einem dedizierten Subnetz befinden, das außer den Azure API Management-Instanzen keine anderen Ressourcen enthält. Wenn versucht wird, eine Azure API Management-Instanz in einem Subnetz eines Ressourcen-Manager-VNet bereitzustellen, das andere Ressourcen enthält, misslingt die Bereitstellung.
>
>

![VPN auswählen][api-management-setup-vpn-select]

Klicken Sie am oberen Bildschirmrand auf **Speichern**.

> [!NOTE]
> Die VIP-Adresse der API Management-Instanz ändert sich bei jeder Aktivierung oder Deaktivierung des VNet.  
> Die VIP-Adresse ändert sich auch, wenn API Management von **Extern** in **Intern** geändert wird oder umgekehrt.
>


> [!IMPORTANT]
> Wenn Sie den API Management-Dienst aus einem VNET entfernen oder das VNET, in dem er bereitgestellt wird, ändern, kann das zuvor verwendete VNET bis zu 4 Stunden gesperrt bleiben. Während dieses Zeitraums ist es nicht möglich, das VNET zu löschen oder in ihm eine neue Ressource bereitzustellen.

## <a name="enable-vnet-powershell"> </a>Aktivieren der VNet-Verbindung mithilfe von PowerShell-Cmdlets
Sie können VNet-Verbindungen auch mithilfe der PowerShell-Cmdlets aktivieren.

* **Erstellen eines API Management-Diensts in einem VNet**: Verwenden Sie das Cmdlet [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) zum Erstellen eines Azure API Management-Diensts in einem VNet.

* **Bereitstellen eines vorhandenen API Management-Diensts in einem VNet**: Verwenden Sie das Cmdlet [Update-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment), um einen vorhandenen Azure API Management-Dienst innerhalb eines virtuellen Netzwerks zu verschieben.

## <a name="connect-vnet"> </a>Herstellen einer Verbindung mit einem innerhalb eines virtuellen Netzwerk gehosteten Webdienst
Nachdem Ihr API Management-Dienst mit dem VNet verbunden wurde, unterscheidet sich der Zugriff auf Back-Ende-Dienste innerhalb dieses Netzwerks nicht vom Zugriff auf öffentliche Dienste. Geben Sie einfach die lokale IP-Adresse oder den Hostnamen Ihres Webdiensts (falls der DNS-Server für das VNet konfiguriert wurde) im Feld **Webdienst-URL** ein, wenn Sie eine neue API erstellen oder eine vorhandene API bearbeiten.

![API über VPN hinzufügen][api-management-setup-vpn-add-api]

## <a name="network-configuration-issues"> </a>Gängige Probleme mit der Netzwerkkonfiguration
Es folgt eine Liste gängiger Konfigurationsprobleme, die beim Bereitstellen des API Management-Diensts in einem virtuellen Netzwerk auftreten können.

* **Benutzerdefiniertes Setup des DNS-Servers**: Der API Management-Dienst hängt von mehreren Azure-Diensten ab. Wenn API Management in einem VNet mit einem benutzerdefiniertem DNS-Server gehostet wird, muss es die Hostnamen dieser Azure-Dienste auflösen können. Orientieren Sie sich beim benutzerdefinierten DNS-Setup an [diesen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) Anweisungen. Sehen Sie sich zur Bezugnahme die nachstehende Tabelle mit den Ports und anderen Netzwerkanforderungen an.

> [!IMPORTANT]
> Wenn Sie einen benutzerdefinierten DNS-Server für das VNET verwenden, wird empfohlen, diesen **vor** dem Bereitstellen eines API Management-Diensts einzurichten. Andernfalls muss der CloudService, in dem der Dienst gehostet wird, neu gestartet werden, damit die neuen DNS-Servereinstellungen übernommen werden.
>

* **Für API Management erforderliche Ports**: Ein- und ausgehender Datenverkehr im Subnetz, in dem API Management bereitgestellt ist, kann mithilfe einer [Netzwerksicherheitsgruppe][Network Security Group] gesteuert werden. Wenn diese Ports nicht verfügbar sind, funktioniert API Management möglicherweise nicht ordnungsgemäß und kann möglicherweise nicht mehr aufgerufen werden. Eine bestehende Sperre für mindestens einen dieser Ports ist eine weitere gängige Fehlkonfiguration, die beim Verwenden von API Management in einem VNet auftritt.

Beim Hosten einer API Management-Dienstinstanz in einem VNET werden die in der folgenden Tabelle angegebenen Ports verwendet.

| Quell-/Zielport(s) | Richtung | Transportprotokoll | Zweck | Quelle/Ziel | Zugriffstyp |
| --- | --- | --- | --- | --- | --- |
| * / 80, 443 |Eingehend |TCP |Kommunikation zwischen Clients und API Management |INTERNET/VIRTUAL_NETWORK |Extern |
| */3443 |Eingehend |TCP |Verwaltungsendpunkt für Azure-Portal und PowerShell |INTERNET/VIRTUAL_NETWORK |Extern & Intern |
| * / 80, 443 |Ausgehend |TCP |Abhängigkeit von Azure Storage und Azure Service Bus |VIRTUAL_NETWORK/INTERNET |Extern & Intern |
| * / 1433 |Ausgehend |TCP |Abhängigkeit von Azure SQL |VIRTUAL_NETWORK/INTERNET |Extern & Intern |
| * / 11000 - 11999 |Ausgehend |TCP |Abhängigkeit von Azure SQL V12 |VIRTUAL_NETWORK/INTERNET |Extern & Intern |
| * / 14000 - 14999 |Ausgehend |TCP |Abhängigkeit von Azure SQL V12 |VIRTUAL_NETWORK/INTERNET |Extern & Intern |
| * / 9350 - 9354 |Ausgehend |TCP |Abhängigkeit von Service Bus |VIRTUAL_NETWORK/INTERNET |Extern & Intern |
| * / 5671 |Ausgehend |AMQP |Abhängigkeit für Richtlinie zum Anmelden bei Event Hub |VIRTUAL_NETWORK/INTERNET |Extern & Intern |
| 6381 - 6383/6381 - 6383 |Ein- und ausgehend |UDP |Abhängigkeit von Redis-Cache |VIRTUAL_NETWORK/VIRTUAL_NETWORK |Extern & Intern |-
| */445 |Ausgehend |TCP |Abhängigkeit von Azure File Share für GIT |VIRTUAL_NETWORK/INTERNET |Extern & Intern |
| * / * | Eingehend |TCP |Lastenausgleich von Azure-Infrastruktur | AZURE_LOAD_BALANCER/VIRTUAL_NETWORK |Extern & Intern |

* **SSL-Funktionalität**: Zum Aktivieren der Erstellung und Überprüfung von SSL-Zertifikatketten benötigt der API Management-Dienst eine ausgehende Netzwerkverbindung mit ocsp.msocsp.com, mscrl.microsoft.com und crl.microsoft.com.

* **Setup von Express Route**: Eine gängige Kundenkonfiguration sieht das Definieren einer eigenen Standardroute (0.0.0.0/0) vor, die ausgehenden Internetdatenverkehr stattdessen zwingt, die lokale Infrastruktur zu durchlaufen. Bei diesem Datenverkehr funktioniert die Verbindung mit Azure API Management nicht mehr, da ausgehender Datenverkehr entweder lokal blockiert oder mittels NAT in eine nicht mehr nachvollziehbare Gruppe von Adressen übersetzt wird, die nicht mehr mit verschiedenen Azure-Endpunkten funktionieren. Die Lösung besteht darin, mindestens eine benutzerdefinierte Route (User-Defined Route, [UDR][UDRs]) im Subnetz zu definieren, das den Azure API Management-Dienst enthält. Eine benutzerdefinierte Route definiert subnetzspezifische Routen, die anstelle der Standardroute berücksichtigt werden.
  Nach Möglichkeit sollte die folgende Konfiguration verwendet werden:
 * Die ExpressRoute-Konfiguration kündigt 0.0.0.0/0 an und tunnelt standardmäßig allen ausgehenden Datenverkehr zwangsweise lokal.
 * Die für das Subnetz mit dem Azure API Management-Dienst geltende UDR definiert 0.0.0.0/0 mit „Internet“ als Typ des nächsten Hops.
 Gemeinsam führen diese Schritte dazu, dass die benutzerdefinierte Route auf Subnetzebene Vorrang vor erzwungenen ExpressRoute-Tunneln hat. Dadurch wird ausgehender Internetzugriff aus dem Azure API Management-Dienst sichergestellt.

>[!WARNING]  
>Azure API Management wird nicht mit ExpressRoute-Konfigurationen unterstützt, die **fälschlicherweise Routen „über Kreuz“ vom öffentlichen Peeringpfad zum privaten Peeringpfad ankündigen**. ExpressRoute-Konfigurationen, für die öffentliches Peering konfiguriert ist, erhalten Routenankündigungen von Microsoft für zahlreiche Microsoft Azure-IP-Adressbereiche. Wenn diese Adressbereiche fälschlicherweise „über Kreuz“ im privaten Peeringpfad angekündigt werden, führt dies dazu, dass alle ausgehenden Netzwerkpakete aus dem Subnetz des Azure API Management-Diensts fälschlicherweise zwangsweise zur lokalen Netzwerkinfrastruktur eines Kunden getunnelt werden. Dieser Netzwerkdatenfluss unterbricht Azure API Management. Die Lösung für dieses Problem besteht darin, „Über-Kreuz-Ankündigungen“ von Routen vom öffentlichen Peeringpfad zum privaten Peeringpfad zu verhindern.

## <a name="limitations"> </a>Einschränkungen
* Ein Subnetz, das API Management-Instanzen enthält, kann keine anderen Azure-Ressourcentypen enthalten.
* Das Subnetz und der API Management-Dienst müssen sich im selben Abonnement befinden.
* Ein Subnetz, das API Management-Instanzen enthält, kann nicht zwischen Abonnements verschoben werden.
* Bei der Verwendung eines internen virtuellen Netzwerks ist nur eine IP-Adresse aus dem unter [RFC 1918](https://tools.ietf.org/html/rfc1918) angegebenen Bereich verfügbar, es kann keine öffentliche IP-Adresse bereitgestellt werden.
* Bei der API Management-Bereitstellung in mehreren Regionen mit konfigurierten internen virtuellen Netzwerken sind Benutzer dafür verantwortlich, ihren eigenen Lastenausgleich zu verwalten, da sie Besitzer des DNS sind.


## <a name="related-content"> </a>Verwandte Inhalte
* [Verbinden eines virtuellen Netzwerks mit dem Back-End über VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site-ipsecike-vpn-tunnel)
* [Herstellen einer Verbindung mit einem virtuellen Netzwerk in verschiedenen Bereitstellungsmodellen](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Verwenden des API-Inspektors zur Verfolgung von Aufrufen in Azure API Management](api-management-howto-api-inspector.md)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-type.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-private.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-public.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/virtual-networks-nsg.md

