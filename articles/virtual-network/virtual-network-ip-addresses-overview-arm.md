---
title: IP-Adressen | Microsoft Docs
description: "Enthält Informationen zu öffentlichen und privaten IP-Adressen in Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-resource-manager
ms.assetid: 610b911c-f358-4cfe-ad82-8b61b87c3b7e
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 6e96471c4f61e1ebe15c23f87ac646001d8e30ee
ms.openlocfilehash: 38dc72d7248584006a478413b5da9a6b230e6b89


---
# <a name="ip-addresses-in-azure"></a>IP-Adressen in Azure
Sie können Azure-Ressourcen IP-Adressen zuweisen, um die Kommunikation mit anderen Azure-Ressourcen, Ihrem lokalen Netzwerk und dem Internet zu ermöglichen. In Azure können zwei Arten von IP-Adressen verwendet werden.

* **Öffentliche IP-Adressen**: Sie werden für die Kommunikation mit dem Internet verwendet. Hierzu zählen auch öffentliche Azure-Dienste.
* **Private IP-Adressen**: Sie werden für die Kommunikation innerhalb eines virtuellen Azure-Netzwerks (VNet) und innerhalb Ihres lokalen Netzwerks verwendet, wenn Sie Ihr Netzwerk mithilfe eines VPN-Gateways oder einer ExpressRoute-Verbindung auf Azure ausdehnen.

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../resource-manager-deployment-model.md).  Dieser Artikel befasst sich mit dem Resource Manager-Bereitstellungsmodell, das von Microsoft für die meisten neuen Bereitstellungen anstatt des [klassischen Bereitstellungsmodells](virtual-network-ip-addresses-overview-classic.md) empfohlen wird.
> 

Wenn Sie mit dem klassischen Bereitstellungsmodell vertraut sind, sollten Sie sich die [Unterschiede bei IP-Adressen zwischen dem klassischen Bereitstellungsmodell und dem Resource Manager-Bereitstellungsmodell](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) ansehen.

## <a name="public-ip-addresses"></a>Öffentliche IP-Adressen
Öffentliche IP-Adressen ermöglichen Azure-Ressourcen die Kommunikation mit dem Internet und öffentlichen Azure-Diensten wie [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL-Datenbanken](../sql-database/sql-database-technical-overview.md) und [Azure Storage](../storage/storage-introduction.md).

