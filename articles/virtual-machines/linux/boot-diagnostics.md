---
title: "Startdiagnose für virtuelle Linux-Computer in Azure | Microsoft-Dokumentation"
description: "Übersicht über die beiden Debugfeatures für virtuelle Linux-Computer in Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: delhan
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 70254d39b5c6326166f7e29fdfc99533835502f9
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-linux-virtual-machines-in-azure"></a>Verwenden der Startdiagnose zum Beheben von Problemen mit virtuellen Linux-Computern in Azure

In Azure ist jetzt Unterstützung von zwei Debuggingfunktionen verfügbar: Konsolenausgaben- und Screenshotunterstützung für das Resource Manager-Bereitstellungsmodell für virtuelle Azure-Computer. 

Wenn Sie Ihr eigenes Image in Azure verwenden oder sogar eines der Plattformimages starten, kann sich ein virtueller Computer aus zahlreichen Gründen in einem nicht startfähigen Zustand befinden. Diese Funktionen ermöglichen Ihnen ein einfaches Diagnostizieren und Wiederherstellten Ihrer virtuellen Computer nach Startfehlern.

Für virtuelle Linux-Computer können Sie die Ausgabe des Konsolenprotokolls problemlos über das Portal anzeigen:

![Azure-Portal](./media/boot-diagnostics/screenshot1.png)
 
Azure ermöglicht Ihnen jedoch sowohl für virtuelle Windows- als auch Linux-Computer auch das Anzeigen eines Screenshots des virtuellen Computers im Hypervisor:

![Error](./media/boot-diagnostics/screenshot2.png)

Beide Funktionen werden für virtuelle Azure-Computer in allen Regionen unterstützt. Hinweis: Es kann bis zu 10 Minuten dauern, bis Screenshots und Ausgaben in Ihrem Speicherkonto angezeigt werden.

## <a name="common-boot-errors"></a>Häufige Startfehler

- [Dateisystemprobleme](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/)
- [Kernelprobleme](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)
- [FSTAB-Fehler](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/ )

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Aktivieren der Diagnose auf einem virtuellen Computer
1. Wählen Sie beim Erstellen eines neuen virtuellen Computers über das Vorschauportal in der Dropdownliste „Bereitstellungsmodell“ die Option **Azure Resource Manager** aus:
 
    ![Ressourcen-Manager](./media/boot-diagnostics/screenshot3.jpg)

2. Konfigurieren Sie die Überwachungsoption für die Auswahl des Speicherkontos, in dem diese Diagnosedateien abgelegt werden sollen.
 
    ![Erstellen eines virtuellen Computers](./media/boot-diagnostics/screenshot4.jpg)

3. Wenn Sie aus einer Azure Resource Manager-Vorlage bereitstellen, navigieren Sie zur Ressource des virtuellen Computers und fügen den Diagnoseprofilabschnitt an. Denken Sie daran, den API-Versionsheader „2015-06-15“ zu verwenden.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Das Diagnoseprofil ermöglicht Ihnen die Auswahl des Speicherkontos, in dem diese Protokolle abgelegt werden sollen.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

## <a name="update-an-existing-virtual-machine"></a>Aktualisieren eines vorhandenen virtuellen Computers

Zum Aktivieren der Startdiagnose über das Portal können Sie auch einen vorhandenen virtuellen Computer über das Portal aktualisieren. Wählen Sie die Option „Startdiagnose“, und speichern Sie. Starten Sie den virtuellen Computer neu, damit die Einstellungen übernommen werden.

![Aktualisieren eines vorhandenen virtuellen Computers](./media/boot-diagnostics/screenshot5.png)
