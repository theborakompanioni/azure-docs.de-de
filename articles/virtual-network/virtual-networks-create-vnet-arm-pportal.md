---
title: Erstellen eines virtuellen Netzwerks mit dem Azure-Portal | Microsoft Docs
description: Es wird beschrieben, wie Sie ein virtuelles Netzwerk mit dem Azure-Portal erstellen | Resource Manager
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/8/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 6e96471c4f61e1ebe15c23f87ac646001d8e30ee
ms.openlocfilehash: d68f6a7e935f530630ee33f48cfad1b9e01e66a8


---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Erstellen eines virtuellen Netzwerks im Azure-Portal

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure verfügt über zwei Bereitstellungsmodelle: Azure Resource Manager und klassisch. Microsoft empfiehlt, Ressourcen mit dem Resource Manager-Bereitstellungsmodell zu erstellen. Weitere Informationen zu den Unterschieden zwischen den beiden Modellen finden Sie im Artikel zum Thema [Understand Azure deployment models](../resource-manager-deployment-model.md) (Grundlegendes zu Azure-Bereitstellungsmodellen).
 
In diesem Artikel wird beschrieben, wie Sie mit dem Resource Manager-Bereitstellungsmodell über das Azure-Portal ein VNet erstellen. Sie können ein VNet per Resource Manager auch mit anderen Tools erstellen oder dafür das klassische Bereitstellungsmodell verwenden, indem Sie in der folgenden Liste eine andere Option wählen:

> [!div class="op_single_selector"]
- [Portal](virtual-networks-create-vnet-arm-pportal.md)
- [PowerShell](virtual-networks-create-vnet-arm-ps.md)
- [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-create-vnet-arm-cli.md)
- [Vorlage](virtual-networks-create-vnet-arm-template-click.md)
- [Portal (klassisch)](virtual-networks-create-vnet-classic-pportal.md)
- [PowerShell (klassisch)](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [CLI (klassisch)](virtual-networks-create-vnet-classic-cli.md)


[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Führen Sie die folgenden Schritte aus, um mit dem Azure-Portal ein virtuelles Netzwerk zu erstellen:

1. Navigieren Sie in einem Browser zu http://portal.azure.com, und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2. Klicken Sie auf **Neu** > **Netzwerk** > **Virtuelles Netzwerk**, wie in der folgenden Abbildung gezeigt:

    ![Neues virtuelles Netzwerk](./media/virtual-network-create-vnet-arm-pportal/1.png)

3. Stellen Sie auf dem angezeigten Blatt **Virtuelles Netzwerk** sicher, dass *Resource Manager* ausgewählt ist, und klicken Sie auf **Erstellen**. Dies ist in der folgenden Abbildung dargestellt:

    ![Virtuelles Netzwerk](./media/virtual-network-create-vnet-arm-pportal/2.png)
    
4. Geben Sie auf dem angezeigten Blatt **Virtuelles Netzwerk erstellen** den Text *TestVNet* unter **Name**, *192.168.0.0/16* als **Adressraum**, *FrontEnd* als **Subnetzname**, *192.168.1.0/24* als **Subnetzadressbereich** und *TestRG* als **Ressourcengruppe** ein. Wählen Sie Ihr **Abonnement** und einen **Standort** aus, und klicken Sie wie in der folgenden Abbildung gezeigt auf die Schaltfläche **Erstellen**:

    ![Virtuelles Netzwerk erstellen](./media/virtual-network-create-vnet-arm-pportal/3.png)

    Alternativ dazu können Sie eine vorhandene Ressourcengruppe auswählen. Weitere Informationen zu Ressourcengruppen finden Sie im Artikel [Übersicht über den Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups). Sie können auch einen anderen Standort auswählen. Weitere Informationen zu Azure-Standorten und -Regionen finden Sie im Artikel [Azure-Regionen](https://azure.microsoft.com/regions).

5. Mit dem Portal können Sie beim Erstellen eines VNet nur ein Subnetz erstellen. In diesem Szenario muss nach dem VNet ein zweites Subnetz erstellt werden. Klicken Sie zum Erstellen des zweiten Subnetzes auf **Alle Ressourcen** und dann auf dem Blatt **Alle Ressourcen** auf **TestVNet**. Dies ist in der folgenden Abbildung dargestellt:

    ![Erstelltes VNet](./media/virtual-network-create-vnet-arm-pportal/4.png)

6. Klicken Sie auf dem angezeigten Blatt **TestVNet** auf **Subnetz** und dann auf **+Subnetz**. Geben Sie auf dem Blatt **Subnetz hinzufügen** *BackEnd* als **Name** und *192.168.2.0/24* als **Adressbereich** ein, und klicken Sie dann wie hier gezeigt auf **OK**:

    ![Hinzufügen des Subnetzes](./media/virtual-network-create-vnet-arm-pportal/5.png)

7. Die zwei Subnetze werden wie folgt aufgeführt:
    
    ![Liste der Subnetze im VNet](./media/virtual-network-create-vnet-arm-pportal/6.png)

In diesem Artikel wurde beschrieben, wie Sie zu Testzwecken ein virtuelles Netzwerk mit zwei Subnetzen erstellen. Es ist ratsam, vor dem Erstellen eines virtuellen Netzwerks für die Verwendung in der Produktion die Artikel [Virtuelle Netzwerke im Überblick](virtual-networks-overview.md) und [Planen und Entwerfen von Azure Virtual Networks](virtual-network-vnet-plan-design-arm.md) zu lesen, um sich vollständig mit virtuellen Netzwerken und allen dazugehörigen Einstellungen vertraut zu machen. 

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie folgende Verbindungen hergestellt werden:

- Eine Verbindung eines virtuellen Computers (VM) mit einem virtuellen Netzwerk. Lesen Sie hierzu den Artikel [Erstellen eines virtuellen Windows-Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) oder [Erstellen eines virtuellen Linux-Computers](../virtual-machines/virtual-machines-linux-quick-create-portal.md). Anstatt gemäß den Schritten in den Artikeln ein VNet und ein Subnetz zu erstellen, können Sie für die Verbindung mit der VM auch ein vorhandenes VNet und Subnetz auswählen.
- Eine Verbindung des virtuellen Netzwerks mit anderen virtuellen Netzwerken. Lesen Sie hierzu den Artikel [Konfigurieren einer VNet-zu-VNet-Verbindung über das Azure-Portal](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
- Eine Verbindung des virtuellen Netzwerks mit einem lokalen Netzwerk. Verwenden Sie hierfür ein Site-to-Site-VPN oder eine ExpressRoute-Verbindung. Informationen zur Vorgehensweise finden Sie in den Artikeln [Add a Site-to-Site connection to a VNet with an existing VPN gateway connection](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) (Hinzufügen einer Site-to-Site-Verbindung zu einem VNet mit einer vorhandenen VPN-Gatewayverbindung) und [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).


<!--HONumber=Nov16_HO3-->


