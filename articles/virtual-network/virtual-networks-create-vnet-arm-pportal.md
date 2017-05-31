---
title: Erstellen eines virtuellen Azure-Netzwerks | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein virtuelles Netzwerk mit mehreren Subnetzen erstellen.
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: jdial
ms.custom: 
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: 19857ad1e970ad32359708ded320c53a4778ef8c
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="create-a-virtual-network-with-multiple-subnets"></a>Erstellen eines virtuellen Netzwerks mit mehreren Subnetzen

In diesem Tutorial erfahren Sie, wie Sie ein einfaches virtuelles Azure-Netzwerk (VNet) mit separaten öffentlichen und privaten Subnetzen erstellen. Sie können Azure-Ressourcen wie virtuelle Computer (VMs), App Service-Umgebungen, VM-Skalierungsgruppen, HDInsight und Cloud Services mit Subnetzen verbinden. Ressourcen, die mit VNets verbunden sind, können über das private Azure-Netzwerk miteinander kommunizieren.

Die folgenden Abschnitte enthalten Schritte zum Bereitstellen eines Azure VNet mit dem [Azure-Portal](#portal), der [Azure-Befehlszeilenschnittstelle](#cli) (CLI), Azure [PowerShell](#powershell) und einer Azure Resource Manager-[Vorlage](#template). Das Ergebnis ist unabhängig davon identisch, mit welchem Tool Sie das VNet bereitstellen. Über einen Klick auf den Link des jeweiligen Tools gelangen Sie direkt zu diesem Abschnitt des Artikels. Weitere Informationen zu allen VNet- und Subnetzeinstellungen finden Sie in den Artikeln [Verwalten von VNets](virtual-network-manage-network.md) und [Verwalten von Subnetzen](virtual-network-manage-subnet.md).

## <a name="portal"></a>Azure-Portal

1. Navigieren Sie in einem Internetbrowser zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) registrieren.
2. Klicken Sie im Portal auf **+ Neu** > **Netzwerk** > **Virtuelles Netzwerk**.
3. Lassen Sie auf dem eingeblendeten Blatt **Virtuelles Netzwerk** unter **Bereitstellungsmodell auswählen** die Option *Resource Manager* ausgewählt, und klicken Sie auf **Erstellen**.
4. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die folgenden Werte ein, und klicken Sie anschließend auf die Schaltfläche **Erstellen**:

    |Einstellung|Wert|
    |---|---|
    |Name|*MyVnet*|
    |Adressraum|*10.0.0.0/16*|
    |Subnetzname|Öffentlich|
    |Subnetzadressbereich|*10.0.0.0/24*|
    |Ressourcengruppe|Lassen Sie **Neu erstellen** ausgewählt, und geben Sie *MyResourceGroup* ein.|
    |Abonnement und Standort|Wählen Sie Ihr Abonnement und Ihren Standort aus.

    Wenn Sie noch nicht mit Azure vertraut sind, informieren Sie sich über [Ressourcengruppen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [Abonnements](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) und [Standorte](https://azure.microsoft.com/regions) (die auch als Regionen bezeichnet werden).
6. Im Portal können Sie beim Erstellen eines VNet nur ein Subnetz erstellen. In diesem Tutorial wird nach dem Erstellen des VNet ein zweites Subnetz erstellt. Sie können später über das Internet zugängliche Ressourcen mit dem Subnetz *Public* verbinden. Außerdem können Sie Ressourcen, auf die aus dem Internet nicht zugegriffen werden kann, mit einem privaten Subnetz verbinden. Geben Sie zum Erstellen des zweiten Subnetzes oben im Portal *MyVnet* in das Feld *Ressourcen suchen* ein. Wenn **MyVnet** in den Suchergebnissen angezeigt wird, klicken Sie darauf. Wenn Ihr Abonnement mehrere VNets mit demselben Namen enthält, werden unter jedem VNet mit demselben Namen die Namen von Ressourcengruppen aufgeführt. Klicken Sie unbedingt auf das Ergebnis „MyVnet“ mit *MyResourceGroup* darunter.
7. Klicken Sie auf dem eingeblendeten Blatt **MyVnet** unter **EINSTELLUNGEN** auf **Subnetze**.
8. Klicken Sie auf dem Blatt **MyVnet: Subnetze** auf **+ Subnetz**.
9. Geben Sie *Private* in **Name** und *10.0.1.0/24* in **Adressbereich** auf dem Blatt **Subnetz hinzufügen** ein, und klicken Sie dann auf **OK**.
10. Überprüfen Sie die Subnetze auf dem Blatt **MyVnet: Subnetze**. Die von Ihnen erstellten Subnetze **Public** und **Private** werden angezeigt.
11. **Optional:** Zum Löschen der Ressourcen, die in diesem Tutorial erstellt werden, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-portal) dieses Artikels aus.

## <a name="cli"></a>Befehlszeilenschnittstelle (CLI)
Obwohl CLI-Befehle unter Windows, Linux oder macOS identisch sind, gibt es bei den Betriebssystemshells Unterschiede bei der Skripterstellung. Die folgenden Anweisungen gelten für das Ausführen eines Bash-Skripts, das CLI-Befehle enthält:

1. Navigieren Sie in einem Internetbrowser zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) registrieren.
2. Klicken Sie rechts oben im Portal auf die Leiste *Ressourcen suchen* und dann auf das Symbol **>_**, um eine Bash Azure Cloud Shell (Vorschauversion) zu öffnen. Der Cloud Shell-Bereich wird unten im Portal angezeigt. Nach wenigen Sekunden wird die Eingabeaufforderung **username@Azure:~$** angezeigt. Die Cloud Shell meldet Sie automatisch bei Azure mit den Anmeldeinformationen an, mit denen Sie sich beim Portal authentifiziert haben.
3. Kopieren Sie in Ihrem Browser das folgende Skript:
    ```azurecli
    #!/bin/bash
    
    # Create a resource group.
    az group create \
      --name MyResourceGroup \
      --location eastus
    
    # Create a virtual network with one subnet.
    az network vnet create \
      --name MyVnet \
      --resource-group MyResourceGroup \
      --subnet-name Public
    
    # Create an additional subnet within the VNet.
    az network vnet subnet create \
      --name Private \
      --address-prefix 10.0.1.0/24 \
      --vnet-name MyVnet \
      --resource-group MyResourceGroup
    ```
