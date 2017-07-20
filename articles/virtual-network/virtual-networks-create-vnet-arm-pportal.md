---
title: Erstellen eines virtuellen Azure-Netzwerks mit Subnetzen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein virtuelles Netzwerk mit mehreren Subnetzen in Azure erstellen.
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
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: 1ecfba0470c31d1119b5e6c0f9d8538abe40f834
ms.contentlocale: de-de
ms.lasthandoff: 06/01/2017


---
# <a name="create-a-virtual-network-with-multiple-subnets"></a>Erstellen eines virtuellen Netzwerks mit mehreren Subnetzen

In diesem Tutorial erfahren Sie, wie Sie ein einfaches virtuelles Azure-Netzwerk mit separaten öffentlichen und privaten Subnetzen erstellen. Sie können Azure-Ressourcen wie virtuelle Computer, Azure App Service-Umgebungen, VM-Skalierungsgruppen, Azure HDInsight und andere Clouddienste mit Subnetzen verbinden. Ressourcen, die mit virtuellen Netzwerken verbunden sind, können über das private Azure-Netzwerk miteinander kommunizieren.

In den folgenden Abschnitten werden die Schritte beschrieben, mit denen Sie ein virtuelles Netzwerk über das [Azure-Portal](#portal), die Azure-Befehlszeilenschnittstelle ([Azure-CLI](#azure-cli)), [Azure PowerShell](#powershell) und eine [Azure Resource Manager-Vorlage](#resource-manager-template) bereitstellen können. Das Ergebnis ist identisch, unabhängig davon, mit welchem Tool Sie das virtuelle Netzwerk bereitstellen. Klicken Sie auf den Link für ein Tool, um zum entsprechenden Abschnitt des Tutorials zu wechseln. Weitere Informationen zu allen Einstellungen für virtuelle Netzwerke und Subnetze finden Sie unter [Erstellen, Ändern oder Löschen virtueller Netzwerke](virtual-network-manage-network.md) und [Erstellen, Ändern oder Löschen von Subnetzen virtueller Netzwerke](virtual-network-manage-subnet.md).

## <a name="portal"></a>Azure-Portal

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) in einem Internetbrowser. Melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p)registrieren.
2. Klicken Sie im Portal auf **+ Neu** > **Netzwerk** > **Virtuelles Netzwerk**.
3. Lassen Sie auf dem Blatt **Virtuelles Netzwerk** unter **Bereitstellungsmodell auswählen** die Option **Resource Manager** ausgewählt, und klicken Sie auf **Erstellen**.
4. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die folgenden Werte ein, und klicken Sie anschließend auf **Erstellen**:

    |Einstellung|Wert|
    |---|---|
    |Name|MyVnet|
    |Adressraum|10.0.0.0/16|
    |Subnetzname|Öffentlich|
    |Subnetzadressbereich|10.0.0.0/24|
    |Ressourcengruppe|Lassen Sie **Neu erstellen** ausgewählt, und geben Sie **MyResourceGroup** ein.|
    |Abonnement und Standort|Wählen Sie Ihr Abonnement und Ihren Standort aus.

    Wenn Sie noch nicht mit Azure vertraut sind, informieren Sie sich über [Ressourcengruppen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [Abonnements](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) und [Standorte](https://azure.microsoft.com/regions) (die auch als *Regionen* bezeichnet werden).
6. Im Portal können Sie beim Erstellen eines virtuellen Netzwerks nur ein Subnetz erstellen. In diesem Tutorial erstellen Sie nach dem virtuellen Netzwerk ein zweites Subnetz. Sie können später über das Internet zugängliche Ressourcen mit dem Subnetz **Public** verbinden. Sie können auch andere Ressourcen, auf die nicht über das Internet zugegriffen werden kann, mit dem Subnetz **Private** verbinden. Geben Sie zum Erstellen des zweiten Subnetzes oben auf der Seite **MyVnet** im Feld **Ressourcen suchen** ein. Klicken Sie in den Suchergebnissen auf **MyVnet**. Wenn Ihr Abonnement mehrere virtuelle Netzwerke mit dem gleichen Namen umfasst, überprüfen Sie die Ressourcengruppen, die unter den einzelnen virtuellen Netzwerken aufgeführt werden. Achten Sie darauf, auf das Suchergebnis **MyVnet** mit der Ressourcengruppe **MyResourceGroup** zu klicken.
7. Klicken Sie auf dem eingeblendeten Blatt **MyVnet** unter **EINSTELLUNGEN** auf **Subnetze**.
8. Klicken Sie auf dem Blatt **MyVnet: Subnetze** auf **+ Subnetz**.
9. Geben Sie auf dem Blatt **Subnetz hinzufügen** unter **Name**, den Namen **Private** ein. Geben Sie unter **Adressenbereich** den Wert **10.0.1.0/24** ein.  Klicken Sie auf **OK**.
10. Überprüfen Sie die Subnetze auf dem Blatt **MyVnet: Subnetze**. Die von Ihnen erstellten Subnetze **Public** und **Private** werden angezeigt.
11. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-portal) in diesem Artikel aus.

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Die Azure-CLI-Befehle sind unter Windows, Linux und macOS identisch. Es gibt jedoch Unterschiede bei der Skripterstellung für die Betriebssystemshells. Die folgenden Anweisungen gelten für das Ausführen eines Bash-Skripts, das Azure-CLI-Befehle enthält:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) in einem Internetbrowser. Melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p)registrieren.
2. Klicken Sie oben auf der Portalseite rechts neben dem Feld **Ressourcen suchen** auf das Symbol **>_**, um eine Bash Azure Cloud Shell (Vorschauversion) zu öffnen. Der Cloud Shell-Bereich wird unten im Portal angezeigt. Nach wenigen Sekunden wird eine **username@Azure:~$**-Eingabeaufforderung angezeigt. Cloud Shell meldet Sie automatisch mit den Anmeldeinformationen bei Azure an, mit denen Sie sich beim Portal angemeldet haben.
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
    
    # Create an additional subnet within the virtual network.
    az network vnet subnet create \
      --name Private \
      --address-prefix 10.0.1.0/24 \
      --vnet-name MyVnet \
      --resource-group MyResourceGroup
    ```
4. Erstellen Sie eine Skriptdatei, und speichern Sie sie. Geben Sie an der Cloud Shell-Eingabeaufforderung `nano myscript.sh --nonewlines` ein. Der Befehl startet den GNU Nano-Editor mit einer leeren Datei „myscript.sh“. Platzieren Sie den Mauszeiger im Editorfenster, klicken Sie mit der rechten Maustaste, und klicken Sie dann auf **Einfügen**.  
5. Drücken Sie zum Speichern der Datei als „myscript.sh“ STRG + X, geben Sie **Y** ein, und drücken Sie dann die EINGABETASTE. Im Cloud Shell-Speicher werden gespeicherte Dateien nicht sitzungsübergreifend beibehalten. Wenn Sie das Skript über mehrere Cloud Shell-Sitzungen beibehalten möchten, richten Sie [persistenten Speicher](../cloud-shell/persisting-shell-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json) für Cloud Shell ein, und speichern Sie die Datei in diesem Speicher.
6. Führen Sie an der Cloud Shell-Eingabeaufforderung den Befehl `chmod +x myscript.sh` aus, um die Datei als ausführbar zu kennzeichnen.
7. Geben Sie zum Ausführen des Skripts `./myscript.sh` ein.
8. Um die Subnetze für das virtuelle Netzwerk nach dem Ausführen des Skripts zu überprüfen, kopieren Sie den folgenden Befehl, und fügen Sie ihn im Bash Cloud Shell-Bereich ein:
    ```azurecli
    az network vnet subnet list --resource-group MyResourceGroup --vnet-name MyVnet --output table
    ```
9. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-cli) in diesem Artikel aus.

## <a name="powershell"></a>PowerShell

1. Installieren Sie die neueste Version des PowerShell-Moduls [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Sie noch nicht mit Azure PowerShell vertraut sind, lesen Sie die [Übersicht über Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Um eine PowerShell-Sitzung zu starten, gehen Sie zu **Start**, geben Sie **powershell** ein, und klicken Sie dann auf **PowerShell**.
3. Geben Sie im PowerShell-Fenster den Befehl `login-azurermaccount` ein, um sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) anzumelden.
4. Kopieren Sie in Ihrem Browser das folgende Skript:
    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name MyResourceGroup `
      -Location eastus
    
    # Create two subnets.
    $Subnet1 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Public `
      -AddressPrefix 10.0.0.0/24
    $Subnet2 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Private `
      -AddressPrefix 10.0.1.0/24
    
    # Create a virtual network.
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName MyResourceGroup `
      -Location eastus `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet1,$Subnet2
    #
    ```
5. Klicken Sie zum Ausführen des Skripts im PowerShell-Fenster mit der rechten Maustaste.
6. Um die Subnetze für das virtuelle Netzwerk zu überprüfen, kopieren Sie den folgenden Befehl, und fügen Sie ihn im PowerShell-Fenster ein:
    ```powershell
    $Vnet = $Vnet.subnets | Format-Table Name, AddressPrefix
    ```
7. **Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte im Abschnitt [Löschen von Ressourcen](#delete-powershell) in diesem Artikel aus.

## <a name="resource-manager-template"></a>Resource Manager-Vorlage

Sie können ein virtuelles Netzwerk über eine Azure Resource Manager-Vorlage bereitstellen. Weitere Informationen zu Vorlagen finden Sie in der [Übersicht über Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#template-deployment). Informationen zum Zugriff auf die Vorlage und den Parametern finden Sie auf der Seite zur Vorlage zum [Erstellen eines virtuellen Netzwerks mit zwei Subnetzen](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/). Sie können die Vorlage über das [Portal](#template-portal), die [Azure CLI](#template-cli) oder [PowerShell](#template-powershell) bereitstellen.

**Optional:** Zum Löschen der Ressourcen, die Sie in diesem Tutorial erstellt haben, führen Sie die Schritte in den Unterabschnitten von [Löschen von Ressourcen](#delete) in diesem Artikel aus.

### <a name="template-portal"></a>Azure-Portal

1. Öffnen Sie in Ihrem Browser die [Vorlagenseite](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets).
2. Klicken Sie auf die Schaltfläche **Bereitstellen in Azure**. Dadurch wird die Anmeldeseite für das Azure-Portal geöffnet.
3. Melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) beim Portal an. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p)registrieren.
4. Geben Sie die folgende Werte für die Parameter ein:

    |Parameter|Wert|
    |---|---|
    |Abonnement|Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe|MyResourceGroup|
    |Ort|Standort auswählen|
    |VNet-Name|MyVnet|
    |VNet-Adresspräfix|10.0.0.0/16|
    |Subnet1Prefix|10.0.0.0/24|
    |Subnet1Name|Public|
    |Subnet2Prefix|10.0.1.0/24|
    |Subnet2Name|Private|

5. Stimmen Sie den Geschäftsbedingungen zu, und klicken Sie anschließend auf **Kaufen**, um das virtuelle Netzwerk bereitzustellen.

### <a name="template-cli"></a>Azure-Befehlszeilenschnittstelle

1. Melden Sie sich beim [Portal](https://portal.azure.com) mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p)registrieren.
2. Klicken Sie rechts neben dem Feld **Ressourcen suchen** auf das Symbol **>_**, um eine Bash Azure Cloud Shell (Vorschauversion) zu öffnen. Der Cloud Shell-Bereich wird unten im Portal angezeigt. Nach wenigen Sekunden wird eine **username@Azure:~$**-Eingabeaufforderung angezeigt. Cloud Shell meldet Sie automatisch mit den Anmeldeinformationen bei Azure an, mit denen Sie sich beim Azure-Portal angemeldet haben.
3. Geben Sie den folgenden Befehl ein, um eine Ressourcengruppe für das virtuelle Netzwerk zu erstellen:  `az group create --name MyResourceGroup --location eastus`
4. Sie können die Vorlage mithilfe einer der folgenden Optionen für die Parameter bereitstellen:
    - **Standardwerte für Parameter**. Geben Sie den folgenden Befehl ein:   `az group deployment create --resource-group MyResourceGroup --name VnetTutorial --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json`
    - **Benutzerdefinierte Werte für Parameter**. Laden Sie die Vorlage herunter, und bearbeiten Sie sie, bevor Sie die Vorlage bereitstellen. Sie können die Vorlage auch mithilfe von Parametern in der Befehlszeile bereitstellen oder die Vorlage mit einer separaten Parameterdatei bereitstellen. Klicken Sie zum Herunterladen der Vorlagen- und Parameterdateien auf der Webseite zum [Erstellen eines virtuellen Netzwerks mit zwei Subnetzen](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) auf die Schaltfläche **Auf GitHub suchen**. Klicken Sie in GitHub auf die Datei **azuredeploy.parameters.json** oder **azuredeploy.json**. Klicken Sie dann auf die Schaltfläche **Raw**, um die Datei anzuzeigen. Kopieren Sie im Browser den Inhalt der Datei. Speichern Sie den Inhalt in einer Datei auf Ihrem Computer. Sie können die Parameterwerte in der Vorlage ändern oder die Vorlage mit einer separaten Parameterdatei bereitstellen.  

    Um mehr über das Bereitstellen von Vorlagen mit diesen Methoden zu erfahren, geben Sie `az group deployment create --help` ein.

### <a name="template-powershell"></a>PowerShell

1. Installieren Sie die neueste Version des PowerShell-Moduls [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Sie noch nicht mit Azure PowerShell vertraut sind, lesen Sie die [Übersicht über Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Um eine PowerShell-Sitzung zu starten, gehen Sie zu **Start**, geben Sie **powershell** ein, und klicken Sie dann auf **PowerShell**.
3. Geben Sie im PowerShell-Fenster den Befehl `login-azurermaccount` ein, um sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) anzumelden.
4. Geben Sie den folgenden Befehl ein, um eine Ressourcengruppe für das virtuelle Netzwerk zu erstellen:  `New-AzureRmResourceGroup -Name MyResourceGroup -Location eastus`
5. Sie können die Vorlage mithilfe einer der folgenden Optionen für die Parameter bereitstellen:
    - **Standardwerte für Parameter**. Geben Sie den folgenden Befehl ein:   `New-AzureRmResourceGroupDeployment -Name VnetTutorial -ResourceGroupName MyResourceGroup -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json`        
    - **Benutzerdefinierte Werte für Parameter**. Laden Sie die Vorlage herunter, und bearbeiten Sie sie, bevor Sie sie bereitstellen. Sie können die Vorlage auch mithilfe von Parametern in der Befehlszeile bereitstellen oder die Vorlage mit einer separaten Parameterdatei bereitstellen. Klicken Sie zum Herunterladen der Vorlagen- und Parameterdateien auf der Webseite zum [Erstellen eines virtuellen Netzwerks mit zwei Subnetzen](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) auf die Schaltfläche **Auf GitHub suchen**. Klicken Sie in GitHub auf die Datei **azuredeploy.parameters.json** oder **azuredeploy.json**. Klicken Sie dann auf die Schaltfläche **Raw**, um die Datei anzuzeigen. Kopieren Sie im Browser den Inhalt der Datei. Speichern Sie den Inhalt in einer Datei auf Ihrem Computer. Sie können die Parameterwerte in der Vorlage ändern oder die Vorlage mit einer separaten Parameterdatei bereitstellen.  

    Um mehr über das Bereitstellen von Vorlagen mit diesen Methoden zu erfahren, geben Sie `Get-Help New-AzureRmResourceGroupDeployment` ein. 

## <a name="delete"></a>Löschen von Ressourcen
Wenn Sie dieses Tutorial abgeschlossen haben, sollten Sie die Ressourcen, die Sie im Tutorial erstellt haben, wieder löschen, damit keine Nutzungsgebühren anfallen. Durch das Löschen einer Ressourcengruppe werden auch alle Ressourcen der Ressourcengruppe gelöscht.

### <a name="delete-portal"></a>Azure-Portal

1. Geben Sie in das Suchfeld im Portal **MyResourceGroup** ein. Klicken Sie in den Suchergebnissen auf **MyResourceGroup**.
2. Klicken Sie auf dem Blatt **MyResourceGroup** auf das **Löschsymbol**.
3. Um den Löschvorgang zu bestätigen, geben Sie in das Feld **Geben Sie den Ressourcengruppennamen ein** den Namen **MyResourceGroup** ein, und klicken Sie dann auf **Löschen**.

### <a name="delete-cli"></a>Azure-Befehlszeilenschnittstelle

Geben Sie an der Cloud Shell-Eingabeaufforderung den folgenden Befehl ein: `az group delete --name MyResourceGroup --yes`

### <a name="delete-powershell"></a>PowerShell

Geben Sie an der PowerShell-Eingabeaufforderung den folgenden Befehl ein: `Remove-AzureRmResourceGroup -Name MyResourceGroup`

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu allen Einstellungen für virtuelle Netzwerke und Subnetze finden Sie unter [Erstellen, Ändern oder Löschen virtueller Netzwerke](virtual-network-manage-network.md#view-vnet) und [Erstellen, Ändern oder Löschen von Subnetzen virtueller Netzwerke](virtual-network-manage-subnet.md#create-subnet). Für unterschiedliche Anforderungen gibt es verschiedene Optionen für die Verwendung virtueller Netzwerke und Subnetze in einer Produktionsumgebung.
- Erstellen Sie [Netzwerksicherheitsgruppen](virtual-networks-nsg.md) (NSGs) für Subnetze, und wenden Sie sie an, um eingehenden und ausgehenden Subnetzdatenverkehr zu filtern.
- Erstellen Sie einen [virtuellen Windows-Computer](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder einen [virtuellen Linux-Computer](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json), und verbinden Sie ihn mit dem virtuellen Netzwerk.
- Stellen Sie durch [Peering in virtuellen Netzwerken](virtual-network-peering-overview.md) eine Verbindung zwischen dem virtuellen Netzwerk und einem anderen virtuellen Netzwerk am gleichen Standort her .
- Verbinden Sie das virtuelle Netzwerk mit einem lokalen Netzwerk. Verwenden Sie hierfür ein [Site-to-Site-VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder eine [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Verbindung.

