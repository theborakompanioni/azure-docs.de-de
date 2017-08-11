---
title: Erstellen eines Peerings virtueller Azure-Netzwerke mithilfe verschiedener Bereitstellungsmodelle und Abonnements | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Peering zwischen virtuellen Netzwerken erstellen, die mithilfe unterschiedlicher Bereitstellungsmodelle in unterschiedlichen Azure-Abonnements erstellt wurden.
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
ms.openlocfilehash: 93d5676e9188e67f1f6a9bba1d4d30a93b3883d8
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="create-a-virtual-network-peering---different-deployment-models-and-subscriptions"></a>Erstellen eines Peerings virtueller Netzwerke mithilfe verschiedener Bereitstellungsmodelle und Abonnements

In diesem Tutorial erfahren Sie, wie Sie ein Peering zwischen virtuellen Netzwerken erstellen, die in verschiedenen Bereitstellungsmodellen eingerichtet wurden. Die virtuellen Netzwerke befinden sich in unterschiedlichen Abonnements. Das Peering zweier virtueller Netzwerke ermöglicht es Ressourcen, in verschiedenen virtuellen Netzwerken untereinander mit derselben Bandbreite und Latenz zu kommunizieren, als befänden sie sich im selben virtuellen Netzwerk. Weitere Informationen hierzu finden Sie unter [Peering virtueller Netzwerke](virtual-network-peering-overview.md). 

