---
title: Problembehandlung bei Fehlern im Zusammenhang mit Windows-VM-Erweiterungen | Microsoft Docs
description: "Erfahren Sie mehr über die Problembehandlung für Fehler bei Azure Windows-VM-Erweiterungen."
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: top-support-issue,azure-resource-manager
ms.assetid: 878ab9b6-c3e6-40be-82d4-d77fecd5030f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fd50ff64b538232a9a1945efaffacef3a06cfcdb


---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Problembehandlung bei Fehlern im Zusammenhang mit Azure Windows-VM-Erweiterungen
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Anzeigen des Erweiterungsstatus
Azure Resource Manager-Vorlagen können aus Azure PowerShell ausgeführt werden. Sobald die Vorlage ausgeführt wird, kann der Erweiterungsstatus im Azure-Ressourcen-Explorer oder in den Befehlszeilentools angezeigt werden.

Beispiel:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Hier ist die Beispielausgabe:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Problembehandlung bei Erweiterungsfehlern
### <a name="re-running-the-extension-on-the-vm"></a>Erneutes Ausführen der Erweiterung auf dem virtuellen Computer
Wenn Sie mithilfe der benutzerdefinierten Skripterweiterung Skripts auf dem virtuellen Computer ausführen, könnten ab und an Fehler auftreten, bei denen der virtuelle Computer erfolgreich erstellt wurde, das Skript jedoch fehlgeschlagen ist. Unter diesen Bedingungen ist die empfohlene Vorgehensweise zum Beheben dieses Fehlers das Entfernen der Erweiterung und das erneute Ausführen der Vorlage.
Hinweis: In Zukunft wird diese Funktionalität verbessert, damit es nicht mehr notwendig ist, die Erweiterung zu deinstallieren.

#### <a name="remove-the-extension-from-azure-powershell"></a>Entfernen der Erweiterung aus Azure PowerShell
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Nachdem die Erweiterung entfernt wurde, kann die Vorlage erneut ausgeführt werden, um die Skripts auf dem virtuellen Computer auszuführen.




<!--HONumber=Nov16_HO3-->


