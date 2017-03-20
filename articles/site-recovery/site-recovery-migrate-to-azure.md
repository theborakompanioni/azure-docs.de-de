---
title: Migrieren zu Azure mit Site Recovery | Microsoft-Dokumentation
description: "Dieser Artikel enthält eine Übersicht über die Migration von VMs und physischen Servern zu Azure mit Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/04/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 981155c38bdc8cb54639d2271be1f3bd3036125c
ms.lasthandoff: 03/09/2017


---
# <a name="migrate-to-azure-with-site-recovery"></a>Durchführen der Migration zu Azure mit Site Recovery

Dieser Artikel enthält eine Übersicht über die Verwendung des Azure Site Recovery-Diensts für die Migration von virtuellen Computern und physischen Servern.

Site Recovery ist ein Azure-Dienst, der einen Beitrag zu Ihrer BCDR-Strategie leistet, indem die Replikation von lokalen physischen Servern und virtuellen Computern in die Cloud (Azure) oder in ein sekundäres Datencenter orchestriert wird. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Weitere Informationen finden Sie unter [Was ist Site Recovery?](site-recovery-overview.md) Sie können Site Recovery auch zum Migrieren Ihrer vorhandenen lokalen Workloads zu Azure verwenden, um die Umstellung auf die Cloud zu beschleunigen und in den Genuss der vielen Features von Azure zu kommen.

Eine kurze Übersicht über das Durchführen der Migration finden Sie in diesem Video.
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/ASRHowTo-Video2-Migrate-Virtual-Machines-to-Azure/player]

In diesem Artikel wird die Bereitstellung über das [Azure-Portal](https://portal.azure.com) beschrieben. Sie können das [klassische Azure-Portal](https://manage.windowsazure.com/) verwenden, um vorhandene Site Recovery-Tresore zu verwalten, aber Sie können keine neuen Tresore erstellen.

Am Ende dieses Artikels können Sie einen Kommentar eingeben. Im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)können Sie technische Fragen stellen.


## <a name="what-do-we-mean-by-migration"></a>Was bedeutet „Migration“?

Sie können Site Recovery für die Replikation von lokalen VMs und physischen Servern in Azure oder an einem sekundären Standort bereitstellen. Sie replizieren Computer, führen bei Ausfällen dafür das Failover vom primären Standort durch und lassen nach der Wiederherstellung dann das Failback zurück zum primären Standort folgen. Zusätzlich können Sie Site Recovery zum Migrieren von VMs und physischen Servern zu Azure verwenden, damit Benutzer über Azure-VMs darauf zugreifen können. Die Migration umfasst die Replikation und das Failover vom primären Standort zu Azure und eine vollständige Migrationsgeste.

## <a name="what-can-site-recovery-migrate"></a>Was kann mit Site Recovery migriert werden?

Sie haben folgende Möglichkeiten:

- Migrieren Sie Workloads, die auf lokalen Hyper-V-VMs, VMware-VMs und physischen Servern ausgeführt werden, um die Ausführung auf Azure-VMs zu ermöglichen. Sie können in diesem Szenario auch eine vollständige Replikation mit Failback durchführen.
- Migrieren Sie [Azure IaaS-VMs](site-recovery-migrate-azure-to-azure.md) zwischen Azure-Regionen. Derzeit wird für dieses Szenario nur die Migration unterstützt. Ein Failback wird also nicht unterstützt.
- Migrieren Sie [AWS Windows-Instanzen](site-recovery-migrate-aws-to-azure.md) zu Azure IaaS-VMs. Derzeit wird für dieses Szenario nur die Migration unterstützt. Ein Failback wird also nicht unterstützt.

## <a name="migrate-on-premises-vms-and-physical-servers"></a>Migrieren von lokalen VMs und physischen Servern

Zum Migrieren von lokalen Hyper-V-VMs, VMware-VMs und physischen Servern führen Sie fast die gleichen Schritte wie bei der regulären Replikation aus. Sie richten einen Recovery Services-Tresor ein, konfigurieren die erforderlichen Verwaltungsserver (je nachdem, was migriert werden soll), fügen sie dem Tresor hinzu und geben die Replikationseinstellungen an. Sie aktivieren die Replikation für die zu migrierenden Computer und führen ein schnelles Testfailover durch, um sicherzustellen, dass alles wie gewünscht funktioniert.

Nachdem Sie sich vergewissert haben, dass die Replikationsumgebung funktioniert, verwenden Sie ein geplantes oder ungeplantes Failover. Dies richtet sich danach, [was für Ihr Szenario unterstützt wird](site-recovery-failover.md). Für die Migration müssen Sie kein Commit für ein Failover durchführen. Wählen Sie stattdessen die Option **Migration abschließen** für jeden Computer, den Sie migrieren möchten. Mit der Aktion **Migration abschließen** wird der Migrationsprozess abgeschlossen, die Replikation für den Computer wird entfernt, und die Site Recovery-Berechnung von Kosten für den Computer wird beendet.

![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

## <a name="migrate-between-azure-regions"></a>Migrieren zwischen Azure-Regionen

Sie können mit Site Recovery für Azure-VMs die Migration zwischen Regionen durchführen. In diesem Szenario wird nur die Migration unterstützt. Anders ausgedrückt: Sie können die Azure-VMs replizieren und dafür ein Failover in eine andere Region durchführen, aber es ist kein Failback möglich. In diesem Szenario richten Sie einen Recovery Services-Tresor ein, stellen einen lokalen Konfigurationsserver zum Verwalten der Replikation bereit, fügen ihn dem Tresor hinzu und geben die Replikationseinstellungen an. Sie aktivieren die Replikation für die Computer, die Sie migrieren möchten, und führen ein schnelles Testfailover aus. Anschließend führen Sie ein ungeplantes Failover aus, indem Sie die Option **Migration abschließen** verwenden.

## <a name="migrate-aws-to-azure"></a>Migrieren von AWS zu Azure

Sie können AWS-Instanzen zu Azure-VMs migrieren. In diesem Szenario wird nur die Migration unterstützt. Anders ausgedrückt: Sie können die AWS-Instanzen replizieren und dafür ein Failover zu Azure durchführen, aber es ist kein Failback möglich. AWS-Instanzen werden für Migrationszwecke genauso wie physische Server behandelt. Sie richten einen Recovery Services-Tresor ein, stellen einen lokalen Konfigurationsserver zum Verwalten der Replikation bereit, fügen ihn dem Tresor hinzu und geben die Replikationseinstellungen an. Sie aktivieren die Replikation für die Computer, die Sie migrieren möchten, und führen ein schnelles Testfailover aus. Anschließend führen Sie ein ungeplantes Failover aus, indem Sie die Option **Migration abschließen** verwenden.




## <a name="next-steps"></a>Nächste Schritte

- [Migrieren von VMware-VMs zu Azure](site-recovery-vmware-to-azure.md)
- [Migrieren von Hyper-V-VMs in VMM-Clouds zu Azure](site-recovery-vmm-to-azure.md)
- [Migrieren von Hyper-V-VMs ohne VMM zu Azure](site-recovery-hyper-v-site-to-azure.md)
- [Migrieren von Azure-VMs zwischen Azure-Regionen](site-recovery-migrate-azure-to-azure.md)
- [Migrieren von AWS-Instanzen zu Azure](site-recovery-migrate-aws-to-azure.md)

