---
title: "Vorbereiten von Computern für die Einrichtung der Notfallwiederherstellung zwischen Azure-Regionen nach der Migration zu Azure mit Site Recovery | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt das Vorbereiten von Computern für die Einrichtung der Notfallwiederherstellung zwischen Azure-Regionen nach der Migration in Azure mithilfe von Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: ponatara
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 2aee0fb8d1ba1ff1584bee91b4d1cc34b654d97f
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="replicate-azure-vms-to-another-region-after-migration-to-azure-by-using-azure-site-recovery"></a>Sie können mit Azure Site Recovery virtuelle Azure-Computer nach der Migration in Azure in einer anderen Region replizieren.

>[!NOTE]
> Die Azure Site Recovery-Replikation für virtuelle Azure-Computer ist derzeit als Vorschauversion verfügbar.

## <a name="overview"></a>Übersicht

In diesem Artikel finden Sie Informationen zum Vorbereiten von virtuellen Azure-Computern für die Replikation zwischen zwei Azure-Regionen, nachdem diese Computer mithilfe von Azure Site Recovery aus einer lokalen Umgebung zu Azure migriert wurden.

## <a name="disaster-recovery-and-compliance"></a>Notfallwiederherstellung und Konformität
Heute verlagern immer mehr Unternehmen ihre Workloads in Azure. Wenn Unternehmen unternehmenskritische Produktionsworkloads in Azure verlagern, ist das Einrichten einer Notfallwiederherstellung für diese Workloads obligatorisch für die Einhaltung von Richtlinien und zum Schutz vor Störungen in einer Azure-Region.

## <a name="steps-for-preparing-migrated-machines-for-replication"></a>Schritte zum Vorbereiten von migrierten Computern für die Replikation
So bereiten Sie migrierte Computer für das Einrichten der Replikation in einer anderen Azure-Region vor

1. Schließen Sie die Migration ab.
2. Installieren Sie ggf. den Azure-Agent.
3. Entfernen Sie Mobility Service.  
4. Starten Sie den virtuellen Computer neu.

Diese Schritte werden in den folgenden Abschnitten genauer beschrieben.

### <a name="step-1-migrate-workloads-running-on-hyper-v-vms-vmware-vms-and-physical-servers-to-run-on-azure-vms"></a>Schritt 1: Migrieren der Workloads, die auf Hyper-V-VMs, VMware-VMs und physischen Servern ausgeführt werden, um die Ausführung auf virtuellen Azure-Computern zu ermöglichen

Führen Sie die Schritte im Artikel [Durchführen der Migration von virtuellen Azure IaaS-Computern zu Azure mit Azure Site Recovery](site-recovery-migrate-to-azure.md) aus, um die Replikation einzurichten und die lokalen Hyper-V-, VMware- und physischen Workloads in Azure zu migrieren. 

Nach der Migration müssen Sie kein Commit oder einen Löschvorgang für ein Failover durchführen. Wählen Sie stattdessen die Option **Migration abschließen** für jeden Computer aus, den Sie migrieren möchten:
1. Klicken Sie unter **Replizierte Elemente** mit der rechten Maustaste auf die VM, und klicken Sie dann auf **Migration abschließen**. Klicken Sie auf **OK**, um den Schritt abzuschließen. Sie können den Status in den Eigenschaften des virtuellen Computers nachverfolgen, indem Sie den Auftrag „Migration abschließen“ unter **Site Recovery-Aufträge** überwachen.
2. Mit der Aktion **Migration abschließen** wird der Migrationsprozess abgeschlossen, die Replikation für den Computer wird entfernt, und die Site Recovery-Berechnung von Kosten für den Computer wird beendet.

   ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

### <a name="step-2-install-the-azure-vm-agent-on-the-virtual-machine"></a>Schritt 2: Installieren des Azure-VM-Agent auf dem virtuellen Computer
Der [Azure-VM-Agent](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) muss auf dem virtuellen Computer installiert werden, damit die Site Recovery-Erweiterung funktioniert und den virtuellen Computer schützen kann.

