---
title: Erstellen eines (klassischen) virtuellen Azure-Netzwerks mit mehreren Subnetzen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein (klassisches) virtuelles Netzwerk mit mehreren Subnetzen in Azure erstellen.
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: jdial
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 95c2f4fe40590a8d809f634fb5b2c92d07421bb0
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="create-a-virtual-network-classic-with-multiple-subnets"></a>Erstellen eines (klassischen) virtuellen Netzwerks mit mehreren Subnetzen

> [!IMPORTANT]
> Azure bietet zwei [verschiedene Bereitstellungsmodelle](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) für das Erstellen und Verwenden von Ressourcen: Resource Manager-Bereitstellung und klassische Bereitstellung. Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen virtuellen Netzwerke die Erstellung mit dem [Resource Manager](virtual-networks-create-vnet-arm-pportal.md)-Bereitstellungsmodell.

In diesem Tutorial erfahren Sie, wie Sie ein einfaches (klassisches) virtuelles Azure-Netzwerk mit separaten öffentlichen und privaten Subnetzen erstellen. Sie können Azure-Ressourcen wie virtuelle Computer und Clouddienste in einem Subnetz erstellen. In (klassischen) virtuellen Netzwerken erstellte Ressourcen können miteinander und mit Ressourcen in anderen Netzwerken, die mit einem virtuellen Netzwerk verbunden sind, kommunizieren.

Erfahren Sie mehr über alle Einstellungen für [virtuelle Netzwerke](virtual-network-manage-network.md) und [Subnetze](virtual-network-manage-subnet.md).

> [!WARNING]
> (Klassische) virtuelle Netzwerke werden von Azure sofort gelöscht, wenn ein [Abonnement deaktiviert wird](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit). Das Löschen (klassischer) virtueller Netzwerke erfolgt unabhängig davon, ob Ressourcen im Netzwerk vorhanden sind. Wenn Sie das Abonnement später wieder aktivieren, müssen sämtliche Ressourcen, die im virtuellen Netzwerk vorhanden waren, neu erstellt werden.

