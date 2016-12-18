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
ms.date: 10/20/2016
ms.author: antonba
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7dfbf0e460dc8de13a4a71c1f925c93f6238df5d


---
# <a name="how-to-use-azure-api-management-with-virtual-networks"></a>Verwenden von Azure API Management mit virtuellen Netzwerken
Mit Azure Virtual Networks (VNets) können Sie alle Ihre Azure-Ressourcen in einem Netzwerk platzieren, das nicht über das Internet geroutet werden kann, und zu dem Sie den Zugang kontrollieren. Diese Netzwerke können dann mit verschiedenen VPN-Technologien mit Ihren lokalen Netzwerken verbunden werden. Beginnen Sie mit dem folgenden Thema, um weitere Informationen zu Azure Virtual Networks zu erhalten: [Virtuelle Netzwerke](../virtual-network/virtual-networks-overview.md).

Azure API Management kann mit einem virtuellen Netzwerk (virtual network, VNet) verbunden werden, um auf Back-End-Dienste innerhalb des Netzwerks zugreifen zu können, damit das Entwicklerportal und das API-Gateway innerhalb des Netzwerks zugänglich sind.

## <a name="enable-vpn"> </a>VNet-Verbindung Aktivieren
> Die VNet-Konnektivität steht nur in den Tarifen **Premium** und **Developer** zur Verfügung. Um zwischen diesen Tarifen zu wechseln, öffnen Sie Ihren API Management-Dienst im Azure-Portal, und öffnen Sie dann die Registerkarte **Scale and pricing (Skalieren und Preise)**. Wählen Sie im Abschnitt **Tarif** den Premium-Tarif, und klicken Sie auf „Speichern“.
> 
> 

Um die VNet-Konnektivität zu aktivieren, öffnen Sie Ihren API Management-Dienst im Azure-Portal, und öffnen Sie die Seite **Virtuelles Netzwerk**.

![Menü Virtuelles Netzwerk in API Management][api-management-using-vnet-menu]

Wählen Sie den gewünschten Zugriffstyp aus:

* **Extern**: Auf das API Management-Gateway und Entwicklerportal kann vom öffentlichen Internet über einen externen Lastenausgleich zugegriffen werden. Das Gateway kann auf Ressourcen innerhalb des öffentlichen Netzwerks zugreifen.

![Öffentliches Peering][api-management-vnet-public]

* **Intern**: Auf das API Management-Gateway und Entwicklerportal kann nur aus dem virtuellen Netzwerk heraus über einen internen Lastenausgleich zugegriffen werden. Das Gateway kann auf Ressourcen innerhalb des öffentlichen Netzwerks zugreifen.

![Privates Peering][api-management-vnet-private]

Es wird daraufhin eine Liste aller Regionen angezeigt, in denen Ihr API Management-Dienst bereitgestellt wird. Wählen Sie ein VNet und Subnetz für jede Region aus. Die Liste der VNets wird sowohl mit klassischen als auch virtuellen ARM-Netzwerken aufgefüllt, die in Ihrem Azure-Abonnement für die konfigurierte Region zur Verfügung stehen.

![VPN auswählen][api-management-setup-vpn-select]

Klicken Sie am oberen Bildschirmrand auf **Speichern**. 

> Während der Aktualisierung können Sie keine weiteren Verwaltungsvorgänge für den API Management-Dienst durchführen. Das API Management-Gateway und Entwicklerportal stehen weiterhin zur Verfügung.
> Beachten Sie, dass sich die VIP-Adresse der API Management-Instanz wahrscheinlich bei jeder Aktivierung oder Deaktivierung des VNet ändert.
> 
> 

