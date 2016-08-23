<properties
	pageTitle="Automatische Skalierung von Skalierungsgruppen virtueller Linux-Computer | Microsoft Azure"
	description="Einrichten der automatischen Skalierung für eine Skalierungsgruppe virtueller Linux-Computer mit der Azure-Befehlszeilenschnittstelle"
	services="virtual-machine-scale-sets"
	documentationCenter=""
	authors="davidmu1"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machine-scale-sets"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/10/2016"
	ms.author="davidmu"/>

# Automatisches Skalieren von Linux-Computern in einer VM-Skalierungsgruppe

VM-Skalierungsgruppen erleichtern Ihnen das Bereitstellen und Verwalten identischer virtueller Computer als Gruppe. Skalierungsgruppen stellen eine hoch skalierbare und anpassbare Compute-Ebene für Anwendungen mit Hyperskalierung bereit und bieten Unterstützung für Images der Windows-Plattform, Images der Linux-Plattform, benutzerdefinierte Images und Erweiterungen. Weitere Informationen finden Sie unter [Virtual Machine Scale Sets Overview](virtual-machine-scale-sets-overview.md) (Übersicht über VM-Skalierungsgruppen).

In diesem Tutorial erfahren Sie, wie Sie eine VM-Skalierungsgruppe mit virtuellen Linux-Computern erstellen, indem Sie die aktuellste Version von Ubuntu Linux verwenden und die Computer der Gruppe automatisch skalieren. Hierzu erstellen Sie eine Azure Resource Manager-Vorlage und stellen diese mit der Azure-Befehlszeilenschnittstelle bereit. Weitere Informationen zu Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../resource-group-authoring-templates.md). Weitere Informationen zur automatischen Skalierung von Skalierungsgruppen finden Sie unter [Automatic scaling and Virtual Machine Scale Sets](virtual-machine-scale-sets-autoscale-overview.md) (Automatische Skalierung und VM-Skalierungsgruppen).

In diesem Tutorial stellen Sie die folgenden Ressourcen und Erweiterungen bereit:

- Microsoft.Storage/storageAccounts
- Microsoft.Network/virtualNetworks
- Microsoft.Network/publicIPAddresses
- Microsoft.Network/loadBalancers
- Microsoft.Network/networkInterfaces
- Microsoft.Compute/virtualMachines
- Microsoft.Compute/virtualMachineScaleSets
- Microsoft.Insights.VMDiagnosticsSettings
- Microsoft.Insights/autoscaleSettings

Weitere Informationen zu Ressourcen-Manager-Ressourcen finden Sie unter [Azure-Compute, -Netzwerk- und -Speicheranbieter unter dem Azure-Ressourcen-Manager](../virtual-machines/virtual-machines-linux-compare-deployment-models.md).

[Installieren Sie die Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), bevor Sie mit den Schritten in diesem Tutorial beginnen.

## Schritt 1: Erstellen einer Ressourcengruppe und eines Speicherkontos

