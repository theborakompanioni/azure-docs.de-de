---
title: Erstellen eines Peerings in virtuellen Azure-Netzwerken | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Peering zwischen zwei virtuellen Netzwerken erstellen.
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
ms.date: 06/06/2017
ms.author: jdial;narayan;annahar
ms.translationtype: Human Translation
ms.sourcegitcommit: 245ce9261332a3d36a36968f7c9dbc4611a019b2
ms.openlocfilehash: 666165115e80a39d02386719b0d7e64d7ae17749
ms.contentlocale: de-de
ms.lasthandoff: 06/09/2017


---
# <a name="create-a-virtual-network-peering"></a>Erstellen eines Peerings in virtuellen Netzwerken

In diesem Tutorial erfahren Sie, wie Sie ein Peering zwischen zwei virtuellen Netzwerken erstellen. Das Peering zweier virtueller Netzwerke ermöglicht es Ressourcen, in verschiedenen virtuellen Netzwerken untereinander mit derselben Bandbreite und Latenz zu kommunizieren, als befänden sie sich im selben virtuellen Netzwerk. Ein Peering kann nur zwischen zwei virtuellen Netzwerken erstellt werden, die sich in derselben Azure-Region befinden. Weitere Informationen zum Peering in virtuellen Netzwerken finden Sie im Übersichtsartikel [Peering in virtuellen Netzwerken](virtual-network-peering-overview.md). 

