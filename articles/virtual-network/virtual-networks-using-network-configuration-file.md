---
title: "Konfigurieren eines virtuellen Azure-Netzwerks (klassisch) – Netzwerkkonfigurationsdatei | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie virtuelle Netzwerke (klassisch) erstellen und ändern, indem Sie eine Netzwerkkonfigurationsdatei exportieren, bearbeiten und importieren."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: jdial
ms.custom: 
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c69630688e4bcd68ab3b4ee6d9fdb0e0c46d04b
ms.openlocfilehash: 6baf1a642778bdd281ae641611bc09ee26aaec70
ms.contentlocale: de-de
ms.lasthandoff: 06/24/2017


---
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a>Konfigurieren eines virtuellen Netzwerks (klassisch) mithilfe einer Netzwerkkonfigurationsdatei
> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und klassische Bereitstellungen](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Resource Manager-Bereitstellungsmodells.

Sie können ein virtuelles Netzwerk (klassisch) mit einer Netzwerkkonfigurationsdatei unter Verwendung der Azure-Befehlszeilenschnittstelle (CLI) 1.0 oder Azure PowerShell erstellen und konfigurieren. Sie können ein virtuelles Netzwerk nicht mithilfe einer Netzwerkkonfigurationsdatei im Azure Resource Manager-Bereitstellungsmodell erstellen oder ändern. Sie können nicht das Azure-Portal verwenden, um ein virtuelles Netzwerk (klassisch) mithilfe einer Netzwerkkonfigurationsdatei zu erstellen oder zu ändern. Jedoch können Sie das Azure-Portal dazu verwenden, ein virtuelles Netzwerk (klassisch) zu erstellen, ohne eine Netzwerkkonfigurationsdatei zu verwenden.

Um ein virtuelles Netzwerk (klassisch) mithilfe einer Netzwerkkonfigurationsdatei zu erstellen und zu ändern, müssen Sie die Datei exportieren, ändern und importieren.

## <a name="export-a-network-configuration-file"></a>Exportieren einer Netzwerkkonfigurationsdatei

Sie können PowerShell oder die Azure CLI verwenden, um eine Netzwerkkonfigurationsdatei zu exportieren. PowerShell exportiert eine XML-Datei, während die Azure CLI eine JSON-Datei exportiert.

### <a name="powershell"></a>PowerShell
 
1. [Installieren Sie Azure PowerShell, und melden Sie sich bei Azure an](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Ändern Sie das Verzeichnis (und vergewissern Sie sich, dass es vorhanden ist) und den Dateinamen im folgenden Befehl nach Bedarf, und führen Sie dann den Befehl zum Exportieren der Netzwerkkonfigurationsdatei aus:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure-Befehlszeilenschnittstelle 1.0

1. [Installieren Sie die Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Führen Sie die verbleibenden Schritte über eine Eingabeaufforderung der Azure CLI 1.0 durch.
2. Melden Sie sich bei Azure an, indem Sie den Befehl `azure login` eingeben.
3. Stellen Sie durch Eingabe des Befehls `azure config mode asm` sicher, dass Sie sich im ASM-Modus befinden.
4. Ändern Sie das Verzeichnis (und vergewissern Sie sich, dass es vorhanden ist) und den Dateinamen im folgenden Befehl nach Bedarf, und führen Sie dann den Befehl zum Exportieren der Netzwerkkonfigurationsdatei aus:
    
    ```azurecli
    azure network export c:\azure\networkconfig.json
    ```

## <a name="create-or-modify-a-network-configuration-file"></a>Erstellen oder Ändern einer Netzwerkkonfigurationsdatei

Bei der Netzwerkkonfigurationsdatei handelt es sich um eine XML-Datei (bei Verwendung von PowerShell) oder eine JSON-Datei (bei Verwendung der Azure CLI). Sie können die Datei in einem beliebigen Text- oder XML/JSON-Editor bearbeiten. Im Artikel [Azure Virtual Network Configuration Schema (Konfigurationsschema von Azure Virtual Network)](https://msdn.microsoft.com/library/azure/jj157100.aspx) finden Sie weitere Informationen für alle Einstellungen. Weitere Erläuterungen der Einstellungen finden Sie unter [Anzeigen von virtuellen Netzwerken und Einstellungen](virtual-network-manage-network.md#view-vnet). Die Änderungen, die Sie an der Datei vornehmen:

- müssen dem Schema entsprechen, oder es tritt ein Fehler beim Import der Netzwerkkonfigurationsdatei auf.
- überschreiben alle vorhandenen Netzwerkeinstellungen für Ihr Abonnement, weshalb Sie beim Vornehmen von Änderungen sehr vorsichtig sein sollten. Verweisen Sie beispielsweise auf die folgenden Netzwerkkonfigurationsdateien. Angenommen, die ursprüngliche Datei enthielt zwei **VirtualNetworkSite**-Instanzen, und Sie haben diese geändert, wie in den Beispielen gezeigt. Wenn Sie die Datei importieren, löscht Azure das virtuelle Netzwerk für die **VirtualNetworkSite**-Instanz, die Sie in der Datei entfernt haben. Bei diesem vereinfachten Szenario wird davon ausgegangen, dass sich keine Ressourcen im virtuellen Netzwerk befanden. Wäre dies der Fall gewesen, hätte das virtuelle Netzwerk nicht gelöscht werden können, und es wäre ein Fehler beim Import aufgetreten.

> [!IMPORTANT]
> Azure betrachtet ein Subnetz, in dem etwas bereitgestellt wurde, als **in Gebrauch**. Wenn Sie ein Subnetz in Gebrauch ist, kann es nicht geändert werden. Verschieben Sie vor dem Ändern von Subnetzinformationen in einer Netzwerkkonfigurationsdatei alles, was im Subnetz bereitgestellt haben, in ein anderes Subnetz, das nicht geändert wird. Weitere Informationen finden Sie unter [Verschieben eines virtuellen Computers oder einer Rolleninstanz in ein anderes Subnetz](virtual-networks-move-vm-role-to-subnet.md).

### <a name="example-xml-for-use-with-powershell"></a>XML-Beispiel für die Verwendung mit PowerShell

Die folgende Beispiel-Netzwerkkonfigurationsdatei erstellt ein virtuelles Netzwerk namens *MeinVirtuellesNetzwerk* mit einem Adressraum von *10.0.0.0/16* in der Azure-Region *USA, Osten*. Das virtuelle Netzwerk enthält ein Subnetz mit dem Namen *MeinSubnetz* mit einem Adresspräfix von *10.0.0.0/24*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
  <VirtualNetworkConfiguration>
    <Dns />
    <VirtualNetworkSites>
      <VirtualNetworkSite name="myVirtualNetwork" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="mySubnet">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
  </VirtualNetworkConfiguration>
</NetworkConfiguration>
```

Wenn die Netzwerkkonfigurationsdatei, die Sie exportiert haben, keinen Inhalt hat, können Sie die XML im vorherigen Beispiel kopieren und in eine neue Datei einfügen.

### <a name="example-json-for-use-with-the-azure-cli-10"></a>Beispiel-JSON für die Verwendung mit der Azure CLI 1.0

Die folgende Beispiel-Netzwerkkonfigurationsdatei erstellt ein virtuelles Netzwerk namens *MeinVirtuellesNetzwerk* mit einem Adressraum von *10.0.0.0/16* in der Azure-Region *USA, Osten*. Das virtuelle Netzwerk enthält ein Subnetz mit dem Namen *MeinSubnetz* mit einem Adresspräfix von *10.0.0.0/24*.

```json
{
   "VirtualNetworkConfiguration" : {
      "Dns" : "",
      "VirtualNetworkSites" : [
         {
            "AddressSpace" : [ "10.0.0.0/16" ],
            "Location" : "East US",
            "Name" : "myVirtualNetwork",
            "Subnets" : [
               {
                  "AddressPrefix" : "10.0.0.0/24",
                  "Name" : "mySubnet"
               }
            ]
         }
      ]
   }
}
```

Wenn die Netzwerkkonfigurationsdatei, die Sie exportiert haben, keinen Inhalt hat, können Sie die JSON im vorherigen Beispiel kopieren und in eine neue Datei einfügen.

## <a name="import-a-network-configuration-file"></a>Importieren einer Netzwerkkonfigurationsdatei

Sie können PowerShell oder die Azure CLI verwenden, um eine Netzwerkkonfigurationsdatei zu importieren. PowerShell importiert eine XML-Datei während die Azure CLI eine JSON-Datei importiert. Wenn beim Import ein Fehler auftritt, bestätigen Sie, dass die Datei dem [Netzwerkkonfigurationsschema](https://msdn.microsoft.com/library/azure/jj157100.aspx) entspricht. 

### <a name="powershell"></a>PowerShell
 
1. [Installieren Sie Azure PowerShell, und melden Sie sich bei Azure an](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Ändern Sie das Verzeichnis und den Dateinamen im folgenden Befehl nach Bedarf, und führen Sie dann den Befehl zum Importieren der Netzwerkkonfigurationsdatei aus:
 
    ```powershell
    Set-AzureVNetConfig  -ConfigurationPath c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure-Befehlszeilenschnittstelle 1.0

1. [Installieren Sie die Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Führen Sie die verbleibenden Schritte über eine Eingabeaufforderung der Azure CLI 1.0 durch.
2. Melden Sie sich bei Azure an, indem Sie den Befehl `azure login` eingeben.
3. Stellen Sie durch Eingabe des Befehls `azure config mode asm` sicher, dass Sie sich im ASM-Modus befinden.
4. Ändern Sie das Verzeichnis und den Dateinamen im folgenden Befehl nach Bedarf, und führen Sie dann den Befehl zum Importieren der Netzwerkkonfigurationsdatei aus:

    ```azurecli
    azure network import c:\azure\networkconfig.json
    ```
