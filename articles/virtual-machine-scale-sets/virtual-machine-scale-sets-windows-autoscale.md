---
title: Automatische Skalierung von Skalierungsgruppen virtueller Windows-Computer | Microsoft Docs
description: "Einrichten der automatischen Skalierung für eine Skalierungsgruppe virtueller Windows-Computer mit Azure PowerShell"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: b2565bef6643decd1b96fc3cc5b01003916e9685
ms.lasthandoff: 03/31/2017


---
# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Automatisches Skalieren von Computern in einer VM-Skalierungsgruppe
VM-Skalierungsgruppen erleichtern das Bereitstellen und Verwalten identischer virtueller Computer als Gruppe. Skalierungsgruppen stellen eine hoch skalierbare und anpassbare Compute-Ebene für Anwendungen mit Hyperskalierung bereit und bieten Unterstützung für Images der Windows-Plattform, Images der Linux-Plattform, benutzerdefinierte Images und Erweiterungen. Weitere Informationen zu Skalierungsgruppen finden Sie unter [VM-Skalierungsgruppen](virtual-machine-scale-sets-overview.md).

In diesem Tutorial erfahren Sie, wie Sie eine Skalierungsgruppe mit virtuellen Windows-Computern erstellen und die Computer der Gruppe automatisch skalieren. Hierzu erstellen Sie die Skalierungsgruppe und richten die Skalierung ein, indem Sie eine Azure Resource Manager-Vorlage erstellen und mithilfe von Azure PowerShell bereitstellen. Weitere Informationen zu Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md). Weitere Informationen zur automatischen Skalierung von Skalierungsgruppen finden Sie unter [Automatic scaling and Virtual Machine Scale Sets](virtual-machine-scale-sets-autoscale-overview.md)(Automatische Skalierung und VM-Skalierungsgruppen).

In diesem Artikel stellen Sie die folgenden Ressourcen und Erweiterungen bereit:

* Microsoft.Storage/storageAccounts
* Microsoft.Network/virtualNetworks
* Microsoft.Network/publicIPAddresses
* Microsoft.Network/loadBalancers
* Microsoft.Network/networkInterfaces
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.Insights.VMDiagnosticsSettings
* Microsoft.Insights/autoscaleSettings

Weitere Informationen über Resource Manager-Ressource finden Sie unter [Azure Resource Manager im Vergleich zur klassischen Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="step-1-install-azure-powershell"></a>Schritt 1: Installieren von Azure PowerShell
Unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) erfahren Sie, wie Sie die neueste Version von Azure PowerShell installieren, Ihr Abonnement auswählen und sich bei Azure anmelden.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Schritt 2: Erstellen einer Ressourcengruppe und eines Speicherkontos
1. **Erstellen einer Ressourcengruppe** – Alle Ressourcen müssen in einer Ressourcengruppe bereitgestellt werden. Erstellen Sie mithilfe des Cmdlets [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) eine Ressourcengruppe namens **vmsstestrg1**.
2. **Erstellen eines Speicherkontos** – In diesem Speicherkonto wird die Vorlage gespeichert. Verwenden Sie [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) , um ein Speicherkonto mit dem Namen **vmsstestsa**zu erstellen.

## <a name="step-3-create-the-template"></a>Schritt 3: Erstellen der Vorlage
Mit der Azure-Ressourcen-Manager-Vorlage können Sie die Azure-Ressourcen gemeinsam bereitstellen und verwalten. Sie verwenden hierfür eine JSON-Beschreibung der Ressourcen sowie der zugeordneten Bereitstellungsparameter.

1. Erstellen Sie in Ihrem bevorzugten Editor die Datei „C:\VMSSTemplate.json“, und fügen Sie die JSON-Ausgangsstruktur hinzu, um die Vorlage zu unterstützen.

    ```json
    {
      "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      },
      "variables": {
      },
      "resources": [
      ]
    }
    ```