Sie können ein (klassisches) virtuelles Netzwerk im [Azure-Portal](#portal), über die [Azure CLI 1.0](#azure-cli) (Befehlszeilenschnittstelle) oder mit [PowerShell](#powershell) erstellen.

## <a name="portal"></a>Portal

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) in einem Internetbrowser. Melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p)registrieren.
2. Klicken Sie im Portal auf **+Neu**.
3. Geben Sie auf dem Blatt **Neu**, das geöffnet wird, oben im Feld **Marketplace durchsuchen** den Suchbegriff *Virtuelles Netzwerk* ein.  Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
4. Wählen Sie auf dem Blatt **Virtuelles Netzwerk** im Feld **Bereitstellungsmodell auswählen** die Option **Klassisch** aus, und klicken Sie dann auf **Erstellen**. 
5. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen (klassisch)**  die folgenden Werte ein, und klicken Sie dann auf **Erstellen**:

    |Einstellung|Wert|
    |---|---|
    |Name|myVnet|
    |Adressraum|10.0.0.0/16|
    |Subnetzname|Öffentlich|
    |Subnetzadressbereich|10.0.0.0/24|
    |Ressourcengruppe|Lassen Sie **Neu erstellen** ausgewählt, und geben Sie **myResourceGroup** ein.|
    |Abonnement und Standort|Wählen Sie Ihr Abonnement und Ihren Standort aus.

    Wenn Sie noch nicht mit Azure vertraut sind, informieren Sie sich über [Ressourcengruppen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [Abonnements](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) und [Standorte](https://azure.microsoft.com/regions) (die auch als *Regionen* bezeichnet werden).
4. Im Portal können Sie beim Erstellen eines virtuellen Netzwerks nur ein Subnetz erstellen. In diesem Tutorial erstellen Sie nach dem virtuellen Netzwerk ein zweites Subnetz. Sie können später über das Internet zugängliche Ressourcen im Subnetz **Öffentlich** erstellen. Sie können im Subnetz **Privat** auch andere Ressourcen erstellen, auf die nicht über das Internet zugegriffen werden kann. Geben Sie zum Erstellen des zweiten Subnetzes **MyVnet** in das Feld **Ressourcen suchen** oben im Portal ein. Klicken Sie auf **myVnet**, wenn es in den Suchergebnissen angezeigt wird.
5. Klicken Sie auf dem Blatt **Virtuelles Netzwerk erstellen (klassisch)**, das angezeigt wird, auf **Subnetze** (im Abschnitt **EINSTELLUNGEN**).
6. Klicken Sie auf dem Blatt **myVnet – Subnetze**, das angezeigt wird, auf **+Hinzufügen**.
7. Geben Sie auf dem Blatt **Subnetz hinzufügen** unter **Name** den Namen **Privat** ein. Geben Sie unter **Adressbereich** den Wert **10.0.1.0/24** ein.  Klicken Sie auf **OK**.
8. Auf dem Blatt **myVnet – Subnetze** werden die von Ihnen erstellten Subnetze **Öffentlich** und **Privat** angezeigt.
9. **Optional**: Wenn Sie dieses Tutorial abgeschlossen haben, können Sie die Ressourcen, die Sie erstellt haben, wieder löschen, damit keine Nutzungsgebühren anfallen:
    - Klicken Sie auf dem Blatt **myVnet** auf **Overview**.
    - Klicken Sie auf dem Blatt **myVnet** auf das Symbol **Löschen**.
    - Klicken Sie im Feld **Virtuelles Netzwerk löschen** auf **Ja**, um das Löschen zu bestätigen.

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

1. Sie können entweder [die Azure CLI installieren und konfigurieren](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder die Befehlszeilenschnittstelle in der Azure Cloud Shell verwenden. Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure CLI ist vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `azure <command> --help`. 
2. Melden Sie sich in einer CLI-Sitzung mit folgendem Befehl bei Azure an. Wenn Sie im unten stehenden Feld auf **Jetzt testen** klicken, wird eine Cloud Shell-Instanz geöffnet. Sie können sich bei Ihrem Azure-Abonnement anmelden, ohne den folgenden Befehl einzugeben:

    ```azurecli-interactive
    azure login
    ```

3. Um sicherzustellen, dass sich die CLI im Dienstverwaltungsmodus befindet, geben Sie den folgenden Befehl ein:

    ```azurecli-interactive
    azure config mode asm
    ```

4. Erstellen Sie ein virtuelles Netzwerk mit einem privaten Subnetz:

    ```azurecli-interactive
    azure network vnet create --vnet myVnet --address-space 10.0.0.0 --cidr 16  --subnet-name Private --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --location "East US"
    ```

5. Erstellen Sie ein öffentliches Subnetz im virtuellen Netzwerk:

    ```azurecli-interactive
    azure network vnet subnet create --name Public --vnet-name myVnet --address-prefix 10.0.1.0/24
    ```    
    
6. Überprüfen Sie das virtuelle Netzwerk und die Subnetze:

    ```azurecli-interactive
    azure network vnet show --vnet myVnet
    ```

7. **Optional**: Wenn Sie dieses Tutorial abgeschlossen haben, können Sie die Ressourcen, die Sie erstellt haben, wieder löschen, damit keine Nutzungsgebühren anfallen:

    ```azurecli-interactive
    azure network vnet delete --vnet myVnet --quiet
    ```

> [!NOTE]
> Wenn Sie die Befehlszeilenschnittstelle verwenden, können Sie keine Ressourcengruppen angeben, in der ein (klassisches) virtuelles Netzwerk erstellt werden soll. Azure erstellt das virtuelle Netzwerk in einer Ressourcengruppe namens *Default-Networking*.

## <a name="powershell"></a>PowerShell

1. Installieren Sie die neueste Version des PowerShell-Moduls [Azure](https://www.powershellgallery.com/packages/Azure). Wenn Sie noch nicht mit Azure PowerShell vertraut sind, lesen Sie die [Übersicht über Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Starten Sie eine PowerShell-Sitzung.
3. Melden Sie sich in PowerShell bei Azure an, indem Sie den Befehl `Add-AzureAccount` eingeben.
4. Ändern Sie den folgenden Pfad und Dateinamen in geeignete Angaben, und exportieren Sie dann Ihre vorhandene Netzwerkkonfigurationsdatei:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
    ```

5. Um ein virtuelles Netzwerk mit öffentlichen und privaten Subnetzen zu erstellen, verwenden Sie einen beliebigen Text-Editor, und fügen Sie der Netzwerkkonfigurationsdatei das folgende **VirtualNetworkSite**-Element hinzu.

    ```xml
    <VirtualNetworkSite name="myVnet" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Private">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Public">
            <AddressPrefix>10.0.1.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    ```

    Überprüfen Sie das vollständige [Schema der Netzwerkkonfigurationsdatei](https://msdn.microsoft.com/library/azure/jj157100.aspx).

6. Importieren Sie die Netzwerkkonfigurationsdatei:

    ```powershell
    Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
    ```

    > [!WARNING]
    > Das Importieren einer geänderten Netzwerkkonfigurationsdatei kann zu Änderungen an vorhandenen virtuellen Netzwerken (klassisch) in Ihrem Abonnement führen. Vergewissern Sie sich, dass Sie nur das vorherige virtuelle Netzwerk hinzufügen und keine in Ihrem Abonnement vorhandenen virtuellen Netzwerke ändern oder entfernen. 

7. Überprüfen Sie das virtuelle Netzwerk und die Subnetze:

    ```powershell
    Get-AzureVNetSite -VNetName "myVnet"
    ```

8. **Optional**: Wenn Sie dieses Tutorial abgeschlossen haben, können Sie die Ressourcen, die Sie erstellt haben, wieder löschen, damit keine Nutzungsgebühren anfallen. Um das virtuelle Netzwerk zu löschen, führen Sie die Schritte 4-6 erneut durch, und entfernen Sie diesmal das **VirtualNetworkSite**-Element, das Sie in Schritt 5 hinzugefügt haben.
 
> [!NOTE]
> Wenn Sie PowerShell verwenden, können Sie keine Ressourcengruppen angeben, in der ein (klassisches) virtuelles Netzwerk erstellt werden soll. Azure erstellt das virtuelle Netzwerk in einer Ressourcengruppe namens *Default-Networking*.

---

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu allen Einstellungen für virtuelle Netzwerke und Subnetze finden Sie unter [Verwalten virtueller Netzwerke](virtual-network-manage-network.md) und [Verwalten von Subnetzen virtueller Netzwerke](virtual-network-manage-subnet.md). Für unterschiedliche Anforderungen gibt es verschiedene Optionen für die Verwendung virtueller Netzwerke und Subnetze in einer Produktionsumgebung.
- Erstellen Sie [Netzwerksicherheitsgruppen](virtual-networks-nsg.md) für Subnetze, und wenden Sie sie an, um eingehenden und ausgehenden Subnetzdatenverkehr zu filtern.
- Erstellen Sie einen virtuellen [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- oder [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Computer, und verbinden Sie ihn mit einem virtuellen Netzwerk.
- Zum Verbinden von zwei virtuellen Netzwerken im gleichen Azure-Standort erstellen Sie ein [Peering zwischen diesen virtuellen Netzwerken](create-peering-different-deployment-models.md). Sie können ein Peering zwischen einem virtuellen Netzwerk (Resource Manager) und einem (klassischen) virtuellen Netzwerk erstellen, nicht jedoch zwischen zwei (klassischen) virtuellen Netzwerken.
- Verbinden Sie das virtuelle Netzwerk mit einem lokalen Netzwerk über ein [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder eine [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Verbindung.

