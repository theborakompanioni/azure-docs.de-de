<properties
   pageTitle="Verwenden der DSC-Erweiterung mit VM-Skalierungsgruppen | Microsoft Azure"
   description="Verwenden von VM-Skalierungsgruppen mit der Azure DSC-Erweiterung"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-scale-sets"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="08/29/2016"
   ms.author="zachal"/>

# Verwenden von VM-Skalierungsgruppen mit der Azure DSC-Erweiterung

[VM-Skalierungsgruppen](virtual-machines-windows-vmss-powershell-creating.md) können mit dem Erweiterungshandler [Azure Desired State Configuration (DSC)](virtual-machines-windows-extensions-dsc-overview.md) verwendet werden. VM-Skalierungsgruppen dienen zum Konfigurieren der Bereitstellung virtueller Computer zum Erfüllen der Anforderungen. DSC dient zum Konfigurieren der VMs, sobald sie online geschaltet wurden, damit sie in der Produktionssoftware ausgeführt werden.

## Unterschiede zwischen der Bereitstellung in einer VM und einer VM-Skalierungsgruppe

Die zugrunde liegende Vorlagenstruktur für VM-Skalierungsgruppen unterscheidet sich geringfügig von einem einzelnen virtuellen Computer. Ein Punkt ist, dass bei einer einzelnen VM Erweiterungen unter dem Knoten „virtualMachines“ bereitgestellt werden. Es gibt einen Eintrag des Typs „Extensions“. Hier wird DSC der Vorlage hinzugefügt.

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Ein Knoten einer VM-Skalierungsgruppe weist den Abschnitt „properties“ mit dem Attribut „VirtualMachineProfile“, „extensionProfile“ auf. DSC wird unter „extensions“ hinzugefügt.

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.9",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## Verhalten von VM-Skalierungsgruppen

Das Verhalten von VM-Skalierungsgruppen entspricht dem Verhalten eines einzelnen virtuellen Computers. Beim Erstellen ein neues virtuellen Computers wird er automatisch mit der DSC-Erweiterung bereitgestellt. Wenn eine neuere Version des WMF von der Erweiterung angefordert wird, wird die VM neu gestartet, ehe sie online geschaltet wird. Sobald sie online ist, lädt sie die ZIP-Datei mit der DSC-Konfiguration herunter und stellt sie auf dem virtuellen Computer bereit. Weitere Informationen finden Sie in der [Übersicht über die Azure DSC-Erweiterung](virtual-machines-windows-extensions-dsc-overview.md).

<!---HONumber=AcomDC_0914_2016-->