2. Parameter sind nicht immer erforderlich, ermöglichen aber die Eingabe von Werten, wenn die Vorlage bereitgestellt wird. Fügen Sie die folgenden Parameter unter dem übergeordneten Parameterelement hinzu, das Sie der Vorlage hinzugefügt haben.

    ```json   
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
    * Einen Namen für den separaten virtuellen Computer, der zum Zugreifen auf die Computer in der Skalierungsgruppe verwendet wird.
    * Der Name des Speicherkontos, unter dem die Vorlage gespeichert wird.
    * Die Anzahl virtueller Computer, die zu Beginn in der Skalierungsgruppe erstellt werden sollen.
    * Der Name und das Kennwort des Administratorkontos auf den virtuellen Computern.
    * Ein Namenspräfix für die Ressourcen, die zur Unterstützung der Skalierungsgruppe erstellt werden.

3. Variablen können in einer Vorlage zum Festlegen der Werte verwendet werden, die sich häufig ändern, oder um die Werte festzulegen, die aus einer Kombination von Parameterwerten erstellt werden müssen. Fügen Sie diese Variablen unter dem übergeordneten Variablenelement hinzu, das Sie der Vorlage hinzugefügt haben.

    ```json
    "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
    "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
    "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
    "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
    "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
    "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
    "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
    "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
      "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```
   
    * DNS-Namen, die von den Netzwerkschnittstellen verwendet werden.

        * Die IP-Adressnamen und -präfixe für das virtuelle Netzwerk und die Subnetze.
        * Die Namen und Bezeichner des virtuellen Netzwerks, des Load Balancers und der Netzwerkschnittstellen.
        * Die Speicherkontonamen für die Konten, die den Computern in der Skalierungsgruppe zugeordnet sind.
        * Einstellungen für die Diagnose-Erweiterung, die auf den virtuellen Computern installiert ist. Weitere Informationen zur Diagnoseerweiterung finden Sie unter [Erstellen eines virtuellen Windows-Computers mit Überwachung und Diagnose mithilfe von Azure Resource Manager-Vorlagen](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

4. Fügen Sie die Speicherkontoressource unter dem übergeordneten Ressourcenelement hinzu, das Sie der Vorlage hinzugefügt haben. In dieser Vorlage wird eine Schleife verwendet, um die empfohlenen fünf Speicherkonten zu erstellen, unter denen die Betriebssystem-Datenträger und Diagnosedaten gespeichert werden. Diese Kontengruppe kann in einer Skalierungsgruppe bis zu 100 virtuelle Computer unterstützen. Dies ist derzeit der mögliche Höchstwert. Jedes Speicherkonto wird mit einem in den Variablen definierten Buchstaben und dem Präfix benannt, das Sie in den Parametern für die Vorlage angeben.

    ```json
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
      "apiVersion": "2015-06-15",
      "copy": {
        "name": "storageLoop",
        "count": 5
      },
      "location": "[resourceGroup().location]",
      "properties": { "accountType": "Standard_LRS" }
    },
    ```

5. Fügen Sie die Ressource für das virtuelle Netzwerk hinzu. Weitere Informationen finden Sie unter [Netzwerkressourcenanbieter](../virtual-network/resource-groups-networking.md).

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
        "subnets": [
          {
            "name": "subnet1",
            "properties": { "addressPrefix": "10.0.0.0/24" }
          }
        ]
      }
    },
    ```

6. Fügen Sie die Ressourcen für die öffentliche IP-Adresse hinzu, die vom Load Balancer und der Netzwerkschnittstelle verwendet werden.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP1')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName1')]"
        }
      }
    },
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP2')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName2')]"
        }
      }
    },
    ```

7. Fügen Sie die Load Balancer-Ressource hinzu, die von der Skalierungsgruppe verwendet wird. Weitere Informationen finden Sie unter [Unterstützung des Azure-Ressourcen-Managers für Load Balancer](../load-balancer/load-balancer-arm.md).

    ```json   
    {
      "apiVersion": "2015-06-15",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
              }
            }
          }
        ],
        "backendAddressPools": [ { "name": "bepool1" } ],
        "inboundNatPools": [
          {
            "name": "natpool1",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPortRangeStart": 50000,
              "frontendPortRangeEnd": 50500,
              "backendPort": 3389
            }
          }
        ]
      }
    },
    ```

