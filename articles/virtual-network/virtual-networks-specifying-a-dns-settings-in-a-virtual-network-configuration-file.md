---
title: "Angeben von DNS-Einstellungen in der Konfigurationsdatei für ein virtuelles Netzwerk | Microsoft Docs"
description: "Gewusst wie: Ändern der DNS-Servereinstellungen in einem virtuellen Netzwerk über die Konfigurationsdatei des virtuellen Netzwerks im klassischen Bereitstellungsmodell"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a8905927-92ac-42b5-8c33-8e42c000692c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ec33268915a1888509834ce6a5b2bc782a12ce4a


---
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>Angeben von DNS-Einstellungen in der Konfigurationsdatei eines virtuellen Netzwerks
Eine Netzwerk-Konfigurationsdatei besteht aus zwei Elementen, mit denen Sie DNS-Einstellungen (Domain Name System) angeben können: **DnsServers** und **DnsServerRef**. Sie können eine Liste von DNS-Servern hinzufügen, indem Sie ihre IP-Adressen und Referenznamen für das **DnsServers** -Element angeben. Anschließend können Sie mit dem **DnsServerRef** -Element angeben, welche DNS-Servereinträge aus dem DnsServers-Element für verschiedene Netzwerkstandorte innerhalb des virtuellen Netzwerks verwendet werden.

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dieser Artikel gilt für das klassische Bereitstellungsmodell.

Die Netzwerkkonfigurationsdatei kann die folgenden Elemente enthalten. Der Titel der einzelnen Elemente ist mit einer Seite verknüpft, die zusätzliche Informationen zu den Einstellungen der Werte für das Element enthält.

> [!IMPORTANT]
> Weitere Informationen zum Arbeiten mit der Netzwerkkonfigurationsdatei finden Sie unter [Konfigurieren eines virtuellen Netzwerks mithilfe einer Netzwerkkonfigurationsdatei](virtual-networks-using-network-configuration-file.md). Informationen zu den in einer Netzwerkkonfigurationsdatei enthaltenen Elementen finden Sie unter [Konfigurationsschema für virtuelle Azure-Netzwerke](https://msdn.microsoft.com/library/azure/jj157100.aspx).
> 
> 

[Dns-Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> Das **name**-Attribut im **DnsServer**-Element wird nur als Verweis für das **DnsServerRef**-Element verwendet. Es stellt nicht den Hostnamen für den DNS-Server dar. Jeder **DnsServer** -Attributwert muss im gesamten Microsoft Azure-Abonnement eindeutig sein.
> 
> 

[VirtualNetworkSites-Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> Um diese Einstellung für das VirtualNetworkSites-Element anzugeben, muss dieses zuvor im DNS-Element definiert werden. *name* für DnsServerRef im VirtualNetworkSites-Element muss auf einen Namenswert verweisen, der im DNS-Element für DnsServer *name* angegeben wurde.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum [Konfigurationsschema für Azure Virtual Network](http://go.microsoft.com/fwlink/?LinkId=248093)
* Informationen zu [Azure-Dienstkonfigurationsschemas](https://msdn.microsoft.com/library/windowsazure/ee758710)
* [Konfigurieren eines virtuellen Netzwerks mithilfe einer Netzwerkkonfigurationsdatei](virtual-networks-using-network-configuration-file.md)




<!--HONumber=Nov16_HO3-->