In Azure-Ressourcen-Manager ist eine [öffentliche IP-Adresse](resource-groups-networking.md#public-ip-address) eine Ressource, die über eigene Eigenschaften verfügt. Sie können eine öffentliche IP-Adressressource einer der folgenden Ressourcen zuordnen:

* Virtuelle Computer (VMs)
* Load Balancer mit Internetzugriff
* VPN-Gateways
* Anwendungsgateways

### <a name="allocation-method"></a>Zuordnungsmethode
Es gibt zwei Methoden zum Zuordnen einer IP-Adresse zu einer *öffentlichen* IP-Ressource: *dynamisch* oder *statisch*. Die Standardmethode für die Zuordnung ist *dynamisch*. Bei diesem Verfahren wird eine IP-Adresse **nicht** zum Zeitpunkt ihrer Erstellung zugewiesen. Stattdessen wird die öffentliche IP-Adresse zugeordnet, wenn Sie die zugeordnete Ressource (z.B. einen virtuellen Computer oder einen Load Balancer) starten oder erstellen. Die IP-Adresse wird freigegeben, wenn Sie die Ressource beenden oder löschen. Dies hat zur Folge, dass die IP-Adresse geändert wird, wenn Sie eine Ressource beenden und starten.

Damit die IP-Adresse für die zugeordnete Ressource unverändert bleibt, können Sie die Zuordnungsmethode explizit auf *statisch*festlegen. In diesem Fall wird sofort eine IP-Adresse zugewiesen. Sie wird nur freigegeben, wenn Sie die Ressource löschen oder deren Zuordnungsmethode in *dynamisch*ändern.

> [!NOTE]
> Auch wenn Sie die Zuordnungsmethode auf *statisch* festlegen, können Sie nicht die tatsächliche IP-Adresse angeben, die der *öffentlichen IP-Ressource* zugewiesen wird. Sie wird stattdessen aus einem Pool der verfügbaren IP-Adressen an dem Azure-Standort zugeordnet, an dem die Ressource erstellt wird.
>

Statische öffentliche IP-Adressen werden häufig in den folgenden Szenarien verwendet:

* Endbenutzer müssen Firewallregeln für die Kommunikation mit Ihren Azure-Ressourcen aktualisieren.
* Bei der DNS-Namensauflösung erfordert eine Änderung einer IP-Adresse eine Aktualisierung von A-Datensätzen.
* Ihre Azure-Ressourcen kommunizieren mit anderen Apps oder Diensten, die ein auf IP-Adressen basierendes Sicherheitsmodell verwenden.
* Sie verwenden SSL-Zertifikate, die mit einer IP-Adresse verknüpft sind.

> [!NOTE]
> Die Liste mit den IP-Adressbereichen, aus denen den Azure-Ressourcen öffentliche IP-Adressen (dynamisch/statisch) zugewiesen werden, finden Sie unter [IP-Bereiche des Azure-Datencenters](https://www.microsoft.com/download/details.aspx?id=41653).
>

### <a name="dns-hostname-resolution"></a>DNS-Hostnamenauflösung
Sie können eine DNS-Domänennamensbezeichnung für eine öffentliche IP-Ressource angeben. Dadurch erstellen Sie für „*Domänennamensbezeichnung*.*Standort*.cloudapp.azure.com“ eine Zuordnung zur öffentlichen IP-Adresse auf den von Azure verwalteten DNS-Servern. Wenn Sie beispielsweise eine öffentliche IP-Ressource mit **contoso** als *Domänennamensbezeichnung* am Azure-*Standort* **USA, Westen** erstellen, wird der vollqualifizierte Domänenname (FQDN) **contoso.westus.cloudapp.azure.com** in die öffentliche IP-Adresse der Ressource aufgelöst. Mit diesem FQDN können Sie einen benutzerdefinierten CNAME-Domäneneintrag mit Verweis auf die öffentliche IP-Adresse in Azure erstellen.

> [!IMPORTANT]
> Jede erstellte Domänennamensbezeichnung muss innerhalb des Azure-Standorts eindeutig sein.  
>

### <a name="virtual-machines"></a>Virtuelle Computer
Sie können eine öffentliche IP-Adresse einem virtuellen [Windows](../virtual-machines/virtual-machines-windows-about.md)- oder [Linux](../virtual-machines/virtual-machines-linux-about.md)-Computer zuordnen, indem Sie sie dessen **Netzwerkschnittstelle** zuweisen. Bei einem virtuellen Computer mit mehreren Netzwerkschnittstellen können Sie sie nur der *primären* Netzwerkschnittstelle zuweisen. Sie können einem virtuellen Computer (VM) eine dynamische oder eine statische öffentliche IP-Adresse zuweisen.

### <a name="internet-facing-load-balancers"></a>Load Balancer mit Internetzugriff
Sie können eine öffentliche IP-Adresse einem [Azure Load Balancer](../load-balancer/load-balancer-overview.md) zuordnen, indem Sie sie der **Front-End**-Konfiguration des Load Balancers zuweisen. Diese öffentliche IP-Adresse fungiert als virtuelle IP-Adresse (VIP) mit Lastenausgleich. Sie können einem Load Balancer-Front-End eine dynamische oder eine statische öffentliche IP-Adresse zuweisen. Sie können einem Load Balancer-Front-End auch mehrere öffentliche IP-Adressen zuweisen. Dadurch werden Szenarien mit [mehreren VIPs](../load-balancer/load-balancer-multivip.md) ermöglicht, z.B. mehrinstanzenfähige Umgebungen mit SSL-basierten Websites.

### <a name="vpn-gateways"></a>VPN-Gateways
[Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) wird eine Verbindung zwischen einem virtuellen Azure-Netzwerk (VNet) und anderen Azure-VNets oder lokalen Netzwerken hergestellt. Sie müssen der **IP-Konfiguration** dieses Gateways eine öffentliche IP-Adresse zuweisen, um eine Kommunikation mit dem Remotenetzwerk zu ermöglichen. Derzeit können Sie einem VPN-Gateway nur eine *dynamische* öffentliche IP-Adresse zuweisen.

### <a name="application-gateways"></a>Anwendungsgateways
Sie können eine öffentliche IP-Adresse einem Azure [Application Gateway](../application-gateway/application-gateway-introduction.md)zuordnen, indem Sie sie der **Front-End** -Konfiguration des Gateways zuweisen. Diese öffentliche IP-Adresse fungiert als VIP (virtuelle IP-Adresse) mit Lastenausgleich. Derzeit können Sie der Front-End-Konfiguration eines Anwendungsgateways nur eine *dynamische* öffentliche IP-Adresse zuweisen.

### <a name="at-a-glance"></a>Auf einen Blick
Die nachstehende Tabelle zeigt die spezifischen Eigenschaften, durch die eine öffentliche IP-Adresse einer Ressource der obersten Ebene zugeordnet sein kann und die möglichen Zuweisungsverfahren (dynamisch oder statisch).

| Ressource der obersten Ebene | Zuordnung der IP-Adresse | dynamisch | statisch |
| --- | --- | --- | --- |
| Virtueller Computer |Netzwerkschnittstelle |Ja |Ja |
| Load Balancer |Front-End-Konfiguration |Ja |Ja |
| VPN Gateway |Gateway-IP-Konfiguration |Ja |Nein |
| Application Gateway |Front-End-Konfiguration |Ja |Nein |

## <a name="private-ip-addresses"></a>Private IP-Adressen
Private IP-Adressen ermöglichen Azure-Ressourcen die Kommunikation mit anderen Ressourcen in einem [virtuellen Netzwerk](virtual-networks-overview.md) oder in einem lokalen Netzwerk über ein VPN-Gateway oder eine ExpressRoute-Verbindung, ohne dass dabei eine über das Internet erreichbare IP-Adresse verwendet wird.

Im Azure Resource Manager-Bereitstellungsmodell wird den folgenden Typen von Azure-Ressourcen eine private IP-Adresse zugeordnet.

* VMs
* Interne·Load Balancer (ILBs)
* Anwendungsgateways

### <a name="allocation-method"></a>Zuordnungsmethode
Eine private IP-Adresse wird aus dem Adressbereich des Subnetzes zugeordnet, dem die Ressource hinzugefügt wurde. Der Adressbereich des Subnetzes ist ein Teil des VNet-Adressbereichs.

Es gibt zwei Methoden zum Zuordnen von privaten IP-Adressen: *dynamisch* oder *statisch*. Die Standardmethode ist die *dynamische* Zuordnung. Dabei wird die IP-Adresse aus dem Subnetz der Ressource per DHCP automatisch zugewiesen. Wenn Sie die Ressource beenden und wieder starten, kann sich diese IP-Adresse ändern.

Sie können die Zuordnungsmethode auf *statisch* festlegen, damit die IP-Adresse unverändert bleibt. In diesem Fall müssen Sie eine gültige IP-Adresse angeben, die zum Subnetz der Ressource gehört.

Statische private IP-Adressen werden häufig für Folgendes verwendet:

* VMs, die als Domänencontroller oder DNS-Server fungieren
* Ressourcen, die Firewallregeln mit IP-Adressen erfordern
* Ressourcen, auf die von anderen Apps oder Ressourcen über eine IP-Adresse zugegriffen wird.

### <a name="virtual-machines"></a>Virtuelle Computer
Eine private IP-Adresse wird der **Netzwerkschnittstelle** eines virtuellen [Windows](../virtual-machines/virtual-machines-windows-about.md)- oder [Linux](../virtual-machines/virtual-machines-linux-about.md)-Computers zugewiesen. Bei einer VM mit mehreren Netzwerkschnittstellen wird jeder Schnittstelle eine private IP-Adresse zugewiesen. Für eine Netzwerkschnittstelle können Sie als Zuordnungsmethode „dynamisch“ oder „statisch“ angeben.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Interne DNS-Hostnamensauflösung (für VMs)
Alle Azure-VMs werden standardmäßig mit [von Azure verwalteten DNS-Servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) konfiguriert, sofern nicht explizit benutzerdefinierte DNS-Server konfiguriert werden. Diese DNS-Server stellen die interne Namensauflösung für VMs im gleichen VNet bereit.

Beim Erstellen einer VM wird den von Azure verwalteten DNS-Servern eine Hostnamenzuordnung für die private IP-Adresse hinzugefügt. Bei einer VM mit mehreren Netzwerkschnittstellen wird der Hostname der privaten IP-Adresse der primären Netzwerkschnittstelle zugeordnet.

VMs, die mit von Azure verwalteten DNS-Servern konfiguriert wurden, können die Hostnamen aller VMs innerhalb ihres VNets in deren private IP-Adressen auflösen.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interner Lastenausgleich (Internal Load Balancer, ILB) und Anwendungsgateways
Sie können der **Front-End**-Konfiguration eines [Azure-ILBs](../load-balancer/load-balancer-internal-overview.md) oder eines [Azure Application Gateways](../application-gateway/application-gateway-introduction.md) eine private IP-Adresse zuweisen. Diese private IP-Adresse fungiert als interner Endpunkt und steht nur den Ressourcen innerhalb des entsprechenden virtuellen Netzwerks (VNet) und der damit verbundenen Remotenetzwerke zur Verfügung. Der Front-End-Konfiguration kann eine dynamische oder eine statische private IP-Adresse zugewiesen werden.

### <a name="at-a-glance"></a>Auf einen Blick
Die nachstehende Tabelle zeigt die spezifischen Eigenschaften, durch die eine private IP-Adresse einer Ressource der obersten Ebene zugeordnet sein kann und die möglichen Zuweisungsverfahren (dynamisch oder statisch).

| Ressource der obersten Ebene | Zuordnung der IP-Adresse | dynamisch | statisch |
| --- | --- | --- | --- |
| Virtueller Computer |Netzwerkschnittstelle |Ja |Ja |
| Load Balancer |Front-End-Konfiguration |Ja |Ja |
| Application Gateway |Front-End-Konfiguration |Ja |Ja |

## <a name="limits"></a>Grenzen
Die Grenzwerte für die IP-Adressierung finden Sie in den vollständigen Informationen zu [Grenzwerten für Netzwerke](../azure-subscription-service-limits.md#networking-limits) in Azure. Diese Grenzwerte werden pro Region und pro Abonnement angegeben. Sie können sich [an den Support wenden](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , um die Standardgrenzwerte Ihren Unternehmensanforderungen entsprechend auf die maximalen Grenzwerte zu erhöhen.

## <a name="pricing"></a>Preise
Für öffentliche IP-Adressen wird unter Umständen eine geringe Gebühr berechnet. Weitere Informationen zu den Preisen für IP-Adressen in Azure finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="next-steps"></a>Nächste Schritte
* [Bereitstellen einer VM mit einer statischen öffentlichen IP-Adresse mithilfe des Azure-Portals](virtual-network-deploy-static-pip-arm-portal.md)
* [Bereitstellen einer VM mit einer statischen öffentlichen IP-Adresse über eine Vorlage](virtual-network-deploy-static-pip-arm-template.md)
* [Bereitstellen eines virtuellen Computers mit einer statischen privaten IP-Adresse mithilfe des Azure-Portals](virtual-networks-static-private-ip-arm-pportal.md)



<!--HONumber=Nov16_HO3-->