8. Fügen Sie die Netzwerkschnittstellen-Ressource hinzu, die vom separaten virtuellen Computer verwendet wird. Da auf Computer in einer Skalierungsgruppe nicht über eine öffentliche IP-Adresse zugegriffen werden kann, wird im gleichen virtuellen Netzwerk ein separater virtueller Computer erstellt, um per Remoteverbindung auf die Computer zugreifen zu können.

    ```json  
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
              },
              "subnet": {
                "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
              }
            }
          }
        ]
      }
    },
    ```

9. Fügen Sie den separaten virtuellen Computer in dem gleichen Netzwerk wie die Skalierungsgruppe hinzu.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": { "vmSize": "Standard_A1" },
        "osProfile": {
          "computername": "[parameters('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2012-R2-Datacenter",
            "version": "latest"
          },
          "osDisk": {
            "name": "[concat(parameters('resourcePrefix'), 'os1')]",
            "vhd": {
              "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"        
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        }
      }
    },
    ```

10. Fügen Sie die Ressource für die VM-Skalierungsgruppe hinzu, und geben Sie die Diagnose-Erweiterung an, die auf allen virtuellen Computern der Skalierungsgruppe installiert ist. Viele Einstellungen für diese Ressource ähneln der Ressource für virtuelle Computer. Sie unterscheiden sich hauptsächlich durch das Kapazitätselement, das die Anzahl virtueller Computer in der Skalierungsgruppe angibt, und das upgradePolicy-Element, das angibt, wie virtuelle Computer aktualisiert werden sollen. Die Skalierungsgruppe wird erst erstellt, wenn alle Speicherkonten gemäß der Angabe des dependsOn-Elements erstellt wurden.

    ```json
    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "apiVersion": "2016-03-30",
      "name": "[parameters('vmSSName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "sku": {
        "name": "Standard_A1",
        "tier": "Standard",
        "capacity": "[parameters('instanceCount')]"
      },
      "properties": {
        "upgradePolicy": {
          "mode": "Manual"
        },
        "virtualMachineProfile": {
          "storageProfile": {
            "osDisk": {
              "vhdContainers": [
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "MicrosoftWindowsServer",
              "offer": "WindowsServer",
              "sku": "2012-R2-Datacenter",
              "version": "latest"
            }
          },
          "osProfile": {
            "computerNamePrefix": "[parameters('vmSSName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
          },
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "networkconfig1",
                "properties": {
                  "primary": "true",
                  "ipConfigurations": [
                    {
                      "name": "ip1",
                      "properties": {
                        "subnet": {
                          "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          },
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Insights.VMDiagnosticsSettings",
                "properties": {
                  "publisher": "Microsoft.Azure.Diagnostics",
                  "type": "IaaSDiagnostics",
                  "typeHandlerVersion": "1.5",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint": "https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. Fügen Sie die autoscaleSettings-Ressource hinzu, um zu definieren, wie die Skalierungsgruppe auf der Grundlage der Prozessorauslastung der enthaltenen Computer angepasst werden soll.

    ```json
    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Processor(_Total)\\% Processor Time",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 50.0
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      }
    }
    ```

    In diesem Tutorial sind folgende Werte von Bedeutung:
    
    * **metricName**  
    Dieser Wert entspricht dem Leistungsindikator, den wir in der Variablen „wadperfcounter“ definiert haben. Anhand dieser Variablen führt die Diagnoseerweiterung die Erfassung für den Indikator **Prozessor(_Total)\% Prozessorzeit** durch.
    
    * **metricResourceUri**  
    Dieser Wert ist der Ressourcenbezeichner der VM-Skalierungsgruppe.
    
    * **timeGrain**  
    Dieser Wert beschreibt die Granularität der erfassten Metriken. In dieser Vorlage ist sie auf eine Minute festgelegt.
    
    * **statistic**  
    Dieser Wert bestimmt, wie die Metriken für die Durchführung der automatischen Skalierungsaktion kombiniert werden sollen. Mögliche Werte sind: Average, Min, Max. In dieser Vorlage wird die durchschnittliche CPU-Gesamtauslastung der virtuellen Computer erfasst.
    
    * **timeWindow**  
    Der Zeitbereich, in dem Instanzdaten gesammelt werden. Der Wert muss zwischen fünf Minuten und zwölf Stunden liegen.
    
    * **timeAggregation**  
    Dieser Wert bestimmt, wie die gesammelten Daten im Laufe der Zeit kombiniert werden sollen. Der Standardwert ist "Average". Mögliche Werte sind: Average, Minimum, Maximum, Last, Total, Count.
    
    * **operator**  
    Der Operator, der zum Vergleichen der Metrikdaten und des Schwellenwerts verwendet wird. Mögliche Werte sind: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    
    * **threshold**  
    Mit diesem Wert wird die Skalierungsaktion ausgelöst. In dieser Vorlage werden die Computer der Skalierungsgruppe hinzugefügt, wenn die durchschnittliche CPU-Nutzung für die Computer der Gruppe über 50 % liegt.
    
    * **direction**  
    Dieser Wert bestimmt, welche Aktion bei Erreichen des Schwellenwerts ausgeführt werden soll. Mögliche Werte sind „Increase“ oder „Decrease“. In dieser Vorlage wird die Anzahl der virtuellen Computer in der Skalierungsgruppe erhöht, wenn der Schwellenwert im festgelegten Zeitfenster über 50 % steigt.
    
    * **type**  
    Die Art der auszuführenden Aktion. Dieser Wert muss auf „ChangeCount“ festgelegt werden.
    
    * **value**  
    Die Anzahl virtueller Computer, die der Skalierungsgruppe hinzugefügt bzw. aus dieser entfernt werden sollen. Dieser Wert muss 1 oder höher lauten. Der Standardwert ist 1. In dieser Vorlage wird die Anzahl der Computer in der Skalierungsgruppe um 1 erhöht, wenn der Schwellenwert erreicht wird.
    
    * **cooldown**  
    Dieser Wert gibt an, wie lange zwischen der letzten Skalierungsaktion und der nächsten Aktion gewartet werden soll. Dieser Wert muss zwischen einer Minute und einer Woche liegen.

12. Speichern Sie die Vorlagendatei.    

## <a name="step-4-upload-the-template-to-storage"></a>Schritt 4: Hochladen der Vorlage in den Speicher
Zum Hochladen der Vorlage müssen Sie den Namen und den primären Schlüssel des in Schritt 1 erstellten Speicherkontos kennen.

1. Legen Sie im Microsoft Azure PowerShell-Fenster eine Variable fest, mit der der Name des Speicherkontos angegeben wird, das Sie in Schritt 1 erstellt haben.
   
    ```powershell
    $storageAccountName = "vmstestsa"
    ```

2. Legen Sie eine Variable fest, mit der der primäre Schlüssel des Speicherkontos angegeben wird.
   
    ```powershell
    $storageAccountKey = "<primary-account-key>"
    ```
   
   Sie erhalten diesen Schlüssel, indem Sie auf das Schlüsselsymbol klicken, wenn Sie im Azure-Portal die Speicherkontoressource anzeigen.
3. Erstellen Sie das Speicherkonto-Kontextobjekt, das zum Überprüfen von Vorgängen für das Speicherkonto verwendet wird.
   
    ```powershell
    $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
    ```

4. Erstellen Sie den Container zum Speichern der Vorlage.

    ```powershell
    $containerName = "templates"
    New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob
    ```

5. Laden Sie die Vorlagendatei in den neuen Container hoch.

    ```powershell   
    $blobName = "VMSSTemplate.json"
    $fileName = "C:\" + $BlobName
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx
    ```

## <a name="step-5-deploy-the-template"></a>Schritt 5: Bereitstellen der Vorlage
Nachdem Sie die Vorlage erstellt haben, können Sie mit dem Bereitstellen der Ressourcen beginnen. Verwenden Sie diesen Befehl, um den Prozess zu starten:

```powershell
New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"
```

Wenn Sie die EINGABETASTE drücken, werden Sie aufgefordert, Werte für die von Ihnen zugewiesenen Variablen anzugeben. Geben Sie die folgenden Werte an:

```powershell
vmName: vmsstestvm1
  vmSSName: vmsstest1
  instanceCount: 5
  adminUserName: vmadmin1
  adminPassword: VMpass1
  resourcePrefix: vmsstest
```

Die Bereitstellung aller Ressourcen dauert etwa 15 Minuten.

> [!NOTE]
> Sie können die Ressourcen auch über das Portal bereitstellen. Verwenden Sie dazu den folgenden Link: „https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>“
> 
> 

## <a name="step-6-monitor-resources"></a>Schritt 6: Überwachen von Ressourcen
Mit den folgenden Methoden können Sie Informationen zu VM-Skalierungsgruppen erhalten:

* Azure-Portal: Über das Portal können Sie derzeit eine begrenzte Menge an Informationen erhalten.
* [Azure-Ressourcen-Explorer](https://resources.azure.com/) : Dieses Tool eignet sich perfekt zum Untersuchen des aktuellen Zustands Ihrer Skalierungsgruppe. Wenn Sie diesem Pfad folgen, wird die Instanzansicht für die von Ihnen erstellte Skalierungsgruppe angezeigt:
  
    subscriptions > {Ihr Abonnement} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

* Azure PowerShell: Verwenden Sie den folgenden Befehl, um Informationen zu erhalten:

  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  ```
  
  Oder
  
  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView
  ```

* Stellen Sie eine Verbindung mit dem separaten virtuellen Computer her. (Gehen Sie dazu wie bei jedem anderen Computer vor.) Anschließend können Sie per Remoteverbindung auf die virtuellen Computer in der Skalierungsgruppe zugreifen und einzelne Prozesse überwachen.

> [!NOTE]
> Eine vollständige REST-API zum Abrufen von Informationen zu Skalierungsgruppen finden Sie unter [VM-Skalierungsgruppen](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-the-resources"></a>Schritt 7: Entfernen der Ressourcen
Da in Azure die genutzten Ressourcen in Rechnung gestellt werden, empfiehlt es sich grundsätzlich, nicht mehr benötigte Ressourcen zu löschen. Sie müssen nicht jede Ressource einzeln aus einer Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden automatisch auch alle darin enthaltenen Ressourcen gelöscht.

  ```powershell
  Remove-AzureRmResourceGroup -Name vmsstestrg1
  ```

Wenn Sie die Ressourcengruppe beibehalten möchten, können Sie auch nur die Skalierungsgruppe löschen.

  ```powershell
  Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"
  ```

## <a name="next-steps"></a>Nächste Schritte
* Verwalten Sie die Skalierungsgruppe, die Sie gerade erstellt haben, mithilfe der Informationen unter [Manage virtual machines in a Virtual Machine Scale Set](virtual-machine-scale-sets-windows-manage.md) (Verwalten virtueller Computer in einer VM-Skalierungsgruppe).
* Informieren Sie sich unter [Vertikale automatische Skalierung mit VM-Skalierungsgruppen](virtual-machine-scale-sets-vertical-scale-reprovision.md)
* Beispiele für Überwachungsfeatures von Azure Monitor finden Sie unter [Azure Monitor – PowerShell-Schnellstartbeispiele](../monitoring-and-diagnostics/insights-powershell-samples.md)
* Informationen zu Benachrichtigungsfeatures finden Sie unter [Verwenden von automatischen Skalierungsvorgängen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).
* Informationen zur Verwendung von Überwachungsprotokollen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen in Azure Monitor finden Sie [hier](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).