4. Erstellen Sie eine Skriptdatei, und speichern Sie sie. Geben Sie an der Cloud Shell-Eingabeaufforderung `nano myscript.sh --nonewlines` ein. Der Befehl startet den GNU Nano-Editor mit einer leeren Datei „myscript.sh“. Platzieren Sie die Maus im Editorfenster, klicken Sie mit der rechten Maustaste, und klicken Sie auf **Einfügen**. Cloud Shell-Speicher wird nicht sitzungsübergreifend beibehalten. Wenn Sie das Skript Cloud Shell-sitzungsübergreifend beibehalten möchten, richten Sie [persistenten Speicher](../cloud-shell/persisting-shell-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json) für Cloud Shell ein. 
5. Halten Sie auf der Tastatur die Tasten **STRG+X** gedrückt. Geben Sie dann **Y** ein, und drücken Sie die **EINGABETASTE**, um die Datei als „myscript.sh“ zu speichern.
6. Markieren Sie an der Cloud Shell-Eingabeaufforderung die Datei mit dem Befehl `chmod +x myscript.sh` als ausführbar.
7. Führen Sie das Skript durch Eingeben von `./myscript.sh` aus.
8. Sobald das Skript abgeschlossen wurde, überprüfen Sie die Subnetze für das VNet, indem Sie den folgenden Befehl kopieren und an der Bash Cloud Shell einfügen:
    ```azurecli
    az network vnet subnet list --resource-group MyResourceGroup --vnet-name MyVnet --output table
    ```
9. **Optional:** Zum Löschen der Ressourcen, die in diesem Tutorial erstellt werden, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-cli) dieses Artikels aus.

