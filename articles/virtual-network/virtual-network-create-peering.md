---
title: "Erstellen eines Peerings virtueller Azure-Netzwerke gemäß dem Resource Manager-Modell in demselben Abonnement | Microsoft-Dokumentation"
description: In diesem Artikel erfahren Sie, wie Sie ein Peering zwischen virtuellen Netzwerken erstellen, die mithilfe des Resource Manager in demselben Azure-Abonnement erstellt wurden.
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 026bca75-2946-4c03-b4f6-9f3c5809c69a
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: jdial;narayan;annahar
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: a32a6b33e04c603325ab3612f61e5852682eac7d
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="create-a-virtual-network-peering---resource-manager-same-subscription"></a>Erstellen eines Peerings virtueller Netzwerke gemäß dem Resource Manager-Modell in demselben Abonnement

In diesem Tutorial erfahren Sie, wie Sie ein Peering zwischen virtuellen Netzwerken erstellen, die mit dem Resource Manager erstellt wurden. Beide virtuellen Netzwerke müssen zu demselben Abonnement gehören. Das Peering zweier virtueller Netzwerke ermöglicht es Ressourcen, in verschiedenen virtuellen Netzwerken untereinander mit derselben Bandbreite und Latenz zu kommunizieren, als befänden sie sich im selben virtuellen Netzwerk. Weitere Informationen hierzu finden Sie unter [Peering virtueller Netzwerke](virtual-network-peering-overview.md). 

