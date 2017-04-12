---
title: "Problembehandlung bei Fehlern im Zusammenhang mit Erweiterungen für virtuelle Linux-Computer | Microsoft Docs"
description: "Erfahren Sie mehr über die Problembehandlung für Fehler bei Azure-Erweiterungen für virtuelle Linux-Computer."
services: virtual-machines-linux
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: top-support-issue,azure-resource-manager
ms.assetid: f05d93f3-42fc-4a09-9798-d92f7929c417
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 589890de379d0b729de1f1ba9e604e0ec0496f50
ms.lasthandoff: 04/03/2017


---
# <a name="troubleshooting-azure-linux-vm-extension-failures"></a>Problembehandlung für Fehler bei Azure-Erweiterungen für virtuelle Linux-Computer
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Anzeigen des Erweiterungsstatus
Azure Resource Manager-Vorlagen können von der Azure-Befehlszeilenschnittstelle ausgeführt werden. Sobald die Vorlage ausgeführt wird, kann der Erweiterungsstatus im Azure-Ressourcen-Explorer oder in den Befehlszeilentools angezeigt werden.

Beispiel:

Azure-Befehlszeilenschnittstelle:

      azure vm get-instance-view


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

## <a name="troubleshooting-extenson-failures"></a>Problembehandlung bei Erweiterungsfehlern:
### <a name="re-running-the-extension-on-the-vm"></a>Erneutes Ausführen der Erweiterung auf dem virtuellen Computer
Wenn Sie mithilfe der benutzerdefinierten Skripterweiterung Skripts auf dem virtuellen Computer ausführen, könnten ab und an Fehler auftreten, bei denen der virtuelle Computer erfolgreich erstellt wurde, das Skript jedoch fehlgeschlagen ist. Unter diesen Bedingungen ist die empfohlene Vorgehensweise zum Beheben dieses Fehlers das Entfernen der Erweiterung und das erneute Ausführen der Vorlage.
Hinweis: In Zukunft wird diese Funktionalität verbessert, damit es nicht mehr notwendig ist, die Erweiterung zu deinstallieren.

#### <a name="remove-the-extension-from-azure-cli"></a>Entfernen der Erweiterung aus der Azure-Befehlszeilenschnittstelle
      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

Wobei "publsher-name" dem Erweiterungstyp aus der Ausgabe von "azure vm get-instance-view" entspricht und der Name der Name der Erweiterungsressource aus der Vorlage ist.

Nachdem die Erweiterung entfernt wurde, kann die Vorlage erneut ausgeführt werden, um die Skripts auf dem virtuellen Computer auszuführen.


