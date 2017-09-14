---
title: Erstellen eines virtuellen Azure-Netzwerks mit mehreren Subnetzen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein virtuelles Netzwerk mit mehreren Subnetzen in Azure erstellen.
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: jdial
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: f82a95ec9543b2d53ef28bf7f15315e23cf4893a
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---
# <a name="create-a-virtual-network-with-multiple-subnets"></a>Erstellen eines virtuellen Netzwerks mit mehreren Subnetzen

In diesem Tutorial erfahren Sie, wie Sie ein einfaches virtuelles Azure-Netzwerk mit separaten öffentlichen und privaten Subnetzen erstellen. Ressourcen in virtuellen Netzwerken können miteinander und mit Ressourcen in anderen Netzwerken, die mit einem virtuellen Netzwerk verbunden sind, kommunizieren. Sie können Azure-Ressourcen wie virtuelle Computer, App Service-Umgebungen, VM-Skalierungsgruppen, Azure HDInsight und Clouddienste in denselben oder in unterschiedlichen Subnetzen innerhalb eines virtuellen Netzwerks erstellen. Durch die Erstellung von Ressourcen in unterschiedlichen Subnetzen können Sie den ein- und ausgehenden Netzwerkdatenverkehr von Subnetzen mithilfe von [Netzwerksicherheitsgruppen](virtual-networks-create-nsg-arm-pportal.md) unabhängig voneinander filtern und über virtuelle Netzwerkgeräte wie eine Firewall [Datenverkehr zwischen Subnetzen weiterleiten](virtual-network-create-udr-arm-ps.md), wenn Sie diese Option festlegen. 

