---
title: Verwenden der DSC-Erweiterung mit VM-Skalierungsgruppen | Microsoft Docs
description: Verwenden von VM-Skalierungsgruppen mit der Azure DSC-Erweiterung
services: virtual-machine-scale-sets
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: c8f047b5-0e6c-4ef3-8a47-f1b284d32942
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: c0d9ea4f506371ca21a08f6e1d6128c00123cbac


---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Verwenden von VM-Skalierungsgruppen mit der Azure DSC-Erweiterung
[VM-Skalierungsgruppen](virtual-machine-scale-sets-overview.md) können mit dem Erweiterungshandler [Azure-Konfiguration für den gewünschten Zustand](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) verwendet werden. VM-Skalierungsgruppen bieten eine Möglichkeit, eine große Anzahl von virtuellen Computern bereitzustellen und zu verwalten, und lassen sich je nach Auslastung elastisch hoch- und herunterskalieren. DSC dient zum Konfigurieren der VMs, sobald sie online geschaltet wurden, damit sie in der Produktionssoftware ausgeführt werden.

## <a name="differences-between-deploying-to-vm-and-vmss"></a>Unterschiede zwischen der Bereitstellung in einer VM und einer VM-Skalierungsgruppe
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
                          "script": "DscExtension.ps1",
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
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
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

## <a name="behavior-for-vmss"></a>Verhalten von VM-Skalierungsgruppen
Das Verhalten von VM-Skalierungsgruppen entspricht dem Verhalten eines einzelnen virtuellen Computers. Beim Erstellen ein neues virtuellen Computers wird er automatisch mit der DSC-Erweiterung bereitgestellt. Wenn eine neuere Version des WMF von der Erweiterung angefordert wird, wird die VM neu gestartet, ehe sie online geschaltet wird. Sobald sie online ist, lädt sie die ZIP-Datei mit der DSC-Konfiguration herunter und stellt sie auf dem virtuellen Computer bereit. Weitere Informationen finden Sie in der [Übersicht über die Azure DSC-Erweiterung](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die [Azure Resource Manager-Vorlage für die DSC-Erweiterung](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)an.

Erfahren Sie, [wie die DSC-Erweiterung Anmeldeinformationen sicher verarbeitet](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Weitere Informationen zum Azure DSC-Erweiterungs-Handler finden Sie unter [Einführung in den Handler der Azure-Erweiterung zum Konfigurieren des gewünschten Zustands](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Weitere Informationen zu PowerShell DSC finden Sie im [PowerShell-Dokumentationscenter](https://msdn.microsoft.com/powershell/dsc/overview). 




<!--HONumber=Nov16_HO3-->