>[!IMPORTANT]
>Ab Version 9.7.0.0 wird mit dem Mobility Service-Installationsprogramm auf virtuellen Windows-Computern auch der jeweils neueste verfügbare Azure-VM-Agent installiert. Bei der Migration erfüllt der virtuelle Computer die Voraussetzung der Agentinstallation für die Verwendung von VM-Erweiterungen, einschließlich der Site Recovery-Erweiterung. Der Azure-VM-Agent muss nur dann manuell installiert werden, wenn auf dem migrierten Computer Mobility Service mit Version 9.6 oder früher installiert ist.

Die folgende Tabelle enthält zusätzliche Informationen zum Installieren des VM-Agents und zum Überprüfen, ob er installiert wurde:

| **Vorgang** | **Windows** | **Linux** |
| --- | --- | --- |
| Installieren des VM-Agents |Laden Sie den [Agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)herunter, und installieren Sie ihn. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. |Installieren Sie den neuesten [Linux-Agent](../virtual-machines/linux/agent-user-guide.md). Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. Es wird empfohlen, den Agent aus dem Repository Ihrer Distribution zu installieren. Es wird *nicht empfohlen*, den Linux-VM-Agent direkt von GitHub zu installieren.  |
| Überprüfen der VM-Agent-Installation |1. Navigieren Sie auf dem virtuellen Azure-Computer zum Ordner „C:\WindowsAzure\Packages“. Dieser Ordner enthält die Datei „WaAppAgent.exe“. <br>2. Klicken Sie mit der rechten Maustaste auf die Datei, wechseln Sie zu **Eigenschaften**, und wählen Sie dann die Registerkarte **Details** aus. Im Feld mit der **Produktversion** sollte 2.6.1198.718 oder eine höhere Version angegeben sein. |– |


### <a name="step-3-remove-the-mobility-service-from-the-migrated-virtual-machine"></a>Schritt 3: Entfernen von Mobility Service vom migrierten virtuellen Computer

Wenn Sie Ihre lokalen VMware-Computer oder physischen Server unter Windows/Linux migriert haben, müssen Sie Mobility Service auf dem migrierten virtuellen Computer manuell entfernen/deinstallieren.

>[!IMPORTANT]
>Dieser Schritt ist für in Azure migrierte virtuelle Hyper-V-Computer nicht erforderlich.

#### <a name="uninstall-the-mobility-service-on-a-windows-server-vm"></a>Deinstallieren von Mobility Service auf einem virtuellen Windows Server-Computer
Verwenden Sie eine der folgenden Methoden, um Mobility Service auf einem Windows Server-Computer zu deinstallieren.

##### <a name="uninstall-by-using-the-windows-ui"></a>Deinstallieren über die Windows-Benutzeroberfläche
1. Wählen Sie in der Systemsteuerung **Programme** aus.
2. Wählen Sie **Microsoft Azure Site Recovery Mobility Service/Masterzielserver** und dann **Deinstallieren** aus.

##### <a name="uninstall-at-a-command-prompt"></a>Deinstallieren über die Eingabeaufforderung
1. Öffnen Sie ein Eingabeaufforderungsfenster als ein Administrator.
2. Führen Sie zum Deinstallieren von Mobility Service den folgenden Befehl aus:

   ```
   MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
   ```

#### <a name="uninstall-the-mobility-service-on-a-linux-computer"></a>Deinstallieren von Mobility Service auf einem Linux-Computer
1. Melden Sie sich auf dem Linux-Server als ein **Root**-Benutzer an.
2. Navigieren Sie in einem Terminal zu „/user/local/ASR“.
3. Führen Sie zum Deinstallieren von Mobility Service den folgenden Befehl aus:

   ```
   uninstall.sh -Y
   ```

### <a name="step-4-restart-the-vm"></a>Schritt 4: Neustart des virtuellen Computers

Starten Sie den virtuellen Computer nach dem Deinstallieren von Mobility Service neu, bevor Sie die Replikation in einer anderen Azure-Region einrichten.


## <a name="next-steps"></a>Nächste Schritte
- Schützen von Workloads durch die [Replikation virtueller Azure-Computer](site-recovery-azure-to-azure.md)
- Weitere Informationen zu [Netzwerkrichtlinien zum Replizieren von virtuellen Azure-Computern](site-recovery-azure-to-azure-networking-guidance.md)

