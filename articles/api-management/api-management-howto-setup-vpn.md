---
title: Einrichten von VPN-Verbindungen in Azure API Management
description: "Erfahren Sie, wie Sie eine VPN-Verbindung in Azure API Management einrichten und über die VPN-Verbindung auf Webdienste zugreifen."
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.assetid: 55a2a1e1-d07e-4111-9ce3-8837ed5040d6
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apipm
translationtype: Human Translation
ms.sourcegitcommit: 30ec6f45da114b6c7bc081f8a2df46f037de61fd
ms.openlocfilehash: 98e88b1c69de8732f1a4ae54c8ecd0b11a547f35


---
# <a name="how-to-setup-vpn-connections-in-azure-api-management"></a>Einrichten von VPN-Verbindungen in Azure API Management
Mit der VPN-Unterstützung von API Management können Sie Ihr API Management-Gateway mit einer Azure Virtual Network-Instanz (klassisch) verbinden. Auf diese Weise können Ihre API Management-Kunden eine sichere Verbindung mit ihren Back-End-Webdiensten herstellen, die lokal vorliegen oder auf die aus anderen Gründen nicht über das öffentliche Internet zugegriffen werden kann.

> [!NOTE]
> Azure API Management arbeitet mit klassischen VNETs. Informationen zum Erstellen eines klassischen VNET finden Sie unter [Erstellen eines virtuellen Netzwerks (klassisch) über das Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Informationen zum Herstellen einer Verbindung von klassischen VNETs mit ARM-VNETS finden Sie unter [Herstellen einer Verbindung zwischen klassischen VNets und neuen VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).
> 
> 

## <a name="enable-vpn"> </a>Aktivieren von VPN-Verbindungen
> Die VPN-Konnektivität steht nur in den Tarifen **Premium** und **Developer** zur Verfügung. Um zu diesem Tarif zu wechseln, öffnen Sie Ihren API Management-Dienst im [klassischen Azure-Portal][Azure Classic Portal] und öffnen dann die Registerkarte **Skalieren**. Wählen Sie im Abschnitt **Allgemein** den Premium-Tarif, und klicken Sie auf "Speichern".
> 
> 

Um die VPN-Konnektivität zu aktivieren, öffnen Sie Ihren API Management-Dienst im [klassischen Azure-Portal][Azure Classic Portal] und wechseln zur Registerkarte **Konfigurieren**. 

Legen Sie im VPN-Abschnitt die Option **VPN-Verbindung** auf **Ein** fest.

![Registerkarte "Konfigurieren" der API Management-Instanz][api-management-setup-vpn-configure]

Es wird daraufhin eine Liste aller Regionen angezeigt, in denen Ihr API Management-Dienst bereitgestellt wird.

Wählen Sie ein VPN und Subnetz für jede Region. Die Liste der VPNs wird basierend auf den virtuellen Netzwerken aufgefüllt, die in Ihrem Azure-Abonnement für die konfigurierte Region zur Verfügung stehen.

![VPN auswählen][api-management-setup-vpn-select]

Klicken Sie unten auf dem Bildschirm auf **Speichern** . Während des Updates können Sie über das klassische Azure-Portal keine weiteren Vorgänge für den API Management-Dienst durchführen. Das Dienstgateway bleibt verfügbar, und Laufzeitaufrufe sollten nicht beeinträchtigt werden.

Beachten Sie, dass sich die VIP-Adresse des Gateways bei jeder Aktivierung oder Deaktivierung des VPN ändert.

## <a name="connect-vpn"> </a>Herstellen einer Verbindung mit einem Webdienst hinter dem VPN
Nachdem Ihr API Management-Dienst mit dem VPN verbunden wurde, unterscheidet sich der Zugriff auf Webdienste im virtuellen Netzwerk nicht vom Zugriff auf öffentliche Dienste. Geben Sie einfach die lokale Adresse oder den Hostnamen Ihres Webdiensts (falls der DNS-Server für Azure Virtual Network konfiguriert wurde) im Feld **Webdienst-URL** ein, wenn Sie eine neue API erstellen oder eine vorhandene API bearbeiten.

![API über VPN hinzufügen][api-management-setup-vpn-add-api]

## <a name="required-ports-for-api-management-vpn-support"></a>Erforderliche Ports für die API Management-VPN-Unterstützung
Beim Hosten einer API Management-Dienstinstanz in einem VNET werden die in der folgenden Tabelle angegebenen Ports verwendet. Wenn diese Ports gesperrt sind, funktioniert der Dienst möglicherweise nicht ordnungsgemäß. Eine bestehende Sperre für mindestens einen dieser Ports ist die häufigste Fehlkonfiguration, die beim Verwenden von API Management in einem VNET auftritt.

| Port(s) | Richtung | Transportprotokoll | Zweck | Quelle/Ziel |
| --- | --- | --- | --- | --- |
| 80, 443 |Eingehend |TCP |Kommunikation zwischen Clients und API Management |INTERNET/VIRTUAL_NETWORK |
| 80, 443 |Ausgehend |TCP |API Management-Abhängigkeit von Azure Storage und Azure Service Bus |VIRTUAL_NETWORK/INTERNET |
| 1433 |Ausgehend |TCP |API Management-Abhängigkeiten von SQL |VIRTUAL_NETWORK/INTERNET |
| 9350, 9351, 9352, 9353, 9354 |Ausgehend |TCP |API Management-Abhängigkeiten von Service Bus |VIRTUAL_NETWORK/INTERNET |
| 5671 |Ausgehend |AMQP |API Management-Abhängigkeit für Richtlinie zum Anmelden bei Event Hub |VIRTUAL_NETWORK/INTERNET |
| 6381, 6382, 6383 |Eingehend/ausgehend |UDP |API Management-Abhängigkeiten von Redis Cache |VIRTUAL_NETWORK/VIRTUAL_NETWORK |
| 445 |Ausgehend |TCP |API Management-Abhängigkeit von Azure File Share für GIT |VIRTUAL_NETWORK/INTERNET |

## <a name="custom-dns"> </a>Setup eines benutzerdefinierten DNS-Servers
API Management hängt von einer Reihe von Azure-Diensten ab. Wenn eine API Management-Dienstinstanz in einem VNET gehostet wird, in dem ein benutzerdefinierter DNS-Server verwendet wird, muss sie Hostnamen dieser Azure-Dienste auflösen können. Orientieren Sie sich beim benutzerdefinierten DNS-Setup an [diesen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) Anweisungen.  

## <a name="related-content"> </a>Verwandte Inhalte
* [Erstellen eines virtuellen Netzwerks mit einer Site-to-Site-VPN-Verbindung mithilfe des klassischen Azure-Portals][Create a virtual network with a site-to-site VPN connection using the Azure Classic Portal]
* [Verwenden des API-Inspektors zur Verfolgung von Aufrufen in Azure API Management][How to use the API Inspector to trace calls in Azure API Management]

[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[Azure Classic Portal]: https://manage.windowsazure.com/

[Create a virtual network with a site-to-site VPN connection using the Azure Classic Portal]: ../vpn-gateway/vpn-gateway-site-to-site-create.md
[How to use the API Inspector to trace calls in Azure API Management]: api-management-howto-api-inspector.md



<!--HONumber=Dec16_HO3-->


