---
title: Bereitstellen von Computeressourcen mit Azure Resource Manager-Vorlagen | Microsoft Docs
description: ".NET Core-Tutorial für virtuelle Azure-Computer"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: b026fe81-1bc1-4899-ac32-886091671498
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: b746e4218ea505ea5e0a1579608e0ecfa10e87e7


---
# <a name="application-architecture-with-azure-resource-manager-templates"></a>Anwendungsarchitektur mit Azure Resource Manager-Vorlagen
Beim Entwickeln einer Azure Resource Manager-Bereitstellung müssen Computeanforderungen Azure-Ressourcen und -Diensten zugeordnet werden. Wenn eine Anwendung aus mehreren HTTP-Endpunkten, einer Datenbank und einem Cachedienst für Daten besteht, müssen die Azure-Ressourcen, auf denen diese Komponenten gehostet werden, rationalisiert werden. Die Music Store-Beispielanwendung enthält beispielsweise eine Webanwendung, die auf einem virtuellen Computer gehostet wird, sowie eine SQL-Datenbank, die in Azure SQL-Datenbank gehostet wird. 

In diesem Dokument erfahren Sie, wie die Music Store-Computeressourcen in der Azure Resource Manager-Beispielvorlage konfiguriert werden. Alle Abhängigkeiten und individuellen Konfigurationen werden hervorgehoben. Stellen Sie am besten vorab eine Instanz der Lösung in Ihrem Azure-Abonnement bereit, und orientieren Sie sich an der Azure Resource Manager-Vorlage. Die vollständige Vorlage finden Sie hier – [Music Store Deployment on Windows (Music Store-Bereitstellung unter Windows)](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="virtual-machine"></a>Virtual Machine
Die Music Store-Anwendung enthält eine Webanwendung, mit der Kunden nach Musik suchen und diese kaufen können. Es gibt mehrere Azure-Dienste, mit denen Webanwendungen gehostet werden können. In diesem Beispiel wird ein virtueller Computer verwendet. Mit der Music Store-Beispielvorlage wird ein virtueller Computer bereitgestellt, ein Webserver installiert und die Music Store-Website installiert und konfiguriert. Für diesen Artikel gehen wir nur auf die Bereitstellung des virtuellen Computers ein. Die Konfiguration des Webservers und der Anwendung wird in einem nachfolgenden Artikel beschrieben.

Ein virtueller Computer kann einer Vorlage mit dem Visual Studio-Assistenten „Neue Ressource hinzufügen“ hinzugefügt werden, oder indem gültiger JSON-Code in die Bereitstellungsvorlage eingefügt wird. Zum Bereitstellen eines virtuellen Computers werden außerdem mehrere dazugehörige Ressourcen benötigt. Wenn Sie Visual Studio zum Erstellen der Vorlage verwenden, werden diese Ressourcen für Sie erstellt. Bei der manuellen Erstellung der Vorlage müssen diese Ressourcen eingefügt und konfiguriert werden.

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Virtual Machine JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285)(JSON-Code für virtuellen Computer).

```json
{
  {
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
  ........<truncated>  
}
```

Nach der Bereitstellung werden die Eigenschaften des virtuellen Computers im Azure-Portal angezeigt.

![Virtual Machine](./media/virtual-machines-windows-dotnet-core/vm-win.png)

## <a name="storage-account"></a>Speicherkonto
Speicherkonten verfügen über viele Speicheroptionen und -funktionen. Im Kontext von virtuellen Azure-Computern enthält ein Speicherkonto die virtuellen Festplatten des virtuellen Computers sowie zusätzliche Datenträger für Daten. Das Music Store-Beispiel umfasst ein Speicherkonto für die virtuelle Festplatte der einzelnen virtuellen Computer einer Bereitstellung. 

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Storage Account](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98)(Speicherkonto).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

Ein Speicherkonto ist einem virtuellen Computer in der Resource Manager-Vorlagendeklaration des virtuellen Computers zugeordnet. 

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Virtual Machine and Storage Account association](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321)(Zuordnung von virtuellem Computer und Speicherkonto).

