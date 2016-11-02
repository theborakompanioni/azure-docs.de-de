<properties
    pageTitle="Wie funktioniert Site Recovery? | Microsoft Azure"
    description="Dieser Artikel enthält eine Übersicht über die Site Recovery-Architektur."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/13/2016"
    ms.author="raynew"/>


# <a name="how-does-azure-site-recovery-work?"></a>Wie funktioniert Azure Site Recovery?

Lesen Sie diesen Artikel, um sich über die Architektur des Azure Site Recovery-Diensts und die dafür erforderlichen Komponenten zu informieren.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.


## <a name="overview"></a>Übersicht

Sie benötigen eine Strategie für die Bereiche Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR), damit Workloads und Daten bei geplanten und ungeplanten Ausfallzeiten verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können. 

Site Recovery ist ein Azure-Dienst als Beitrag zu Ihrer BCDR-Strategie. Er dient zum Orchestrieren der Replikation von lokalen physischen Servern und virtuellen Computern in der Cloud (Azure) oder in einem sekundären Datencenter. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Der Artikel [Was ist Site Recovery?](site-recovery-overview.md) enthält hierzu eine Übersicht.

## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery im Azure-Portal

Azure verfügt über zwei unterschiedliche [Bereitstellungsmodelle](../resource-manager-deployment-model.md) zum Erstellen und Verwenden von Ressourcen: das Azure Resource Manager-Modell und das klassische Dienstverwaltungsmodell. Außerdem verfügt Azure über zwei Portale: das [klassische Azure-Portal](https://manage.windowsazure.com/) mit Unterstützung des klassischen Bereitstellungsmodells und das [Azure-Portal](https://portal.azure.com) mit Unterstützung für beide Bereitstellungsmodelle.

- Site Recovery ist sowohl im klassischen Portal als auch im Azure-Portal verfügbar.
- Im klassischen Azure-Portal können Sie Site Recovery mit dem klassischen Dienstverwaltungsmodell unterstützen.
- Im Azure-Portal können Sie das klassische Modell oder Resource Manager-Bereitstellungen unterstützen. 

Die Informationen in diesem Artikel gelten für Bereitstellungen mit dem klassischen Portal und dem Azure-Portal. Die Tabelle enthält eine Zusammenfassung der Unterschiede.

**Replikat** | **Azure-Portal** | **Klassisches Portal**
--- | --- | ---
**VMware-VM-Replikation in Azure** | Vereinfachter Bereitstellungsprozess<br/><br/> Failover von VMs in klassischem oder Resource Manager-basiertem Speicher<br/><br/> Replikation in klassischem oder Resource Manager-basiertem Speicher<br/><br/> Verwendung von klassischen oder Resource Manager-Netzwerken zum Herstellen der Verbindung für Azure-VMs nach dem Failover<br/><br/> Verwendung von LRS- oder GRS-Speicher | Ausschließliches Failover in klassischen Speicher<br/><br/> Ausschließliche Verwendung von klassischen Netzwerken zum Herstellen der Verbindung für VMs nach dem Failover<br/><br/> Verwendung von GRS-Speicher
**Hyper-V-VM-Replikation (ohne VMM) in Azure** | Vereinfachter Bereitstellungsprozess<br/><br/> Failover von VMs in klassischem oder Resource Manager-basiertem Speicher<br/><br/> Replikation in klassischem oder Resource Manager-basiertem Speicher<br/><br/> Verwendung von klassischen oder Resource Manager-Netzwerken zum Herstellen der Verbindung für Azure-VMs nach dem Failover
**Hyper-V-VM-Replikation (mit VMM) in Azure** | Vereinfachter Bereitstellungsprozess<br/><br/> Failover von VMs in klassischem oder Resource Manager-basiertem Speicher<br/><br/> Replikation in klassischem oder Resource Manager-basiertem Speicher<br/><br/> Verwendung von klassischen oder Resource Manager-Netzwerken zum Herstellen der Verbindung für Azure-VMs nach dem Failover<br/><br/> Erforderliche Einrichtung der Netzwerkzuordnung | Ausschließliches Failover in klassischen Speicher<br/><br/> Ausschließliche Verwendung von klassischen Netzwerken zum Herstellen der Verbindung für VMs nach dem Failover
**Hyper-V-VM-Replikation (mit VMM) am sekundären Standort** | Vereinfachter Bereitstellungsprozess<br/><br/> Failover von VMs in klassischem oder Resource Manager-basiertem Speicher<br/><br/> Replikation in klassischem oder Resource Manager-basiertem Speicher<br/><br/> Verwendung von klassischen oder Resource Manager-Netzwerken zum Herstellen der Verbindung für Azure-VMs nach dem Failover<br/><br/> Erforderliche Einrichtung der Netzwerkzuordnung | Ausschließliches Failover in klassischen Speicher<br/><br/> Ausschließliche Verwendung von klassischen Netzwerken zum Herstellen der Verbindung für VMs nach dem Failover<br/><br/> Sie können die Speicherzuordnung einrichten. <br/><br/> SAN-Replikation wird nicht unterstützt.



## <a name="deployment-scenarios"></a>Bereitstellungsszenarien

Site Recovery kann bereitgestellt werden, um die Replikation in unterschiedlichen Szenarien zu orchestrieren:

- **Replizieren von VMware-VMs**: Sie können lokale virtuelle VMware-Computer in Azure-Speicher oder einem sekundären Datencenter replizieren.
- **Replizieren von physischen Computern**: Sie können physische Computer mit Windows oder Linux in Azure-Speicher oder einem sekundären Datencenter replizieren. Der Prozess für das Replizieren von physischen Computern ist fast identisch mit dem Prozess für das Replizieren von VMware-VMs.
- **Replizieren von Hyper-V-VMs**: Wenn Ihre Hyper-V-Hosts in System Center-VMM-Clouds verwaltet werden, können Sie VMs in Azure oder in einem sekundären Datencenter replizieren. Wenn Hosts nicht per VMM verwaltet werden, ist nur die Replikation in Azure möglich. Sie können Hyper-V-VMs, die nicht per VMM verwaltet werden, in Azure-Speicher replizieren.
- **Migrieren von VMs**: Sie können Site Recovery verwenden, um [Azure-IaaS-VMs](site-recovery-migrate-azure-to-azure.md) zwischen Regionen zu migrieren oder um [AWS Windows-Instanzen](site-recovery-migrate-aws-to-azure.md) zu Azure-IaaS-VMs zu migrieren. Die vollständige Replikation wird derzeit nicht unterstützt. Sie können für die Migration (Failover) replizieren, aber ein Failback ist nicht möglich. 

Site Recovery kann die meisten auf diesen virtuellen Maschinen und physischen Servern ausgeführten Apps replizieren. Eine vollständige Zusammenfassung der unterstützten Apps finden Sie unter [Welche Workloads können mit Azure Site Recovery geschützt werden?](site-recovery-workload.md)


## <a name="replicate-vmware-virtual-machines-to-azure"></a>Repliziert virtuelle VMware-Computer zu Azure

![Verbessert](./media/site-recovery-components/arch-enhanced.png)

**Komponente** | **Details**
--- | ---
**Azure** | **Konto**: Sie benötigen ein Azure-Konto.<br/><br/> **Speicher**: Sie benötigen ein Azure-Speicherkonto, um replizierte Daten zu speichern. Sie können ein klassisches Konto oder ein Resource Manager-Speicherkonto verwenden. Das Konto kann den Typ LRS oder GRS aufweisen, wenn Sie die Bereitstellung über das Azure-Portal durchführen. Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden bei einem Failover hochgefahren.<br/><br/> **Netzwerk**: Sie benötigen ein virtuelles Azure-Netzwerk, mit dem die Azure-VMs eine Verbindung herstellen, wenn sie während eines Failovers erstellt werden. Im Azure-Portal können Netzwerke unter dem klassischen Service-Manager-Modell oder dem Resource Manager-Modell erstellt werden.
**Lokaler Konfigurationsserver** |  Sie benötigen einen lokalen Windows Server 2012 R2-Computer, auf dem der Konfigurationsserver und andere Site Recovery-Komponenten ausgeführt werden.<br/><br/> Hierbei sollte es sich um eine hoch verfügbare VMware-VM handeln.<br/><br/> Zu den Komponenten auf dem Server gehören:<br/><br/> **Konfigurationsserver**: Koordiniert die Kommunikation zwischen Ihrer lokalen Umgebung und Azure und verwaltet die Datenreplikation und Wiederherstellung.<br/><br/> **Prozessserver**: Fungiert als Replikationsgateway. Sie empfängt Replikationsdaten aus geschützten Quellcomputern, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet die Daten an den Azure-Speicher. Außerdem wickelt er die Pushinstallation des Mobilitätsdiensts auf geschützten Computern ab und führt die automatische Ermittlung von virtuellen VMware-Computern durch. Bei zunehmender Größe der Bereitstellung können Sie zusätzlich separate dedizierte Prozessserver hinzufügen, um steigende Mengen von Replikationsdatenverkehr zu bewältigen.<br/><br/> **Masterzielserver**: Verarbeitet die Replikationsdaten während des Failbacks von Azure.
**Lokale Virtualisierungsserver** | Mindestens einen vSphere-Host. Zum Verwalten der Hosts wird außerdem ein vCenter-Server empfohlen.
**Zu replizierende VMs** | Auf jeder VMware-VM, die Sie in Azure replizieren möchten, muss die Mobilitätsdienstkomponente installiert sein. Dieser Dienst erfasst die Datenschreibvorgänge auf dem Computer und sendet sie an den Prozessserver. Die Komponente kann manuell oder mittels Pushvorgang automatisch durch den Prozessserver installiert werden, wenn die Replikation für den Computer aktiviert wird.

- [Informieren Sie sich](site-recovery-vmware-to-azure.md#azure-prerequisites) über die Anforderungen für die Bereitstellung mit dem Azure-Portal.
- [Informieren Sie sich](site-recovery-failback-azure-to-vmware.md) über das Failback im Azure-Portal.


## <a name="replicate-physical-servers-to-azure"></a>Replizieren physischer Server in Azure

![Verbessert](./media/site-recovery-components/arch-enhanced.png)

**Komponente** | **Details**
--- | ---
**Azure** | **Konto**: Sie benötigen ein Azure-Konto.<br/><br/> **Speicher**: Sie benötigen ein Azure-Speicherkonto, um replizierte Daten zu speichern. Sie können ein klassisches Konto oder ein Resource Manager-Speicherkonto verwenden. Das Konto kann den Typ LRS oder GRS aufweisen, wenn Sie die Bereitstellung über das Azure-Portal durchführen. Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden bei einem Failover hochgefahren.<br/><br/> **Netzwerk**: Sie benötigen ein virtuelles Azure-Netzwerk, mit dem die Azure-VMs eine Verbindung herstellen, wenn sie während eines Failovers erstellt werden. Im Azure-Portal können Netzwerke unter dem klassischen Service-Manager-Modell oder dem Resource Manager-Modell erstellt werden.
**Lokaler Konfigurationsserver** |  Sie benötigen einen lokalen Windows Server 2012 R2-Computer, auf dem der Konfigurationsserver und andere Site Recovery-Komponenten ausgeführt werden.<br/><br/> Es kann ein virtueller oder ein physischer Server verwendet werden.<br/><br/> Zu den Komponenten auf dem Server gehören:<br/><br/> **Konfigurationsserver**: Koordiniert die Kommunikation zwischen Ihrer lokalen Umgebung und Azure und verwaltet die Datenreplikation und Wiederherstellung.<br/><br/> **Prozessserver**: Fungiert als Replikationsgateway. Sie empfängt Replikationsdaten aus geschützten Quellcomputern, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet die Daten an den Azure-Speicher. Außerdem wickelt er die Pushinstallation des Mobilitätsdiensts auf geschützten Computern ab und führt die automatische Ermittlung von virtuellen VMware-Computern durch. Bei zunehmender Größe der Bereitstellung können Sie zusätzlich separate dedizierte Prozessserver hinzufügen, um steigende Mengen von Replikationsdatenverkehr zu bewältigen.<br/><br/> **Masterzielserver**: Verarbeitet die Replikationsdaten während des Failbacks von Azure.
**Zu replizierende Computer** | Auf jedem physischen Windows- bzw. Linux-Computer, den Sie in Azure replizieren möchten, muss die Mobilitätsdienstkomponente installiert sein. Dieser Dienst erfasst die Datenschreibvorgänge auf dem Computer und sendet sie an den Prozessserver. Die Komponente kann manuell oder mittels Pushvorgang automatisch durch den Prozessserver installiert werden, wenn die Replikation für den Computer aktiviert wird.
**Failback** | Das Failback vom Typ „physisch zu physisch“ wird nicht unterstützt. Dies bedeutet, dass ein Failback auf eine VMware-VM erforderlich ist, wenn Sie für physische Server ein Failover zu Azure mit anschließendem Failback durchführen möchten. Ein Failback zu einem physischen Server ist nicht möglich. Sie benötigen eine Azure-VM für das Failback. Falls Sie den Konfigurationsserver nicht als VMware-VM bereitgestellt haben, müssen Sie einen separaten Masterzielserver als VMware-VM einrichten. Dies ist erforderlich, weil der Masterzielserver mit dem VMware-Speicher interagiert und diesem Speicher zugeordnet wird, um die Datenträger auf einer VMware-VM wiederherzustellen.


- [Informieren Sie sich](site-recovery-vmware-to-azure.md#azure-prerequisites) über die Anforderungen für die Bereitstellung mit dem Azure-Portal.
- [Informieren Sie sich](site-recovery-failback-azure-to-vmware.md) über das Failback im Azure-Portal.


## <a name="replicate-hyper-v-vms-not-managed-by-vmm-to-azure"></a>Replikation von nicht per VMM verwalteten Hyper-V-VMs in Azure

![Hyper-V-Standort zu Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Komponente** | **Details**
--- | ---
**Azure** | **Konto**: Sie benötigen ein Azure-Konto.<br/><br/> **Speicher**: Sie benötigen ein Azure-Speicherkonto, um replizierte Daten zu speichern. Sie können ein klassisches Konto oder ein Resource Manager-Speicherkonto verwenden. Das Konto muss den Typ GRS haben. Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden bei einem Failover hochgefahren.<br/><br/> **Netzwerk**: Sie benötigen ein virtuelles Azure-Netzwerk, mit dem die Azure-VMs eine Verbindung herstellen, wenn sie während eines Failovers erstellt werden.
**Hyper-V-Hosts/-VMs** | Mindestens ein Hyper-V-Host, der auf mindestens einer VM ausgeführt wird.<br/><br/> Der Site Recovery-Anbieter und der Recovery Services-Agent werden während der Bereitstellung auf jedem Host installiert.

- [Informieren Sie sich](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) über die Anforderungen für die Bereitstellung mit dem Azure-Portal.
- [Informieren Sie sich](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) über die Anforderungen für die Bereitstellung mit dem klassischen Portal.



## <a name="replicate-hyper-v-vms-managed-by-vmm-to-azure"></a>Replikation von per VMM verwalteten Hyper-V-VMs in Azure


![VMM zu Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

**Komponente** | **Details**
--- | ---
**Azure** | **Konto**: Sie benötigen ein Azure-Konto.<br/><br/> **Speicher**: Sie benötigen ein Azure-Speicherkonto, um replizierte Daten zu speichern. Sie können ein klassisches Konto oder ein Resource Manager-Speicherkonto verwenden. Das Konto muss den Typ GRS haben. Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden bei einem Failover hochgefahren.<br/><br/> **Netzwerk**: Sie benötigen ein virtuelles Azure-Netzwerk, mit dem die Azure-VMs eine Verbindung herstellen, wenn sie während eines Failovers erstellt werden.
**VMM-Server** | Sie benötigen mindestens einen lokalen VMM-Server mit mindestens einer privaten Cloud. Der Site Recovery-Anbieter wird während der Bereitstellung auf jedem Server installiert.
**Hyper-V-Hosts/-VMs** | Mindestens ein Hyper-V-Host, der auf mindestens einer VM ausgeführt wird.<br/><br/> Der Recovery Services-Agent wird während der Bereitstellung auf jedem Host installiert.


- [Informieren Sie sich](site-recovery-vmm-to-azure.md#azure-requirements) über die Anforderungen für die Bereitstellung mit dem Azure-Portal.
- [Informieren Sie sich](site-recovery-vmm-to-azure-classic.md#before-you-start) über die Anforderungen für die Bereitstellung mit dem klassischen Portal.




## <a name="replicate-vmware-virtual-machines-or-physical-server-to-a-secondary-site"></a>Replikation von virtuellen VMware-Computern oder physischen Servern an einem sekundären Standort

![VMware zu VMware](./media/site-recovery-components/vmware-to-vmware.png)


**Komponente** | **Details**
--- | ---
**Azure** | **Konto**: Sie stellen dieses Szenario mit InMage Scout bereit. Hierfür benötigen Sie ein Azure-Konto.<br/><br/> Nachdem Sie einen Site Recovery-Tresor erstellt haben, laden Sie InMage Scout herunter und installieren die aktuellen Updates, um die Bereitstellung einzurichten.
**Primärer Standort** | **Prozessserver**: Richten Sie die Prozessserverkomponente an Ihrem primären Standort ein, um das Caching, die Komprimierung und die Datenoptimierung zu behandeln. Außerdem erfolgt hierüber die Pushinstallation des vereinheitlichten Agents auf Maschinen, die Sie schützen möchten.<br/><br/> **VMware ESX/ESXi und vCenter**: Sie benötigen einen VMware EXS/ESXi-Hypervisor und optional einen VMware vCenter-Server.<br/><br/> **VMs** | Während der Bereitstellung wird der vereinheitlichte Agent auf den zu schützenden Computern installiert. Der Agent fungiert als Kommunikationsanbieter zwischen allen Komponenten.
**Sekundärer Standort** | **Konfigurationsserver**: Der Konfigurationsserver ist die erste Komponente, die Sie installieren. Er wird am sekundären Standort installiert, um Ihre Bereitstellung zu verwalten, zu konfigurieren und zu überwachen, und zwar entweder mit der Verwaltungswebsite oder der vContinuum-Konsole. Es gibt nur einen Konfigurationsserver in einer Bereitstellung, der auf einem Computer mit Windows Server 2012 R2 installiert sein muss.<br/><br/> **vContinuum-Server (sekundärer Standort)**: Der Server ist an demselben Ort wie der Konfigurationsserver installiert. Er enthält eine Konsole zum Verwalten und Überwachen Ihrer geschützten Umgebung. Bei einer standardmäßigen Installation ist der vContinuum-Server der erste Masterzielserver und verfügt über eine Installation des vereinheitlichten Agents.<br/><br/> **Masterzielserver**: Der Masterzielserver enthält replizierte Daten. Er empfängt Daten vom Prozessserver und erstellt einen Replikatcomputer am sekundären Standort. Außerdem enthält er die Punkte für die Beibehaltung der Daten. Die Anzahl der benötigten Masterzielserver richtet sich nach der Anzahl der Computer, die Sie schützen. Wenn Sie ein Failback zum primären Standort durchführen möchten, benötigen Sie dort ebenfalls einen Masterzielserver.


Zum Replizieren von VMware-VMs oder physischen Servern an einem sekundären Standort laden Sie die Anwendung InMage Scout herunter, die im Azure Site Recovery-Abonnement enthalten ist. Der Download ist über das Azure-Portal oder das klassische Azure-Portal möglich.

Sie richten die Komponentenserver an jedem Standort ein (Konfiguration, Prozess, Masterziel) und installieren den vereinheitlichten Agent auf Computern, die Sie replizieren möchten. Nach der ersten Replikation sendet der Agent auf jedem Computer die Deltareplikationsänderungen an den Prozessserver. Der Prozessserver optimiert die Daten und überträgt sie an den Masterzielserver am sekundären Standort. Der Konfigurationsserver verwaltet den Replikationsprozess.


## <a name="replicate-hyper-v-vms-managed-by-vmm-to-a-secondary-site"></a>Replikation von per VMM verwalteten Hyper-V-VMs an einem sekundären Standort

![Lokal zu lokal](./media/site-recovery-components/arch-onprem-onprem.png)

**Komponente** | **Details**
--- | ---
**Azure** | **Konto**: Sie benötigen ein Azure-Konto.
**VMM-Server** | Wir empfehlen Ihnen die Verwendung eines VMM-Servers am primären Standort und eines VMM-Servers am sekundären Standort. Für jeden Server ist mindestens eine private Cloud erforderlich.<br/><br/> Während der Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf dem VMM-Server.
**Hyper-V-Hosts/-VMs** | Mindestens ein Hyper-V-Host, der in den VMM-Clouds am primären und sekundären Standort ausgeführt wird<br/><br/> Jeder Host sollte über mindestens eine zu replizierende VM verfügen.<br/><br/>. Der Recovery Services-Agent wird während der Bereitstellung auf jedem Host installiert.

- [Informieren Sie sich](site-recovery-vmm-to-vmm.md#azure-prerequisites) über die Anforderungen der Bereitstellung im Azure-Portal.
- [Informieren Sie sich](site-recovery-vmm-to-vmm-classic.md#before-you-start) über die Anforderungen der Bereitstellung im klassischen Portal.


## <a name="replicate-hyper-v-vms-managed-by-vmm-to-a-secondary-site-using-san-replication"></a>Replikation von per VMM verwalteten Hyper-V-VMs an einem sekundären Standort mithilfe der SAN-Replikation

![SAN-Replikation](./media/site-recovery-components/arch-onprem-onprem-san.png)

Sie können in VMM-Clouds verwaltete Hyper-V-VMs per SAN-Replikation an einem sekundären Standort replizieren, indem Sie das klassische Portal verwenden. Dieses Szenario wird im Azure-Portal derzeit nicht unterstützt.

**Komponente** | **Details**
--- | ---
**Azure** | **Konto**: Sie benötigen ein Azure-Konto.
**VMM-Server** | Wir empfehlen Ihnen die Verwendung eines VMM-Servers am primären Standort und eines VMM-Servers am sekundären Standort. Für jeden Server ist mindestens eine private Cloud erforderlich.<br/><br/> Während der Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf dem VMM-Server.
**SAN** | Ein unterstütztes SAN-Array, das vom primären VMM-Server verwaltet wird.<br/><br/> Das SAN sollte eine Netzwerkinfrastruktur mit einem anderen SAN-Array am sekundären Standort gemeinsam nutzen.
**Hyper-V-Hosts/-VMs** | Mindestens ein Hyper-V-Host, der in den VMM-Clouds am primären und sekundären Standort ausgeführt wird<br/><br/> Jeder Host sollte über mindestens eine zu replizierende VM verfügen.<br/><br/>. Der Recovery Services-Agent wird während der Bereitstellung auf jedem Host installiert.

Bei diesem Szenario installieren Sie den Azure Site Recovery-Anbieter während der Bereitstellung von Site Recovery auf den VMM-Servern. Der Anbieter koordiniert und orchestriert die Replikation mit dem Site Recovery-Dienst über das Internet. Die Daten werden zwischen den primären und sekundären Speicherarrays per synchroner SAN-Replikation repliziert.

[Erfahren Sie mehr](site-recovery-vmm-san.md#before-you-start) über die Voraussetzungen für die Bereitstellung.

## <a name="hyper-v-protection-lifecycle"></a>Lebenszyklus des Hyper-V-Schutzes

Dieser Workflow verdeutlicht den Prozess zum Schützen, Replizieren und Durchführen des Failovers für virtuelle Hyper-V-Maschinen.

1. **Aktivieren des Schutzes**: Sie richten den Site Recovery-Tresor ein, konfigurieren die Replikationseinstellungen für eine VMM-Cloud oder eine Hyper-V-Site und aktivieren den Schutz für die VMs. Es wird ein Auftrag mit der Bezeichnung **Schutz aktivieren** initiiert, und er kann auf der Registerkarte **Aufträge** überwacht werden. Im Rahmen des Auftrags wird überprüft, ob der Computer die Voraussetzungen erfüllt. Anschließend wird die [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx)-Methode aufgerufen, mit der die Replikation zu Azure mit den von Ihnen konfigurierten Einstellungen durchgeführt wird. Im Rahmen des Auftrags **Schutz aktivieren** wird auch die [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx)-Methode aufgerufen, um eine vollständige VM-Replikation zu initialisieren.
2. **Anfängliche Replikation**: Es wird eine Momentaufnahme des virtuellen Computers erstellt, und die virtuellen Festplatten werden einzeln nacheinander repliziert, bis sie alle zu Azure oder in das sekundäre Datencenter kopiert werden. Wie lange dieser Vorgang dauert, richtet sich nach der VM-Größe, der Netzwerkbandbreite und der Methode für die anfängliche Replikation. Falls es während der anfänglichen Replikation zu Festplattenänderungen kommt, werden diese Änderungen mit dem Replication Tracker für Hyper-V-Replikate in Form von Hyper-V-Replikationsprotokollen (.hrl) nachverfolgt, die sich in demselben Ordner wie die Festplatten befinden. Jeder Datenträger verfügt über eine zugeordnete HRL-Datei, die an den sekundären Speicher gesendet wird. Beachten Sie, dass die Momentaufnahme- und Protokolldateien Festplattenressourcen belegen, während die Replikation durchgeführt wird. Nach Abschluss der anfänglichen Replikation wird die VM-Momentaufnahme gelöscht, und die Festplatten-Deltaänderungen im Protokoll werden synchronisiert und zusammengeführt.
3. **Fertigstellen des Schutzes**: Nachdem die anfängliche Replikation abgeschlossen ist, werden mit dem Auftrag zum **Fertigstellen des Schutzes** das Netzwerk und andere Einstellungen nach der Replikation konfiguriert, damit der virtuelle Computer geschützt ist. Wenn Sie eine Replikation zu Azure durchführen, müssen Sie die Einstellungen für den virtuellen Computer unter Umständen so optimieren, dass er bereit für das Failover ist. An diesem Punkt können Sie ein Testfailover durchführen, um zu überprüfen, ob alles wie erwartet funktioniert.
4. **Replikation**: Nach der ersten Replikation beginnt die Deltasynchronisierung gemäß den Replikationseinstellungen.
    - **Replikationsfehler**: Wenn die Deltareplikation fehlschlägt und eine vollständige Replikation einen hohen Aufwand in Bezug auf die Bandbreite oder Dauer bedeuten würde, wird eine Neusynchronisierung durchgeführt. Wenn die HRL-Dateien beispielsweise 50% des Festplattenspeichers füllen, wird die VM für die Neusynchronisierung gekennzeichnet. Bei der Neusynchronisierung wird die Menge der gesendeten Daten verringert, indem Prüfsummen für die virtuellen Quell- und Zielmaschinen berechnet werden und nur das Delta gesendet wird. Nach Abschluss der Neusynchronisierung wird die Deltareplikation fortgesetzt. Standardmäßig ist die Neusynchronisierung so geplant, dass sie automatisch außerhalb der Geschäftszeiten durchgeführt wird, aber Sie können eine virtuelle Maschine auch manuell neu synchronisieren.
    - **Replikationsfehler**: Wenn ein Replikationsfehler auftritt, wird die integrierte Wiederholungsfunktion verwendet. Bei Auftreten eines Fehlers, für den keine Wiederherstellung möglich ist, z.B. ein Authentifizierungs- oder Autorisierungsfehler oder ein Replikatcomputer in einem ungültigen Zustand, wird nicht versucht, den Vorgang zu wiederholen. Bei einem Fehler, bei dem eine Wiederherstellung möglich ist, z.B. einem Netzwerkfehler oder einer geringen Menge an verbleibendem Festplattenspeicher/Arbeitsspeicher, wird eine Wiederholung mit zunehmenden Intervallen durchgeführt (1, 2, 4, 8, 10 und dann alle 30 Minuten).
4. **Geplante/ungeplante Failover**: Sie können geplante oder ungeplante Failover je nach Bedarf durchführen. Wenn Sie ein geplantes Failover durchführen, werden die Quell-VMs heruntergefahren, um sicherzustellen, dass kein Datenverlust auftritt. Nach der Erstellung der Replikat-VMs werden diese in den Zustand „Commit ausstehend“ versetzt. Sie müssen den Commit-Vorgang durchführen, um das Failover abzuschließen, es sei denn, Sie verwenden SAN. In diesem Fall wird der Commit-Vorgang automatisch durchgeführt. Nachdem der primäre Standort betriebsbereit ist, kann ein Failback durchgeführt werden. Wenn Sie zu Azure repliziert haben, erfolgt die umgekehrte Replikation automatisch. Starten Sie andernfalls manuell eine umgekehrte Replikation.


![Workflow](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Nächste Schritte

[Vorbereiten der Bereitstellung](site-recovery-best-practices.md)



<!--HONumber=Oct16_HO2-->