1. **Anmelden bei Microsoft Azure** – Stellen Sie in der Befehlszeilenschnittstelle (Bash, Terminal, Eingabeaufforderung) sicher, dass Sie sich im Resource Manager-Modus befinden, indem Sie `azure config mode arm` eingeben. [Melden Sie sich dann mit Ihrer Geschäfts- oder Schulkonto-ID an](../xplat-cli-connect.md#use-the-log-in-method), indem Sie `azure login` eingeben und die Schritte befolgen, um eine interaktive Anmeldeoberfläche für Ihr Azure-Konto zu erhalten.

	> [AZURE.NOTE] Wenn Sie über eine Arbeits- oder Schulkonto-ID verfügen und wissen, dass die zweistufige Authentifizierung nicht aktiviert ist, können Sie `azure login -u` zusammen mit der Arbeits- oder Schulkonto-ID verwenden, um sich ohne eine interaktive Sitzung anzumelden. Wenn Sie nicht über eine Arbeits- oder Schulkonto-ID verfügen, können Sie eine [Arbeits- oder Schulkonto-ID mit Ihrem persönlichen Microsoft-Konto erstellen](../virtual-machines/resource-group-create-work-id-from-personal.md).

2. **Erstellen einer Ressourcengruppe** – Alle Ressourcen müssen in einer Ressourcengruppe bereitgestellt werden. Geben Sie der Ressourcengruppe für dieses Tutorial den Namen **vmsstest1**.

        azure group create vmsstestrg1 centralus

3. **Bereitstellen eines Speicherkontos in der neuen Ressourcengruppe** – In diesem Tutorial werden mehrere Speicherkonten für die VM-Skalierungsgruppe verwendet. Erstellen Sie ein Speicherkonto mit dem Namen **vmsstestsa**. Lassen Sie das Fenster mit der Befehlszeilenschnittstelle zur Ausführung weiterer Schritte in diesem Tutorial geöffnet:

        azure storage account create -g vmsstestrg1 -l centralus --kind Storage --sku-name LRS vmsstestsa

## Schritt 2: Erstellen der Vorlage
Mit der Azure-Ressourcen-Manager-Vorlage können Sie die Azure-Ressourcen gemeinsam bereitstellen und verwalten. Sie verwenden hierfür eine JSON-Beschreibung der Ressourcen sowie der zugeordneten Bereitstellungsparameter.

1. Erstellen Sie in Ihrem bevorzugten Editor die Datei „C:\\VMSSTemplate.json“, und fügen Sie die JSON-Ausgangsstruktur hinzu, um die Vorlage zu unterstützen.

        {
          "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
          }
          "variables": {
          }
          "resources": [
          ]
        }

2. Parameter sind nicht immer erforderlich, aber sie erleichtern die Vorlagenverwaltung. Sie ermöglichen das Angeben von Werten für die Vorlage und das Beschreiben des Typs eines Werts (des Standardwerts, falls erforderlich) und unter Umständen der zulässigen Werte des Parameters. Fügen Sie die folgenden Parameter unter dem übergeordneten Parameterelement hinzu, das Sie der Vorlage hinzugefügt haben.

        "vmName": {
          "type": "string"
        },
        "vmSSName": {
          "type": "string"
        },
        "instanceCount": {
          "type": "string"
        },
        "adminUsername": {
          "type": "string"
        },
        "adminPassword": {
          "type": "securestring"
        },
        "resourcePrefix": {
          "type": "string"
        }
            
	- Einen Namen für den separaten virtuellen Computer, der zum Zugreifen auf die Computer in der Skalierungsgruppe verwendet wird.
	- Einen Namen für das Speicherkonto, unter dem die Vorlage gespeichert wird.
	- Die Anzahl der Instanzen von virtuellen Computern, die zu Beginn in der Skalierungsgruppe erstellt werden sollen.
	- Ein Name und Kennwort des Administratorkontos auf den virtuellen Maschinen.
	- Ein Präfix für die Ressourcen, die in der Ressourcengruppe erstellt werden.