## <a name="enable-vnet-powershell"> </a>Aktivieren der VNet-Verbindung mithilfe von PowerShell-Cmdlets
Sie können die VNet-Konnektivität auch mithilfe des PowerShell-Cmdlets [Set-AzureRmApiManagementVirtualNetworks](https://msdn.microsoft.com/library/mt619277.aspx) aktivieren.

## <a name="connect-vnet"> </a>Herstellen einer Verbindung mit einem innerhalb eines virtuellen Netzwerk gehosteten Webdienst
Nachdem Ihr API Management-Dienst mit dem VNet verbunden wurde, unterscheidet sich der Zugriff auf Back-Ende-Dienste innerhalb dieses Netzwerks nicht vom Zugriff auf öffentliche Dienste. Geben Sie einfach die lokale IP-Adresse oder den Hostnamen Ihres Webdiensts (falls der DNS-Server für das VNet konfiguriert wurde) im Feld **Webdienst-URL** ein, wenn Sie eine neue API erstellen oder eine vorhandene API bearbeiten.

![API über VPN hinzufügen][api-management-setup-vpn-add-api]

## <a name="custom-dns"> </a>Setup eines benutzerdefinierten DNS-Servers
API Management hängt von einer Reihe von Azure-Diensten ab. Wenn API Management in einem VNet mit einem benutzerdefiniertem DNS-Server gehostet wird, muss es Hostnamen dieser Azure-Dienste auflösen können. Orientieren Sie sich beim benutzerdefinierten DNS-Setup an [diesen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) Anweisungen. Weitere Informationen finden Sie in der folgenden Tabelle der Diagramme und Ports.

## <a name="ports-required"> </a>Für API Management benötigte Ports
> Wenn diese Ports nicht verfügbar sind, funktioniert API Management möglicherweise nicht ordnungsgemäß, und kann möglicherweise nicht mehr aufgerufen werden. Eine bestehende Sperre für mindestens einen dieser Ports ist die häufigste Fehlkonfiguration, die beim Verwenden von API Management in einem VNET auftritt.
> 
> 

Beim Hosten einer API Management-Dienstinstanz in einem VNET werden die in der folgenden Tabelle angegebenen Ports verwendet.

| Port(s) | Richtung | Transportprotokoll | Zweck | Quelle / Ziel | Zugriffstyp |
| --- | --- | --- | --- | --- | --- |
| 80, 443 |Eingehend |TCP |Kommunikation zwischen Clients und API Management |INTERNET/VIRTUAL_NETWORK |Extern |
| 3443 |Eingehend |TCP |Verwaltungsendpunkt |INTERNET/VIRTUAL_NETWORK |Extern & Intern |
| 80, 443 |Ausgehend |TCP |Abhängigkeit von Azure Storage und Azure Service Bus |VIRTUAL_NETWORK/INTERNET |Extern & Intern |
| 1433 |Ausgehend |TCP |Abhängigkeit von Azure SQL |VIRTUAL_NETWORK/INTERNET |Extern & Intern |
| 9350, 9351, 9352, 9353, 9354 |Ausgehend |TCP |Abhängigkeit von Service Bus |VIRTUAL_NETWORK/INTERNET |Extern & Intern |
| 5671 |Ausgehend |AMQP |Abhängigkeit für Richtlinie zum Anmelden bei Event Hub |VIRTUAL_NETWORK/INTERNET |Extern & Intern |
| 6381, 6382, 6383 |Eingehend/ausgehend |UDP |Abhängigkeit von Redis-Cache |VIRTUAL_NETWORK/VIRTUAL_NETWORK |Extern & Intern |
| 445 |Ausgehend |TCP |Abhängigkeit von Azure File Share für GIT |VIRTUAL_NETWORK/INTERNET |Extern & Intern |

## <a name="limitations"> </a>Einschränkungen
* Ein Subnetz, das API Management-Instanzen enthält, kann keine anderen Azure-Ressourcentypen enthalten.
* Das Subnetz und der API Management-Dienst müssen sich im selben Abonnement befinden.
* Ein Subnetz, das API Management-Instanzen enthält, kann nicht zwischen Abonnements verschoben werden.
* Bei der Verwendung eines internen virtuellen Netzwerks ist nur eine IP-Adresse aus dem unter [RFC 1918](https://tools.ietf.org/html/rfc1918) angegebenen Bereich verfügbar, es kann keine öffentliche IP-Adresse bereitgestellt werden.
* Bei der API Management-Bereitstellung in mehreren Regionen mit konfigurierten internen virtuellen Netzwerken sind Benutzer dafür verantwortlich, Ihren eigenen Lastenausgleich zu verwalten, da Sie über DNS verfügen.

## <a name="related-content"> </a>Verwandte Inhalte
* [Erstellen eines virtuellen Netzwerks mit einer Standort-zu-Standort-VPN-Verbindung mit dem Azure-Portal][Erstellen eines virtuellen Netzwerks mit einer Standort-zu-Standort-VPN-Verbindung mit dem Azure-Portal]
* [Verwenden des API-Inspektors zur Verfolgung von Aufrufen in Azure API Management][Verwenden des API-Inspektors zur Verfolgung von Aufrufen in Azure API Management]

[api-management-verwendung-vnet-menü]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-type.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-privat]: ./media/api-management-using-with-vnet/api-management-vnet-private.png
[api-management-vnet-öffentlich]: ./media/api-management-using-with-vnet/api-management-vnet-public.png

[Aktivieren von VPN-Verbindungen]: #enable-vpn
[Herstellen einer Verbindung mit einem Webdienst hinter dem VPN]: #connect-vpn
[Verwandte Inhalte]: #related-content


[Erstellen eines virtuellen Netzwerks mit einer Standort-zu-Standort-VPN-Verbindung mit dem Azure-Portal]: ../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[Verwenden des API-Inspektors zur Verfolgung von Aufrufen in Azure API Management]: api-management-howto-api-inspector.md



<!--HONumber=Nov16_HO3-->