Die Schritte zum Erstellen eines Peerings virtueller Netzwerke sind je nachdem unterschiedlich, ob sich die virtuellen Netzwerke im selben oder unterschiedlichen Abonnements befinden und mit welchem [Azure-Bereitstellungsmodell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die virtuellen Netzwerke erstellt werden. Erfahren Sie, wie Sie ein Peering virtueller Netzwerke in anderen Szenarien erstellen, indem Sie in der folgenden Tabelle auf das gewünschte Szenario klicken:

|Azure-Bereitstellungsmodell  | Azure-Abonnement  |
|--------- |---------|
|[Beide mit Resource Manager](create-peering-different-subscriptions.md) |Unterschiedlich|
|[Einmal Resource Manager, einmal klassisch](create-peering-different-deployment-models.md) |identisch|
|[Einmal Resource Manager, einmal klassisch](create-peering-different-deployment-models-subscriptions.md) |Unterschiedlich|

Ein Peering zweier virtueller Netzwerke, die über das klassische Bereitstellungsmodell bereitgestellt wurden, ist nicht möglich. Ein Peering kann nur zwischen zwei virtuellen Netzwerken erstellt werden, die sich in derselben Azure-Region befinden. Wenn Sie eine Verbindung zwischen virtuellen Netzwerken herstellen möchten, die beide über das klassische Bereitstellungsmodell erstellt wurden oder sich in verschiedenen Azure-Regionen befinden, können Sie ein [Azure VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nutzen. 

Ein Peering in Netzwerken lässt sich mithilfe des [Azure-Portals](#portal), mit der [Azure-Befehlszeilenschnittstelle](#cli) (CLI), mit [Azure PowerShell](#powershell) oder mit der [Azure Resource Manager-Vorlage](#template) erstellen. Klicken Sie auf einen der vorherigen Tool-Links. So gelangen Sie direkt zu den Anleitungen zum Erstellen von Peerings in virtuellen Netzwerken mit dem Tool Ihrer Wahl.

## <a name="portal"></a>Erstellen eines Peerings: Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. Das Konto, mit dem Sie sich anmelden, muss über die Berechtigungen verfügen, die zum Erstellen eines Peerings virtueller Netzwerke erforderlich sind. Weitere Informationen finden Sie im Abschnitt [Berechtigungen](#permissions) dieses Artikels.
2. Klicken Sie auf **+ Neu**, dann auf **Network**, und klicken Sie zuletzt auf **Virtuelles Netzwerk**.
3. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die Werte für die folgenden Einstellungen an, oder wählen Sie sie aus, und klicken Sie anschließend auf **Erstellen**:
    - **Name**: *myVNet1*
    - **Adressraum**: *10.0.0.0/16*
    - **Subnetzname**: *Standard*
    - **Subnetzadressbereich**: *10.0.0.0/24*
    - **Abonnement**: Wählen Sie Ihr Abonnement aus.
    - *Ressourcengruppe*: Wählen Sie **Neu erstellen** aus, und geben Sie **myResourceGroup** ein.
    - **Standort**: *USA, Osten*
4. Führen Sie die Schritte 2 und 3 erneut aus, und geben Sie bei Schritt 3 die folgenden Werte an:
    - **Name**: *myVnet2*
    - **Adressraum**: *10.1.0.0/16*
    - **Subnetzname**: *Standard*
    - **Subnetzadressbereich**: *10.1.0.0/24*
    - **Abonnement**: Wählen Sie Ihr Abonnement aus.
    - **Ressourcengruppe**: Wählen Sie **Vorhandene verwenden** aus, und wählen Sie *myResourceGroup* aus.
    - **Standort**: *USA, Osten*
5. Geben Sie im oben im Portal im Feld **Ressourcen suchen** *myResourceGroup* ein. Klicken Sie auf **myResourceGroup**, wenn sie in den Suchergebnissen angezeigt wird. Es öffnet sich ein Blatt für die Ressourcengruppe **myResourceGroup**. Die Ressourcengruppe enthält die beiden virtuellen Netzwerke, die in den vorherigen Schritten erstellt wurden.
6. Klicken Sie auf **myVNet1**.
7. Klicken Sie auf dem sich öffnenden Blatt **myVnet1** links in der vertikalen Liste der Optionen auf **Peerings**.
8. Klicken Sie auf dem sich öffnenden Blatt **myVnet1 – Peerings** auf **+ Hinzufügen**
9. Geben Sie auf dem sich öffnenden Blatt **Peering hinzufügen** die folgenden Optionen ein, oder wählen Sie sie aus, und klicken Sie auf **OK**:
     - **Name**: *myVnet1ToMyVnet2*
     - **Bereitstellungsmodell für das virtuelle Netzwerk**: Wählen Sie **Resource Manager** aus. 
     - **Abonnement**: Wählen Sie Ihr Abonnement aus.
     - **Virtuelles Netzwerk**: Klicken Sie auf **Virtuelles Netzwerk auswählen**, und klicken Sie dann auf **myVnet2**.
     - **Zugriff auf virtuelles Netzwerk zulassen**: Vergewissern Sie sich, dass **Aktiviert** ausgewählt ist.
    In diesem Tutorial werden keine anderen Einstellungen verwendet. Weitere Informationen zu allen Peeringeinstellungen finden Sie unter [Create a peering (Erstellen eines Peerings)](virtual-network-manage-peering.md#create-a-peering).
10. Nachdem Sie zuvor auf **OK** geklickt haben, wird das Blatt **Peering hinzufügen** geschlossen, und Sie gelangen wieder zum Blatt **myVnet1 – Peerings**. Nach wenigen Sekunden wird das von Ihnen erstellte Peering auf dem Blatt angezeigt. In der Spalte **Peeringstatus** wird für das Peering **myVnet1ToMyVnet2** der Status **Initiiert** angezeigt. Sie haben jetzt ein Peering zwischen Vnet1 und Vnet2 erstellt. Jetzt müssen Sie eines zwischen myVnet2 und myVnet1 erstellen. Das Peering muss für beide Richtungen erstellt werden, damit die Ressourcen in den virtuellen Netzwerken miteinander kommunizieren können.
11. Führen Sie die Schritte 5 bis 10 erneut für myVnet2 aus.  Nennen Sie das Peering *myVnet2ToMyVnet1*.
12. Klicken Sie auf **OK**, um das Peerings für myVnet2 zu erstellen. Nach einigen Sekunden wird das Peering **myVnet2ToMyVnet1** als **Verbunden** in der Spalte **Perringstatus** aufgeführt.
13. Führen Sie die Schritte 5 bis 7 erneut für myVnet1 aus. Der **Peeringstatus** für **myVnet1ToVNet2** lautet jetzt auch **Verbunden**. Wenn in der Spalte **Peeringstatus** für beide virtuellen Netzwerke der Status **Verbunden** angezeigt wird, wurde das Peering erfolgreich erstellt.
14. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
15. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-portal) dieses Artikels aus.

Alle Azure-Ressourcen, die Sie in einem der virtuellen Netzwerke erstellen, sind in der Lage, miteinander über ihre IP-Adressen zu kommunizieren. Wenn Sie die standardmäßige Azure-Namensauflösung für virtuelle Netzwerke verwenden, können die Ressourcen in den virtuellen Netzwerken Namen nicht netzwerkübergreifend auflösen. Wenn Sie Namen netzwerkübergreifend in einem Peering auflösen möchten, müssen Sie einen eigenen DNS-Server erstellen. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="cli"></a>Erstellen eines Peerings: Azure CLI

Für das folgende Skript gilt:

- Es erfordert mindestens Version 2.0.4 der Azure CLI. Führen Sie den Befehl `az --version` aus, um die Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json) Informationen dazu.
- Es wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Anstatt die CLI und ihre Abhängigkeiten zu installieren, können Sie die Azure Cloud Shell verwenden. Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure CLI ist vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Klicken Sie im folgenden Skript auf die Schaltfläche **Ausprobieren**, um eine Cloud Shell aufzurufen, über die Sie sich bei Ihrem Azure-Konto anmelden können. Um das Skript auszuführen, klicken Sie auf die Schaltfläche **Kopieren**. Fügen Sie den Inhalt in Ihre Cloud Shell ein.

1. Erstellen Sie eine Ressourcengruppe und zwei virtuelle Netzwerke.

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout the script.
    rgName="myResourceGroup"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network 1.
    az network vnet create \
      --name myVnet1 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Create virtual network 2.
    az network vnet create \
      --name myVnet2 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.1.0.0/16
    ```

2. Erstellen Sie ein Peering zwischen den beiden virtuellen Netzwerken.
 
    ```azurecli-interactive
    # Get the id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet1 \
      --query id --out tsv)

    # Get the id for VNet2.
    vnet2Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet2 \
      --query id \
      --out tsv)

    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group $rgName \
      --vnet-name myVnet1 \
      --remote-vnet-id $vnet2Id \
      --allow-vnet-access

    # Peer VNet2 to VNet1.
    az network vnet peering create \
      --name myVnet2ToMyVnet1 \
      --resource-group $rgName \
      --vnet-name myVnet2 \
      --remote-vnet-id $vnet1Id \
      --allow-vnet-access
    ```

3. Nachdem das Skript ausgeführt wurde, überprüfen Sie die Peerings für jedes virtuelle Netzwerk. 

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    Führen den vorherigen Befehl erneut aus, und ersetzen Sie dabei *myVnet1* durch *myVnet2*. Die Ausgabe beider Befehle zeigt in der Spalte **PeeringState** den Status **Verbunden** an.

     Alle Azure-Ressourcen, die Sie in einem der virtuellen Netzwerke erstellen, sind in der Lage, miteinander über ihre IP-Adressen zu kommunizieren. Wenn Sie die standardmäßige Azure-Namensauflösung für virtuelle Netzwerke verwenden, können die Ressourcen in den virtuellen Netzwerken Namen nicht netzwerkübergreifend auflösen. Wenn Sie Namen netzwerkübergreifend in einem Peering auflösen möchten, müssen Sie einen eigenen DNS-Server erstellen. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

4. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
5. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-cli) in diesem Artikel aus.


## <a name="powershell"></a>Erstellen eines Peerings: PowerShell

1. Installieren Sie die neueste Version des PowerShell-Moduls [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Sie noch nicht mit Azure PowerShell vertraut sind, lesen Sie die [Übersicht über Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Um eine PowerShell-Sitzung zu starten, wechseln Sie zu **Start**, geben Sie **PowerShell** ein, und klicken Sie dann auf **PowerShell**.
3. Melden Sie sich in PowerShell bei Azure an, indem Sie den Befehl `login-azurermaccount` eingeben. Das Konto, mit dem Sie sich anmelden, muss über die Berechtigungen verfügen, die zum Erstellen eines Peerings virtueller Netzwerke erforderlich sind. Weitere Informationen finden Sie im Abschnitt [Berechtigungen](#permissions) dieses Artikels.
4. Erstellen Sie eine Ressourcengruppe und zwei virtuelle Netzwerke. Um das Skript auszuführen, kopieren Sie das folgende Skript. Fügen Sie es in PowerShell ein. Nachdem die letzte Zeile auf dem Bildschirm erschienen ist, drücken Sie `Enter`:

    ```powershell
    # Variables for common values used throughout the script.
    $rgName='myResourceGroup'
    $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network 1.
    $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location

    # Create virtual network 2.
    $vnet2 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet2' `
      -AddressPrefix '10.1.0.0/16' `
      -Location $location
    ```

