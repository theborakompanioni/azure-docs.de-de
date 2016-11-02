<properties 
	pageTitle="Häufig gestellte Fragen (FAQ) zu Azure Site Recovery | Microsoft Azure" 
	description="Dieser Artikel enthält häufig gestellte Fragen zu Azure Site Recovery." 
	services="site-recovery" 
	documentationCenter=""
	authors="rayne-wiselman"
	manager="jwhit"
	editor=""/>

<tags 
	ms.service="get-started-article"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na" 
	ms.workload="storage-backup-recovery"
	ms.date="07/12/2016" 
	ms.author="raynew"/>


# Häufig gestellte Fragen (FAQ) zu Azure Site Recovery
## Informationen zum Artikel

Dieser Artikel enthält häufig gestellte Fragen zur Azure Site Recovery. Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese bitte im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## Allgemein

###Welche Funktion hat Site Recovery?

Site Recovery unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung, indem die Replikation von lokalen virtuellen Computern und physischen Servern in Azure oder ein sekundäres Datencenter aufeinander abgestimmt werden. [Weitere Informationen](site-recovery-overview.md).


### Was kann mit Site Recovery geschützt werden?

- **Virtuelle Hyper-V-Computer**: Mit Site Recovery kann jede Workload, die auf einer Hyper-V-VM ausgeführt wird, geschützt werden.
- **Physische Server**: Mit Site Recovery können physische Server unter Windows oder Linux geschützt werden.
- **Virtuelle VMware-Computer**: Mit Site Recovery kann jede Workload, die auf einer VMware-VM ausgeführt wird, geschützt werden.

### Unterstützt Site Recovery das Azure Resource Manager-Modell? 

Außer im klassischen Azure-Portal ist Site Recovery im Azure-Portal mit Unterstützung für den Resource Manager verfügbar. Für die meisten Bereitstellungsszenarien bietet Site Recovery im Azure-Portal eine vereinfachte Bereitstellung, und Sie können VMs und physische Server in klassischen Speicher oder Resource Manager-Speicher replizieren. Folgende Bereitstellungen werden unterstützt:

- [Replizieren von VMware-VMs oder physischen Servern in Azure im Azure-Portal](site-recovery-vmware-to-azure.md)
- [Replizieren von Hyper-V-VMs in VMM-Clouds in Azure im Azure-Portal](site-recovery-vmm-to-azure.md)
- [Replizieren von Hyper-V-VMs (ohne VMM) in Azure im Azure-Portal](site-recovery-hyper-v-site-to-azure.md)
- [Replizieren von Hyper-V-VMs in VMM-Clouds an einem sekundären Standort im Azure-Portal](site-recovery-vmm-to-vmm.md)


### Was benötige ich in Hyper-V, um die Replikation mit Site Recovery zu orchestrieren? 

Was Sie für Hyper-V-Hostserver benötigen, richtet sich nach dem Bereitstellungsszenario. Sehen Sie sich die Voraussetzungen für Hyper-V an:

