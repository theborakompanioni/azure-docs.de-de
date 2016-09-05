
<properties
	pageTitle="Azure Site Recovery: Replizieren von virtuellen Hyper-V-Computern auf einem einzelnen VMM-Server | Microsoft Azure"
	description="Dieser Artikel beschreibt die Replikation von Hyper-V-VMs, wenn Sie nur einen einzelnen VMM-Server haben."
	services="site-recovery"
	documentationCenter=""
	authors="rayne-wiselman"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="site-recovery"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="backup-recovery"
	ms.date="08/24/2016"
	ms.author="raynew"/>

#  Replizieren virtueller Hyper-V-Computer auf einem einzelnen VMM-Server

In diesem Artikel erfahren Sie, wie Sie virtuelle Hyper-V-Computer auf einem Hyper-V-Hostserver in einer VMM-Cloud bereitstellen, wenn Ihre Bereitstellung nur einen einzelnen VMM-Server enthält.

Azure verfügt über zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen: Azure Resource Manager- und klassische Bereitstellung. Azure verfügt auch über zwei Portale: das klassische Azure-Portal mit Unterstützung des klassischen Bereitstellungsmodells und das Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle. Dieser Artikel enthält eine Anleitung zum Einrichten der Replikation im Azure-Portal.


Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese bitte in den Disqus-Kommentaren am Ende dieses Artikels oder im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## Übersicht

Wenn Sie Hyper-V-VMs, die sich auf Hyper-V-Hosts in VMM befinden, replizieren möchten und nur über einen einzelnen VMM-Server verfügen, können Sie [zu Azure](site-recovery-vmm-to-azure.md) oder zwischen Clouds auf dem einzelnen VMM-Server replizieren.

Es wird empfohlen, dass Sie zu Azure replizieren, da Failover und Wiederherstellung bei der Replikation zwischen Clouds nicht nahtlos erfolgen und einige manuelle Schritte erfordern. Wenn Sie nur mithilfe des VMM-Servers replizieren möchten, haben Sie folgende Möglichkeiten:

- Replikation mit einem einzelnen, eigenständigen VMM-Server
- Replikation mit einem einzelnen VMM-Server, der in einem gestreckten Windows-Cluster bereitgestellt wurde


## Replikation zwischen Standorten mit einem einzelnen, eigenständigen VMM-Server

![Eigenständiger virtueller VMM-Computer](./media/site-recovery-single-vmm/single-vmm-standalone.png)

In diesem Szenario stellen Sie den einzelnen VMM-Server als virtuellen Computer an einem primären Standort bereit und replizieren diese VM mit Site Recovery und Hyper-V-Replikat zu einem sekundären Standort.

1. Richten Sie VMM auf einer Hyper-V-VM ein. Sie sollten die von VMM verwendete SQL Server-Instanz auf der gleichen VM installieren, um Zeit zu sparen. Wenn Sie eine Remoteinstanz von SQL Server verwenden möchten und ein Ausfall auftritt, müssen Sie diese Instanz vor dem Wiederherstellen von VMM wiederherstellen.
2. Stellen Sie sicher, dass für den VMM-Server mindestens zwei Clouds konfiguriert sind. Eine Cloud enthält die virtuellen Computer, die repliziert werden sollen, und die andere Cloud dient als sekundärer Standort. Die Cloud, die die virtuellen Computer enthält, die Sie schützen möchten, sollte Folgendes aufweisen:

	- Eine oder mehrere VMM-Hostgruppen mit einem oder mehreren Hyper-V-Hostservern in jeder Hostgruppe.
	- Mindestens eine Hyper-V-VM auf jedem Hyper-V-Hostserver.

3. Erstellen Sie einen Recovery Services-Tresor, generieren Sie einen Tresorregistrierungsschlüssel, laden Sie diesen herunter, und registrieren Sie den VMM-Server im Tresor. Während der Registrierung installieren Sie den Azure Site Recovery-Anbieter auf den VMM-Servern.
4. Richten Sie eine oder mehrere Clouds auf der VMM-VM ein, und fügen Sie die Hyper-V-Hosts zu diesen Clouds hinzu.
3. Konfigurieren Sie Schutzeinstellungen für die Clouds. Geben Sie als Quell- und Zielspeicherorte jeweils den Namen des einzelnen VMM-Servers an. Bei der Konfiguration der Netzwerkzuordnung ordnen Sie das VM-Netzwerk für die Cloud mit den zu schützenden virtuellen Computern dem VM-Netzwerk für die Cloud für die Replikation zu.
4. Aktivieren Sie die erste Replikation für zu schützende virtuelle Computer über das Netzwerk, da sich beide Clouds auf demselben Server befinden.
4. Aktivieren Sie in der Hyper-V-Manager-Konsole Hyper-V-Replikat auf dem Hyper-V-Host, der die VMM-VM enthält, und aktivieren Sie die Replikation auf dem virtuellen Computer. Achten Sie darauf, dass Sie die VMM-VM nicht zu Clouds hinzufügen, die von Site Recovery geschützt werden. Dadurch wird sichergestellt, dass die Einstellungen des Hyper-V-Replikats nicht von Site Recovery überschrieben werden.
5. Wenn Sie Wiederherstellungspläne erstellen möchten, geben Sie den gleichen VMM-Server für Quelle und Ziel an.