## <a name="powershell"></a>PowerShell
1. Installieren Sie die neueste Version des Azure PowerShell-Moduls [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Azure PowerShell für Sie neu ist, lesen Sie den Artikel [Übersicht über Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Starten Sie eine PowerShell-Sitzung durch Klicken auf die Windows-Schaltfläche „Start“. Geben Sie **powershell** ein, und klicken Sie dann in den Suchergebnissen auf **PowerShell**.
3. Geben Sie im PowerShell-Fenster den Befehl `login-azurermaccount` ein, um sich mit Ihrem Azure-[Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) anzumelden. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) registrieren.
4. Kopieren Sie in Ihrem Browser das folgende Skript:
    ```powershell
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name MyResourceGroup `
      -Location eastus
    
    # Create two subnets
    $Subnet1 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Public `
      -AddressPrefix 10.0.0.0/24
    $Subnet2 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Private `
      -AddressPrefix 10.0.1.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName MyResourceGroup `
      -Location eastus `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet1,$Subnet2
    #
    ```
5. Um das Skript auszuführen, klicken Sie im PowerShell-Fenster mit der rechten Maustaste.
6. Überprüfen Sie die Subnetze für das VNet, indem Sie den folgenden Befehl kopieren und in Ihr PowerShell-Fenster einfügen:
    ```powershell
    $Vnet = $Vnet.subnets | Format-Table Name, AddressPrefix
    ```
7. **Optional:** Zum Löschen der Ressourcen, die in diesem Tutorial erstellt werden, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-powershell) dieses Artikels aus.

## <a name="template"></a>Vorlage

Sie können ein VNet mit einer Azure Resource Manager-Vorlage bereitstellen. Weitere Informationen zu Vorlagen finden Sie im Artikel mit der [Übersicht über Azure Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#template-deployment). Um auf die Vorlage zuzugreifen und mehr über ihre Parameter zu erfahren, wechseln Sie zur Website mit Informationen zum [Erstellen eines VNet mit zwei Subnetzen](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/). Sie können die Vorlage über das [Portal](#template-portal), die [CLI](#template-cli) oder [PowerShell](#template-powershell) bereitstellen.

**Optional:** Zum Löschen der Ressourcen, die in diesem Tutorial erstellt werden, führen Sie die Schritte in einem der Unterabschnitte des Abschnitts [Löschen von Ressourcen](#delete) dieses Artikels aus.

### <a name="template-portal"></a>Portal

1. Öffnen Sie in Ihrem Browser die Vorlage [Webseite](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets).
2. Klicken Sie auf die Schaltfläche **In Azure bereitstellen**, über die die Anmeldeseite für das Azure Portal geöffnet wird.
3. Melden Sie sich beim Portal mit Ihrem Azure-[Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) registrieren.
4. Geben Sie die folgende Werte für die Parameter ein:

    |Parameter|Wert|
    |---|---|
    |Abonnement|Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe|MyResourceGroup|
    |Ort|Wählen Sie einen Standort aus.|
    |VNet-Name|MyVnet|
    |VNet-Adresspräfix|10.0.0.0/16|
    |Subnet1Prefix|10.0.0.0/24|
    |Subnet1Name|Public|
    |Subnet2Prefix|10.0.1.0/24|
    |Subnet2Name|Private|

5. Stimmen Sie den Geschäftsbedingungen zu, und klicken Sie anschließend auf **Kaufen**, um das VNet bereitzustellen.

### <a name="template-cli"></a>CLI

1. Navigieren Sie in einem Internetbrowser zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) registrieren.
2. Klicken Sie rechts oben im Portal auf die Leiste *Ressourcen suchen* und dann auf das Symbol **>_**, um eine Bash Azure Cloud Shell (Vorschauversion) zu öffnen. Der Cloud Shell-Bereich wird unten im Portal angezeigt. Nach wenigen Sekunden wird die Eingabeaufforderung **username@Azure:~$** angezeigt. Die Cloud Shell meldet Sie automatisch bei Azure mit den Anmeldeinformationen an, mit denen Sie sich beim Portal authentifiziert haben.
3. Sie können durch Eingabe des folgenden Befehls eine Ressourcengruppe für das VNet erstellen:  `az group create --name MyResourceGroup --location eastus`
4. Sie können die Vorlage mit dem folgenden Befehl bereitstellen:
    - **Standardparameterwerte:** Geben Sie den folgenden Befehl ein:   `az group deployment create --resource-group MyResourceGroup --name VnetTutorial --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json`
    - **Benutzerdefinierte Parameterwerte:** Laden Sie die Vorlage herunter, und ändern Sie sie, bevor Sie sie bereitstellen. Stellen Sie die Vorlage mit über die Befehlszeile eingegebenen Parametern oder mithilfe einer getrennten Parameterdatei bereit. Sie können die Vorlagen- und Parameterdateien herunterladen, indem Sie auf der Webseite zum [Erstellen eines VNet mit zwei Subnetzen](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) auf die Schaltfläche **Auf GitHub suchen** klicken. Klicken Sie im GitHub auf die Datei **azuredeploy.parameters.json** oder **azuredeploy.json** und dann auf die Schaltfläche **Raw** für die Datei. Kopieren Sie in Ihrem Browser den Inhalt, und speichern Sie ihn in einer Datei auf Ihrem Computer. Ändern Sie die Parameterwerte in der Vorlage, oder stellen Sie die Vorlage mit einer getrennten Parameterdatei bereit.  

    Um mehr über das Bereitstellen von Vorlagen mit diesen Methoden zu erfahren, geben Sie `az group deployment create --help` ein.

### <a name="template-powershell"></a>PowerShell

1. Installieren Sie die neueste Version des Azure PowerShell-Moduls [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Azure PowerShell für Sie neu ist, lesen Sie den Artikel [Übersicht über Azure PowerShell](/azure/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Starten Sie eine PowerShell-Sitzung durch Klicken auf die Windows-Schaltfläche „Start“. Geben Sie **powershell** ein, und klicken Sie dann in den Suchergebnissen auf **PowerShell**.
3. Geben Sie im PowerShell-Fenster den Befehl `login-azurermaccount` ein, um sich mit Ihrem Azure-[Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) anzumelden. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) registrieren.
4. Sie können durch Eingabe des folgenden Befehls eine Ressourcengruppe für das VNet erstellen:  `New-AzureRmResourceGroup -Name MyResourceGroup -Location eastus`
5. Sie können die Vorlage mit dem folgenden Befehl bereitstellen:
    - **Standardparameterwerte:** Geben Sie hierfür den folgenden Befehl ein:   `New-AzureRmResourceGroupDeployment -Name VnetTutorial -ResourceGroupName MyResourceGroup -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json`        
    - **Benutzerdefinierte Parameterwerte:** Laden Sie hierfür die Vorlage herunter, und ändern Sie sie, bevor Sie sie bereitstellen. Stellen Sie die Vorlage mit über die Befehlszeile eingegebenen Parametern oder mithilfe einer getrennten Parameterdatei bereit. Sie können die Vorlagen- und Parameterdateien herunterladen, indem Sie auf der Webseite zum [Erstellen eines VNet mit zwei Subnetzen](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) auf die Schaltfläche **Auf GitHub suchen** klicken. Klicken Sie im GitHub auf die Datei **azuredeploy.parameters.json** oder **azuredeploy.json** und dann auf die Schaltfläche **Raw** für die Datei. Kopieren Sie in Ihrem Browser den Inhalt, und speichern Sie ihn in einer Datei auf Ihrem Computer. Ändern Sie die Parameterwerte in der Vorlage, oder stellen Sie die Vorlage mit einer getrennten Parameterdatei bereit.  

    Um mehr über das Bereitstellen von Vorlagen mit diesen Methoden zu erfahren, geben Sie `Get-Help New-AzureRmResourceGroupDeployment` ein. 

## <a name="delete"></a>Löschen von Ressourcen
Nach Abschluss dieses Tutorials empfiehlt es sich, die Ressourcen zu löschen, damit keine Nutzungsgebühren anfallen. Beim Löschen einer Ressourcengruppe werden auch alle darin enthaltenen Ressourcen gelöscht.

### <a name="delete-portal"></a>Portal

1. Beginnen Sie oben im Portal mit der Eingabe von *MyResourceGroup* in das Feld *Ressourcen suchen*. Wenn **MyResourceGroup** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
2. Klicken Sie oben auf dem angezeigten Blatt „MyResourceGroup“ auf das Symbol „Löschen“.
3. Um den Löschvorgang zu bestätigen, geben Sie *MyResourceGroup* in das Feld **Geben Sie den Ressourcengruppennamen ein** ein und klicken dann auf **Löschen**.

### <a name="delete-cli"></a>CLI

Geben Sie an der Cloud Shell-Eingabeaufforderung den folgenden Befehl ein: `az group delete --name MyResourceGroup --yes`

### <a name="delete-powershell"></a>PowerShell

Geben Sie an der PowerShell-Eingabeaufforderung den folgenden Befehl ein: `Remove-AzureRmResourceGroup -Name MyResourceGroup`

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu allen VNet- und Subnetzeinstellungen finden Sie in den Artikeln [Verwalten von VNets](virtual-network-manage-network.md#view-vnet) und [Verwalten von Subnetzen](virtual-network-manage-subnet.md#create-subnet). Es stehen verschiedene Optionen zur Verfügung, die Ihnen das Erstellen von VNets und Subnetzen für Produktionsumgebungen zum Erfüllen unterschiedlicher Anforderungen ermöglichen.
- Filtern Sie ein- und ausgehenden Subnetzdatenverkehr durch Erstellen und Aktivieren von [Netzwerksicherheitsgruppen](virtual-networks-nsg.md) (NSGs) für Subnetze.
- Erstellen Sie eine [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- oder [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-VM, und verbinden Sie sie mit dem VNet.
- Verbinden Sie das VNet mit einem anderen VNet am gleichen Standort über [VNet-Peering](virtual-network-peering-overview.md).
- Verbinden Sie das VNet mit einem lokalen Netzwerk über ein [Standort-zu-Standort-VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (virtuelles privates Netzwerk) oder eine [ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Verbindung.

