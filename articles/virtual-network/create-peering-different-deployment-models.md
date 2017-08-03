---
title: Erstellen eines Peerings virtueller Azure-Netzwerke mithilfe verschiedener Bereitstellungsmodelle im selben Abonnement | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Peering zwischen virtuellen Netzwerken erstellen, die mithilfe unterschiedlicher Bereitstellungsmodelle im selben Azure-Abonnement erstellt wurden.
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: jdial;narayan;annahar
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: 7d75d85863ce4b06ef1f552e0d583dec302f7ace
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="create-a-virtual-network-peering---different-deployment-models-same-subscription"></a>Erstellen eines Peerings virtueller Netzwerke mithilfe verschiedener Bereitstellungsmodelle im selben Abonnement 

In diesem Tutorial erfahren Sie, wie Sie ein Peering zwischen virtuellen Netzwerken erstellen, die in verschiedenen Bereitstellungsmodellen eingerichtet wurden. Beide virtuellen Netzwerke müssen zum selben Abonnement gehören. Das Peering zweier virtueller Netzwerke ermöglicht es Ressourcen, in verschiedenen virtuellen Netzwerken untereinander mit derselben Bandbreite und Latenz zu kommunizieren, als befänden sie sich im selben virtuellen Netzwerk. Weitere Informationen hierzu finden Sie unter [Peering virtueller Netzwerke](virtual-network-peering-overview.md). 