In den folgenden Abschnitten werden die Schritte beschrieben, mit denen Sie ein virtuelles Netzwerk über das [Azure-Portal](#portal), die Azure-Befehlszeilenschnittstelle ([Azure CLI](#azure-cli)), [Azure PowerShell](#powershell) und eine [Azure Resource Manager-Vorlage](#resource-manager-template) erstellen können. Das Ergebnis ist identisch, unabhängig davon, mit welchem Tool Sie das virtuelle Netzwerk erstellen. Klicken Sie auf den Link für ein Tool, um zum entsprechenden Abschnitt des Tutorials zu wechseln. Erfahren Sie mehr über alle Einstellungen für [virtuelle Netzwerke](virtual-network-manage-network.md) und [Subnetze](virtual-network-manage-subnet.md).

Dieser Artikel enthält die Schritte zum Erstellen eines virtuellen Netzwerks über das Resource Manager-Bereitstellungsmodell. Dies ist das empfohlene Bereitstellungsmodell für das Erstellen neuer virtueller Netzwerke. Wenn Sie ein virtuelles Netzwerk (klassisch) erstellen möchten, finden Sie unter [Erstellen eines virtuellen Netzwerks (klassisch)](create-virtual-network-classic.md) weitere Informationen. Wenn Sie mit den Azure-Bereitstellungsmodellen nicht vertraut sind, lesen Sie unter [Grundlegendes zu Azure-Bereitstellungsmodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nach.

## <a name="portal"></a>Azure-Portal

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) in einem Internetbrowser. Melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p)registrieren.
2. Klicken Sie im Portal auf **+ Neu** > **Netzwerk** > **Virtuelles Netzwerk**.
3. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die folgenden Werte ein, und klicken Sie anschließend auf **Erstellen**:

    |Einstellung|Wert|
    |---|---|
    |Name|myVnet|
    |Adressraum|10.0.0.0/16|
    |Subnetzname|Öffentlich|
    |Subnetzadressbereich|10.0.0.0/24|
    |Ressourcengruppe|Lassen Sie **Neu erstellen** ausgewählt, und geben Sie **myResourceGroup** ein.|
    |Abonnement und Standort|Wählen Sie Ihr Abonnement und Ihren Standort aus.

    Wenn Sie noch nicht mit Azure vertraut sind, informieren Sie sich über [Ressourcengruppen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [Abonnements](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) und [Standorte](https://azure.microsoft.com/regions) (die auch als *Regionen* bezeichnet werden).
4. Im Portal können Sie beim Erstellen eines virtuellen Netzwerks nur ein Subnetz erstellen. In diesem Tutorial erstellen Sie nach dem virtuellen Netzwerk ein zweites Subnetz. Sie können später über das Internet zugängliche Ressourcen im Subnetz **Öffentlich** erstellen. Sie können auch andere Ressourcen, auf die nicht über das Internet zugegriffen werden kann, im Subnetz **Private** erstellen. Geben Sie zum Erstellen des zweiten Subnetzes oben auf der Seite **myVnet** im Feld **Ressourcen suchen** ein. Klicken Sie in den Suchergebnissen auf **myVnet**. Wenn Ihr Abonnement mehrere virtuelle Netzwerke mit dem gleichen Namen umfasst, überprüfen Sie die Ressourcengruppen, die unter den einzelnen virtuellen Netzwerken aufgeführt werden. Achten Sie darauf, auf das Suchergebnis **myVnet** mit der Ressourcengruppe **myResourceGroup** zu klicken.
5. Klicken Sie auf dem eingeblendeten Blatt **myVnet** unter **EINSTELLUNGEN** auf **Subnetze**.
6. Klicken Sie auf dem Blatt **myVnet: Subnetze** auf **+ Subnetz**.
7. Geben Sie auf dem Blatt **Subnetz hinzufügen** unter **Name**, den Namen **Private** ein. Geben Sie unter **Adressenbereich** den Wert **10.0.1.0/24** ein.  Klicken Sie auf **OK**.
8. Überprüfen Sie die Subnetze auf dem Blatt **myVnet: Subnetze**. Die von Ihnen erstellten Subnetze **Public** und **Private** werden angezeigt.
9. **Optional:** Führen Sie die unter [Nächste Schritte](#next-steps) aufgeführten zusätzlichen Tutorials durch, um den ein- und ausgehenden Netzwerkdatenverkehr aller Subnetze mithilfe von Netzwerksicherheitsgruppen zu filtern, den Datenverkehr zwischen Subnetzen über ein virtuelles Netzwerkgerät weiterzuleiten oder das virtuelle Netzwerk mit anderen virtuellen Netzwerken oder lokalen Netzwerken zu verbinden.
10. **Optional:** Löschen Sie die Ressourcen, die Sie in diesem Tutorial erstellt haben, indem Sie die Schritte unter [Löschen von Ressourcen](#delete-portal) durchführen.

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Die Azure-CLI-Befehle sind unter Windows, Linux und macOS identisch. Es gibt jedoch Unterschiede bei der Skripterstellung für die Betriebssystemshells. Das Skript in den folgenden Schritten wird in einer Bash-Shell ausgeführt. 

1. [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> --help`. Anstatt die CLI und ihre Voraussetzungen zu installieren, können Sie die Azure Cloud Shell verwenden. Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure-Befehlszeilenschnittstelle ist bei der Cloud Shell vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Um die Cloud Shell zu verwenden, klicken Sie oben im [Portal](https://portal.azure.com) auf die Cloud Shell-Schaltfläche (**>_**), oder klicken Sie einfach in den nachfolgenden Schritten auf die Schaltfläche *Jetzt testen*. 
2. Wenn die Befehlszeilenschnittstelle lokal ausgeführt wird, melden Sie sich mit dem Befehl `az login` bei Azure an. Wenn Sie die Cloud Shell verwenden, sind Sie bereits angemeldet.
3. Prüfen Sie das folgende Skript und die enthaltenen Kommentare. Kopieren Sie das Skript in Ihrem Browser, und fügen Sie ihn in die CLI-Sitzung ein:

    ```azurecli-interactive
    #!/bin/bash
    
    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus
    
    # Create a virtual network with one subnet named Public.
    az network vnet create \
      --name myVnet \
      --resource-group myResourceGroup \
      --subnet-name Public
    
    # Create an additional subnet named Private in the virtual network.
    az network vnet subnet create \
      --name Private \
      --address-prefix 10.0.1.0/24 \
      --vnet-name myVnet \
      --resource-group myResourceGroup
    ```
    
4. Überprüfen Sie nach Abschluss der Skriptausführung die Subnetze auf das virtuelle Netzwerk. Kopieren Sie den folgenden Befehl, und fügen Sie ihn in Ihre CLI-Sitzung ein:

    ```azurecli
    az network vnet subnet list --resource-group myResourceGroup --vnet-name myVnet --output table
    ```

5. **Optional:** Führen Sie die unter [Nächste Schritte](#next-steps) aufgeführten zusätzlichen Tutorials durch, um den ein- und ausgehenden Netzwerkdatenverkehr aller Subnetze mithilfe von Netzwerksicherheitsgruppen zu filtern, den Datenverkehr zwischen Subnetzen über ein virtuelles Netzwerkgerät weiterzuleiten oder das virtuelle Netzwerk mit anderen virtuellen Netzwerken oder lokalen Netzwerken zu verbinden.
6. **Optional**: Löschen Sie die Ressourcen, die Sie in diesem Tutorial erstellt haben, indem Sie die Schritte unter [Löschen von Ressourcen](#delete-cli) durchführen.

## <a name="powershell"></a>PowerShell

1. Installieren Sie die neueste Version des PowerShell-Moduls [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Sie noch nicht mit Azure PowerShell vertraut sind, lesen Sie die [Übersicht über Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Melden Sie sich in einer PowerShell-Sitzung mithilfe des Befehls `login-azurermaccount` mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) in Azure an.

3. Prüfen Sie das folgende Skript und die enthaltenen Kommentare. Kopieren Sie das Skript in Ihrem Browser, und fügen Sie ihn in die PowerShell-Sitzung ein:

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name myResourceGroup `
      -Location eastus
    
    # Create the public and private subnets.
    $Subnet1 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Public `
      -AddressPrefix 10.0.0.0/24
    $Subnet2 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Private `
      -AddressPrefix 10.0.1.0/24
    
    # Create a virtual network.
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Location eastus `
      -Name myVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet1,$Subnet2
    ```

4. Um die Subnetze für das virtuelle Netzwerk zu überprüfen, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die PowerShell-Sitzung ein:

    ```powershell
    $Vnet.subnets | Format-Table Name, AddressPrefix
    ```

5. **Optional:** Führen Sie die unter [Nächste Schritte](#next-steps) aufgeführten zusätzlichen Tutorials durch, um den ein- und ausgehenden Netzwerkdatenverkehr aller Subnetze mithilfe von Netzwerksicherheitsgruppen zu filtern, den Datenverkehr zwischen Subnetzen über ein virtuelles Netzwerkgerät weiterzuleiten oder das virtuelle Netzwerk mit anderen virtuellen Netzwerken oder lokalen Netzwerken zu verbinden.
6. **Optional**: Löschen Sie die Ressourcen, die Sie in diesem Tutorial erstellt haben, indem Sie die Schritte unter [Löschen von Ressourcen](#delete-powershell) durchführen.

## <a name="resource-manager-template"></a>Resource Manager-Vorlage

Sie können ein virtuelles Netzwerk über eine Azure Resource Manager-Vorlage bereitstellen. Weitere Informationen zu Vorlagen finden Sie in der [Übersicht über Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#template-deployment). Informationen zum Zugriff auf die Vorlage und den Parametern finden Sie auf der Seite zur Vorlage zum [Erstellen eines virtuellen Netzwerks mit zwei Subnetzen](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/). Sie können die Vorlage über das [Portal](#template-portal), die [Azure CLI](#template-cli) oder [PowerShell](#template-powershell) bereitstellen.

Optionale Schritte nach der Bereitstellung der Vorlage:

1. Führen Sie die unter [Nächste Schritte](#next-steps) aufgeführten zusätzlichen Tutorials durch, um den ein- und ausgehenden Netzwerkdatenverkehr aller Subnetze mithilfe von Netzwerksicherheitsgruppen zu filtern, den Datenverkehr zwischen Subnetzen über ein virtuelles Netzwerkgerät weiterzuleiten oder das virtuelle Netzwerk mit anderen virtuellen Netzwerken oder lokalen Netzwerken zu verbinden.
2. Löschen Sie die Ressourcen, die Sie in diesem Tutorial erstellt haben, indem Sie die Schritte in allen Unterabschnitten unter [Löschen von Ressourcen](#delete) durchführen.

### <a name="template-portal"></a>Azure-Portal

1. Öffnen Sie in Ihrem Browser die [Vorlagenseite](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets).
2. Klicken Sie auf die Schaltfläche **Bereitstellen in Azure**. Wenn Sie noch nicht bei Azure angemeldet sind, melden Sie sich auf der angezeigten Anmeldeseite im Azure-Portal an.
3. Melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) beim Portal an. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p)registrieren.
4. Geben Sie die folgende Werte für die Parameter ein:

    |Parameter|Wert|
    |---|---|
    |Abonnement|Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe|myResourceGroup|
    |Standort|Standort auswählen|
    |VNet-Name|myVnet|
    |VNet-Adresspräfix|10.0.0.0/16|
    |Subnet1Prefix|10.0.0.0/24|
    |Subnet1Name|Public|
    |Subnet2Prefix|10.0.1.0/24|
    |Subnet2Name|Private|

5. Stimmen Sie den Geschäftsbedingungen zu, und klicken Sie anschließend auf **Kaufen**, um das virtuelle Netzwerk bereitzustellen.

### <a name="template-cli"></a>Azure-Befehlszeilenschnittstelle

1. [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> --help`. Anstatt die CLI und ihre Voraussetzungen zu installieren, können Sie die Azure Cloud Shell verwenden. Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure-Befehlszeilenschnittstelle ist bei der Cloud Shell vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Um die Cloud Shell zu verwenden, klicken Sie oben im [Portal](https://portal.azure.com) auf die Cloud Shell-Schaltfläche (**>_**), oder klicken Sie einfach in den nachfolgenden Schritten auf die Schaltfläche **Jetzt testen**. 
2. Wenn die Befehlszeilenschnittstelle lokal ausgeführt wird, melden Sie sich mit dem Befehl `az login` bei Azure an. Wenn Sie die Cloud Shell verwenden, sind Sie bereits angemeldet.
3. Kopieren Sie den folgenden Befehl, und fügen Sie ihn in die CLI-Sitzung ein, um eine Ressourcengruppe für das virtuelle Netzwerk zu erstellen:

    ```azurecli-interactive
    az group create --name myResourceGroup --location eastus
    ```
    
4. Sie können die Vorlage mithilfe einer der folgenden Optionen für die Parameter bereitstellen:
    - **Standardwerte für Parameter**. Geben Sie den folgenden Befehl ein:
    
        ```azurecli-interactive
        az group deployment create --resource-group myResourceGroup --name VnetTutorial --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json`
        ```
    - **Benutzerdefinierte Werte für Parameter**. Laden Sie die Vorlage herunter, und bearbeiten Sie sie, bevor Sie die Vorlage bereitstellen. Sie können die Vorlage auch mithilfe von Parametern in der Befehlszeile bereitstellen oder die Vorlage mit einer separaten Parameterdatei bereitstellen. Klicken Sie zum Herunterladen der Vorlagen- und Parameterdateien auf der Webseite zum [Erstellen eines virtuellen Netzwerks mit zwei Subnetzen](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) auf die Schaltfläche **Auf GitHub suchen**. Klicken Sie in GitHub auf die Datei **azuredeploy.parameters.json** oder **azuredeploy.json**. Klicken Sie dann auf die Schaltfläche **Raw**, um die Datei anzuzeigen. Kopieren Sie im Browser den Inhalt der Datei. Speichern Sie den Inhalt in einer Datei auf Ihrem Computer. Sie können die Parameterwerte in der Vorlage ändern oder die Vorlage mit einer separaten Parameterdatei bereitstellen.  

    Um mehr über das Bereitstellen von Vorlagen mit diesen Methoden zu erfahren, geben Sie `az group deployment create --help` ein.

### <a name="template-powershell"></a>PowerShell

1. Installieren Sie die neueste Version des PowerShell-Moduls [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Sie noch nicht mit Azure PowerShell vertraut sind, lesen Sie die [Übersicht über Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Geben Sie in einer PowerShell-Sitzung den Befehl `login-azurermaccount` ein, um sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) anzumelden.
3. Geben Sie den folgenden Befehl ein, um eine Ressourcengruppe für das virtuelle Netzwerk zu erstellen:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
    ```
    
4. Sie können die Vorlage mithilfe einer der folgenden Optionen für die Parameter bereitstellen:
    - **Standardwerte für Parameter**. Geben Sie den folgenden Befehl ein:
    
        ```powershell
        New-AzureRmResourceGroupDeployment -Name VnetTutorial -ResourceGroupName myResourceGroup -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json
        ```
        
    - **Benutzerdefinierte Werte für Parameter**. Laden Sie die Vorlage herunter, und bearbeiten Sie sie, bevor Sie sie bereitstellen. Sie können die Vorlage auch mithilfe von Parametern in der Befehlszeile bereitstellen oder die Vorlage mit einer separaten Parameterdatei bereitstellen. Klicken Sie zum Herunterladen der Vorlagen- und Parameterdateien auf der Webseite zum [Erstellen eines virtuellen Netzwerks mit zwei Subnetzen](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) auf die Schaltfläche **Auf GitHub suchen**. Klicken Sie in GitHub auf die Datei **azuredeploy.parameters.json** oder **azuredeploy.json**. Klicken Sie dann auf die Schaltfläche **Raw**, um die Datei anzuzeigen. Kopieren Sie im Browser den Inhalt der Datei. Speichern Sie den Inhalt in einer Datei auf Ihrem Computer. Sie können die Parameterwerte in der Vorlage ändern oder die Vorlage mit einer separaten Parameterdatei bereitstellen.  

    Um mehr über das Bereitstellen von Vorlagen mit diesen Methoden zu erfahren, geben Sie `Get-Help New-AzureRmResourceGroupDeployment` ein. 

## <a name="delete"></a>Löschen von Ressourcen

Wenn Sie dieses Tutorial abgeschlossen haben, möchten Sie die Ressourcen, die Sie erstellt haben, möglicherweise wieder löschen, damit keine Nutzungsgebühren anfallen. Durch das Löschen einer Ressourcengruppe werden auch alle Ressourcen in dieser Ressourcengruppe gelöscht.

### <a name="delete-portal"></a>Azure-Portal

1. Geben Sie in das Suchfeld im Portal **myResourceGroup** ein. Klicken Sie in den Suchergebnissen auf **myResourceGroup**.
2. Klicken Sie auf dem Blatt **myResourceGroup** auf das **Löschsymbol**.
3. Um den Löschvorgang zu bestätigen, geben Sie in das Feld **TYPE THE RESOURCE GROUP NAME** (Ressourcengruppennamen eingeben) **myResourceGroup** ein, und klicken Sie dann auf **Löschen**.

### <a name="delete-cli"></a>Azure-Befehlszeilenschnittstelle

Geben Sie in einer CLI-Sitzung den folgenden Befehl ein:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

Geben Sie in einer PowerShell-Sitzung den folgenden Befehl ein:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu allen Einstellungen für virtuelle Netzwerke und Subnetze finden Sie unter [Verwalten virtueller Netzwerke](virtual-network-manage-network.md#view-vnet) und [Verwalten von Subnetzen virtueller Netzwerke](virtual-network-manage-subnet.md#create-subnet). Für unterschiedliche Anforderungen gibt es verschiedene Optionen für die Verwendung virtueller Netzwerke und Subnetze in einer Produktionsumgebung.
- Filtern Sie ein- und ausgehenden Subnetzdatenverkehr, indem Sie [Netzwerksicherheitsgruppen](virtual-networks-nsg.md) erstellen und auf Subnetze anwenden.
- Leiten Sie den Datenverkehr zwischen Subnetzen über ein virtuelles Netzwerkgerät weiter, indem Sie [benutzerdefinierte Routen](virtual-network-create-udr-arm-ps.md) erstellen und diese auf alle Subnetze anwenden.
- Erstellen Sie einen virtuellen [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- oder [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Computer in einem bestehenden virtuellen Netzwerk.
- Verbinden Sie zwei virtuelle Netzwerke, indem Sie ein [Peering virtueller Netzwerke](virtual-network-peering-overview.md) zwischen diesen virtuellen Netzwerken erstellen.
- Verbinden Sie das virtuelle Netzwerk mit einem lokalen Netzwerk über ein [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder eine [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Verbindung.

