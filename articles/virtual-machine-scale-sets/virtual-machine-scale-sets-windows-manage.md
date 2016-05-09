<properties
	pageTitle="Verwalten von VMs in einer VM-Skalierungsgruppe | Microsoft Azure"
	description="Verwalten virtueller Computer in einer VM-Skalierungsgruppe mit Azure PowerShell."
	services="virtual-machine-scale-sets"
	documentationCenter=""
	authors="davidmu1"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machine-scale-sets"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/22/2016"
	ms.author="davidmu"/>

# Verwalten virtueller Computer in einer VM-Skalierungsgruppe

Azure PowerShell bietet ein hohes Maß an Leistung und Flexibilität beim Verwalten von Ressourcen in Microsoft Azure. Verwalten Sie mit den in diesem Artikel beschriebenen Aufgaben VM-Ressourcen in Ihrer VM-Skalierungsgruppe.

- [Anzeigen von Informationen zu einer VM-Skalierungsgruppe](#displayvm)
- [Starten eines virtuellen Computers in einer Skalierungsgruppe](#start)
- [Stoppen eines virtuellen Computers in einer Skalierungsgruppe](#stop)
- [Neustarten eines virtuellen Computers in einer Skalierungsgruppe](#restart)
- [Löschen eines virtuellen Computers aus einer Skalierungsgruppe](#delete)

Alle Aufgaben im Zusammenhang mit dem Verwalten eines virtuellen Computers in einer Skalierungsgruppe erfordern, dass Sie die Instanz-ID des Computers kennen, den Sie verwalten möchten. Sie können mit dem [Azure-Ressourcen-Explorer](https://resources.azure.com) die Instanz-ID eines virtuellen Computers in einer Skalierungsgruppe finden. Sie können mit dem Ressourcen-Explorer auch den Status der Aufgaben überprüfen, die Sie fertig stellen.

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

## <a id="displayvm"></a>Anzeigen von Informationen zu einer VM-Skalierungsgruppe

Sie können allgemeine Informationen über eine Skalierungsgruppe abrufen, was auch als Instanzansicht bezeichnet wird. Oder Sie können spezifischere Informationen abrufen, z.B. Informationen zu den Ressourcen in der Gruppe.

Ersetzen Sie in diesem Befehl *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, und *scale set name* durch den Namen der VM-Skalierungsgruppe, und führen Sie den Befehl aus:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Folgendes sollte angezeigt werden:

    Sku                      :  {
                                  "name": "Standard_A0",
                                  "tier": "Standard",
                                  "capacity": 4
                                }
    UpgradePolicy            :  {
                                  "mode": "Manual"
                                }
    VirtualMachineProfile    :  {
                                  "osProfile": {
                                    "computerNamePrefix": "myvmss1",
                                    "adminUserName": "user1",
                                    "adminPassword": null,
                                    "customData": null,
                                    "windowsConfiguration": {
                                      "provisionVMAgent": true,
                                      "enableAutomaticUpdates": true,
                                      "timeZone": null,
                                      "additionalUnattendContent": null,
                                      "winRM": null
                                    }
                                    "linuxConfiguration": null,
                                    "secrets": []
                                  },
                                  "storageProfile": {
                                    "imageReference": {
                                      "publisher": "MicrosoftWindowsServer",
                                      "offer": "WindowsServer",
                                      "sku": "2012-R2-Datacenter",
                                      "version": "latest"
                                    },
                                    "osDisk": {
                                      "name": "vmssosdisk",
                                      "caching": "ReadOnly",
                                      "createOption": "FromImage",
                                      "osType": null,
                                      "image": null,
                                      "vhdContainers": [
                                        "https://amyst1.blob.core.windows.net/vmss",
                                        "https://gmyst1.blob.core.windows.net/vmss",
                                        "https://mmyst1.blob.core.windows.net/vmss",
                                        "https://smyst1.blob.core.windows.net/vmss",
                                        "https://ymyst1.blob.core.windows.net/vmss"
                                      ]
                                    }
                                  },
                                  "networkProfile": {
                                    "networkInterfaceConfigurations": [
                                      {
                                        "name": "myresnc2",
                                        "properties.primary": true,
                                        "properties.ipConfigurations": [
                                          {
                                            "name": "ip1",
                                            "properties.subnet": {
                                              "id": "/subscriptions/{subscription-id}/resourceGroups/rg1/providers/Microsoft.Network/virtualNetworks/myresvn1/subnets/myressn1"
                                            },
                                            "properties.loadBalancerBackendAddressPools": [
                                              {
                                                "id": "/subscriptions/{subscription-id}/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/myreslb1/backendAddressPools/bepool1"
                                              }
                                            ],
                                            "properties.loadBalancerInboundNatPools": [
                                              {
                                                "id": "/subscriptions/{subscription-id}/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/myreslb1/inboundNatPools/natpool1"
                                              }
                                            ],
                                            "id": null
                                          }
                                        ],
                                        "id": null
                                      }
                                    ]
                                  },
                                  "extensionProfile": {
                                    "extensions": [
                                      {
                                        "name": "Microsoft.Insights.VMDiagnosticsSettings",
                                        "properties.publisher": "Microsoft.Azure.Diagnostics",
                                        "properties.type": "IaaSDiagnostics",
                                        "properties.typeHandlerVersion": "1.5",
                                        "properties.autoUpgradeMinorVersion": true,
                                        "properties.settings": {
                                          "xmlCfg": "{encoded configuration}",
                                          "storageAccount": "amyst1"
                                        },
                                        "properties.protectedSettings": null,
                                        "properties.provisioningState": null,
                                        "id": null
                                      }
                                    ]
                                  }
                                }
    ProvisioningState           : Succeeded
    Id                          : /subscriptions/{subscription-id}/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                        : myvmss1
	Type                        : Microsoft.Compute/virtualMachineScaleSets
	Location                    : westus
	Tags.Count                  : 0
	Tags                        :

Um allgemeine Informationen abzurufen, ersetzen Sie *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, und *scale set name* durch den Namen der VM-Skalierungsgruppe, und führen Sie den Befehl aus:

	Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView

Folgendes sollte angezeigt werden:

    VirtualMachine   :  {
                          "statusesSummary": [
                            {
                              "code": "ProvisioningState/succeeded",
                              "count": 4
                            }
                          ]
                        }
    Extensions.Count :  1
    Extensions       :  {
                          "name": "Microsoft.Insights.VMDiagnosticsSettings",
                          "statusesSummary": [
                            {
                              "code": "ProvisioningState/succeeded",
                              "count": 4
                            }
                          ]
                        }
	Statuses.Count   :  1
	Statuses         :  {
                          "code": "ProvisioningState/succeeded",
                          "level": "Info",
                          "displayStatus": "Provisioning succeeded",
                          "message": null,
                          "time": "2016-03-14T20:29:37.170809Z"
                        }

## <a id="start"></a>Starten eines virtuellen Computers in einer Skalierungsgruppe

Ersetzen Sie in diesem Befehl *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, *VM scale set name* durch den Namen der Skalierungsgruppe und *instance id* durch den Bezeichner der VM, die Sie neu starten möchten, und führen Sie den Befehl aus:

    Start-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId "instance id"

Im Ressourcen-Explorer sehen wir, dass der Status der Instanz **running** ist:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

## <a id="stop"></a>Stoppen eines virtuellen Computers in einer Skalierungsgruppe

Ersetzen Sie in diesem Befehl *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, *scale set name* durch den Namen der Skalierungsgruppe und *instance id* durch den Bezeichner der VM, die Sie stoppen möchten, und führen Sie den Befehl aus:

	Stop-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId "instance id"

Im Ressourcen-Explorer sehen wir, dass der Status der Instanz **deallocated** ist:

	"statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]

## <a id="restart"></a>Neustarten eines virtuellen Computers in einer Skalierungsgruppe

Ersetzen Sie in diesem Befehl *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, *scale set name* durch den Namen der Skalierungsgruppe und *instance id* durch den Bezeichner der VM, die Sie neu starten möchten, und führen Sie den Befehl aus:

	Restart-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId "instance id"

## <a id="delete"></a>Entfernen eines virtuellen Computers aus einer Skalierungsgruppe

Ersetzen Sie in diesem Befehl *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, *scale set name* durch den Namen der Skalierungsgruppe und *instance id* durch den Bezeichner der VM, die Sie aus der Skalierungsgruppe entfernen möchten, und führen Sie den Befehl aus:

	Remove-AzureRmVmssVM -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId "instance id"

<!---HONumber=AcomDC_0427_2016-->