Die Schritte zum Erstellen eines Peerings virtueller Netzwerke sind je nachdem unterschiedlich, ob sich die virtuellen Netzwerke im selben oder unterschiedlichen Abonnements befinden und mit welchem [Azure-Bereitstellungsmodell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die virtuellen Netzwerke erstellt werden. Erfahren Sie, wie Sie ein Peering virtueller Netzwerke in anderen Szenarien erstellen, indem Sie in der folgenden Tabelle auf das gewünschte Szenario klicken:

|Azure-Bereitstellungsmodell  | Azure-Abonnement  |
|--------- |---------|
|[Beide mit Resource Manager](virtual-network-create-peering.md) |Identisch|
|[Beide mit Resource Manager](create-peering-different-subscriptions.md) |Unterschiedlich|
|[Eines mit Resource Manager, das andere klassisch](create-peering-different-deployment-models-subscriptions.md) |Unterschiedlich|

Ein Peering zweier virtueller Netzwerke, die über das klassische Bereitstellungsmodell bereitgestellt wurden, ist nicht möglich. Ein Peering kann nur zwischen zwei virtuellen Netzwerken erstellt werden, die sich in derselben Azure-Region befinden. Wenn Sie eine Verbindung zwischen virtuellen Netzwerken herstellen möchten, die beide über das klassische Bereitstellungsmodell erstellt wurden oder sich in verschiedenen Azure-Regionen befinden, können Sie ein [Azure VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nutzen. 

Zum Erstellen eines Peerings virtueller Netzwerke können Sie das [Azure-Portal](#portal), die [Azure CLI](#cli) (Befehlszeilenschnittstelle) oder [Azure PowerShell](#powershell) verwenden. Klicken Sie auf einen der vorherigen Tool-Links. So gelangen Sie direkt zu den Anleitungen zum Erstellen von Peerings in virtuellen Netzwerken mit dem Tool Ihrer Wahl.

## <a name="cli"></a>Erstellen eines Peerings: Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Das Konto, mit dem Sie sich anmelden, muss über die Berechtigungen verfügen, die zum Erstellen eines Peerings virtueller Netzwerke erforderlich sind. Weitere Informationen finden Sie im Abschnitt [Berechtigungen](#permissions) dieses Artikels.
2. Klicken Sie auf **+ Neu**, dann auf **Network**, und klicken Sie zuletzt auf **Virtuelles Netzwerk**.
3. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die Werte für die folgenden Einstellungen an, oder wählen Sie sie aus, und klicken Sie anschließend auf **Erstellen**:
    - **Name**: *myVNet1*
    - **Adressraum**: *10.0.0.0/16*
    - **Subnetzname**: *Standard*
    - **Subnetzadressbereich**: *10.0.0.0/24*
    - **Abonnement**: Wählen Sie Ihr Abonnement aus.
    - *Ressourcengruppe*: Wählen Sie **Neu erstellen** aus, und geben Sie **myResourceGroup** ein.
    - **Standort**: *USA, Osten*
4. Klicken Sie auf **+ NEU**. Geben Sie im Feld **Marketplace durchsuchen** *Virtuelles Netzwerk* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf. 
5. Wählen Sie auf dem Blatt **Virtuelles Netzwerk** im Feld **Bereitstellungsmodell auswählen** die Option **Klassisch** aus, und klicken Sie dann auf **Erstellen**.
6. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die Werte für die folgenden Einstellungen an, oder wählen Sie sie aus, und klicken Sie anschließend auf **Erstellen**:
    - **Name**: *myVnet2*
    - **Adressraum**: *10.1.0.0/16*
    - **Subnetzname**: *Standard*
    - **Subnetzadressbereich**: *10.1.0.0/24*
    - **Abonnement**: Wählen Sie Ihr Abonnement aus.
    - **Ressourcengruppe**: Wählen Sie **Vorhandene verwenden** aus, und wählen Sie *myResourceGroup* aus.
    - **Standort**: *USA, Osten*
7. Geben Sie im oben im Portal im Feld **Ressourcen suchen** *myResourceGroup* ein. Klicken Sie auf **myResourceGroup**, wenn sie in den Suchergebnissen angezeigt wird. Es öffnet sich ein Blatt für die Ressourcengruppe **myResourceGroup**. Die Ressourcengruppe enthält die beiden virtuellen Netzwerke, die in den vorherigen Schritten erstellt wurden.
8. Klicken Sie auf **myVNet1**.
9. Klicken Sie auf dem sich öffnenden Blatt **myVnet1** links in der vertikalen Liste der Optionen auf **Peerings**.
10. Klicken Sie auf dem sich öffnenden Blatt **myVnet1 – Peerings** auf **+ Hinzufügen**
11. Geben Sie auf dem sich öffnenden Blatt **Peering hinzufügen** die folgenden Optionen ein, oder wählen Sie sie aus, und klicken Sie auf **OK**:
     - **Name**: *myVNet1ToMyVnet2*
     - **Bereitstellungsmodell für das virtuelle Netzwerk**: Wählen Sie **Klassisch** aus. 
     - **Abonnement**: Wählen Sie Ihr Abonnement aus.
     - **Virtuelles Netzwerk**: Klicken Sie auf **Virtuelles Netzwerk auswählen**, und klicken Sie dann auf **myVnet2**.
     - **Zugriff auf virtuelles Netzwerk zulassen**: Vergewissern Sie sich, dass **Aktiviert** ausgewählt ist.
    In diesem Tutorial werden keine anderen Einstellungen verwendet. Weitere Informationen zu allen Peeringeinstellungen finden Sie unter [Create a peering (Erstellen eines Peerings)](virtual-network-manage-peering.md#create-a-peering).
12. Nachdem Sie zuvor auf **OK** geklickt haben, wird das Blatt **Peering hinzufügen** geschlossen, und Sie gelangen wieder zum Blatt **myVnet1 – Peerings**. Nach wenigen Sekunden wird das von Ihnen erstellte Peering auf dem Blatt angezeigt. In der Spalte **Peeringstatus** wird für das Peering **myVnet1ToMyVnet2** der Status **Verbunden** angezeigt.

    Das Peering ist nun hergestellt. Alle Azure-Ressourcen, die Sie in einem der virtuellen Netzwerke erstellen, sind in der Lage, miteinander über ihre IP-Adressen zu kommunizieren. Wenn Sie die standardmäßige Azure-Namensauflösung für virtuelle Netzwerke verwenden, können die Ressourcen in den virtuellen Netzwerken Namen nicht netzwerkübergreifend auflösen. Wenn Sie Namen netzwerkübergreifend in einem Peering auflösen möchten, müssen Sie einen eigenen DNS-Server erstellen. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
13. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
14. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-portal) dieses Artikels aus.

## <a name="cli"></a>Erstellen eines Peerings: Azure CLI

1. [Installieren](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Sie die Azure CLI 1.0 zum Erstellen des virtuellen Netzwerks (klassisch).
2. Starten Sie eine Befehlssitzung, und melden Sie sich mit dem Befehl `azure login` bei Azure an.
3. Führen Sie die CLI in Dienstverwaltungsmodus aus, indem Sie den Befehl `azure config mode asm` eingeben.
4. Geben Sie den folgenden Befehl ein, um das virtuelle Netzwerk (klassisch) zu erstellen:
 
    ```azurecli
    azure network vnet create --vnet myVnet2 --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```

5. Erstellen Sie eine Ressourcengruppe und ein virtuelles Netzwerk (Ressourcen-Manager). Sie können entweder die CLI 1.0 oder 2.0 ([installieren](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)) verwenden. In diesem Tutorial wird die CLI 2.0 zum Erstellen des virtuellen Netzwerks (Ressourcen-Manager) verwendet, da die Version 2.0 zum Erstellen des Peerings erforderlich ist. Führen Sie das folgende Bash CLI-Skript auf dem lokalen Computer aus, auf dem die CLI 2.0.4 oder höher installiert ist. Optionen zum Ausführen von Bash CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Sie können das Skript auch mithilfe der Azure Cloud Shell ausführen. Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure CLI ist vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Klicken Sie im folgenden Skript auf die Schaltfläche **Ausprobieren**, um eine Cloud Shell aufzurufen, über die Sie sich bei Ihrem Azure-Konto anmelden können. Um das Skript auszuführen, klicken Sie auf die Schaltfläche **Kopieren**. Fügen Sie den Inhalt in Ihre Cloud Shell ein, drücken Sie dann `Enter`.

    ```azurecli-interactive
    #!/bin/bash

    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus

    # Create the virtual network (Resource Manager).
    az network vnet create \
      --name myVnet1 \
      --resource-group myResourceGroup \
      --location eastus \
      --address-prefix 10.0.0.0/16
    ```

6. Erstellen Sie ein Peering zwischen zwei virtuellen Netzwerken, die mit verschiedenen Bereitstellungsmodellen erstellt wurden. Kopieren Sie das folgende Skript in einen Texteditor auf Ihrem PC. Ersetzen Sie `<subscription id>` durch Ihre Abonnement-ID. Wenn Sie Ihre Abonnement-ID nicht kennen, geben Sie den Befehl `az account show` ein. Der Wert für **id** in der Ausgabe ist Ihre Abonnement-ID. Fügen Sie das geänderte Skript in Ihre CLI-Sitzung ein, und drücken Sie dann `Enter`.

    ```azurecli-interactive
    # Get the id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group myResourceGroup \
      --name myVnet1 \
      --query id --out tsv)

    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --remote-vnet-id /subscriptions/<subscription id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2 \
      --allow-vnet-access
    ```
7. Nachdem das Skript ausgeführt wurde, überprüfen Sie das Peering für das virtuelle Netzwerk (Ressourcen-Manager). Kopieren Sie den folgenden Befehl, fügen Sie ihn in Ihre CLI-Sitzung ein, und drücken Sie dann `Enter`:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    Die Ausgabe zeigt in der Spalte **PeeringState** den Status **Connected**. 

    Alle Azure-Ressourcen, die Sie in einem der virtuellen Netzwerke erstellen, sind in der Lage, miteinander über ihre IP-Adressen zu kommunizieren. Wenn Sie die standardmäßige Azure-Namensauflösung für virtuelle Netzwerke verwenden, können die Ressourcen in den virtuellen Netzwerken Namen nicht netzwerkübergreifend auflösen. Wenn Sie Namen netzwerkübergreifend in einem Peering auflösen möchten, müssen Sie einen eigenen DNS-Server erstellen. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
8. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
9. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-cli) in diesem Artikel aus.

## <a name="powershell"></a>Erstellen eines Peerings: PowerShell

1. Installieren Sie die neueste Version der PowerShell-Module [Azure](https://www.powershellgallery.com/packages/Azure) und [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Sie noch nicht mit Azure PowerShell vertraut sind, lesen Sie die [Übersicht über Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Starten Sie eine PowerShell-Sitzung.
3. Melden Sie sich in PowerShell bei Azure an, indem Sie den Befehl `Add-AzureAccount` eingeben.
4. Um ein virtuelles Netzwerk (klassisch) mit PowerShell zu erstellen, müssen Sie eine neue Netzwerkkonfigurationsdatei erstellen oder eine vorhandene ändern. Erfahren Sie, wie Sie [Netzwerkkonfigurationsdateien exportieren, aktualisieren und importieren](virtual-networks-using-network-configuration-file.md). Die Datei muss das folgende **VirtualNetworkSite**-Element für das in diesem Tutorial verwendete virtuelle Netzwerk enthalten:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
      <AddressSpace>
        <AddressPrefix>10.1.0.0/16</AddressPrefix>
      </AddressSpace>
      <Subnets>
        <Subnet name="default">
          <AddressPrefix>10.1.0.0/24</AddressPrefix>
        </Subnet>
      </Subnets>
    </VirtualNetworkSite>
    ```

    > [!WARNING]
    > Das Importieren einer geänderten Netzwerkkonfigurationsdatei kann zu Änderungen an vorhandenen virtuellen Netzwerken (klassisch) in Ihrem Abonnement führen. Vergewissern Sie sich, dass Sie nur das vorherige virtuelle Netzwerk hinzufügen und keine in Ihrem Abonnement vorhandenen virtuellen Netzwerke ändern oder entfernen. 
5. Melden Sie sich bei Azure an, um durch Eingeben des Befehls `login-azurermaccount` das virtuelle Netzwerk (Ressourcen-Manager) zu erstellen. Das Konto, mit dem Sie sich anmelden, muss über die Berechtigungen verfügen, die zum Erstellen eines Peerings virtueller Netzwerke erforderlich sind. Weitere Informationen finden Sie im Abschnitt [Berechtigungen](#permissions) dieses Artikels.
6. Erstellen Sie eine Ressourcengruppe und ein virtuelles Netzwerk (Ressourcen-Manager). Kopieren Sie das Skript, fügen Sie es in PowerShell ein, und drücken Sie dann `Enter`.

    ```powershell
    # Create a resource group.
      New-AzureRmResourceGroup -Name myResourceGroup -Location eastus

    # Create the virtual network (Resource Manager).
      $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus
    ```

7. Erstellen Sie ein Peering zwischen zwei virtuellen Netzwerken, die mit verschiedenen Bereitstellungsmodellen erstellt wurden. Kopieren Sie das folgende Skript in einen Texteditor auf Ihrem PC. Ersetzen Sie `<subscription id>` durch Ihre Abonnement-ID. Wenn Sie Ihre Abonnement-ID nicht kennen, geben Sie den Befehl `Get-AzureRmSubscription` ein. Der Wert für **Id** in der Ausgabe ist Ihre Abonnement-ID. Um das Skript auszuführen, kopieren Sie das geänderte Skript aus Ihrem Texteditor. Klicken Sie dann in der PowerShell-Sitzung mit der rechten Maustaste, und drücken Sie anschließend `Enter`.

    ```powershell
    # Peer VNet1 to VNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name myVnet1ToMyVnet2 `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId /subscriptions/<subscription Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2
    ```

8. Nachdem das Skript ausgeführt wurde, überprüfen Sie das Peering für das virtuelle Netzwerk (Ressourcen-Manager). Kopieren Sie den folgenden Befehl, fügen Sie ihn in Ihre PowerShell-Sitzung ein, und drücken Sie dann `Enter`:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Die Ausgabe zeigt in der Spalte **PeeringState** den Status **Connected**.

    Alle Azure-Ressourcen, die Sie in einem der virtuellen Netzwerke erstellen, sind in der Lage, miteinander über ihre IP-Adressen zu kommunizieren. Wenn Sie die standardmäßige Azure-Namensauflösung für virtuelle Netzwerke verwenden, können die Ressourcen in den virtuellen Netzwerken Namen nicht netzwerkübergreifend auflösen. Wenn Sie Namen netzwerkübergreifend in einem Peering auflösen möchten, müssen Sie einen eigenen DNS-Server erstellen. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

9. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
10. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-powershell) in diesem Artikel aus.
 
## <a name="permissions"></a>Berechtigungen

Die Konten, mit denen Sie das Peering in virtuellen Netzwerken erstellt haben, müssen die erforderliche Rolle oder die erforderlichen Berechtigungen haben. Wenn Sie beispielsweise ein Peering zwischen den virtuellen Netzwerken myVnet1 und myVnet2 erstellen, muss Ihr Konto mindestens die folgende Rolle oder die folgenden Berechtigungen für jedes virtuelle Netzwerk haben:
    
|Virtuelles Netzwerk|Bereitstellungsmodell|Rolle|Berechtigungen|
|---|---|---|---|
|myVnet1|Ressourcen-Manager|[Mitwirkender von virtuellem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klassisch|[Mitwirkender von klassischem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|–|
|myVnet2|Ressourcen-Manager|[Mitwirkender von virtuellem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klassisch|[Mitwirkender von klassischem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Erfahren Sie mehr über [integrierte Rollen](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) und das Zuweisen bestimmter Berechtigungen zu [benutzerdefinierten Rollen](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (nur für Resource Manager).

## <a name="delete"></a>Löschen von Ressourcen
Wenn Sie dieses Tutorial abgeschlossen haben, möchten Sie die Ressourcen, die Sie in diesem Tutorial erstellt haben, möglicherweise wieder löschen, damit keine Nutzungsgebühren anfallen. Durch das Löschen einer Ressourcengruppe werden auch alle Ressourcen in dieser Ressourcengruppe gelöscht.

### <a name="delete-portal"></a>Azure-Portal

1. Geben Sie in das Suchfeld im Portal **myResourceGroup** ein. Klicken Sie in den Suchergebnissen auf **myResourceGroup**.
2. Klicken Sie auf dem Blatt **myResourceGroup** auf das **Löschsymbol**.
3. Um den Löschvorgang zu bestätigen, geben Sie in das Feld **TYPE THE RESOURCE GROUP NAME** (Ressourcengruppennamen eingeben) **myResourceGroup** ein, und klicken Sie dann auf **Löschen**.

### <a name="delete-cli"></a>Azure-Befehlszeilenschnittstelle

1. Löschen Sie das virtuelle Netzwerk (Ressourcen-Manager) mit der Azure CLI 2.0 und folgendem Befehl:

    ```azurecli-interactive
    az group delete --name myResourceGroup --yes
    ```

2. Löschen Sie das virtuelle Netzwerk (klassisch) mit der Azure CLI 1.0 und folgenden Befehlen:

    ```azurecli
    azure config mode asm

    azure network vnet delete --vnet myVnet2 --quiet
    ```

### <a name="delete-powershell"></a>PowerShell

1. Geben Sie den folgenden Befehl zum Löschen des virtuellen Netzwerks (Ressourcen-Manager) ein:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroup -Force
    ```

2. Um ein virtuelles Netzwerk (klassisch) mit PowerShell zu löschen, müssen Sie eine vorhandene Netzwerkkonfigurationsdatei ändern. Erfahren Sie, wie Sie [Netzwerkkonfigurationsdateien exportieren, aktualisieren und importieren](virtual-networks-using-network-configuration-file.md). Entfernen Sie das folgende „VirtualNetworkSite“-Element für das in diesem Tutorial verwendete virtuelle Netzwerk:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
      <AddressSpace>
        <AddressPrefix>10.1.0.0/16</AddressPrefix>
      </AddressSpace>
      <Subnets>
        <Subnet name="default">
          <AddressPrefix>10.1.0.0/24</AddressPrefix>
        </Subnet>
      </Subnets>
    </VirtualNetworkSite>
    ```

    > [!WARNING]
    > Das Importieren einer geänderten Netzwerkkonfigurationsdatei kann zu Änderungen an vorhandenen virtuellen Netzwerken (klassisch) in Ihrem Abonnement führen. Vergewissern Sie sich, dass Sie nur das vorherige virtuelle Netzwerk entfernen und keine anderen in Ihrem Abonnement vorhandenen virtuellen Netzwerke ändern oder entfernen. 

## <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich eingehend mit den wichtigen [Einschränkungen und Verhalten eines Peerings in virtuellen Netzwerken](virtual-network-manage-peering.md#requirements-and-constraints) vertraut, bevor Sie ein Peering in virtuellen Netzwerken für die Produktion erstellen.
- Erfahren Sie alles über [Peering-Einstellungen in virtuellen Netzwerken](virtual-network-manage-peering.md#create-a-peering).
- Erfahren Sie, wie Sie eine [Hub-Spoke-Netzwerktopologie ](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) mit einem Peering in virtuellen Netzwerken erstellen.