Wenn Sie eine Verbindung zwischen virtuellen Netzwerken in verschiedenen Azure-Regionen herstellen möchten, können Sie eine [Azure VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nutzen. 

Ein Peering in Netzwerken lässt sich mithilfe des [Azure-Portals](#portal), mit [Azure PowerShell](#powershell), mit der [Azure-Befehlszeilenschnittstelle](#cli) (CLI) oder mit der [Azure Resource Manager-Vorlage](#template) erstellen. Klicken Sie auf einen der vorherigen Tool-Links. So gelangen Sie direkt zu den Anleitungen zum Erstellen von Peerings in virtuellen Netzwerken mit dem Tool Ihrer Wahl.

## <a name="portal"></a>Erstellen eines Peerings: Azure-Portal

Die folgenden Schritte zum Erstellen eines Peerings zwischen zwei virtuellen Netzwerken, die mit dem Resource Manager bereitgestellt wurden und im gleichen Abonnement vorhanden sind. Ein Peering kann aber auch zwischen [zwei virtuellen Netzwerken in verschiedenen Abonnements](#different-subscriptions) oder [einem virtuellen Netzwerk (Resource Manager) und einem virtuellen Netzwerk (klassisch)](#different-models) erstellt werden. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. Das Konto, mit dem Sie sich anmelden, braucht die erforderlichen Berechtigungen, um ein Peering in virtuellen Netzwerken zu erstellen. Weitere Informationen finden Sie im Abschnitt [Berechtigungen](#permissions) dieses Artikels.
2. Klicken Sie auf **+ Neu**, dann auf **Network**, und klicken Sie zuletzt auf **Virtuelles Netzwerk**.
3. Lassen Sie auf dem Blatt **Virtuelles Netzwerk** unter **Bereitstellungsmodell auswählen** die Option **Resource Manager** ausgewählt, und klicken Sie auf **Erstellen**.
4. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die Werte für die folgenden Einstellungen an, oder wählen Sie sie aus, und klicken Sie anschließend auf **Erstellen**:
    - **Name**: *myVNet1*
    - **Adressraum**: *10.0.0.0/16*
    - **Subnetzname**: *Standard*
    - **Subnetzadressbereich**: *10.0.0.0/24*
    - **Abonnement**: Wählen Sie Ihr Abonnement aus.
    - *Ressourcengruppe*: Wählen Sie **Neu erstellen** aus, und geben Sie **myResourceGroup** ein.
    - **Standort**: *USA, Osten*
5. Führen Sie die Schritte 2–4 erneut aus, und geben Sie bei Schritt 4 die folgenden Werte an:
    - **Name**: *myVnet2*
    - **Adressraum**: *10.1.0.0/16*
    - **Subnetzname**: *Standard*
    - **Subnetzadressbereich**: *10.1.0.0/24*
    - **Abonnement**: Wählen Sie Ihr Abonnement aus.
    - **Ressourcengruppe**: Wählen Sie **Vorhandene verwenden** aus, und wählen Sie *myResourceGroup* aus.
    - **Standort**: *USA, Osten*
6. Geben Sie im oben im Portal im Feld **Ressourcen suchen** *myResourceGroup* ein. Klicken Sie auf **myResourceGroup**, wenn sie in den Suchergebnissen angezeigt wird. Es öffnet sich ein Blatt für die Ressourcengruppe **myResourceGroup**. Die Ressourcengruppe enthält die beiden virtuellen Netzwerke, die in den vorherigen Schritten erstellt wurden.
7. Klicken Sie auf **myVNet1**.
8. Klicken Sie auf dem sich öffnenden Blatt **myVnet1** links in der vertikalen Liste der Optionen auf **Peerings**.
9. Klicken Sie auf dem sich öffnenden Blatt **myVnet1 – Peerings** auf **+ Hinzufügen**
10. Geben Sie auf dem sich öffnenden Blatt **Peering hinzufügen** die folgenden Optionen ein, oder wählen Sie sie aus, und klicken Sie auf **OK**:
     - **Name**: *myVnet1ToMyVnet2*
     - **Bereitstellungsmodell für das virtuelle Netzwerk**: Wählen Sie **Resource Manager** aus. 
     - **Abonnement**: Wählen Sie Ihr Abonnement aus.
     - **Virtuelles Netzwerk**: Klicken Sie auf **Virtuelles Netzwerk auswählen**, und klicken Sie dann auf **myVnet2**.
     - **Zugriff auf virtuelles Netzwerk zulassen**: Vergewissern Sie sich, dass **Aktiviert** ausgewählt ist.
    In diesem Tutorial werden keine anderen Einstellungen verwendet. Weitere Informationen zu allen Peeringeinstellungen finden Sie unter [Create a peering (Erstellen eines Peerings)](virtual-network-manage-peering.md#create-peering).
11. Nachdem Sie zuvor auf **OK** geklickt haben, wird das Blatt **Peering hinzufügen** geschlossen, und Sie gelangen wieder zum Blatt **myVnet1 – Peerings**. Nach wenigen Sekunden wird das von Ihnen erstellte Peering auf dem Blatt angezeigt. In der Spalte **Peeringstatus** wird für das Peering **myVnet1ToMyVnet2** der Status **Initiiert** angezeigt. Sie haben jetzt ein Peering zwischen Vnet1 und Vnet2 erstellt. Jetzt müssen Sie eines zwischen Vnet2 und Vnet1 erstellen. Das Peering muss für beide Richtungen erstellt werden, damit die Ressourcen in den virtuellen Netzwerken miteinander kommunizieren können.
12. Führen Sie die Schritte 6–11 erneut für myVnet2 aus.  Nennen Sie das Peering *myVnet2ToMyVnet1*.
13. Klicken Sie auf **OK**, um das Peerings für myVnet2 zu erstellen. Nach einigen Sekunden wird das Peering **myVnet2ToMyVnet1** als **Verbunden** in der Spalte **Perringstatus** aufgeführt.
14. Führen Sie die Schritte 6–8 erneut für myVnet1 aus. Der **Peeringstatus** für **myVnet1ToVNet2** lautet jetzt auch **Verbunden**. Wenn in der Spalte **Peeringstatus** für beide virtuellen Netzwerke der Status **Verbunden** angezeigt wird, wurde das Peering erfolgreich erstellt.
15. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
16. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-portal) dieses Artikels aus.

Alle Azure-Ressourcen, die Sie in einem der virtuellen Netzwerke erstellen, sind in der Lage, miteinander über ihre IP-Adressen zu kommunizieren. Wenn Sie die standardmäßige Azure-Namensauflösung für virtuelle Netzwerke verwenden, können die Ressourcen in den virtuellen Netzwerken Namen nicht netzwerkübergreifend auflösen. Wenn Sie Namen netzwerkübergreifend in einem Peering auflösen möchten, müssen Sie einen eigenen DNS-Server erstellen. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="cli"></a>Erstellen eines Peerings: Azure CLI

Für das folgende Skript gilt:

- Es erfordert mindestens Version 2.0.4 der Azure CLI. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json) Informationen dazu.
- Es wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

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
    #
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
    #
    ```
3. Nachdem das Skript ausgeführt wurde, überprüfen Sie die Peerings für jedes virtuelle Netzwerk. Kopieren Sie den folgenden Befehl, und fügen Sie ihn dann in das Befehlsfenster ein:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    #
    ```
Führen den vorherigen Befehl erneut aus, und ersetzen Sie dabei *myVnet1* durch *myVnet2*. Die Ausgabe beider Befehle zeigt in der Spalte **PeeringState** den Status **Verbunden** an.
4. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
5. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-cli) dieses Artikels aus.

Alle Azure-Ressourcen, die Sie in einem der virtuellen Netzwerke erstellen, sind in der Lage, miteinander über ihre IP-Adressen zu kommunizieren. Wenn Sie die standardmäßige Azure-Namensauflösung für virtuelle Netzwerke verwenden, können die Ressourcen in den virtuellen Netzwerken Namen nicht netzwerkübergreifend auflösen. Wenn Sie Namen netzwerkübergreifend in einem Peering auflösen möchten, müssen Sie einen eigenen DNS-Server erstellen. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
 
## <a name="powershell"></a>Erstellen eines Peerings: PowerShell

1. Installieren Sie die neueste Version des PowerShell-Moduls [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Azure PowerShell für Sie neu ist, lesen Sie den Artikel [Übersicht über Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Um eine PowerShell-Sitzung zu starten, wechseln Sie zu **Start**, geben Sie **PowerShell** ein, und klicken Sie dann auf **PowerShell**.
3. Melden Sie sich im PowerShell-Fenster mit dem Befehl `login-azurermaccount` bei Azure an. Das Konto, mit dem Sie sich anmelden, braucht die erforderlichen Berechtigungen, um ein Peering in virtuellen Netzwerken zu erstellen. Weitere Informationen finden Sie im Abschnitt [Berechtigungen](#permissions) dieses Artikels.
4. Kopieren Sie das folgende Skript in Ihrem Browser, und klicken Sie dann mit der rechten Maustaste in das PowerShell-Fenster, um das Skript auszuführen, das eine Ressourcengruppe und zwei virtuelle Netzwerke erstellt:
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
    #
    ```
5. Kopieren Sie das folgende Skript in Ihrem Browser, und klicken Sie dann mit der rechten Maustaste in das PowerShell-Fenster, um das Skript auszuführen, das ein Peering in virtuellen Netzwerken zwischen den beiden virtuellen Netzwerken erstellt:
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
    #
    ```
6. Um die Subnetze für das virtuelle Netzwerk zu überprüfen, kopieren Sie den folgenden Befehl, und fügen Sie ihn in das PowerShell-Fenster ein:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    #
    ```

Führen den vorherigen Befehl erneut aus, und ersetzen Sie dabei *myVnet1* durch *myVnet2*. Die Ausgabe beider Befehle zeigt in der Spalte **PeeringState** den Status **Verbunden** an.
7. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
8. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-powershell) dieses Artikels aus.

Alle Azure-Ressourcen, die Sie in einem der virtuellen Netzwerke erstellen, sind in der Lage, miteinander über ihre IP-Adressen zu kommunizieren. Wenn Sie die standardmäßige Azure-Namensauflösung für virtuelle Netzwerke verwenden, können die Ressourcen in den virtuellen Netzwerken Namen nicht netzwerkübergreifend auflösen. Wenn Sie Namen netzwerkübergreifend in einem Peering auflösen möchten, müssen Sie einen eigenen DNS-Server erstellen. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="template"></a>Erstellen eines Peerings: Resource Manager-Vorlage

1. Sehen Sie sich die Vorlage [Create a vNet to vNet connection using vNet Peering](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) (Erstellen einer vNet-zu-vNet-Verbindung mithilfe von vNet-Peering) an. Mit der Vorlage wird eine Anleitung für Ihre Bereitstellung über das Azure-Portal, PowerShell oder die Azure-CLI geliefert. Melden Sie sich bei dem Tool Ihrer Wahl an, um die Vorlage mit einem Konto bereitzustellen, das über die erforderlichen Berechtigungen zum Erstellen eines Peerings in virtuellen Netzwerk verfügt. Weitere Informationen finden Sie im Abschnitt [Berechtigungen](#permissions) dieses Artikels.
2. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
3. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie mithilfe des Azure-Portals, von PowerShell oder der Azure-CLI die Schritte im Abschnitt [Löschen von Ressourcen](#delete) dieses Artikels aus.

## <a name="different-models"></a>Peering von virtuellen Netzwerken, die über verschiedene Bereitstellungsmodelle erstellt wurden

In den vorherigen Abschnitten wurde erläutert, wie das Peering zwischen zwei virtuellen Netzwerken über das Resource Manager-Bereitstellungsmodell erstellt wird. Sie können auch ein Peering zwischen einem virtuellen Netzwerk (Resource Manager) und einem virtuellen Netzwerk (klassisch) erstellen. Es ist jedoch nicht möglich, ein Peering zwischen zwei virtuellen Netzwerken zu erstellen, die über das klassische Bereitstellungsmodell erstellt wurden. Weitere Informationen zu den Azure-Bereitstellungsmodellen finden Sie im Artikel [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Um das Peering in virtuellen Netzwerken zu erstellen, führen Sie die Schritte 1 bis 11 im Abschnitt [Portal](#portal) dieses Artikels durch. Wählen Sie jedoch beim Erstellen von myVnet2 in Schritt 3 das **klassische** Bereitstellungsmodell aus.

Wenn Sie ein Peering zwischen einem virtuellen Netzwerk (Resource Manager) und einem virtuellen Netzwerk (klassisch) erstellen, konfigurieren Sie das Peering nur für die Verbindung vom virtuellen Netzwerk (klassisch) zum virtuellen Netzwerk (Resource Manager). Nachdem Sie das Peering für das virtuelle Netzwerk (Resource Manager) erstellt haben, wird dessen Peeringstatus als **Updating** (Wird aktualisiert) angezeigt. Der Status ändert sich nach ein paar Sekunden automatisch in **Verbunden**, weil Sie kein Peering für das virtuelle Netzwerk (klassisch) erstellen müssen. 

> [!NOTE]
> Obwohl dieses Thema in diesem Tutorial nicht behandelt wird, könnten Sie die Skripts in den Abschnitten [Azure-CLI](#cli) und [PowerShell](#powershell) dieses Artikels anpassen, damit sie die in diesem Abschnitt behandelten Unterschiede widerspiegeln.

## <a name="different-subscriptions"></a>Peering zwischen virtuellen Netzwerken in unterschiedlichen Abonnements erstellen

In den vorherigen Abschnitten wurde erläutert, wie ein Peering zwischen zwei virtuellen Netzwerken im gleichen Abonnement erstellt wird. Sie können aber auch ein Peering zwischen virtuellen Netzwerken in unterschiedlichen Abonnements erstellen, sofern beide Abonnements dem gleichen Azure Active Directory-Mandanten zugeordnet sind. Wenn Sie noch keinen AD-Mandanten besitzen, [erstellen Sie einen](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Wenn die Abonnements verschiedenen Active Directory-Mandanten zugeordnet sind, ist es nicht möglich, ein Peering zwischen den entsprechenden virtuellen Netzwerken zu erstellen. Die Schritte zum Erstellen des Peerings unterscheiden sich geringfügig, je nach dem Bereitstellungsmodell, mit dem die virtuellen Netzwerke erstellt wurden:

### <a name="different-subscriptions-same-deployment-model"></a>Beide virtuellen Netzwerke wurden über den Resource Manager erstellt

1. Führen Sie die Schritte 1 bis 7 im Abschnitt [Portal](#portal) dieses Artikel aus, entscheiden Sie sich jedoch für myVnet2 in Schritt 5 für ein anderes Abonnement (das dem gleichen Azure Active Directory-Mandanten zugeordnet ist) als Sie in Schritt 4 bei myVnet1 auswählen.
2. Klicken Sie im sich öffnenden Blatt **myVnet1** links in der vertikalen Liste der Optionen auf **Zugriffssteuerung (IAM)**.
3. Klicken Sie auf dem sich öffnenden Blatt **myVnet1 – Zugriffssteuerung (IAM)** auf **+ Hinzufügen**.
4. Wählen Sie auf dem sich öffnenden Blatt **Berechtigungen hinzufügen** im Feld **Rolle** die Option **Netzwerkmitwirkender** aus.
5. Wählen Sie im Feld **Auswählen** einen Benutzernamen aus, oder geben Sie eine E-Mail-Adresse ein, um einen zu suchen. Die Liste der angezeigten Benutzer stammt aus dem gleichen Azure Active Directory-Mandanten, in dem sich auch das virtuelle Netzwerk befindet, für das Sie das Peering erstellen.
6. Klicken Sie auf **Speichern**.
7. Klicken Sie im sich öffnenden Blatt **myVnet1 - Zugriffssteuerung (IAM)** links in der vertikalen Liste der Optionen auf **Eigenschaften**. Kopieren Sie die **Ressourcen-ID**, die dem folgenden Format entspricht: /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet1.
8. Führen Sie die Schritte 1 bis 7 in diesem Abschnitt für myVnet2 aus.
9. Um sicherzustellen, dass die Autorisierung erfolgreich aktiviert wurde, melden Sie sich bei Azure ab, und melden Sie sich dann erneut an. Wenn Sie für die beiden virtuellen Netzwerke unterschiedliche Konten verwendet haben, melden Sie sich in beiden Konten ab und wieder an.
10. Geben Sie im oben im Portal im Feld **Ressourcen suchen** *myResourceGroup* ein. Klicken Sie auf **myResourceGroup**, wenn sie in den Suchergebnissen angezeigt wird. Es öffnet sich ein Blatt für die Ressourcengruppe **myResourceGroup**. Die Ressourcengruppe enthält die beiden virtuellen Netzwerke, die in den vorherigen Schritten erstellt wurden.
11. Klicken Sie auf **myVNet1**.
12. Klicken Sie auf dem sich öffnenden Blatt **myVnet1** links in der vertikalen Liste der Optionen auf **Peerings**.
13. Klicken Sie auf dem sich öffnenden Blatt **myVnet1 – Peerings** auf **+ Hinzufügen**
14. Geben Sie auf dem sich öffnenden Blatt **Peering hinzufügen** die folgenden Optionen ein, oder wählen Sie sie aus, und klicken Sie auf **OK**:
     - **Name**: *myVnet1ToMyVnet2*
     - **Bereitstellungsmodell für das virtuelle Netzwerk**: Wählen Sie **Klassisch** aus. 
     - **Ich kenne meine Ressourcen-ID**: Aktivieren Sie dieses Kontrollkästchen.
     - **Ressourcen-ID**: Geben Sie die Ressourcen-ID für myVnet2 ein. Dieses Feld wird nur angezeigt, wenn das Kontrollkästchen neben **Ich kenne meine Ressourcen-ID** aktiviert ist.
     - **Zugriff auf virtuelles Netzwerk zulassen**: Vergewissern Sie sich, dass **Aktiviert** ausgewählt ist.
    In diesem Tutorial werden keine anderen Einstellungen verwendet. Weitere Informationen zu allen Peeringeinstellungen finden Sie unter [Create a peering (Erstellen eines Peerings)](virtual-network-manage-peering.md#create-peering).
15. Nachdem Sie zuvor auf **OK** geklickt haben, wird das Blatt **Peering hinzufügen** geschlossen, und Sie gelangen wieder zum Blatt **myVnet1 – Peerings**. Nach wenigen Sekunden wird das von Ihnen erstellte Peering auf dem Blatt angezeigt. In der Spalte **Peeringstatus** wird für das Peering **myVnet1ToMyVnet2** der Status **Initiiert** angezeigt. Sie haben jetzt ein Peering zwischen Vnet1 und Vnet2 erstellt. Jetzt müssen Sie eines zwischen Vnet2 und Vnet1 erstellen. Das Peering muss für beide Richtungen erstellt werden, damit die Ressourcen in den virtuellen Netzwerken miteinander kommunizieren können.
16. Führen Sie die Schritte 10 bis 14 in diesem Abschnitt erneut für myVnet2 aus.  Nennen Sie das Peering *myVnet2ToMyVnet1*, und geben Sie unter **Ressourcen-ID** die Ressourcen-ID für myVnet1 ein.
17. Klicken Sie auf **OK**, um das Peerings für myVnet2 zu erstellen. Nach einigen Sekunden wird das Peering **myVnet2ToMyVnet1** als **Verbunden** in der Spalte **Perringstatus** aufgeführt.
18. Führen Sie die Schritte 10 und 11 in diesem Abschnitt für MyVnet1 erneut aus. Der **Peeringstatus** für **myVnet1ToVNet2** lautet jetzt auch **Verbunden**. Wenn in der Spalte **Peeringstatus** für beide virtuellen Netzwerke der Status **Verbunden** angezeigt wird, wurde das Peering erfolgreich erstellt.
19. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
20. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-portal) dieses Artikels aus.

### <a name="different-subscriptions-different-deployment-models"></a>Ein virtuelles Netzwerk (Resource Manager), ein virtuelles Netzwerk (klassisch)
 
[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-preview.md)]

1. Sie sollten sich für die Vorschauversion registrieren, denn andernfalls können Sie kein Peering für virtuelle Netzwerke erstellen, die mit verschiedenen Bereitstellungsmodellen aus verschiedenen Abonnements erstellt wurden. Die Registrierung ist nicht über das Portal oder mithilfe der Azure-CLI möglich, sondern ausschließlich mit PowerShell. Um sich für die Vorschau zu registrieren, führen Sie die folgenden Aufgaben aus:
    - Installieren Sie die neueste Version des PowerShell-Moduls [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Azure PowerShell für Sie neu ist, lesen Sie den Artikel [Übersicht über Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - Um eine PowerShell-Sitzung auf einem Windows-PC zu starten, wechseln Sie zu **Start**, geben Sie **PowerShell** ein, und klicken Sie dann auf **PowerShell**.
    - Melden Sie sich im PowerShell-Fenster mit dem Befehl `login-azurermaccount` bei Azure an. Das Konto, mit dem Sie sich anmelden, braucht die erforderlichen Berechtigungen, um ein Peering in virtuellen Netzwerken zu erstellen. Weitere Informationen finden Sie im Abschnitt [Berechtigungen](#permissions) dieses Artikels.
    - Registrieren Sie die Vorschaufunktion für beide Azure-Abonnements, und geben Sie die folgenden Befehle über PowerShell ein: 
    
    ```powershell
    Register-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    #
    ```
    Während der Registrierung für die Vorschau müssen Sie bei allen Abonnements angemeldet sein. Fahren Sie nicht mit folgenden Schritte fort, bis die Ausgabe **RegistrationState**, die Sie nach Ausführung der beiden Befehle erhalten, in beiden Abonnements **Registriert** lautet.

    ```powershell
    Get-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    #
    ```

2. Führen Sie die Schritte 1 bis 7 im Abschnitt [Portal](#portal) dieses Artikels mit folgenden Änderungen aus:
    - Wählen Sie für myVnet2 in Schritt 5 ein anderes Abonnement aus (das dem gleichen Azure Active Directory-Mandanten zugeordnet ist) als Sie in Schritt 4 bei myVnet1 auswählen.
    - Wählen Sie beim Erstellen von myVnet2 in Schritt 3 **Klassisch** aus.
3. Geben Sie im oben im Portal im Feld **Ressourcen suchen** *myResourceGroup* ein. Klicken Sie auf **myResourceGroup**, wenn sie in den Suchergebnissen angezeigt wird. Es öffnet sich ein Blatt für die Ressourcengruppe **myResourceGroup**. Die Ressourcengruppe enthält die beiden virtuellen Netzwerke, die in den vorherigen Schritten erstellt wurden.
4. Klicken Sie auf **myVNet1**.
5. Führen Sie die Schritte 2 bis 9 im Abschnitt [Beide virtuellen Netzwerke wurden über den Resource Manager erstellt](#different-subscriptions-same-deployment-model) dieses Artikels aus.
6. Geben Sie im oben im Portal im Feld **Ressourcen suchen** *myResourceGroup* ein. Klicken Sie auf **myResourceGroup**, wenn sie in den Suchergebnissen angezeigt wird. Es öffnet sich ein Blatt für die Ressourcengruppe **myResourceGroup**. Die Ressourcengruppe enthält die beiden virtuellen Netzwerke, die in den vorherigen Schritten erstellt wurden.
7. Klicken Sie auf **myVNet1**.
8. Klicken Sie auf dem sich öffnenden Blatt **myVnet1** links in der vertikalen Liste der Optionen auf **Peerings**.
9. Klicken Sie auf dem sich öffnenden Blatt **myVnet1 – Peerings** auf **+ Hinzufügen**
10. Geben Sie auf dem sich öffnenden Blatt **Peering hinzufügen** die folgenden Optionen ein, oder wählen Sie sie aus, und klicken Sie auf **OK**:
     - **Name**: *myVNet1ToMyVnet2*
     - **Bereitstellungsmodell für das virtuelle Netzwerk**: Wählen Sie **Klassisch** aus. 
     - **Ich kenne meine Ressourcen-ID**: Aktivieren Sie dieses Kontrollkästchen.
     - **Ressourcen-ID**: Geben Sie die Ressourcen-ID für myVnet2 ein. Dieses Feld wird nur angezeigt, wenn das Kontrollkästchen neben **Ich kenne meine Ressourcen-ID** aktiviert ist.
     - **Zugriff auf virtuelles Netzwerk zulassen**: Vergewissern Sie sich, dass **Aktiviert** ausgewählt ist.
    In diesem Tutorial werden keine anderen Einstellungen verwendet. Weitere Informationen zu allen Peeringeinstellungen finden Sie unter [Create a peering (Erstellen eines Peerings)](virtual-network-manage-peering.md#create-peering).

    Wenn Sie ein Peering zwischen einem virtuellen Netzwerk (Resource Manager) und einem virtuellen Netzwerk (klassisch) erstellen, konfigurieren Sie das Peering nur für die Verbindung vom virtuellen Netzwerk (klassisch) zum virtuellen Netzwerk (Resource Manager). Nachdem Sie das Peering für das virtuelle Netzwerk (Resource Manager) erstellt haben, wird dessen Peeringstatus als **Updating** (Wird aktualisiert) angezeigt. Der Status ändert sich nach ein paar Sekunden automatisch in **Verbunden**, weil Sie kein Peering für das virtuelle Netzwerk (klassisch) erstellen müssen. 
11. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
12. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-portal) dieses Artikels aus.

> [!NOTE]
> Obwohl dieses Thema in diesem Tutorial nicht behandelt wird, könnten Sie die Skripts in den Abschnitten [Azure-CLI](#cli) und [PowerShell](#powershell) dieses Artikels anpassen, damit sie die in diesem Abschnitt behandelten Unterschiede widerspiegeln.

## <a name="permissions"></a>Berechtigungen

Die Konten, mit denen Sie das Peering in virtuellen Netzwerken erstellt haben, müssen die erforderliche Rolle oder die erforderlichen Berechtigungen haben. Wenn Sie beispielsweise ein Peering zwischen den virtuellen Netzwerken VNet1 und VNet2 erstellen, muss Ihr Konto mindestens die folgende Rolle oder die folgenden Berechtigungen für jedes virtuelle Netzwerk haben:
    
|Virtuelles Netzwerk|Bereitstellungsmodell|Rolle|Berechtigungen|
|---|---|---|---|
|VNet1|Ressourcen-Manager|[Mitwirkender von virtuellem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klassisch|[Mitwirkender von klassischem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|–|
|VNet2|Ressourcen-Manager|[Mitwirkender von virtuellem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klassisch|[Mitwirkender von klassischem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Erfahren Sie mehr über [integrierte Rollen](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) und das Zuweisen bestimmter Berechtigungen zu [benutzerdefinierten Rollen](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (nur für Resource Manager).

## <a name="delete"></a>Löschen von Ressourcen
Wenn Sie dieses Tutorial abgeschlossen haben, möchten Sie die Ressourcen, die Sie in diesem Tutorial erstellt haben, möglicherweise wieder löschen, damit keine Nutzungsgebühren anfallen. Durch das Löschen einer Ressourcengruppe werden auch alle Ressourcen in dieser Ressourcengruppe gelöscht.

### <a name="delete-portal"></a>Azure-Portal

1. Geben Sie in das Suchfeld im Portal **myResourceGroup** ein. Klicken Sie in den Suchergebnissen auf **myResourceGroup**.
2. Klicken Sie auf dem Blatt **myResourceGroup** auf das **Löschsymbol**.
3. Um den Löschvorgang zu bestätigen, geben Sie in das Feld **TYPE THE RESOURCE GROUP NAME** (Ressourcengruppennamen eingeben) **myResourceGroup** ein, und klicken Sie dann auf **Löschen**.

### <a name="delete-cli"></a>Azure-Befehlszeilenschnittstelle

geben Sie über eine Linux-, macOS- oder Windows-Befehlsshell den folgenden Befehl ein. Klicken Sie alternativ oben rechts neben dem folgenden Codeblock auf die Schaltfläche „Try It“ (Testen), um die Cloud-Shell zu starten. Kopieren Sie dann den Beispielcode mit der Schaltfläche „Kopieren“ aus, und fügen Sie ihn in die Cloud-Shell ein.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

Geben Sie an der PowerShell-Eingabeaufforderung den folgenden Befehl ein:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich eingehend mit den wichtigen [Einschränkungen und Verhalten eines Peerings in virtuellen Netzwerken](virtual-network-manage-peering.md#about-peering) vertraut, bevor Sie ein Peering in virtuellen Netzwerken für die Produktion erstellen.
- Erfahren Sie alles über [Peering-Einstellungen in virtuellen Netzwerken](virtual-network-manage-peering.md#create-peering).
- Erfahren Sie, wie Sie eine [Hub-Spoke-Netzwerktopologie ](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) mit einem Peering in virtuellen Netzwerken erstellen.

