
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
	ms.date="03/30/2016"
	ms.author="raynew"/>

#  Replizieren virtueller Hyper-V-Computer auf einem einzelnen VMM-Server

In diesem Artikel erfahren Sie, wie Sie virtuelle Hyper-V-Computer auf einem Hyper-V-Hostserver in einer VMM-Cloud bereitstellen, wenn Ihre Bereitstellung nur einen einzelnen VMM-Server enthält.

Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese bitte am unteren Rand oder im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## Übersicht

Sie können virtuelle Hyper-V-Computer auf Hyper-V-Hosts in VMM-Clouds in zwei Arten replizieren:

- Replikation zu Azure. 
- Replikation zu einem sekundären VMM-Standort.

Aber was geschieht, wenn Sie zu einem sekundären VMM-Standort replizieren möchten, aber nur einen einzelnen VMM-Server in Ihrer Bereitstellung haben?

In diesem Szenario haben Sie mehrere Möglichkeiten:

- Replizieren Sie die Hyper-V-VMs in VMM-Clouds zu Azure. [Hier](site-recovery-vmm-to-azure.md) finden Sie weitere Informationen zu diesem Szenario.
- Replikation zwischen Clouds auf einem einzelnen VMM-Server.

Es empfiehlt sich, die erste Option zu verwenden, weil Failover und Wiederherstellung nicht nahtlos in die zweite Option integriert und einige manuelle Schritte erforderlich sind. Wenn Sie zwischen Standorten replizieren möchten, anstatt zu Azure, stehen Ihnen mehrere Optionen zur Verfügung.


## Replikation zwischen Standorten mit einem einzelnen, eigenständigen VMM-Server

Um in diesem Szenario zwischen Standorten zu replizieren, stellen Sie den einzelnen VMM-Server als virtuellen Computer an einem primären Standort bereit und replizieren diese VM mit Site Recovery und Hyper-V-Replikat zu einem sekundären Standort. Gehen Sie dazu folgendermaßen vor:

1. Richten Sie VMM auf einer Hyper-V-VM ein. Ziehen Sie hierbei auch das Verschieben der von VMM verwendeten SQL Server-Instanz auf dem gleichen virtuellen Computer in Betracht. Dies spart Zeit, da nur ein virtueller Computer instanziiert werden muss. Wenn Sie eine Remoteinstanz von SQL Server verwenden möchten, und ein Ausfall auftritt, müssen Sie diese Instanz vor dem Wiederherstellen von VMM wiederherstellen.
2. Stellen Sie sicher, dass für den VMM-Server mindestens zwei Clouds konfiguriert sind. Eine Cloud enthält die virtuellen Computer, die repliziert werden sollen, und die andere Cloud dient als sekundärer Standort. Die Cloud, die die virtuellen Computer enthält, die Sie schützen möchten, sollte Folgendes aufweisen:

	- Eine oder mehrere VMM-Hostgruppen mit einem oder mehreren Hyper-V-Hostservern in jeder Hostgruppe.
	- Mindestens eine Hyper-V-VM auf jedem Hyper-V-Hostserver.
3. Erstellen Sie einen Site Recovery-Tresor, generieren Sie einen Tresorregistrierungsschlüssel und laden Sie diesen herunter, und registrieren Sie den VMM-Server im Tresor. Während der Registrierung installieren Sie den Azure Site Recovery-Anbieter auf den VMM-Servern.
4. Richten Sie eine oder mehrere Clouds auf der VMM-VM ein, und fügen Sie die Hyper-V-Hosts, die zu schützende virtuelle Computer enthalten, zu diesen Clouds hinzu.
3. Konfigurieren Sie in Azure Site Recovery Schutzeinstellungen für die Clouds. Geben Sie unter „Quellspeicherort“ und unter „Zielspeicherort“ jeweils den Namen des einzelnen VMM-Servers an. Bei der Konfiguration der Netzwerkzuordnung ordnen Sie das VM-Netzwerk für die Cloud, die zu schützende virtuelle Computer enthält, dem VM-Netzwerk für die Cloud zu, in die Sie die Replikation vornehmen möchten.
4. Aktivieren Sie die Replikation für zu schützende virtuelle Computer unter Verwendung von **Über das Netzwerk** als Replikationsmethode, da sich beide Clouds auf demselben Server befinden.
4. Aktivieren Sie in der Hyper-V-Manager-Konsole Hyper-V-Replikat auf dem Hyper-V-Host, der die VMM-VM enthält, und aktivieren Sie die Replikation auf dem virtuellen Computer. Fügen Sie die VMM-VM nicht zu Clouds hinzu, die von Site Recovery geschützt werden, um sicherzustellen, dass Einstellungen von Hyper-V Replikat nicht durch Site Recovery außer Kraft gesetzt werden.
5. Wenn Sie Wiederherstellungspläne erstellen möchten, geben Sie den gleichen VMM-Server für Quelle und Ziel an.