- [Replizieren von Hyper-V-VMs (ohne VMM) in Azure](site-recovery-hyper-v-site-to-azure.md#before-you-start)
- [Replizieren von Hyper-V-VMs (mit VMM) in Azure](site-recovery-vmm-to-azure.md#before-you-start)
- [Replizieren von Hyper-V-VMs in ein sekundäres Datencenter](site-recovery-vmm-to-vmm.md#before-you-start)

- Wenn Sie die Replikation an einem sekundären Rechenzentrum durchführen, helfen Ihnen die Informationen unter [Unterstützte Gastbetriebssysteme für Hyper-V-VMs](https://technet.microsoft.com/library/mt126277.aspx) weiter.
- Wenn Sie in Azure replizieren, unterstützt Site Recovery alle Gastbetriebssysteme, die [von Azure unterstützt werden](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### Kann ich virtuelle Computer schützen, wenn Hyper-V auf einem Clientbetriebssystem ausgeführt wird?

Nein. VMs müssen sich auf einem Hyper-V-Hostserver befinden, der auf einem unterstützten Windows-Servercomputer ausgeführt wird. Wenn Sie einen Clientcomputer schützen müssen, können Sie diesen als physischen Computer [in Azure](site-recovery-vmware-to-azure.md) oder in einem [sekundären Rechenzentrum](site-recovery-vmware-to-vmware.md) replizieren.


### Welche Workloads können mit Site Recovery geschützt werden?

Mit Site Recovery können die meisten Workloads geschützt werden, die auf einer unterstützten VM oder einem physischen Server ausgeführt werden. Site Recovery bietet Unterstützung für die anwendungsorientierte Replikation, sodass für Apps ein „intelligenter Zustand“ wiederhergestellt werden kann. Eine Integration in Microsoft-Anwendungen wie SharePoint, Exchange, Dynamics, SQL Server und Active Directory ist möglich. Zudem kann Site Recovery eng in die Produkte führender Anbieter, z.B. Oracle, SAP, IBM und Red Hat, eingebunden werden. [Erfahren Sie mehr](site-recovery-workload.md) über den Schutz von Workloads.


### Müssen Hyper-V-Hosts sich in VMM-Clouds befinden? 

Wenn Sie die Replikation in einem sekundären Rechenzentrum durchführen möchten, müssen Hyper-V-VMs auf Hyper-V-Hostservern in einer VMM-Cloud angeordnet sein. Falls Sie in Azure replizieren möchten, können Sie VMs mit oder ohne VMM-Clouds auf Hyper-V-Hostservern replizieren. [Weitere Informationen](site-recovery-hyper-v-site-to-azure.md)

### Kann ich Site Recovery mit VMM bereitstellen, wenn ich nur über einen VMM-Server verfüge? 

Ja. Sie können VMs auf Hyper-V-Servern in der VMM-Cloud in Azure replizieren, oder Sie können zwischen VMM-Clouds auf demselben Server replizieren. Für die Replikation „lokal zu lokal“ empfehlen wir Ihnen, sowohl am primären Standort als auch am sekundären Standort jeweils einen VMM-Server zu verwenden. [Weitere Informationen](site-recovery-single-vmm.md)

### Welche physischen Server können geschützt werden?

Sie können physische Server unter Windows und Linux in Azure oder an einem sekundären Standort replizieren. [Informieren Sie sich](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) über die Betriebssystemanforderungen. Unabhängig davon, ob Sie physische Server in Azure oder an einem sekundären Standort replizieren, gelten die gleichen Anforderungen.

Beachten Sie, dass physische Server als VMs in Azure ausgeführt werden, wenn der lokale Server ausfällt. Ein Failback auf einen lokalen physischen Server wird derzeit nicht unterstützt, aber ein Failback auf einen virtuellen Computer auf Hyper-V oder VMware ist möglich.


### Welche VMware-VMs können geschützt werden?

Zum Schützen von VMware-VMs benötigen Sie einen vSphere-Hypervisor und virtuelle Computer, auf denen VMware-Tools ausgeführt werden. Außerdem wird empfohlen, einen VMware vCenter-Server zum Verwalten der Hypervisoren einzusetzen. [Erfahren Sie mehr](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) zu den genauen Anforderungen für die Replikation von VMware-Servern und VMs in Azure oder an einem sekundären Standort.

### Kann ich mit Site Recovery die Notfallwiederherstellung für meine Zweigstellen verwalten?

Ja. Wenn Sie Site Recovery zum Orchestrieren von Replikation und Failover in Zweigstellen verwenden, erhalten Sie eine einheitliche Orchestrierung und Anzeige der Workloads aller Zweigstellen an einer zentralen Stelle. Sie können problemlos über Ihren Hauptsitz für alle Zweigstellen Failover ausführen und die Notfallwiederherstellung verwalten, ohne die Zweigstellen zu besuchen.

## Sicherheit

### Werden Replikationsdaten an den Site Recovery-Dienst gesendet?

Nein. Site Recovery fängt replizierte Daten nicht ab und besitzt keine Informationen dazu, was auf Ihren virtuellen Computern oder physischen Servern ausgeführt wird. Replikationsdaten werden zwischen lokalen Hyper-V-Hosts, VMware-Hypervisoren oder physischen Servern und Azure-Speicher an Ihrem sekundären Standort ausgetauscht. Site Recovery hat keine Möglichkeit, diese Daten abzufangen. Nur die Metadaten, die zum Orchestrieren von Replikation und Failover erforderlich sind, werden an den Site Recovery-Dienst gesendet.

Site Recovery ist nach ISO 27001:2013, 27018 für HIPAA und DPA zertifiziert und durchläuft gerade die Prüfungen für SOC2 und FedRAMP JAB.


### Aus Gründen der Compliance müssen sogar die Metadaten aus unseren lokalen Umgebungen innerhalb der geografischen Region bleiben. Kann Site Recovery uns helfen?

Ja. Durch die Erstellung eines Site Recovery-Tresors in einer Region wird sichergestellt, dass alle Metadaten, die wir zum Ermöglichen und Orchestrieren von Replikation und Failover benötigen, innerhalb der geografischen Grenzen dieser Region bleiben.

### Verschlüsselt Site Recovery die Replikation?

Für virtuelle Computer und physische Server wird bei der Replikation zwischen lokalen Standorten die Verschlüsselung während der Übertragung unterstützt. Für virtuelle Computer und physische Server wird bei der Replikation in Azure sowohl die Verschlüsselung während der Übertragung als auch die Verschlüsselung ruhender Daten (in Azure) unterstützt.


## Replikation


### Gelten Voraussetzungen für die Replikation von virtuellen Computern in Azure?

Virtuelle Computer, die Sie in Azure replizieren möchten, sollten den [Azure-Anforderungen](site-recovery-best-practices.md#virtual-machines) entsprechen.

### Kann ich virtuelle Hyper-V-Computer der 2. Generation in Azure replizieren?

Ja. Während des Failovers konvertiert Site Recovery Computer der 2. Generation in die 1. Generation. Beim Failback werden die Computer wieder in die 2. Generation zurückkonvertiert. [Weitere Informationen](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)

### Wenn ich in Azure repliziere, wie bezahle ich für Azure-VMs? 

Während der regulären Replikation werden Daten in den georedundanten Azure-Speicher repliziert. Dabei fallen keinerlei Gebühren für virtuelle Azure-IaaS-Computer an. Dies ist ein großer Vorteil. Bei einem Failover zu Azure erstellt Site Recovery automatisch virtuelle Azure-IaaS-Computer, und Ihnen werden die in Azure genutzten Computeressourcen in Rechnung gestellt.


### Gibt es ein SDK, mit dem ich den ASR-Workflow automatisieren kann?

Ja. Sie können Site Recovery-Workflows mithilfe der REST-API, PowerShell oder Azure SDK automatisieren. Derzeit unterstützte Szenarien für die Bereitstellung von Site Recovery mit PowerShell:

- [Replizieren von Hyper-V-VMs in VMM-Clouds in klassischem Azure PowerShell](site-recovery-deploy-with-powershell.md)
- [Replizieren von Hyper-V-VMs in VMM-Clouds im Azure PowerShell Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Replizieren von Hyper-V-VMs ohne VMM in klassischem Azure PowerShell](site-recovery-hyper-v-site-to-azure-classic.md)
- [Replizieren von Hyper-V-VMs ohne VMM in Azure PowerShell Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)


### Wenn ich in Azure repliziere, welche Art von Speicherkonto benötige ich?

- **Klassisches Azure-Portal**: Wenn Sie Site Recovery im klassischen Azure-Portal bereitstellen, benötigen Sie ein [georedundantes Standardspeicherkonto](../storage/storage-redundancy.md#geo-redundant-storage). Storage Premium wird derzeit nicht unterstützt. Das Konto muss sich in der gleichen Region wie der Site Recovery-Tresor befinden.

- **Azure-Portal**: Wenn Sie Site Recovery im Azure-Portal bereitstellen, benötigen Sie ein LRS- oder GRS-Speicherkonto. Wir empfehlen Ihnen die Verwendung von GRS, damit Resilienz für die Daten besteht, wenn es zu einem regionalen Ausfall kommt oder wenn die primäre Region nicht wiederhergestellt werden kann. Das Konto muss sich in derselben Region wie der Recovery Services-Tresor befinden. Derzeit wird Storage Premium nur unterstützt, wenn Sie VMware-VMs oder physische Server replizieren.

### Wie oft kann ich Daten replizieren?

- **Hyper-V:** Hyper-V-VMs können alle 30 Sekunden, 5 Minuten oder 15 Minuten repliziert werden. Wenn Sie die SAN-Replikation eingerichtet haben, erfolgt die Replikation synchron.
- **VMware- und physische Server:** Hier ist eine Replikationshäufigkeit irrelevant. Die Replikation ist fortlaufend.

### Kann die Replikation von vorhandenen Wiederherstellungsstandorten auf einen weiteren tertiären Standort erweitert werden?

Eine erweiterte oder verkettete Replikation wird nicht unterstützt. Fordern Sie dieses Feature im [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).


### Kann ich eine Offlinereplikation durchführen, wenn ich zum ersten Mal in Azure repliziere? 

Dies wird nicht unterstützt. Fordern Sie dieses Feature im [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).


### Können bestimmte Datenträger von der Replikation ausgeschlossen werden?

Dies wird unterstützt, wenn Sie in Azure mithilfe des Azure-Portals [VMware-VMs und physische Server replizieren](site-recovery-vmware-to-azure.md#exclude-disks-from-replication).


### Können virtuelle Computer mit dynamischen Datenträgern repliziert werden?

Dynamische Datenträger werden unterstützt, wenn virtuelle Hyper-V-Computer repliziert werden. Diese werden auch beim Replizieren von VMware-VMs und physischen Computern in Azure unterstützt, wenn Sie das [Azure-Portal](site-recovery-vmware-to-azure-classic.md) oder das [klassische Azure-Portal mit erweiterter Bereitstellung](site-recovery-vmware-to-azure.md) verwenden. Beachten Sie, dass es sich beim Betriebssystem-Datenträger um einen Basisdatenträger handeln muss.

### Kann ich die Bandbreite drosseln, die für den Hyper-V-Replikationsdatenverkehr zugeordnet ist?

Ja. In den Bereitstellungsartikeln erfahren Sie mehr über die Drosselung der Bandbreite:

- [Kapazitätsplanung für die Replikation von VMware-VMs und physischen Servern](site-recovery-vmware-to-azure.md#step-5-capacity-planning)
- [Kapazitätsplanung für die Replikation von Hyper-V-VMs in VMM-Clouds](site-recovery-vmm-to-azure.md#step-5-capacity-planning)
- [Kapazitätsplanung für die Replikation von Hyper-V-VMs ohne VMM](site-recovery-hyper-v-site-to-azure.md#step-5-capacity-planning)

## Failover


### Wie greife ich nach einem Failover auf Azure auf die virtuellen Azure-Computer zu? 

Sie können auf die Azure-VMs über eine sichere Internetverbindung, über eine Site-to-Site-VPN-Verbindung oder über Azure ExpressRoute zugreifen. Sie müssen eine Reihe von Vorbereitungen treffen, um eine Verbindung herzustellen. Weitere Informationen finden Sie in:

- [Connect to Azure VMs after failover of VMware VMs or physical servers](hsite-recovery-vmware-to-azure.md#step-7-test-the-deployment) (Herstellen einer Verbindung mit Azure-VMs nach einem Failover von VMware-VMs oder physischen Servern)
- [Connect to Azure VMs after failover of Hyper-V VMs in VMM clouds](site-recovery-vmm-to-azure.md#step-7-test-your-deployment) (Herstellen einer Verbindung mit Azure-VMs nach einem Failover von Hyper-V-VMs in VMM-Clouds)
- [Connect to Azure VMs after failover of Hyper-V VMs without VMM](site-recovery-hyper-v-site-to-azure.md#step-7-test-the-deployment) (Herstellen einer Verbindung mit Azure-VMs nach einem Failover von Hyper-V-VMs ohne VMM)


### Wie wird bei einem Failover auf Azure in Azure sichergestellt, dass die Daten stabil sind?

Azure ist auf Resilienz ausgelegt. Site Recovery ist bereits für ein Failover auf ein sekundäres Azure-Datencenter (gemäß Azure-SLA) konzipiert, falls sich die Notwendigkeit ergibt. Wenn dies der Fall ist, stellen wir sicher, dass Ihre Metadaten und Tresore innerhalb der gleichen geografischen Region bleiben, die Sie für Ihren Tresor ausgewählt haben.

### Wenn ich eine Replikation zwischen zwei Datencentern ausführe, was geschieht, wenn in meinem primären Datencenter ein unerwarteter Ausfall auftritt?

Sie können ein ungeplantes Failover vom sekundären Standort auslösen. Site Recovery benötigt keine Verbindung mit dem primären Standort, um das Failover auszuführen.

### Erfolgt ein Failover automatisch?

Ein Failover erfolgt nicht automatisch. Sie initiieren Failover mit einem Mausklick im Portal, oder Sie können die [Site Recovery-PowerShell](https://msdn.microsoft.com/library/dn850420.aspx) verwenden, um ein Failover auslösen. Ein Failback ist auch eine einfache Aktion im Site Recovery-Portal.

Zum Automatisieren könnten Sie den lokalen Orchestrator oder Operations Manager verwenden, um einen Fehler bei virtuellen Computern zu erkennen und dann das Failover mithilfe des SDK auszulösen.

- Weitere Informationen zu Wiederherstellungsplänen finden Sie [hier](site-recovery-create-recovery-plans.md).
- Weitere Informationen zum Failover finden Sie [hier](site-recovery-failover.md).
- Weitere Informationen zu Failbacks von VMware-VMs und physischen Servern finden Sie [hier](site-recovery-failback-azure-to-vmware.md).


## Dienstanbieter


### Ich bin ein Dienstanbieter. Ist Site Recovery für dedizierte oder gemeinsam genutzte Infrastrukturmodelle geeignet?
Ja, Site Recovery unterstützt dedizierte und gemeinsam genutzte Infrastrukturmodelle.

### Für einen Dienstanbieter wird die Identität meines Mandanten an den Site Recovery-Dienst weitergegeben?
Nein. Die Mandantenidentifizierung bleibt anonym. Ihre Mandanten benötigen keinen Zugriff auf das Site Recovery-Portal. Nur der Administrator des Dienstanbieters interagiert mit dem Portal.


### Werden die Anwendungsdaten meiner Mandanten jemals an Azure übermittelt?
Bei der Replikation zwischen den Standorten von Dienstanbietern werden die Anwendungsdaten niemals an Azure übermittelt. Die Daten werden während der Übertragung verschlüsselt und direkt zwischen den Standorten des Dienstanbieters repliziert.

Wenn Sie in Azure replizieren, werden Anwendungsdaten an den Azure-Speicher gesendet, jedoch nicht an den Site Recovery-Dienst. Daten werden während der Übertragung verschlüsselt und bleiben in Azure verschlüsselt.


### Erhalten meine Mandanten eine Rechnung über Azure-Dienste?

Nein. Azure rechnet direkt mit dem Dienstanbieter ab. Mandantenspezifische Rechnungen müssen von den Dienstanbietern selbst erstellt werden.

### Wenn ich in Azure repliziere, müssen wir dann ständig virtuelle Computer in Azure ausführen?

Nein. Die Daten werden in ein Azure-Speicherkonto in Ihrem Abonnement repliziert. Bei einem Testfailover (Notfallwiederherstellungsübung) oder einem tatsächlichen Failover erstellt Site Recovery automatisch virtuelle Computer in Ihrem Abonnement.

### Stellen Sie die Isolierung auf Mandantenebene sicher, wenn ich in Azure repliziere?

Ja.

### Welche Plattformen werden derzeit unterstützt?

Wir unterstützen Bereitstellungen auf der Grundlage von Azure Pack, Cloud Platform System und System Center (2012 und höher). [Erfahren Sie mehr](https://technet.microsoft.com/library/dn850370.aspx) zur Integration von Azure Pack und Site Recovery.

### Werden einzelne Bereitstellungen von Azure Pack und VMM-Servern unterstützt?

Ja, Sie können virtuelle Hyper-V-Computer in Azure oder zwischen Dienstanbieterstandorten replizieren. Beachten Sie, dass für die Replikation zwischen Dienstanbieterstandorten die Azure-Runbookintegration nicht verfügbar ist.


## Nächste Schritte

- Lesen Sie die [Site Recovery-Übersicht](site-recovery-overview.md).
- Erfahren Sie mehr über die [Site Recovery-Architektur](site-recovery-components.md).

 

<!---HONumber=AcomDC_0720_2016-->