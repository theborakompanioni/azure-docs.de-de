---
title: Herstellen einer Verbindung zwischen Azure Data Lake Store und VNets | Microsoft-Dokumentation
description: Herstellen einer Verbindung zwischen Azure Data Lake Store und VNets
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 683fcfdc-cf93-46c3-b2d2-5cb79f5e9ea5
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9bd7a76ef644df3db72ce95a3ef89a04bec1381f
ms.contentlocale: de-de
ms.lasthandoff: 11/17/2016

---
# <a name="access-azure-data-lake-store-from-vms-within-an-azure-vnet"></a>Zugreifen auf Azure Data Lake Store von virtuellen Computern in einem Azure-VNet
Bei Azure Data Lake Store handelt es sich um einen PaaS-Dienst, der unter öffentlichen Internet-IP-Adressen ausgeführt wird. Jeder Server, der eine Verbindung mit dem öffentlichen Internet herstellen kann, kann in der Regel auch eine Verbindung mit Azure Data Lake Store-Endpunkten herstellen. Standardmäßig können alle virtuellen Computer in Azure-VNets auf das Internet und damit auf Azure Data Lake Store zugreifen. Allerdings ist es möglich, virtuelle Computer in einem VNet so zu konfigurieren, dass sie nicht auf das Internet zugreifen können. Bei diesen virtuellen Computern ist auch der Zugriff auf Azure Data Lake Store eingeschränkt. Der Zugriff auf das öffentliche Internet kann für virtuelle Computer in Azure-VNets mit einer der folgenden Methoden blockiert werden:

* Konfigurieren von Netzwerksicherheitsgruppen (NSG)
* Konfigurieren von benutzerdefinierten Routen (User Defined Routes, UDR)
* Austauschen von Routen, die den Zugriff auf das Internet blockieren, über BGP (Branchenstandardprotokoll für das dynamische Routing) bei Verwendung von ExpressRoute

In diesem Artikel erfahren Sie, wie Sie den Zugriff auf Azure Data Lake Store von virtuellen Azure-Computern aktivieren, deren Zugriff auf Ressourcen mit einer der oben aufgeführten Methoden eingeschränkt wurde.

## <a name="enabling-connectivity-to-azure-data-lake-store-from-vms-with-restricted-connectivity"></a>Aktivieren von Verbindungen mit Azure Data Lake Store von virtuellen Computern mit eingeschränkter Konnektivität
Um von diesen virtuellen Computern auf Azure Data Lake Store zugreifen zu können, müssen Sie sie für den Zugriff auf die IP-Adresse konfigurieren, unter der das Azure Data Lake Store-Konto verfügbar ist. Sie können die IP-Adressen für Ihre Data Lake Store-Konten ermitteln, indem Sie die DNS-Namen der Konten auflösen (`<account>.azuredatalakestore.net`). Dafür können Sie Tools wie **nslookup** verwenden. Öffnen Sie eine Eingabeaufforderung auf dem Computer, und führen Sie den folgenden Befehl aus:

    nslookup mydatastore.azuredatalakestore.net

Die Ausgabe sieht ungefähr wie folgt aus. Der Wert der **Address**-Eigenschaft ist die dem Data Lake Store-Konto zugeordnete IP-Adresse.

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a>Aktivieren der Konnektivität von virtuellen Computern, deren Konnektivität mithilfe von Netzwerksicherheitsgruppen eingeschränkt wurde
Wenn eine NSG-Regel zum Blockieren des Zugriffs auf das Internet verwendet wird, können Sie eine weitere NSG erstellen, die den Zugriff auf die Data Lake Store-IP-Adresse zulässt. Weitere Informationen zu NSG-Regeln finden Sie unter [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md). Anweisungen zum Erstellen von Netzwerksicherheitsgruppen finden Sie unter [How to manage NSGs using the Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) (Verwalten von Netzwerksicherheitsgruppen mit dem Azure-Portal).

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a>Ermöglichen der Verbindungsherstellung von virtuellen Computern, deren Konnektivität mithilfe von UDR oder ExpressRoute eingeschränkt wurde
Wenn der Internetzugriff mithilfe von benutzerdefinierten Routen oder über BGP ausgetauschten Routen blockiert wird, muss eine spezielle Route konfiguriert werden, damit virtuelle Computer in diesen Subnetzen auf Data Lake Store-Endpunkte zugreifen können. Weitere Informationen finden Sie unter [Was sind benutzerdefinierte Routen und IP-Weiterleitung?](../virtual-network/virtual-networks-udr-overview.md). Anleitungen zum Erstellen von benutzerdefinierten Routen finden Sie unter [Erstellen von benutzerdefinierten Routen (UDR) im Resource Manager mit PowerShell](../virtual-network/virtual-network-create-udr-arm-ps.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a>Ermöglichen der Verbindungsherstellung von virtuellen Computern, deren Konnektivität mithilfe von ExpressRoute eingeschränkt wurde
Wenn eine ExpressRoute-Verbindung konfiguriert wird, können die lokalen Server über öffentliches Peering auf Data Lake Store zugreifen. Weitere Informationen zum Konfigurieren von ExpressRoute für öffentliches Peering stehen unter [ExpressRoute – FAQ](../expressroute/expressroute-faqs.md) zur Verfügung.

## <a name="see-also"></a>Weitere Informationen
* [Übersicht über Azure Data Lake-Speicher](data-lake-store-overview.md)
* [Sicherheit in Azure Data Lake Store](data-lake-store-security-overview.md)