3. Variablen können in einer Vorlage zum Festlegen der Werte verwendet werden, die sich häufig ändern, oder um die Werte festzulegen, die aus einer Kombination von Parameterwerten erstellt werden müssen. Fügen Sie diese Variablen unter dem übergeordneten Variablenelement hinzu, das Sie der Vorlage hinzugefügt haben.

        "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
        "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
        "vmSize": "Standard_A0",
        "imagePublisher": "Canonical",
        "imageOffer": "UbuntuServer",
        "imageVersion": "15.10",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "Subnet",
        "subnetPrefix": "10.0.0.0/24",
        "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
        "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
        "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
        "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
        "nicName1": "[concat(parameters('resourcePrefix'),'nc1')]",
        "nicName2": "[concat(parameters('resourcePrefix'),'nc2')]",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "publicIPAddressID1": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIP1'))]",
        "publicIPAddressID2": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIP2'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "nicId": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName2'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "storageAccountType": "Standard_LRS",
        "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
        "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
        "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
        "wadlogs": "<WadCfg><DiagnosticMonitorConfiguration>",
        "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod="PT1M"><PerformanceCounterConfiguration counterSpecifier="\\Processor\\PercentProcessorTime" sampleRate="PT15S" unit="Percent"><annotation displayName="CPU percentage guest OS" locale="de-DE"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId="')]",
        "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
        "wadcfgxend": "[concat('"><MetricAggregation scheduledTransferPeriod="PT1H"/><MetricAggregation scheduledTransferPeriod="PT1M"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
        
	- DNS-Namen, die von den Netzwerkschnittstellen verwendet werden.
	- Die Größe der virtuellen Computer, die in der Skalierungsgruppe verwendet werden. Weitere Informationen zu den Größen von virtuellen Computern finden Sie unter [Größen für virtuelle Computer](../virtual-machines/virtual-machines-size-specs.md).
	- Die Informationen zum Plattformimage zum Festlegen des Betriebssystems, das auf den virtuellen Computern in der Skalierungsgruppe ausgeführt wird. Weitere Informationen zur Auswahl von Images finden Sie unter [Navigieren in und Auswählen von Images für virtuelle Azure-Computer mit Windows PowerShell und der Azure-Befehlszeilenschnittstelle](../virtual-machines/resource-groups-vm-searching.md).
	- Die IP-Adressnamen und -präfixe für das virtuelle Netzwerk und die Subnetze.
	- Die Namen und Bezeichner des virtuellen Netzwerks, des Load Balancers und der Netzwerkschnittstellen.
	- Die Speicherkontonamen für die Konten, die den Computern in der Skalierungsgruppe zugeordnet sind.
	- Einstellungen für die Diagnose-Erweiterung, die auf den virtuellen Computern installiert ist. Weitere Informationen zur Diagnose-Erweiterung finden Sie unter [Erstellen eines virtuellen Windows-Computers mit Überwachung und Diagnose mithilfe von Azure-Ressourcen-Manager-Vorlagen](../virtual-machines/virtual-machines-extensions-diagnostics-windows-template.md).

4. Fügen Sie die Speicherkontoressource unter dem übergeordneten Ressourcenelement hinzu, das Sie der Vorlage hinzugefügt haben. Für diese Vorlage wird eine Schleife verwendet, um die empfohlenen fünf Speicherkonten zu erstellen, unter denen die Betriebssystem-Datenträger und Diagnosedaten gespeichert werden. Diese Kontengruppe kann in einer Skalierungsgruppe bis zu 100 virtuelle Computer unterstützen. Dies ist derzeit der mögliche Höchstwert. Jedes Speicherkonto wird mit einem Buchstaben bezeichnet, der in den Variablen zusammen mit dem Suffix definiert wurde, das Sie in den Parametern für die Vorlage angeben.

        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": {
            "accountType": "[variables('storageAccountType')]"
          }
        },

5. Fügen Sie die Ressource für das virtuelle Netzwerk hinzu. Weitere Informationen finden Sie unter [Netzwerkressourcenanbieter](../virtual-network/resource-groups-networking.md).

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[variables('virtualNetworkName')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[variables('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[variables('subnetName')]",
                "properties": {
                  "addressPrefix": "[variables('subnetPrefix')]"
                }
              }
            ]
          }
        },

6. Fügen Sie die Ressourcen für die öffentliche IP-Adresse hinzu, die vom Load Balancer und der Netzwerkschnittstelle verwendet werden.

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

7. Fügen Sie die Load Balancer-Ressource hinzu, die von der Skalierungsgruppe verwendet wird. Weitere Informationen finden Sie unter [Unterstützung des Azure-Ressourcen-Managers für Load Balancer](../load-balancer/load-balancer-arm.md).
        
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
                    "id": "[variables('publicIPAddressID1')]"
                  }
                }
              }
            ],
            "backendAddressPools": [
              {
                "name": "bepool1"
              }
            ],
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
                  "backendPort": 22
                }
              }
            ]
          }
        },

8. Fügen Sie die Netzwerkschnittstellen-Ressource hinzu, die vom separaten virtuellen Computer verwendet wird. Da Computer in einer VM-Skalierungsgruppe nicht direkt über eine öffentliche IP-Adresse zugänglich sind, wird im virtuellen Netzwerk der Skalierungsgruppe ein separater virtueller Computer erstellt und genutzt, um per Remoteverbindung auf die Computer der Gruppe zuzugreifen.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[variables('nicName1')]",
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
                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/',variables('subnetName'))]"
                  }
                }
              }
            ]
          }
        },

