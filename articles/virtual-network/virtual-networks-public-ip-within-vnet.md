<properties 
   pageTitle="Verwenden von öffentlichen IP-Adressen in einem virtuellen Netzwerk"
   description="Hier erfahren Sie, wie Sie ein virtuelles Netzwerk für die Verwendung öffentlicher IP-Adressen konfigurieren."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# Öffentlicher IP-Adressraum in einem virtuellen Netzwerk (VNet)

Virtuelle Netzwerke (VNets) können öffentliche und private (RFC 1918-Adressblöcke) IP-Adressräume enthalten. Wenn Sie einen öffentlichen IP-Adressbereich hinzufügen, wird dieser als Teil des privaten VNet-IP-Adressraums behandelt, der nur innerhalb des VNets, aus miteinander verbundenen VNets und von Ihrem lokalen Standort aus erreichbar ist.

Die folgende Abbildung zeigt ein VNet, das öffentliche und private IP-Adressräume enthält.

![Öffentliche IP-Adresse – Konzepte](./media/virtual-networks-public-ip-within-vnet/IC775683.jpg)

## Wie füge ich einen öffentlichen IP-Adressbereich hinzu?

Sie fügen einen öffentlichen IP-Adressbereich genau wie einen privaten IP-Adressbereich hinzu: entweder mithilfe einer *netcfg*-Datei oder durch die Konfiguration im [Azure-Portal](http://portal.azure.com). Sie können einen öffentlichen IP-Adressbereich beim Erstellen des VNets oder zu einem späteren Zeitpunkt hinzufügen. Das folgende Beispiel zeigt den öffentlichen und den privaten IP-Adressraum im gleichen VNet.

![Öffentliche IP-Adresse im Portal](./media/virtual-networks-public-ip-within-vnet/IC775684.png)

## Gibt es Einschränkungen?

Es gibt einige IP-Adressbereiche, die nicht zulässig sind:

- 224\.0.0.0/4 (Multicast)

- 255\.255.255.255/32 (Übertragung)

- 127\.0.0.0/8 (Loopback)

- 169\.254.0.0/16 (Verbindungslokal)

- 168\.63.129.16/32 (Internes DNS)

## Nächste Schritte

[Verwalten von DNS-Servern, die von einem virtuellen Netzwerk (VNet) verwendet werden](virtual-networks-manage-dns-in-vnet.md)

<!---HONumber=AcomDC_0810_2016-->