Befolgen Sie die Anweisungen [in diesem Artikel](site-recovery-vmm-to-vmm.md) zum Erstellen eines Tresors, Erhalten einen Schlüssel, Registrieren des Servers und Einrichten des Schutzes.

### Nach einem Ausfall

Beim Auftreten von Ausfällen stellen Sie Workloads auf virtuellen Hyper-V-VMs wie folgt wieder her:

1. Führen Sie unter Verwendung von Hyper-V-Manager mit einem geplanten Failover ein manuelles Failover der VMM-VM zum sekundären Standort aus. 
2. Nach der Wiederherstellung der VMM-VM können Sie sich vom sekundären Standort aus beim Hyper-V Recovery Manager anmelden und ein nicht geplantes Failover der virtuellen Computer vom sekundären zum primären Standort ausführen. Beachten Sie, dass ein manuelles Failover der VMM-VM zum sekundären Standort ausgeführt werden muss, bevor ein Failover der Workload-VMs ausgeführt werden kann.
3. Nach Abschluss des nicht geplanten Failovers können die Benutzer wieder vom primären Standort aus auf sämtliche Ressourcen zugreifen.


![Eigenständiger virtueller VMM-Computer](./media/site-recovery-single-vmm/single-vmm-standalone.png)

## Replikation zwischen Standorten mit einem einzelnen VMM-Server in einem gestreckten Cluster

Anstatt einen eigenständigen VMM-Server als virtuellen Computer bereitzustellen, der zu einem sekundären Standort repliziert wird, können Sie VMM als virtuellen Computer in einem Windows-Failovercluster bereitstellen und so hohe Verfügbarkeit erreichen. Dies bietet Workloadstabilität und Schutz vor Hardwarefehlern. Zum Bereitstellen mit Site Recovery sollte die VMM-VM in einem gestreckten Cluster über geografisch getrennte Standorte bereitgestellt werden. Gehen Sie dazu folgendermaßen vor:

1. Installieren Sie VMM auf einem virtuellen Computer in einem Windows-Failovercluster, und wählen Sie während der Installation die Option, den Server mit hoher Verfügbarkeit auszuführen.
2. Die von VMM verwendete SQL Server-Instanz sollte mithilfe von AlwaysOn-Verfügbarkeitsgruppen (SQL Server) repliziert werden, damit ein Replikat der Datenbank am sekundären Standort verfügbar ist.

Beachten Sie, dass Sie beim Einrichten von Site Recovery jeden VMM-Server im Cluster im Site Recovery-Tresor registrieren müssen. Hierzu installieren Sie den Anbieter auf einem aktiven Knoten und schließen die Installation ab, um die VMM-Server im Tresor zu registrieren. Dann installieren Sie den Anbieter auf den anderen Knoten.
 
### Nach einem Ausfall 

Bei Ausfällen wird für den VMM-Server und die entsprechende SQL Server-Datenbank ein Failover durchgeführt, und der Zugriff erfolgt über den sekundären Standort.

![Gruppierter virtueller VMM-Server](./media/site-recovery-single-vmm/single-vmm-cluster.png)

## Nächste Schritte

[Erfahren Sie mehr](site-recovery-vmm-to-vmm.md) über die detaillierte Site Recovery-Bereitstellung für die VMM-zu-VMM-Replikation.




 

<!---HONumber=AcomDC_0413_2016-->