Die Schritte zum Erstellen eines Peerings virtueller Netzwerke sind je nachdem unterschiedlich, ob sich die virtuellen Netzwerke im selben oder unterschiedlichen Abonnements befinden und mit welchem [Azure-Bereitstellungsmodell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die virtuellen Netzwerke erstellt werden. Erfahren Sie, wie Sie ein Peering virtueller Netzwerke in anderen Szenarien erstellen, indem Sie in der folgenden Tabelle auf das gewünschte Szenario klicken:

|Azure-Bereitstellungsmodell  | Azure-Abonnement  |
|--------- |---------|
|[Beide mit Resource Manager](virtual-network-create-peering.md) |Identisch|
|[Beide mit Resource Manager](create-peering-different-subscriptions.md) |Unterschiedlich|
|[Eines mit Resource Manager, das andere klassisch](create-peering-different-deployment-models.md) |Identisch|

Ein Peering zweier virtueller Netzwerke, die über das klassische Bereitstellungsmodell bereitgestellt wurden, ist nicht möglich. Ein Peering kann nur zwischen zwei virtuellen Netzwerken erstellt werden, die sich in derselben Azure-Region befinden. Bei Erstellen eines Peerings virtueller Netzwerke in verschiedenen Abonnements müssen die Abonnements demselben Azure Active Directory-Mandanten zugeordnet sein. Wenn Sie noch keinen Azure Active Directory-Mandanten haben, können Sie [schnell einen erstellen](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Wenn Sie virtuelle Netzwerke verbinden müssen, die beide mit dem klassischen Bereitstellungsmodell erstellt wurden, sich in verschiedenen Azure-Regionen befinden oder in Abonnements vorhanden sind, die verschiedenen Azure Active Directory-Mandanten zugeordnet sind, können Sie ein Azure [VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) verwenden. 

> [!WARNING]
> Das Erstellen eines Peerings zwischen virtuellen Netzwerken, die mithilfe unterschiedlicher Azure--Bereitstellungsmodelle in unterschiedlichen Azure-Abonnements erstellt wurden, ist derzeit eine Vorschauversionsfunktion. Peerings virtueller Netzwerke, die in diesem Szenario erstellt wurden, bieten ggf. nicht denselben Grad an Verfügbarkeit und Zuverlässigkeit wie Peerings virtueller Netzwerke in Versionen mit allgemeiner Verfügbarkeit. Für in diesem Szenario erstellte Peerings virtueller Netzwerke wird kein Support geboten. Sie weisen eingeschränkte Funktionen auf und stehen ggf. nicht in allen Azure-Regionen zur Verfügung. Aktuelle Hinweise zur Verfügbarkeit und zum Status dieses Features finden Sie auf der Seite [Azure-Updates](https://azure.microsoft.com/updates/?product=virtual-network) .

Zum Erstellen eines Peerings virtueller Netzwerke können Sie das [Azure-Portal](#portal), die [Azure CLI](#cli) (Befehlszeilenschnittstelle) oder [Azure PowerShell](#powershell) verwenden. Klicken Sie auf einen der vorherigen Tool-Links. So gelangen Sie direkt zu den Anleitungen zum Erstellen von Peerings in virtuellen Netzwerken mit dem Tool Ihrer Wahl.

## <a name="register"></a>Registrieren für die Vorschauversion

Um sich für die Vorschauversion zu registrieren, führen Sie die folgenden Schritte für beide Abonnements aus, in denen sich die virtuellen Netzwerke für das Peering befinden. PowerShell wird das einzige Tool, das Sie zum Registrieren für die Vorschauversion verwenden können.

1. Installieren Sie die neueste Version des PowerShell-Moduls [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Sie noch nicht mit Azure PowerShell vertraut sind, lesen Sie die [Übersicht über Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Starten Sie eine PowerShell-Sitzung, und melden Sie sich mit dem Befehl `login-azurermaccount` bei Azure an.
3. Registrieren Sie Ihr Abonnement für die Vorschauversion, indem Sie die folgenden Befehle eingeben:

    ```powershell
    Register-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
    Führen Sie die Schritte in den Abschnitten in diesem Artikel zum Portal, zur Azure CLI oder zu PowerShell erst durch, sobald die Ausgabe von **RegistrationState**, die Sie nach Eingabe des folgenden Befehls erhalten, für beide Abonnements **Registered** lautet:

    ```powershell    
    Get-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    ```

## <a name="portal"></a>Erstellen eines Peerings: Azure-Portal

In diesem Tutorial werden unterschiedliche Konten für jedes Abonnement verwendet. Wenn Sie ein Konto verwenden, das über Berechtigungen für beide Abonnements verfügt, können Sie dasselbe Konto für alle Schritte verwenden. Überspringen Sie die Schritte zum Abmelden vom Portal und zum Zuweisen weiterer Benutzerberechtigungen für die virtuellen Netzwerke. Ehe Sie sich mit den folgenden Schritten beschäftigen, müssen Sie sich für die Vorschauversion registrieren. Führen Sie zur Registrierung die Schritte im Abschnitt [Registrieren für die Vorschauversion](#register) in diesem Artikel aus. Fahren Sie erst mit den verbleibenden Schritten fort, nachdem beide Abonnements für die Vorschauversion registriert wurden.
 
1. Melden Sie sich als UserA beim [Azure-Portal](https://portal.azure.com) an. Das Konto, mit dem Sie sich anmelden, muss über die Berechtigungen verfügen, die zum Erstellen eines Peerings virtueller Netzwerke erforderlich sind. Weitere Informationen finden Sie im Abschnitt [Berechtigungen](#permissions) dieses Artikels.
2. Klicken Sie auf **+ Neu**, dann auf **Network**, und klicken Sie zuletzt auf **Virtuelles Netzwerk**.
3. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die Werte für die folgenden Einstellungen an, oder wählen Sie sie aus, und klicken Sie anschließend auf **Erstellen**:
    - **Name**: *myVnetA*
    - **Adressraum**: *10.0.0.0/16*
    - **Subnetzname**: *Standard*
    - **Subnetzadressbereich**: *10.0.0.0/24*
    - **Abonnement**: Wählen Sie Abonnement A aus.
    - *Ressourcengruppe*: Wählen Sie **Neu erstellen** aus, und geben Sie **myResourceGroupA** ein.
    - **Standort**: *USA, Osten*
4. Geben Sie im oben im Portal *myVnetA* im Feld **Ressourcen suchen** ein. Klicken Sie auf **myVnetA**, sobald es in den Suchergebnissen angezeigt wird. Für das virtuelle Netzwerk **myVnetA** wird ein Blatt angezeigt.
5. Klicken Sie auf dem eingeblendeten Blatt **myVnetA** links in der vertikalen Liste der Optionen auf **Zugriffssteuerung (IAM)**.
6. Klicken Sie auf dem eingeblendeten Blatt **myVnetA – Zugriffssteuerung (IAM)** auf **+ Hinzufügen**.
7. Wählen Sie auf dem sich öffnenden Blatt **Berechtigungen hinzufügen** im Feld **Rolle** die Option **Netzwerkmitwirkender** aus.
8. Wählen Sie im Feld **Auswählen** UserB aus, oder geben Sie die E-Mail-Adresse von UserB ein, um ihn zu suchen. Die Liste der angezeigten Benutzer stammt aus dem gleichen Azure Active Directory-Mandanten, in dem sich auch das virtuelle Netzwerk befindet, für das Sie das Peering erstellen. Klicken Sie auf UserB, sobald er in der Liste angezeigt wird.
9. Klicken Sie auf **Speichern**.
10. Melden Sie sich als UserA vom Portal ab, und melden Sie sich als UserB an.
11. Klicken Sie auf **+ Neu**, geben Sie *Virtuelles Netzwerk*  in das Feld **Marketplace durchsuchen** ein, und klicken Sie dann in den Suchergebnissen auf **Virtuelles Netzwerk**.
12. Wählen Sie auf dem eingeblendeten Blatt **Virtuelles Netzwerk** im Feld **Bereitstellungsmodell auswählen** die Option **Klassisch** aus, und klicken Sie dann auf **Erstellen**.
13.   Geben Sie auf dem Blatt „Virtuelles Netzwerk erstellen (klassisch)“ die folgenden Werte ein:

    - **Name**: *myVnetB*
    - **Adressraum**: *10.1.0.0/16*
    - **Subnetzname**: *Standard*
    - **Subnetzadressbereich**: *10.1.0.0/24*
    - **Abonnement**: Wählen Sie Abonnement B aus.
    - *Ressourcengruppe*: Wählen Sie **Neu erstellen** aus, und geben Sie **myResourceGroupB** ein.
    - **Standort**: *USA, Osten*

14. Geben Sie im oben im Portal *myVnetB* im Feld **Ressourcen suchen** ein. Klicken Sie auf **myVnetB**, wenn es in den Suchergebnissen angezeigt wird. Für das virtuelle Netzwerk **myVnetB** wird ein Blatt angezeigt.
15. Klicken Sie auf dem eingeblendeten Blatt **myVnetB** links in der vertikalen Liste der Optionen auf **Eigenschaften**. Kopieren Sie die **Ressourcen-ID**, die in einem späteren Schritt verwendet wird. Die Ressourcen-ID lautet ähnlich wie im folgenden Beispiel: /subscriptions/<Susbscription ID>/resourceGroups/myResoureGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
16. Führen Sie die Schritte 5-9 für myVnetB aus, und geben Sie in Schritt 8 **UserA** ein.
17. Melden Sie sich als UserB vom Portal ab, und melden Sie sich als UserA an.
18. Geben Sie im oben im Portal *myVnetA* im Feld **Ressourcen suchen** ein. Klicken Sie auf **myVnetA**, sobald es in den Suchergebnissen angezeigt wird. Für das virtuelle Netzwerk **myVnet** wird ein Blatt angezeigt.
19. Klicken Sie auf **myVnetA**.
20. Klicken Sie auf dem eingeblendeten Blatt **myVnetA** links in der vertikalen Liste der Optionen auf **Peerings**.
21. Klicken Sie auf dem eingeblendeten Blatt **myVnetA – Peerings** auf **+ Hinzufügen**
22. Geben Sie auf dem sich öffnenden Blatt **Peering hinzufügen** die folgenden Optionen ein, oder wählen Sie sie aus, und klicken Sie auf **OK**:
     - **Name**: *myVnetAToMyVnetB*
     - **Bereitstellungsmodell für das virtuelle Netzwerk**: Wählen Sie **Klassisch** aus.
     - **Ich kenne meine Ressourcen-ID**: Aktivieren Sie dieses Kontrollkästchen.
     - **Ressourcen-ID**: Geben Sie die Ressourcen-ID für myVnetB aus Schritt 15 ein.
     - **Zugriff auf virtuelles Netzwerk zulassen**: Vergewissern Sie sich, dass **Aktiviert** ausgewählt ist.
    In diesem Tutorial werden keine anderen Einstellungen verwendet. Weitere Informationen zu allen Peeringeinstellungen finden Sie unter [Create a peering (Erstellen eines Peerings)](virtual-network-manage-peering.md#create-a-peering).
23. Nachdem Sie zuvor auf **OK** geklickt haben, wird das Blatt **Peering hinzufügen** geschlossen, und Sie gelangen wieder zum Blatt **myVnetA – Peerings**. Nach wenigen Sekunden wird das von Ihnen erstellte Peering auf dem Blatt angezeigt. In der Spalte **Peeringstatus** wird für das Peering **myVnetAToMyVnetB** der Status **Verbunden** angezeigt. Das Peering ist nun hergestellt. Es ist nicht erforderlich, ein Peering zwischen dem virtuellen Netzwerk (klassisch) und dem virtuellen Netzwerk (Ressourcen-Manager) zu erstellen.

    Alle Azure-Ressourcen, die Sie in einem der virtuellen Netzwerke erstellen, sind in der Lage, miteinander über ihre IP-Adressen zu kommunizieren. Wenn Sie die standardmäßige Azure-Namensauflösung für virtuelle Netzwerke verwenden, können die Ressourcen in den virtuellen Netzwerken Namen nicht netzwerkübergreifend auflösen. Wenn Sie Namen netzwerkübergreifend in einem Peering auflösen möchten, müssen Sie einen eigenen DNS-Server erstellen. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

24. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
25. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-portal) dieses Artikels aus.

## <a name="cli"></a>Erstellen eines Peerings: Azure CLI

In diesem Tutorial werden unterschiedliche Konten für jedes Abonnement verwendet. Wenn Sie ein Konto verwenden, das über Berechtigungen für beide Abonnements verfügt, können Sie dasselbe Konto für alle Schritte verwenden. Überspringen Sie die Schritte zum Abmelden von Azure und zum Entfernen der Skriptzeilen zum Erstellen von Benutzerrollenzuweisungen. Ersetzen Sie UserA@azure.com und UserB@azure.com in allen folgenden Skripts durch die Benutzernamen, die Sie für UserA und UserB verwenden. 

Ehe Sie sich mit den folgenden Schritten beschäftigen, müssen Sie sich für die Vorschauversion registrieren. Führen Sie zur Registrierung die Schritte im Abschnitt [Registrieren für die Vorschauversion](#register) in diesem Artikel aus. Fahren Sie erst mit den verbleibenden Schritten fort, nachdem beide Abonnements für die Vorschauversion registriert wurden.

1. [Installieren](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Sie die Azure CLI 1.0 zum Erstellen des virtuellen Netzwerks (klassisch).
2. Starten Sie eine CLI-Sitzung, und melden Sie sich mit dem Befehl `azure login` als UserB bei Azure an.
3. Führen Sie die CLI in Dienstverwaltungsmodus aus, indem Sie den Befehl `azure config mode asm` eingeben.
4. Geben Sie den folgenden Befehl ein, um das virtuelle Netzwerk (klassisch) zu erstellen:
 
    ```azurecli
    azure network vnet create --vnet myVnetB --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```
5. Die restlichen Schritte müssen mithilfe einer Bash-Shell mit [installierter](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure CLI 2.0.4 oder höher oder Azure Cloud Shell ausgeführt werden. Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure CLI ist vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Klicken Sie in den folgenden Skripts auf die Schaltfläche **Ausprobieren**, um eine Cloud Shell zu öffnen, über die Sie sich bei Ihrem Azure-Konto anmelden. Optionen zum Ausführen von Bash CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 
6. Kopieren Sie das folgende Skript in einen Texteditor auf Ihrem PC. Ersetzen Sie `<SubscriptionB-Id>` durch Ihre Abonnement-ID. Wenn Sie Ihre Abonnement-ID nicht kennen, geben Sie den Befehl `az account show` ein. Der Wert für **id** in der Ausgabe ist Ihre Abonnement-ID. Kopieren Sie das geänderte Skript, fügen Sie es in Ihre CLI 2.0-Sitzung ein, und drücken Sie dann `Enter`. 

    ```azurecli-interactive
    az role assignment create \
      --assignee UserA@azure.com \
      --role "Classic Network Contributor" \
      --scope /subscriptions/<SubscriptionB-Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

    Wenn Sie das virtuelle Netzwerk (klassisch) in Schritt 4 eingerichtet haben, erstellt Azure das virtuelle Netzwerk in der Ressourcengruppe *Default-Networking*.
7. Melden Sie UserB von Azure ab, und melden Sie sich als UserA bei der CLI 2.0 an.
8. Erstellen Sie eine Ressourcengruppe und ein virtuelles Netzwerk (Ressourcen-Manager). Kopieren Sie das folgende Skript, fügen Sie es in Ihre CLI-Sitzung ein, und drücken Sie dann `Enter`. 

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout the script.
    rgName="myResourceGroupA"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network A (Resource Manager).
    az network vnet create \
      --name myVnetA \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Get the id for myVnetA.
    vNetAId=$(az network vnet show \
      --resource-group $rgName \
      --name myVnetA \
      --query id --out tsv)

    # Assign UserB permissions to myVnetA.
    az role assignment create \
      --assignee UserB@azure.com \
      --role "Network Contributor" \
      --scope $vNetAId
    ```

9. Erstellen Sie ein Peering zwischen zwei virtuellen Netzwerken, die mit verschiedenen Bereitstellungsmodellen erstellt wurden. Kopieren Sie das folgende Skript in einen Texteditor auf Ihrem PC. Ersetzen Sie `<SubscriptionB-id>` durch Ihre Abonnement-ID. Wenn Sie Ihre Abonnement-ID nicht kennen, geben Sie den Befehl `az account show` ein. Der Wert für **id** in der Ausgabe ist Ihre Abonnement-ID. Azure erstellt das virtuelle Netzwerk (klassisch), das Sie in Schritt 4 erstellt haben, in einer Ressourcengruppe mit dem Namen*Default-Networking*. Fügen Sie das geänderte Skript in Ihre CLI-Sitzung ein, und drücken Sie dann `Enter`.

    ```azurecli-interactive
    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnetAToMyVnetB \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --remote-vnet-id  /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB \
      --allow-vnet-access
    ```

10. Nachdem das Skript ausgeführt wurde, überprüfen Sie das Peering für das virtuelle Netzwerk (Ressourcen-Manager). Kopieren Sie das folgende Skript, und fügen Sie es in Ihre CLI-Sitzung ein:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --output table
    ```
    Die Ausgabe zeigt in der Spalte **PeeringState** den Status **Connected**.

    Alle Azure-Ressourcen, die Sie in einem der virtuellen Netzwerke erstellen, sind in der Lage, miteinander über ihre IP-Adressen zu kommunizieren. Wenn Sie die standardmäßige Azure-Namensauflösung für virtuelle Netzwerke verwenden, können die Ressourcen in den virtuellen Netzwerken Namen nicht netzwerkübergreifend auflösen. Wenn Sie Namen netzwerkübergreifend in einem Peering auflösen möchten, müssen Sie einen eigenen DNS-Server erstellen. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

11. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
12. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-cli) in diesem Artikel aus.

## <a name="powershell"></a>Erstellen eines Peerings: PowerShell

In diesem Tutorial werden unterschiedliche Konten für jedes Abonnement verwendet. Wenn Sie ein Konto verwenden, das über Berechtigungen für beide Abonnements verfügt, können Sie dasselbe Konto für alle Schritte verwenden. Überspringen Sie die Schritte zum Abmelden von Azure und zum Entfernen der Skriptzeilen zum Erstellen von Benutzerrollenzuweisungen. Ersetzen Sie UserA@azure.com und UserB@azure.com in allen folgenden Skripts durch die Benutzernamen, die Sie für UserA und UserB verwenden. 

Ehe Sie sich mit den folgenden Schritten beschäftigen, müssen Sie sich für die Vorschauversion registrieren. Führen Sie zur Registrierung die Schritte im Abschnitt [Registrieren für die Vorschauversion](#register) in diesem Artikel aus. Fahren Sie erst mit den verbleibenden Schritten fort, nachdem beide Abonnements für die Vorschauversion registriert wurden.

1. Installieren Sie die neueste Version der PowerShell-Module [Azure](https://www.powershellgallery.com/packages/Azure) und [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Sie noch nicht mit Azure PowerShell vertraut sind, lesen Sie die [Übersicht über Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Starten Sie eine PowerShell-Sitzung.
3. Melden Sie sich in PowerShell beim Abonnement von UserB durch Eingeben des Befehls `Add-AzureAccount` als UserB an.
4. Um ein virtuelles Netzwerk (klassisch) mit PowerShell zu erstellen, müssen Sie eine neue Netzwerkkonfigurationsdatei erstellen oder eine vorhandene ändern. Erfahren Sie, wie Sie [Netzwerkkonfigurationsdateien exportieren, aktualisieren und importieren](virtual-networks-using-network-configuration-file.md). Die Datei muss das folgende **VirtualNetworkSite**-Element für das in diesem Tutorial verwendete virtuelle Netzwerk enthalten:

    ```xml
    <VirtualNetworkSite name="myVnetB" Location="East US">
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

5. Melden Sie beim Abonnement von UserB durch Eingeben des Befehls `login-azurermaccount` als UserB zum Verwenden von Ressourcen-Manager-Befehlen an.
6. Weisen Sie dem virtuellen Netzwerk B die Berechtigungen von UserA zu. Kopieren Sie das folgende Skript in einen Text-Editor auf Ihrem PC, und ersetzen Sie `<SubscriptionB-id>` durch die ID von Abonnement B. Wenn Sie die Abonnement-ID nicht kennen, geben Sie den Befehl `Get-AzureRmSubscription` ein, um sie anzuzeigen. Der Wert für **Id** in der Ausgabe ist Ihre Abonnement-ID. Azure erstellt das virtuelle Netzwerk (klassisch), das Sie in Schritt 4 erstellt haben, in einer Ressourcengruppe mit dem Namen*Default-Networking*. Um das Skript auszuführen, kopieren Sie das geänderte Skript. Fügen Sie es in PowerShell ein, und drücken Sie dann `Enter`.
    
    ```powershell 
    New-AzureRmRoleAssignment `
      -SignInName UserA@azure.com `
      -RoleDefinitionName "Classic Network Contributor" `
      -Scope /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

7. Melden Sie von Azure als UserB ab, und melden Sie durch Eingeben des Befehls `login-azurermaccount` beim Abonnement von UserA als UserA an. Das Konto, mit dem Sie sich anmelden, muss über die Berechtigungen verfügen, die zum Erstellen eines Peerings virtueller Netzwerke erforderlich sind. Weitere Informationen finden Sie im Abschnitt [Berechtigungen](#permissions) dieses Artikels.
8. Erstellen Sie das virtuelle Netzwerk (Ressourcen-Manager), indem Sie das folgende Skript kopieren, es in PowerShell einfügen und dann `Enter` drücken:

    ```powershell
    # Variables for common values
      $rgName='MyResourceGroupA'
      $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network A.
    $vnetA = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location
    ```

9. Weisen Sie myVnetA die Berechtigungen von UserB zu. Kopieren Sie das folgende Skript in einen Text-Editor auf Ihrem PC, und ersetzen Sie `<SubscriptionA-Id>` durch die ID von Abonnement A. Wenn Sie die Abonnement-ID nicht kennen, geben Sie den Befehl `Get-AzureRmSubscription` ein, um sie anzuzeigen. Der Wert für **Id** in der Ausgabe ist Ihre Abonnement-ID. Fügen Sie die geänderte Version des Skripts in PowerShell ein, und drücken Sie dann `Enter`, um es auszuführen.

    ```powershell
    New-AzureRmRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

10. Kopieren Sie das folgende Skript in einen Text-Editor auf Ihrem PC, und ersetzen Sie `<SubscriptionB-id>` durch die ID von Abonnement B. Um ein Peering zwischen myVnetA und myVnetB herzustellen, kopieren Sie das geänderte Skript, fügen Sie es in PowerShell ein, und drücken Sie `Enter`.

    ```powershell
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vnetA `
      -RemoteVirtualNetworkId /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

11. Zeigen Sie den Peeringstatus von myVnetA an, indem Sie das folgende Skript kopieren, es in PowerShell einfügen und `Enter` drücken.

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName $rgName `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Der Status ist **Verbunden**. Er ändert sich in **Verbunden**, sobald Sie das Peering von myVnetA mit myVnetB eingerichtet haben.

    Alle Azure-Ressourcen, die Sie in einem der virtuellen Netzwerke erstellen, sind in der Lage, miteinander über ihre IP-Adressen zu kommunizieren. Wenn Sie die standardmäßige Azure-Namensauflösung für virtuelle Netzwerke verwenden, können die Ressourcen in den virtuellen Netzwerken Namen nicht netzwerkübergreifend auflösen. Wenn Sie Namen netzwerkübergreifend in einem Peering auflösen möchten, müssen Sie einen eigenen DNS-Server erstellen. Weitere Informationen finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

12. **Optional**: Obwohl das Erstellen virtueller Computer in diesem Tutorial nicht behandelt wird, können Sie in jedem virtuellen Netzwerk virtuelle Computer erstellen und sie miteinander verbinden, um die Verbindung zu überprüfen.
13. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-powershell) in diesem Artikel aus.

## <a name="permissions"></a>Berechtigungen

Die Konten, mit denen Sie das Peering in virtuellen Netzwerken erstellt haben, müssen die erforderliche Rolle oder die erforderlichen Berechtigungen haben. Wenn Sie beispielsweise ein Peering zwischen den virtuellen Netzwerken myVnetA und myVnetB erstellen, muss Ihr Konto mindestens die folgende Rolle oder die folgenden Berechtigungen für jedes virtuelle Netzwerk haben:
    
|Virtuelles Netzwerk|Bereitstellungsmodell|Rolle|Berechtigungen|
|---|---|---|---|
|myVnetA|Ressourcen-Manager|[Mitwirkender von virtuellem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klassisch|[Mitwirkender von klassischem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|–|
|myVnetB|Ressourcen-Manager|[Mitwirkender von virtuellem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klassisch|[Mitwirkender von klassischem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Erfahren Sie mehr über [integrierte Rollen](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) und das Zuweisen bestimmter Berechtigungen zu [benutzerdefinierten Rollen](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (nur für Resource Manager).

## <a name="delete"></a>Löschen von Ressourcen
Wenn Sie dieses Tutorial abgeschlossen haben, möchten Sie die Ressourcen, die Sie in diesem Tutorial erstellt haben, möglicherweise wieder löschen, damit keine Nutzungsgebühren anfallen. Durch das Löschen einer Ressourcengruppe werden auch alle Ressourcen in dieser Ressourcengruppe gelöscht.

### <a name="delete-portal"></a>Azure-Portal

1. Geben Sie in das Suchfeld im Portal **myResourceGroupA** ein. Klicken Sie in den Suchergebnissen auf **myResourceGroupA**.
2. Klicken Sie auf dem Blatt **myResourceGroupA** auf das **Löschsymbol**.
3. Um den Löschvorgang zu bestätigen, geben Sie in das Feld **Geben Sie den Ressourcengruppennamen ein** den Namen **myResourceGroupA** ein, und klicken Sie dann auf **Löschen**.
4. Geben Sie im oben im Portal *myVnetB* im Feld **Ressourcen suchen** ein. Klicken Sie auf **myVnetB**, wenn es in den Suchergebnissen angezeigt wird. Für das virtuelle Netzwerk **myVnetB** wird ein Blatt angezeigt.
5. Klicken Sie auf dem Blatt **myVnetB** auf **Löschen**.
6. Klicken Sie im Feld **Virtuelles Netzwerk löschen** auf **Ja**, um das Löschen zu bestätigen.

### <a name="delete-cli"></a>Azure CLI

1. Melden Sie sich bei Azure mithilfe der CLI 2.0 zum Löschen des virtuellen Netzwerks (Ressourcen-Manager) mit dem folgenden Befehl an:

    ```azurecli-interactive
    az group delete --name myResourceGroupA --yes
    ```

2. Melden Sie sich bei Azure mithilfe der CLI 1.0 zum Löschen des virtuellen Netzwerks (klassisch) mit den folgenden Befehlen an:

    ```azurecli
    azure config mode asm 

    azure network vnet delete --vnet myVnetB --quiet
    ```

### <a name="delete-powershell"></a>PowerShell

1. Geben Sie an der PowerShell-Eingabeaufforderung den folgenden Befehl zum Löschen des virtuellen Netzwerks (Ressourcen-Manager) an:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupA -Force
    ```

2. Um ein virtuelles Netzwerk (klassisch) mit PowerShell zu löschen, müssen Sie eine vorhandene Netzwerkkonfigurationsdatei ändern. Erfahren Sie, wie Sie [Netzwerkkonfigurationsdateien exportieren, aktualisieren und importieren](virtual-networks-using-network-configuration-file.md). Entfernen Sie das folgende „VirtualNetworkSite“-Element für das in diesem Tutorial verwendete virtuelle Netzwerk:

    ```xml
    <VirtualNetworkSite name="myVnetB" Location="East US">
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