5. Erstellen Sie ein Peering zwischen den beiden virtuellen Netzwerken. Kopieren Sie das folgende Skript. Fügen Sie es in PowerShell ein. Nachdem die letzte Zeile auf dem Bildschirm erschienen ist, drücken Sie `Enter`:
    ```powershell
    # Peer VNet1 to VNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet1ToMyVnet2' `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId $vnet2.Id

    # Peer VNet2 to VNet1.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet2ToMyVnet1' `
      -VirtualNetwork $vnet2 `
      -RemoteVirtualNetworkId $vnet1.Id
    ```
6. Um die Subnetze für das virtuelle Netzwerk zu überprüfen, kopieren Sie den folgenden Befehl, und fügen Sie ihn in PowerShell ein`Enter`:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Führen den vorherigen Befehl erneut aus, und ersetzen Sie dabei *myVnet1* durch *myVnet2*. Die Ausgabe beider Befehle zeigt in der Spalte **PeeringState** den Status **Verbunden** an.
7. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
8. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-powershell) dieses Artikels aus.

Alle Azure-Ressourcen, die Sie in einem der virtuellen Netzwerke erstellen, sind in der Lage, miteinander über ihre IP-Adressen zu kommunizieren. Wenn Sie die standardmäßige Azure-Namensauflösung für virtuelle Netzwerke verwenden, können die Ressourcen in den virtuellen Netzwerken Namen nicht netzwerkübergreifend auflösen. Wenn Sie Namen netzwerkübergreifend in einem Peering auflösen möchten, müssen Sie einen eigenen DNS-Server erstellen. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="template"></a>Erstellen eines Peerings: Resource Manager-Vorlage