Führen Sie die Schritte [in diesem Artikel](site-recovery-vmm-to-vmm.md) zum Erstellen eines Tresors, Registrieren des Servers und Einrichten des Schutzes aus.

### Maßnahmen bei einem Ausfall

Wenn ein vollständiger Ausfall auftritt und Sie den sekundären Standort verwenden müssen, führen Sie folgende Schritte aus:

1.	Führen Sie in der Hyper-V-Manager-Konsole am sekundären Standort ein ungeplantes Failover aus, um ein Failover der VMM-VM vom primären zum sekundären Standort vorzunehmen.
2.	Stellen Sie sicher, dass die VMM-VM am sekundären Standort ausgeführt wird.
3.	Führen Sie im Recovery Services-Tresor ein ungeplantes Failover aus, um ein Failover der Workload-VMs von den primären zu den sekundären Clouds vorzunehmen. Um das ungeplante Failover der VMs abzuschließen, führen Sie ein Commit für das Failover aus, bzw. wählen Sie einen anderen Wiederherstellungspunkt aus.
4.	Sobald das ungeplante Failover abgeschlossen ist, können Benutzer auf die Workloadressourcen am sekundären Standort zugreifen.

Wenn der primäre Standort wieder ordnungsgemäß funktioniert, führen Sie folgende Schritte aus:

1.	Aktivieren Sie in der Hyper-V-Manager-Konsole die umgekehrte Replikation für die VMM-VM, um mit deren Replikation vom sekundären zum primären Standort zu beginnen.
2.	Führen Sie in der Hyper-V-Manager-Konsole ein geplantes Failover aus, um das Failback der VMM-VM zum primären Standort vorzunehmen. Führen Sie ein Commit für das Failover aus, um es abzuschließen. Aktivieren Sie anschließend die umgekehrte Replikation, um mit der Replikation von VMM vom primären zum sekundären Standort zu beginnen.
3.	Aktivieren Sie im Recovery Services-Tresor die umgekehrte Replikation für die Workload-VMs, um mit deren Replikation vom sekundären zum primären Standort zu beginnen.
4.	Führen Sie im Recovery Services-Tresor ein geplantes Failover aus, um das Failback der Workload-VMs zum primären Standort vorzunehmen. Führen Sie ein Commit für das Failover aus, um es abzuschließen. Aktivieren Sie anschließend die umgekehrte Replikation, um mit der Replikation von Workload-VMs vom primären zum sekundären Standort zu beginnen.



## Replikation zwischen Standorten mit einem einzelnen VMM-Server in einem gestreckten Cluster

![Gruppierter virtueller VMM-Server](./media/site-recovery-single-vmm/single-vmm-cluster.png)

Anstatt einen eigenständigen VMM-Server als virtuellen Computer bereitzustellen, der zu einem sekundären Standort repliziert wird, können Sie VMM als virtuellen Computer in einem Windows-Failovercluster bereitstellen und so hohe Verfügbarkeit erreichen. Dies bietet Workloadstabilität und Schutz vor Hardwarefehlern. Zum Bereitstellen mit Site Recovery sollte die VMM-VM in einem gestreckten Cluster über geografisch getrennte Standorte bereitgestellt werden. Gehen Sie dazu folgendermaßen vor:

1. Installieren Sie VMM auf einem virtuellen Computer in einem Windows-Failovercluster, und wählen Sie während der Installation die Option aus, den Server mit hoher Verfügbarkeit auszuführen.
2. Die von VMM verwendete SQL Server-Instanz sollte mithilfe von AlwaysOn-Verfügbarkeitsgruppen (SQL Server) repliziert werden, damit ein Replikat der Datenbank am sekundären Standort verfügbar ist.
3. Führen Sie die Schritte [in diesem Artikel](site-recovery-vmm-to-vmm.md) zum Erstellen eines Tresors, Registrieren des Servers und Einrichten des Schutzes aus. Sie müssen jeden VMM-Server im Cluster im Recovery Services-Tresor registrieren. Installieren Sie dazu den Anbieter auf einem aktiven Knoten, und registrieren Sie den VMM-Server. Dann installieren Sie den Anbieter auf den anderen Knoten.

### Maßnahmen bei einem Ausfall

Bei einem Ausfall wird für den VMM-Server und die entsprechende SQL Server-Datenbank ein Failover auf den sekundären Standort durchgeführt, und der Zugriff erfolgt von dort.


## Nächste Schritte

[Erfahren Sie mehr](site-recovery-vmm-to-vmm.md) über die detaillierte Site Recovery-Bereitstellung für die VMM-zu-VMM-Replikation.

<!---HONumber=AcomDC_0824_2016-->