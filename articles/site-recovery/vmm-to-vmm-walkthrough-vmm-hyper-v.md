---
title: "Einrichten von VMM und Hyper-V für die Replikation an einem sekundären Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Beschreibt, wie System Center VMM-Server und Hyper-V-Hosts für die Replikation an einem sekundären VMM-Standort eingerichtet werden."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d0389e3b-3737-496c-bda6-77152264dd98
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 73bd1790c56ac52166f638de2e80c2a2cfb87e53
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="step-4-set-up-vmm-and-hyper-v-for-hyper-v-vm-replication-to-a-secondary-site"></a>Schritt 4: Einrichten von VMM und Hyper-V für die Replikation virtueller Hyper-V-Computer an einem sekundären Standort 

Nachdem Sie die Netzwerke vorbereitet haben, richten Sie System Center Virtual Machine Manager-Server (VMM) und Hyper-V-Hosts für die Replikation virtueller Hyper-V-Computer an einem sekundären Standort mit [Azure Site Recovery](site-recovery-overview.md) im Azure-Portal ein. 

Kommentare können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) hinterlassen.



## <a name="prepare-vmm-servers"></a>Vorbereiten der VMM-Server 

So bereiten Sie die Bereitstellung vor:


1. Stellen Sie sicher, dass die VMM-Server die [Supportanforderungen](site-recovery-support-matrix-to-sec-site.md#on-premises-servers) und [Voraussetzungen für die Bereitstellung](vmm-to-vmm-walkthrough-prerequisites.md) erfüllen.
2. Stellen Sie sicher, dass die VMM-Server mit dem Internet verbunden sind und Zugriff auf diese URLs haben.
    
    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Stellen Sie bei Verwendung von auf IP-Adressen basierenden Firewallregeln sicher, dass hierfür die Kommunikation mit Azure möglich ist.
    - Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.
    - Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.
3. Stellen Sie sicher, dass der VMM-Server [für die Netzwerkzuordnung vorbereitet](vmm-to-vmm-walkthrough-network.md#prepare-for-network-mapping) ist.


## <a name="prepare-hyper-v-hostsclusters"></a>Vorbereiten der Hyper-V-Hosts/Cluster

1. Stellen Sie sicher, dass die Hyper-V-Hosts/Cluster die [Supportanforderungen](site-recovery-support-matrix-to-sec-site.md#on-premises-servers) und [Voraussetzungen für die Bereitstellung](vmm-to-vmm-walkthrough-prerequisites.md) erfüllen.
2. Überprüfen Sie die Anforderungen für [virtuelle Hyper-V-Computer](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions).
3. Überprüfen Sie die Anforderungen für [Netzwerke](site-recovery-support-matrix-to-sec-site.md#network-configuration) und [Speicher](site-recovery-support-matrix-to-sec-site.md#storage).
4. Stellen Sie sicher, dass die Hyper-V-Hosts mit dem Internet verbunden sind und Zugriff auf diese URLs haben.
    
    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Stellen Sie bei Verwendung von auf IP-Adressen basierenden Firewallregeln sicher, dass hierfür die Kommunikation mit Azure möglich ist.
    - Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.
    - Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.

## <a name="prepare-for-single-server-deployment"></a>Vorbereiten einer einzelnen Serverbereitstellung


Wenn Sie nur über einen einzelnen VMM-Server verfügen, können Sie virtuelle Computer in Hyper-V-Hosts in der VMM-Cloud in [Azure](hyper-v-site-walkthrough-overview.md) oder in einer sekundären VMM-Cloud replizieren, wie in diesem Dokument beschrieben. Wir empfehlen die erste Option, da die Replikation zwischen Clouds nicht übergangslos erfolgt.

Wenn Sie eine Replikation zwischen Clouds durchführen möchten, können Sie entweder einen einzelnen eigenständigen VMM-Server oder einen einzelnen VMM-Server verwenden, der in einem gestreckten Windows-Cluster bereitgestellt ist.

### <a name="replicate-with-a-standalone-vmm-server"></a>Replikation mit einem eigenständigen VMM-Server

In diesem Szenario stellen Sie den einzelnen VMM-Server als virtuellen Computer an einem primären Standort bereit und replizieren diese VM mithilfe von Site Recovery und Hyper-V-Replikat zu einem sekundären Standort.

1. **Richten Sie VMM auf einem virtuellen Hyper-V-Computer ein**. Sie sollten die von VMM verwendete SQL Server-Instanz auf der gleichen VM installieren. Dies spart Zeit, da nur ein virtueller Computer erstellt werden muss. Wenn Sie eine Remoteinstanz von SQL Server verwenden möchten und ein Ausfall auftritt, müssen Sie diese Instanz vor dem Wiederherstellen von VMM wiederherstellen.
2. **Stellen Sie sicher, dass für den VMM-Server mindestens zwei Clouds konfiguriert sind**. Eine Cloud enthält die virtuellen Computer, die repliziert werden sollen, und die andere Cloud dient als sekundärer Standort. Die Cloud mit den virtuellen Computern, die Sie schützen möchten, muss bestimmte [Voraussetzungen](#prerequisites)erfüllen.
3. Richten Sie Site Recovery wie in diesem Artikel beschrieben ein. Erstellen und registrieren Sie den VMM-Server in einem Tresor, richten Sie eine Replikationsrichtlinie ein, und aktivieren Sie die Replikation. Die Quell- und Ziel-VMM-Namen sind identisch. Geben Sie an, dass die anfängliche Replikation über das Netzwerk erfolgt.
4. Beim Einrichten der Netzwerkzuordnung ordnen Sie das VM-Netzwerk für die primäre Cloud dem VM-Netzwerk für die sekundäre Cloud zu.
5. Aktivieren Sie in der Hyper-V-Manager-Konsole Hyper-V-Replikat auf dem Hyper-V-Host, der die VMM-VM enthält, und aktivieren Sie die Replikation auf dem virtuellen Computer. Fügen Sie die VMM-VM nicht zu Clouds hinzu, die von Site Recovery geschützt werden, um sicherzustellen, dass Einstellungen von Hyper-V Replikat nicht durch Site Recovery außer Kraft gesetzt werden.
6. Wenn Sie Wiederherstellungspläne für das Failover erstellen, verwenden Sie den gleichen VMM-Server als Quelle und Ziel.
7. Bei einem vollständigen Ausfall führen Sie Failover und Wiederherstellung wie folgt aus:

   1. Führen Sie in der Hyper-V-Manager-Konsole am sekundären Standort ein ungeplantes Failover durch, um ein Failover des primären virtuellen VMM-Computers auf den sekundären Standort vorzunehmen.
   2. Vergewissern Sie sich, dass der virtuelle VMM-Computer ausgeführt wird, und führen Sie im Tresor ein ungeplantes Failover durch, um für die virtuellen Computer ein Failover von primären auf sekundäre Clouds vorzunehmen. Committen Sie das Failover, und wählen Sie bei Bedarf einen alternativen Wiederherstellungspunkt aus.
   3. Nach Abschluss des nicht geplanten Failovers können die Benutzer wieder vom primären Standort aus auf sämtliche Ressourcen zugreifen.
   4. Wenn der primäre Standort wieder verfügbar ist, aktivieren Sie in der Hyper-V-Manager-Konsole am sekundären Standort die umgekehrte Replikation für den virtuellen VMM-Computer. Dadurch wird für den virtuellen Computer die Replikation vom sekundären an den primären Standort gestartet.
   5. Führen Sie in der Hyper-V-Manager-Konsole am sekundären Standort ein geplantes Failover durch, um ein Failover des virtuellen VMM-Computers auf den primären Standort vorzunehmen. Committen Sie das Failover. Aktivieren Sie anschließend die umgekehrte Replikation, damit der virtuelle VMM-Computer wieder vom primären an den sekundären Standort repliziert wird.
   6. Aktivieren Sie im Recovery Services-Tresor die umgekehrte Replikation für die Workload-VMs, um mit deren Replikation vom sekundären zum primären Standort zu beginnen.
   7. Führen Sie im Recovery Services-Tresor ein geplantes Failover aus, um das Failback der Workload-VMs zum primären Standort vorzunehmen. Führen Sie ein Commit für das Failover aus, um es abzuschließen. Aktivieren Sie anschließend die umgekehrte Replikation, um mit der Replikation von Workload-VMs vom primären zum sekundären Standort zu beginnen.

### <a name="replicate-with-a-stretched-vmm-cluster"></a>Replikation mit einem gestreckten VMM-Cluster

Anstatt einen eigenständigen VMM-Server als virtuellen Computer bereitzustellen, der zu einem sekundären Standort repliziert wird, können Sie VMM als virtuellen Computer in einem Windows-Failovercluster bereitstellen und so hohe Verfügbarkeit erreichen. Dies bietet Workloadstabilität und Schutz vor Hardwarefehlern. Zum Bereitstellen mit Site Recovery sollte die VMM-VM in einem gestreckten Cluster über geografisch getrennte Standorte bereitgestellt werden. Gehen Sie dazu folgendermaßen vor:

1. Installieren Sie VMM auf einem virtuellen Computer in einem Windows-Failovercluster, und wählen Sie während der Installation die Option aus, den Server mit hoher Verfügbarkeit auszuführen.
2. Die von VMM verwendete SQL Server-Instanz sollte mithilfe von SQL Server Always On-Verfügbarkeitsgruppen repliziert werden, damit ein Replikat der Datenbank am sekundären Standort verfügbar ist.
3. Führen Sie die Schritte in diesem Artikel aus, um einen Tresor zu erstellen, den Server zu registrieren und den Schutz einzurichten. Sie müssen jeden VMM-Server im Cluster im Recovery Services-Tresor registrieren. Installieren Sie dazu den Anbieter auf einem aktiven Knoten, und registrieren Sie den VMM-Server. Dann installieren Sie den Anbieter auf den anderen Knoten.
4. Bei einem Ausfall wird für den VMM-Server und die entsprechende SQL Server-Datenbank ein Failover durchgeführt, und der Zugriff erfolgt vom sekundären Standort aus.



## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zu [Schritt 5: Einrichten eines Tresors](vmm-to-vmm-walkthrough-create-vault.md).