1. Sehen Sie sich die Vorlage [Create a vNet to vNet connection using vNet Peering](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) (Erstellen einer vNet-zu-vNet-Verbindung mithilfe von vNet-Peering) an. Mit der Vorlage wird eine Anleitung für Ihre Bereitstellung über das Azure-Portal, PowerShell oder die Azure-CLI geliefert. Melden Sie sich bei dem Tool Ihrer Wahl an, um die Vorlage mit einem Konto bereitzustellen, das über die erforderlichen Berechtigungen zum Erstellen eines Peerings in virtuellen Netzwerk verfügt. Weitere Informationen finden Sie im Abschnitt [Berechtigungen](#permissions) dieses Artikels.
2. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
3. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie mithilfe des Azure-Portals, von PowerShell oder der Azure-CLI die Schritte im Abschnitt [Löschen von Ressourcen](#delete) dieses Artikels aus.

## <a name="permissions"></a>Berechtigungen

Die Konten, mit denen Sie das Peering in virtuellen Netzwerken erstellt haben, müssen die erforderliche Rolle oder die erforderlichen Berechtigungen haben. Wenn Sie beispielsweise ein Peering zwischen den virtuellen Netzwerken VNet1 und VNet2 erstellen, muss Ihr Konto mindestens die folgende Rolle oder die folgenden Berechtigungen für jedes virtuelle Netzwerk haben:
    
|Virtuelles Netzwerk|Rolle|Berechtigungen|
|---|---|---|
|VNet1|[Mitwirkender von virtuellem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
|VNet2|[Mitwirkender von virtuellem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|

Erfahren Sie mehr über [integrierte Rollen](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) und das Zuweisen bestimmter Berechtigungen zu [benutzerdefinierten Rollen](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (nur für Resource Manager).

## <a name="delete"></a>Löschen von Ressourcen
Wenn Sie dieses Tutorial abgeschlossen haben, möchten Sie die Ressourcen, die Sie in diesem Tutorial erstellt haben, möglicherweise wieder löschen, damit keine Nutzungsgebühren anfallen. Durch das Löschen einer Ressourcengruppe werden auch alle Ressourcen in dieser Ressourcengruppe gelöscht.

### <a name="delete-portal"></a>Azure-Portal

1. Geben Sie in das Suchfeld im Portal **myResourceGroup** ein. Klicken Sie in den Suchergebnissen auf **myResourceGroup**.
2. Klicken Sie auf dem Blatt **myResourceGroup** auf das **Löschsymbol**.
3. Um den Löschvorgang zu bestätigen, geben Sie in das Feld **TYPE THE RESOURCE GROUP NAME** (Ressourcengruppennamen eingeben) **myResourceGroup** ein, und klicken Sie dann auf **Löschen**.

### <a name="delete-cli"></a>Azure-Befehlszeilenschnittstelle

Geben Sie den folgenden Befehl ein:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

Geben Sie den folgenden Befehl ein:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -force
```

## <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich eingehend mit den wichtigen [Einschränkungen und Verhalten eines Peerings in virtuellen Netzwerken](virtual-network-manage-peering.md#requirements-and-constraints) vertraut, bevor Sie ein Peering in virtuellen Netzwerken für die Produktion erstellen.
- Erfahren Sie alles über [Peering-Einstellungen in virtuellen Netzwerken](virtual-network-manage-peering.md#create-a-peering).
- Erfahren Sie, wie Sie eine [Hub-Spoke-Netzwerktopologie ](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) mit einem Peering in virtuellen Netzwerken erstellen.