9. Fügen Sie den separaten virtuellen Computer in dem gleichen Netzwerk wie die Skalierungsgruppe hinzu.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[parameters('vmName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "storageLoop",
            "[concat('Microsoft.Network/networkInterfaces/', variables('nicName1'))]"
          ],
          "properties": {
            "hardwareProfile": {
              "vmSize": "[variables('vmSize')]"
            },
            "osProfile": {
              "computername": "[parameters('vmName')]",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "[variables('imagePublisher')]",
                "offer": "[variables('imageOffer')]",
                "sku": "[variables('imageVersion')]",
                "version": "latest"
              },
              "osDisk": {
                "name": "osdisk1",
                "vhd": {
                  "uri":  "[concat('https://',parameters('resourcePrefix'),'sa.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'osdisk1.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName1'))]"
                }
              ]
            }
          }
        },

10.	Fügen Sie die Ressource für die VM-Skalierungsgruppe hinzu, und geben Sie die Diagnose-Erweiterung an, die auf allen virtuellen Computern der Skalierungsgruppe installiert ist. Viele Einstellungen für diese Ressource ähneln der Ressource für virtuelle Computer. Die Hauptunterschiede sind das Hinzufügen des Kapazitätselements, das angibt, wie viele virtuelle Computer in der Skalierungsgruppe initialisiert werden sollten, und „upgradePolicy“ zur Angabe, wie Updates an virtuellen Computern in der Skalierungsgruppe vorgenommen werden. Die Skalierungsgruppe wird erst erstellt, wenn alle Speicherkonten wie mit dem dependsOn-Element angegeben erstellt wurden.

            {
              "type": "Microsoft.Compute/virtualMachineScaleSets",
              "apiVersion": "2016-03-30",
              "name": "[parameters('vmSSName')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "storageLoop",
                "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
              ],
              "sku": {
                "name": "[variables('vmSize')]",
                "tier": "Standard",
                "capacity": "[parameters('instanceCount')]"
              }
              "properties": {
                "upgradePolicy": {
                  "mode": "Manual"
                },
                "virtualMachineProfile": {
                  "storageProfile": {
                    "osDisk": {
                      "vhdContainers": [
                        "[concat('https://', parameters('resourcePrefix'), 'a.blob.core.windows.net/vmss')]",
                        "[concat('https://', parameters('resourcePrefix'), 'g.blob.core.windows.net/vmss')]",
                        "[concat('https://', parameters('resourcePrefix'), 'm.blob.core.windows.net/vmss')]",
                        "[concat('https://', parameters('resourcePrefix'), 's.blob.core.windows.net/vmss')]",
                        "[concat('https://', parameters('resourcePrefix'), 'y.blob.core.windows.net/vmss')]"
                      ],
                      "name": "vmssosdisk",
                      "caching": "ReadOnly",
                      "createOption": "FromImage"
                    },
                    "imageReference": {
                      "publisher": "[variables('imagePublisher')]",
                      "offer": "[variables('imageOffer')]",
                      "sku": "[variables('imageVersion')]",
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
                        "name": "[variables('nicName2')]",
                        "properties": {
                          "primary": "true",
                          "ipConfigurations": [
                            {
                              "name": "ip1",
                              "properties": {
                                "subnet": {
                                  "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/',variables('subnetName'))]"
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
                        "name":"LinuxDiagnostic",
                        "properties": {
                          "publisher":"Microsoft.OSTCExtensions",
                          "type":"LinuxDiagnostic",
                          "typeHandlerVersion":"2.1",
                          "autoUpgradeMinorVersion":false,
                          "settings": {
                            "xmlCfg":"[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                            "storageAccount":"[variables('diagnosticsStorageAccountName')]"
                          },
                          "protectedSettings": {
                            "storageAccountName":"[variables('diagnosticsStorageAccountName')]",
                            "storageAccountKey":"[listkeys(variables('accountid'), '2015-06-15').key1]",
                            "storageAccountEndPoint":"https://core.windows.net"
                          }
                        }
                      }
                    ]
                  }
                }
              }
            },

11.	Fügen Sie die autoscaleSettings-Ressource hinzu, mit der definiert wird, wie die Skalierungsgruppe basierend auf der Prozessornutzung auf den Computern in der Gruppe angepasst wird.
        
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
                          "metricName": "\\Processor\\PercentProcessorTime",
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
    
    In diesem Tutorial lauten die wichtigen Werte wie folgt:
    
    - **metricName**: Entspricht dem Leistungsindikator, den wir in der Variablen „wadperfcounter“ definiert haben. Anhand dieser Variablen führt die Diagnoseerweiterung die Erfassung für den Indikator **Processor\\PercentProcessorTime** durch.
    - **metricResourceUri**: Dies ist der Ressourcenbezeichner der VM-Skalierungsgruppe.
    - **timeGrain**: Dies ist die Granularität der erfassten Metriken. In dieser Vorlage ist sie auf eine Minute festgelegt.
    - **statistic**: Hiermit wird bestimmt, wie die Metriken für die Durchführung der automatischen Skalierungsaktion kombiniert werden. Mögliche Werte sind: Average, Min, Max. In dieser Vorlage suchen wir nach der durchschnittlichen CPU-Gesamtnutzung für die virtuellen Computer der Skalierungsgruppe.
    - **timeWindow**: Dies ist der Zeitbereich, in dem Instanzdaten gesammelt werden. Der Wert muss zwischen fünf Minuten und zwölf Stunden liegen.
    - **timeAggregation**: Hiermit wird bestimmt, wie die gesammelten Daten im Laufe der Zeit kombiniert werden sollen. Der Standardwert ist "Average". Mögliche Werte sind: Average, Minimum, Maximum, Last, Total, Count.
    - **operator**: Dies ist der Operator, der zum Vergleichen der Metrikdaten und des Schwellenwerts verwendet wird. Mögliche Werte sind: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    - **threshold**: Dies ist der Wert, mit dem die Skalierungsaktion ausgelöst wird. In dieser Vorlage werden die Computer der Skalierungsgruppe hinzugefügt, wenn die durchschnittliche CPU-Nutzung für die Computer der Gruppe über 50 % liegt.
    - **direction**: Hiermit wird bestimmt, welche Aktion ausgeführt wird, wenn der Schwellenwert erreicht ist. Mögliche Werte sind „Increase“ oder „Decrease“. In dieser Vorlage wird die Anzahl der virtuellen Computer in der Skalierungsgruppe erhöht, wenn der Schwellenwert im festgelegten Zeitfenster über 50 % steigt.
    - **type**: Dies ist die Art von Aktion, die eintreten soll. Diese Option muss auf „ChangeCount“ festgelegt werden.
    - **value**: Dies ist die Anzahl der virtuellen Computer, die der Skalierungsgruppe hinzugefügt bzw. daraus entfernt werden. Dieser Wert muss 1 oder höher lauten. Der Standardwert ist 1. In dieser Vorlage wird die Anzahl der Computer in der Skalierungsgruppe um 1 erhöht, wenn der Schwellenwert erreicht wird.
    - **cooldown**: Gibt an, wie lange nach der letzten Skalierungsaktion gewartet wird, bevor die nächste Aktion durchgeführt wird. Dieser Wert muss zwischen einer Minute und einer Woche liegen.

12.	Speichern Sie die Vorlagendatei.

## Schritt 3: Hochladen der Vorlage in den Speicher

Die Vorlage kann aus Ihrer Befehlszeilenschnittstelle hochgeladen werden, wenn Sie den Kontonamen und den primären Schlüssel des Speicherkontos kennen, das Sie in Schritt 1 erstellt haben.

1. Führen Sie in Ihrer Befehlszeilenschnittstelle (Bash, Terminal, Eingabeaufforderung) diese Befehle zum Festlegen der Umgebungsvariablen aus, die zum Zugreifen auf das Speicherkonto benötigt werden:

		export AZURE_STORAGE_ACCOUNT={account_name}
		export AZURE_STORAGE_ACCESS_KEY={key}

	Sie erhalten den Schlüssel, indem Sie auf das Schlüsselsymbol klicken, wenn Sie im Azure-Portal die Speicherkontoressource anzeigen. Geben Sie bei der Verwendung einer Windows-Eingabeaufforderung **set** anstelle von „export“ ein.

2. Erstellen Sie den Container zum Speichern der Vorlage:

		azure storage container create -p Blob templates

3. Laden Sie die Vorlagendatei in den neuen Container hoch.

		azure storage blob upload VMSSTemplate.json templates VMSSTemplate.json

## Schritt 4: Bereitstellen der Vorlage

Nachdem Sie die Vorlage erstellt haben, können Sie mit dem Bereitstellen der Ressourcen beginnen. Verwenden Sie diesen Befehl, um den Prozess zu starten:

	azure group deployment create --template-uri https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json vmsstestrg1 vmsstestdp1

Wenn Sie die EINGABETASTE drücken, werden Sie aufgefordert, Werte für die von Ihnen zugewiesenen Variablen anzugeben. Geben Sie die folgenden Werte an:

	vmName: vmsstestvm1
	vmSSName: vmsstest1
	instanceCount: 5
	adminUserName: vmadmin1
	adminPassword: VMpass1
	resourcePrefix: vmsstest

Es dauert ungefähr 15 Minuten, bis alle Ressourcen bereitgestellt wurden.

>[AZURE.NOTE]Sie können zum Bereitstellen der Ressourcen auch das Portal verwenden. Verwenden Sie hierfür den folgenden Link: https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>

## Schritt 5: Überwachen von Ressourcen

Mit den folgenden Methoden können Sie Informationen zu VM-Skalierungsgruppen erhalten:

 - Azure-Portal: Über das Portal können Sie derzeit eine begrenzte Menge an Informationen erhalten.
 - [Azure-Ressourcen-Explorer](https://resources.azure.com/): Dies ist das Tool, das zum Untersuchen des aktuellen Status Ihrer Skalierungsgruppe am besten geeignet ist. Wenn Sie diesem Pfad folgen, wird die Instanzansicht für die von Ihnen erstellte Skalierungsgruppe angezeigt:

		subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

 - Azure-Befehlszeilenschnittstelle: Verwenden Sie den folgenden Befehl, um Informationen zu erhalten:

		azure resource show -n vmsstest1 -r Microsoft.Compute/virtualMachineScaleSets -o 2015-06-15 -g vmsstestrg1

 - Stellen Sie eine Verbindung mit dem virtuellen Jumpbox-Computer her, wie Sie dies auch für jeden anderen Computer tun würden. Sie können dann per Remoteverbindung auf die virtuellen Computer der Skalierungsgruppe zugreifen, um die einzelnen Prozesse zu überwachen.

>[AZURE.NOTE]Eine vollständige REST-API zum Abrufen von Informationen zu Skalierungsgruppen finden Sie unter [VM-Skalierungsgruppen](https://msdn.microsoft.com/library/mt589023.aspx).

## Schritt 6: Entfernen der Ressourcen

Da in Azure die genutzten Ressourcen in Rechnung gestellt werden, empfiehlt es sich grundsätzlich, nicht mehr benötigte Ressourcen zu löschen. Sie müssen nicht jede Ressource einzeln aus einer Ressourcengruppe löschen. Sie können auch die Ressourcengruppe löschen, sodass alle darin enthaltenen Ressourcen automatisch gelöscht werden.

		azure group delete vmsstestrg1

## Nächste Schritte

- Beispiele für Überwachungsfeatures von Azure Insights finden Sie unter [Azure Insights – Schnellstartbeispiele für plattformübergreifende Befehlszeilenschnittstelle](../azure-portal/insights-cli-samples.md).
- Informationen zu Benachrichtigungsfeatures finden Sie unter [Verwenden von automatischen Skalierungsvorgängen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen in Azure Insights](../azure-portal/insights-autoscale-to-webhook-email.md) sowie unter [Verwenden von Überwachungsprotokollen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen in Azure Insights](../azure-portal/insights-auditlog-to-webhook-email.md).
- Sehen Sie sich die Vorlage [Autoscale a VM Scale Set running a Ubuntu/Apache/PHP app](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale) (Automatisches Skalieren einer VM-Skalierungsgruppe mit ausgeführter Ubuntu-/Apache-/PHP-App) an: Diese richtet einen LAMP-Stapel ein, um die automatische Skalierungsfunktion von VM-Skalierungsgruppen auszuführen.

<!---HONumber=AcomDC_0810_2016-->