```json
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

Nach der Bereitstellung wird das Speicherkonto im Azure-Portal angezeigt.

![Speicherkonto](./media/virtual-machines-windows-dotnet-core/storacct-win.png)

Wenn Sie in den Speicherkonto-Blobcontainer klicken, wird die Datei mit dem Treiber für die virtuelle Festplatte für jeden virtuellen Computer angezeigt, der mit der Vorlage bereitgestellt wurde.

![Virtuelle Festplatten](./media/virtual-machines-windows-dotnet-core/vhd-win.png)

Weitere Informationen zu Azure Storage finden Sie in der [Azure Storage-Dokumentation](https://azure.microsoft.com/documentation/services/storage/).

## <a name="virtual-network"></a>Virtuelles Netzwerk
Wenn für einen virtuellen Computer eine interne Vernetzung erforderlich ist, z.B. die Fähigkeit zum Kommunizieren mit anderen virtuellen Computern und Azure-Ressourcen, muss ein virtuelles Azure-Netzwerk verwendet werden.  Ein virtuelles Netzwerk bedeutet nicht automatisch, dass der virtuelle Computer über das Internet zugänglich ist. Für öffentliche Verbindungen ist eine öffentliche IP-Adresse erforderlich, die später in dieser Reihe noch ausführlich beschrieben wird.

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Virtual Network and Subnets](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126)(Virtuelles Netzwerk und Subnetze).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
    "subnets": [
      {
        "name": "[variables('subnetName')]",
        "properties": {
          "addressPrefix": "10.0.0.0/24",
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
          }
        }
      }
    ]
  }
}
```

Im Azure-Portal sieht das virtuelle Netzwerk wie in der folgenden Abbildung aus. Beachten Sie, dass alle virtuellen Computer, die mit der Vorlage bereitgestellt werden, dem virtuellen Netzwerk zugeordnet sind.

![Virtual Network](./media/virtual-machines-windows-dotnet-core/vnet-win.png)

## <a name="network-interface"></a>Netzwerkschnittstelle
 Mit einer Netzwerkschnittstelle wird für einen virtuellen Computer eine Verbindung mit einem virtuellen Netzwerk hergestellt – genauer gesagt mit einem Subnetz, das im virtuellen Netzwerk definiert wurde. 

 Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Network Interface](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156)(Netzwerkschnittstelle).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceName'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'RDP-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[variables('subnetRef')]"
          },
          "loadBalancerBackendAddressPools": [
            {
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

Jede Ressource eines virtuellen Computers enthält ein Netzwerkprofil. Die Netzwerkschnittstelle ist dem virtuellen Computer in diesem Profil zugeordnet.  

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Virtual Machine Network Profile](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330)(Netzwerkprofil für virtuellen Computer).

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

Im Azure-Portal wird die Netzwerkschnittstelle wie in der folgenden Abbildung angezeigt. Die interne IP-Adresse und die Zuordnung des virtuellen Computers werden für die Netzwerkschnittstellenressource angezeigt.

![Netzwerkschnittstelle](./media/virtual-machines-windows-dotnet-core/nic-win.png)

Weitere Informationen zu virtuellen Netzwerken erhalten Sie in der [Dokumentation zu Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>Azure SQL-Datenbank
Zusätzlich zu einem virtuellen Computer, auf dem die Music Store-Website gehostet wird, wird eine Azure SQL-Datenbank zum Hosten der Music Store-Datenbank bereitgestellt. Der Vorteil einer Verwendung der Azure SQL-Datenbank besteht darin, dass kein zweiter Satz mit virtuellen Computern erforderlich ist und Funktionen für die Skalierung und Verfügbarkeit in den Dienst integriert sind.

Sie können eine Azure SQL-Datenbank mit dem Visual Studio-Assistenten „Neue Ressource hinzufügen“ hinzufügen, oder Sie können gültigen JSON-Code in eine Vorlage einfügen. Die SQL Server-Ressource enthält einen Benutzernamen mit Kennwort, für den Administratorrechte für die SQL-Instanz gewährt werden. Außerdem wird eine SQL-Firewallressource hinzugefügt. Standardmäßig können in Azure gehostete Anwendungen eine Verbindung mit der SQL-Instanz herstellen. Damit externe Anwendungen, z.B. SQL Server Management Studio, eine Verbindung mit der SQL-Instanz herstellen können, muss die Firewall konfiguriert werden. Für das Music Store-Demo ist die Standardkonfiguration geeignet. 

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Azure SQL-DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).

```json
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicstoresqlName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('adminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicstoresqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Hier ist eine Ansicht der SQL Server-Instanz und der Music Store-Datenbank im Azure-Portal dargestellt.

![SQL Server](./media/virtual-machines-windows-dotnet-core/sql-win.png)

Weitere Informationen zum Bereitstellen von Azure SQL-Datenbank finden Sie unter [Azure SQL-Datenbankdokumentation](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Nächster Schritt
<hr>

[Schritt 2: Zugriff und Sicherheit in Azure Resource Manager-Vorlagen](virtual-machines-windows-dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)




<!--HONumber=Nov16_HO3-->


