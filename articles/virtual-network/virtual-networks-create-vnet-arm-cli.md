---
title: "Erstellen eines virtuellen Netzwerks über die Azure-Befehlszeilenschnittstelle | Microsoft Docs"
description: Erfahren Sie, wie Sie ein virtuelles Netzwerk mit der Azure-Befehlszeilenschnittstelle erstellen | Resource Manager.
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 75966bcc-0056-4667-8482-6f08ca38e77a
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: ba7a67b8ae57da165f45bd3552a3dfac5f4ef64b
ms.openlocfilehash: 406fd637485799557edbd29fd6223ae535900818


---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Erstellen eines virtuellen Netzwerks über die Azure-Befehlszeilenschnittstelle

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure verfügt über zwei Bereitstellungsmodelle: Azure Resource Manager und klassisch. Microsoft empfiehlt, Ressourcen mit dem Resource Manager-Bereitstellungsmodell zu erstellen. Weitere Informationen zu den Unterschieden zwischen den beiden Modellen finden Sie im Artikel zum Thema [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) (Grundlegendes zu Azure-Bereitstellungsmodellen).
 
In diesem Artikel wird beschrieben, wie Sie mit dem Resource Manager-Bereitstellungsmodell über die Azure-Befehlszeilenschnittstelle ein VNET erstellen. Sie können ein VNet per Resource Manager auch mit anderen Tools erstellen oder dafür das klassische Bereitstellungsmodell verwenden, indem Sie in der folgenden Liste eine andere Option wählen:

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

Führen Sie die folgenden Schritte aus, um mit der Azure-Befehlszeilenschnittstelle ein virtuelles Netzwerk zu erstellen:

1. Installieren und konfigurieren Sie die Azure CLI anhand der Schritte im Artikel [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md).

2. Führen Sie den folgenden Befehl aus, um ein VNET und ein Subnetz zu erstellen:

    ```azurecli
    azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    ```

    Erwartete Ausgabe:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    Verwendete Parameter:

   * **--vnet**. Name des zu erstellenden VNet. In diesem Szenario *TestVNet*
   * **-e (oder --address-space)**. VNet-Adressraum. In diesem Szenario *192.168.0.0*
   * **-i (oder --cidr)**. Netzwerkmaske im CIDR-Format. In diesem Szenario *16*.
   * **-n (oder --subnet-name**). Name des ersten Subnetzes. In diesem Szenario *FrontEnd*.
   * **-p (oder --subnet-start-ip)**. IP-Startadresse für das Subnetz oder Subnetzadressraum. In diesem Szenario *192.168.1.0*.
   * **-r (oder --subnet-cidr)**. Netzwerkmaske im CIDR-Format für das Subnetz. In diesem Szenario *24*.
   * **-l (oder --location)**. Azure-Region, in der das VNet erstellt wird. In diesem Szenario *USA, Mitte*.
3. Führen Sie den folgenden Befehl aus, um ein Subnetz zu erstellen:

    ```azurecli
    azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    ```
   
    Erwartete Ausgabe:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    Verwendete Parameter:

   * **-t (oder --vnet-name)**. Name des VNet, in dem das Subnetz erstellt wird. In diesem Szenario *TestVNet*.
   * **-n (oder --name)**. Name des neuen Subnetzes. In diesem Szenario *BackEnd*.
   * **-a (oder --address-prefix)**. Subnetz-CIDR-Block. In diesem Szenario *192.168.2.0/24*.
4. Führen Sie den folgenden Befehl aus, um die Eigenschaften des neuen VNET anzuzeigen:

    ```azurecli
    azure network vnet show
    ```
   
    Erwartete Ausgabe:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie folgende Verbindungen hergestellt werden:

- Eine Verbindung eines virtuellen Computers (VM) mit einem virtuellen Netzwerk. Lesen Sie hierzu den Artikel [Erstellen eines virtuellen Linux-Computers](../virtual-machines/virtual-machines-linux-quick-create-cli.md). Anstatt gemäß den Schritten in den Artikeln ein VNet und ein Subnetz zu erstellen, können Sie für die Verbindung mit der VM auch ein vorhandenes VNet und Subnetz auswählen.
- Eine Verbindung des virtuellen Netzwerks mit anderen virtuellen Netzwerken. Lesen Sie hierzu den Artikel [Konfigurieren einer VNet-zu-VNet-Verbindung über das Azure-Portal](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
- Eine Verbindung des virtuellen Netzwerks mit einem lokalen Netzwerk. Verwenden Sie hierfür ein Site-to-Site-VPN oder eine ExpressRoute-Verbindung. Informationen zur Vorgehensweise finden Sie in den Artikeln [Add a Site-to-Site connection to a VNet with an existing VPN gateway connection](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) (Hinzufügen einer Site-to-Site-Verbindung zu einem VNet mit einer vorhandenen VPN-Gatewayverbindung) und [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).


<!--HONumber=Feb17_HO2-->


