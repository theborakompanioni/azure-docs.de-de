<properties
	pageTitle="Vorbereiten der Site Recovery-Bereitstellung | Microsoft Azure"
	description="Dieser Artikel beschreibt, wie Sie die Bereitstellung der Replikation mit Azure Site Recovery vorbereiten."
	services="site-recovery"
	documentationCenter=""
	authors="rayne-wiselman"
	manager="jwhit"
	editor="tysonn"/>

<tags
	ms.service="site-recovery"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="storage-backup-recovery"
	ms.date="03/29/2016"
	ms.author="raynew"/>

# Vorbereiten der Azure Site Recovery-Bereitstellung

Lesen Sie diesen Artikel, um eine allgemeine Übersicht über die Bereitstellungsanforderungen für jedes vom Azure Site Recovery-Dienst unterstützte Datenreplikationsszenario zu erhalten. Lesen Sie zunächst die allgemeinen Anforderungen für jedes Szenario, und folgen Sie dann den Links zu speziellen Bereitstellungsdetails im Abschnitt „Voraussetzungen“ der einzelnen Bereitstellungsartikel.

Nach der Lektüre dieses Artikels können Sie Kommentare oder Fragen am Ende des Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## Übersicht

Organisationen benötigen eine Strategie für die Bereiche Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR), mit der bestimmt wird, wie Apps, Workloads und Daten bei geplanten und ungeplanten Ausfallzeiten verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können. Bei Ihrer BCDR-Strategie geht es hauptsächlich um Lösungen, bei denen die Unternehmensdaten sicher sind und wiederhergestellt werden können und Workloads auch nach dem Eintreten eines Notfalls ständig verfügbar sind.

Site Recovery ist ein Azure-Dienst, der einen Beitrag zu Ihrer BCDR-Strategie leistet, indem die Replikation von lokalen physischen Servern und virtuellen Maschinen in die Cloud (Azure) oder in ein sekundäres Rechenzentrum orchestriert wird. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Site Recovery kann in verschiedenen Szenarien verwendet werden und für den Schutz mehrerer Workloads sorgen. Weitere Informationen finden Sie unter [Was ist Azure Site Recovery?](site-recovery-overview.md).


## Anforderungen für das Replizieren von virtuellen Hyper-V-Computern

**Komponente** | **Replizieren in Azure (mit VMM)** | **Replizieren in Azure (ohne VMM)** | **Replizieren an einem sekundären Standort (mit VMM)**
---|---|---|---
**VMM** | Mindestens einen VMM-Server, der unter System Center 2012 R2 ausgeführt wird. Der VMM-Server muss mindestens eine Cloud mit einer oder mehreren VMM-Hostgruppen enthalten. | Nicht zutreffend | In System Center 2012 R2 wird mindestens ein VMM-Server ausgeführt. Es wird ein VMM-Server an jedem Standort empfohlen. Der VMM-Server muss mindestens eine Cloud mit einer oder mehreren VMM-Hostgruppen enthalten. Für die Clouds sollte das Hyper-V-Funktionsprofil eingestellt sein. 
**Hyper-V** | Mindestens ein Hyper-V-Hostserver im lokalen Rechenzentrum mit Windows Server 2012 R2 oder höher. Der Hyper-V-Server muss sich in einer Hostgruppe in einer VMM-Cloud befinden. | Mindestens ein Hyper-V-Server am Quell- und Zielstandort mit Windows Server 2012 R2 oder höher. | Mindestens ein Hyper-V-Server am Quell- und Zielstandort mit Windows Server 2012 oder höher mit den aktuellsten Updates. Der Hyper-V-Server muss sich in einer Hostgruppe in einer VMM-Cloud befinden.
**Virtuelle Computer** | Sie benötigen mindestens eine VM auf dem Hyper-V-Quellserver. VMs, für die eine Replikation in Azure ausgeführt wird, müssen die [Anforderungen für virtuelle Azure-Computer](#azure-virtual-machine-requirements) erfüllen. | Mindestens eine VM auf dem Hyper-V-Quellserver. VMs, für die eine Replikation in Azure ausgeführt wird, müssen die [Anforderungen für virtuelle Azure-Computer](#azure-virtual-machine-requirements) erfüllen. | Mindestens eine VM in der VMM-Quellcloud
**Azure-Konto** | Sie benötigen ein [Azure](https://azure.microsoft.com/)-Konto und ein Azure-Abonnement. | Nicht zutreffend | Sie benötigen ein [Azure](https://azure.microsoft.com/)-Konto und ein Azure-Abonnement.
**Azure-Speicher** | Sie benötigen ein [Azure-Speicherkonto](../storage/storage-redundancy.md#geo-redundant-storage), um replizierte Daten zu speichern. Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden bei einem Failover hochgefahren. | Nicht zutreffend | Sie benötigen ein [Azure-Speicherkonto](../storage/storage-redundancy.md#geo-redundant-storage), um replizierte Daten zu speichern. Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden bei einem Failover hochgefahren.
**Anbieter/Agents** | Während der Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf VMM-Servern und den Azure Recovery Services-Agent auf Hyper-V-Hostservern. Der Anbieter kommuniziert mit Azure Site Recovery. Der Agent verarbeitet die Datenreplikation zwischen Quell- und Zielservern mit Hyper-V. Auf VMs wird nichts installiert. | Während der Bereitstellung installieren Sie den Azure Site Recovery-Anbieter und den Azure Recovery Services-Agent auf einem Hyper-V-Hostserver oder -Cluster. Auf VMs wird nichts installiert. | Während der Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf VMM-Servern für die Kommunikation mit Azure Site Recovery. Replikation zwischen Quell- und Zielservern mit Hyper-V über LAN/VPN.
**Anbieter-/Agent-Konnektivität** | Wenn der Anbieter über einen Proxy eine Verbindung mit dem Site Recovery-Dienst herstellt, müssen Sie sicherstellen, dass der Proxy auf die Site Recovery-URLs zugreifen kann. | Wenn der Anbieter über einen Proxy eine Verbindung mit Site Recovery herstellt, müssen Sie sicherstellen, dass der Proxy auf die Site Recovery-URLs zugreifen kann. | Wenn der Anbieter über einen Proxy eine Verbindung mit Site Recovery herstellt, müssen Sie sicherstellen, dass der Proxy auf die Site Recovery-URLs zugreifen kann.
**Internetkonnektivität** | Über den VMM-Server und die Hyper-V-Hosts. | Über die Hyper-V-Hosts. | Nur auf dem VMM-Server.
**Netzwerkzuordnung** | Richten Sie eine Netzwerkzuordnung ein, damit alle Computer, die im gleichen Azure-Netzwerk ein Failover ausführen, eine Verbindung miteinander herstellen können, und zwar unabhängig vom Wiederherstellungsplan, zu dem sie gehören. Wenn ein Netzwerkgateway im Azure-Zielnetzwerk vorhanden ist, können die virtuellen Computer zudem eine Verbindung mit den lokalen virtuellen Computern herstellen. Wenn Sie keine Netzwerkzuordnung einrichten, können nur Computer Verbindungen miteinander herstellen, für die das Failover in demselben Wiederherstellungsplan festgelegt ist. | Nicht zutreffend | Richten Sie eine Netzwerkzuordnung ein, sodass virtuelle Computer nach einem Failover eine Verbindung mit den entsprechenden Netzwerken herstellen können und replizierte virtuelle Computer optimal auf Hyper-V-Zielhostservern platziert werden. Wenn Sie die Netzwerkzuordnung nicht konfigurieren, werden für replizierte Maschinen nach einem Failover keine Verbindungen mit einem VM-Netzwerk hergestellt.
**Speicherzuordnung** | Nicht zutreffend | Nicht zutreffend | Optional können Sie mit der Speicherzuordnung sicherstellen, dass virtuelle Computer nach dem Failover optimal mit dem Speicher verbunden sind (die replizierte VM wird standardmäßig am auf dem Hyper-V-Zielserver angegebenen Speicherort gespeichert).
**SAN-Replikation** | Nicht zutreffend | Nicht zutreffend | Wenn Sie zwischen zwei lokalen VMM-Standorten eine SAN-Replikation ausführen möchten, können Sie Ihre vorhandene SAN-Umgebung dazu nutzen. Sehen Sie sich die [unterstützten SAN-Arrays](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) an.
**Weitere Informationen** | [Ausführliche Bereitstellungsvoraussetzungen](site-recovery-vmm-to-azure.md#before-you-start) | [Ausführliche Bereitstellungsvoraussetzungen](site-recovery-hyper-v-site-to-azure.md#before-you-start#before-you-start) | [Ausführliche Bereitstellungsvoraussetzungen](site-recovery-vmm-to-vmm.md#before-you-start)




## Anforderungen für die Replikation von VMware-VMs und physischen Servern

In der Tabelle werden die Anforderungen für die Replikation von VMware-VMs und physischen Windows-/Linux-Servern in Azure und an sekundären Standorten zusammengefasst.

>[AZURE.NOTE] Sie können VMware-VMs und physische Server mit einem [erweiterten](site-recovery-vmware-to-azure-classic.md) Bereitstellungsmodell in Azure replizieren oder ein [Legacymodell](site-recovery-vmware-to-azure-classic-legacy.md) nutzen, das bei älteren Bereitstellungen verwendet wurde. Die folgende Tabelle enthält die Bereitstellungsanforderungen für das erweiterte Modell.

**Komponente** | **Replizieren in Azure (erweitert)** | **Replizieren an einem sekundären Standort**
---|---|---
**Lokaler primärer Standort** | Sie installieren einen Verwaltungsserver, auf dem alle Site Recovery-Komponenten (Konfiguration, Prozess, Masterziel) ausgeführt werden. | Sie installieren einen Prozessserver zum Zwischenspeichern, Komprimieren und Verschlüsseln von replizierten Daten, bevor diese an den sekundären Standort gesendet werden. Sie können zusätzliche Prozessserver für den Lastenausgleich oder für Fehlertoleranz installieren. 
**Lokaler sekundärer Standort** | Nicht zutreffend | Sie installieren einen einzelnen Konfigurationsserver, der zum Konfigurieren, Verwalten und Überwachen der Bereitstellung verwendet wird.<br/><br>Es wird empfohlen, einen vContinuum-Server zu installieren, um die Serververwaltung zu vereinfachen.<br/><br/>Sie müssen den Masterzielserver als virtuellen Computer einrichten, der auf dem sekundären vSphere-Server ausgeführt wird. 
**VMware vCenter/ESXi** | Für die Replikation von VMware-VMs (oder das Failback physischer Server) an Ihrem primären Standort benötigen Sie einen vSphere ESX/ESXi-Host am primären Standort. Zum Verwalten der ESXi-Hosts wird außerdem ein vCenter-Server empfohlen. | Am primären und sekundären Standort benötigen Sie einen oder mehrere VMware ESXi-Hosts (sowie optional einen vCenter-Server). 
**Failback** | Sie benötigen eine VMware-Umgebung, um ein Failback von Azure auszuführen. Dies gilt auch für die Replikation von physischen Servern.<br/><br/>Sie müssen einen Prozessserver als virtuellen Azure-Computer einrichten. <br/><br/>Der Konfigurationsserver fungiert als Masterzielserver, wenn jedoch Failbacks bei großen Datenverkehrsvolumen ausgeführt werden, sollten Sie möglicherweise einen zusätzlichen lokalen Masterzielserver einrichten. [Weitere Informationen](site-recovery-failback-azure-to-vmware-classic.md)| Failbacks vom sekundären zum primären Standort sind nur mit VMware möglich, selbst wenn das Failover bei einem physischen Computer ausgeführt wurde. Für das Failback müssen Sie einen Masterzielserver als einen virtuellen Computer auf dem primären vSphere-Server einrichten.
**Azure-Konto** | Sie benötigen ein [Azure](https://azure.microsoft.com/)-Konto und ein Azure-Abonnement. | Nicht zutreffend
**Azure-Speicher** | Sie benötigen ein [Azure-Speicherkonto](../storage/storage-redundancy.md#geo-redundant-storage), um replizierte Daten zu speichern. Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden bei einem Failover hochgefahren. | Nicht zutreffend
**Azure Virtual Network** | Sie benötigen ein virtuelles Azure-Netzwerk, mit dem die virtuellen Azure-Computer eine Verbindung herstellen, wenn ein Failover stattfindet. Sie benötigen außerdem eine VPN-Verbindung (oder Azure ExpressRoute) zwischen dem Azure-Netzwerk und dem lokalen Standort, um nach dem Failover ein Failback auszuführen. | Nicht zutreffend
**Geschützte Computer** | Mindestens ein virtueller VMware-Computer oder ein physischer Windows-/Linux-Server. Während der Bereitstellung wird der Mobilitätsdienst auf jedem Computer installiert, den Sie replizieren möchten. | Mindestens ein virtueller VMware-Computer oder ein physischer Windows-/Linux-Server. Während der Bereitstellung wird der vereinheitlichte Agent auf jedem Computer installiert, den Sie replizieren möchten.
**Konnektivität** | Wenn der Verwaltungsserver über einen Proxy eine Verbindung mit Site Recovery herstellt, müssen Sie sicherstellen, dass der Proxyserver eine Verbindung mit spezifischen URLs herstellen kann. | Der Konfigurationsserver benötigt Zugriff auf das Internet.
**Weitere Informationen** | [Ausführliche Bereitstellungsvoraussetzungen](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment). | Laden Sie das [InMage Scout-Benutzerhandbuch](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf)herunter.


## Anforderungen für virtuelle Azure-Maschinen

Sie können Site Recovery zum Replizieren virtueller Maschinen und physischer Server bereitstellen, auf denen ein von Azure unterstütztes Betriebssystem ausgeführt wird. Dies gilt für die meisten Versionen von Windows und Linux. Sie müssen sicherstellen, dass lokale virtuelle Maschinen, die Sie schützen möchten, den Azure-Anforderungen entsprechen.


**Feature** | **Anforderungen** | **Details**
---|---|---
Hyper-V-Host | Es sollte Windows Server 2012 R2 ausgeführt werden. | Bei der Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn das Betriebssystem nicht unterstützt wird.
VMware-Hypervisor | Unterstütztes Betriebssystem | [Überprüfen der Anforderungen](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
Gastbetriebssystem | Replikation von Hyper-V zu Azure: Site Recovery unterstützt alle Betriebssysteme, die [von Azure unterstützt werden](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> Replikation von VMware und physischen Servern: Lesen Sie die [Voraussetzungen](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) für Windows und Linux. | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Architektur des Gastbetriebssystems | 64 Bit | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Größe des Betriebssystem-Datenträgers | Bis zu 1.023 GB | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Anzahl von Betriebssystem-Datenträgern | 1 | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Anzahl von Datenträgern für Daten | Maximal 16 (der Höchstwert ist eine Funktion der Größe des erstellten virtuellen Computers: 16 = XL) | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Größe des VHD-Datenträgers | Maximal 1.023 GB | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Netzwerkadapter | Es werden mehrere Adapter unterstützt. |
Statische IP-Adresse | Unterstützt | Wenn für den primären virtuellen Computer eine statische IP-Adresse verwendet wird, können Sie die statische IP-Adresse für den virtuellen Computer angeben, der in Azure erstellt wird. Beachten Sie, dass statische IP-Adressen für einen virtuellen Linux-Computer unter Hyper-V nicht unterstützt werden. 
iSCSI-Datenträger | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Freigegebene VHD | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Fiber-Channel-Datenträger | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Festplattenformat| VHD <br/><br/> VHDX | Obwohl VHDX in Azure derzeit nicht unterstützt wird, führt Site Recovery automatisch die Konvertierung von VHDX in VHD durch, wenn Sie das Failover auf Azure anstoßen. Wenn Sie das Failover in den lokalen Speicher durchführen, wird für die virtuellen Computer weiterhin das VHDX-Format verwendet.
Name des virtuellen Computers| Zwischen 1 und 63 Zeichen. Ist auf Buchstaben, Zahlen und Bindestriche beschränkt. Muss mit einem Buchstaben oder einer Zahl beginnen und enden. | Aktualisieren Sie den Wert in den Eigenschaften für virtuelle Computer in Site Recovery.
Typ des virtuellen Computers | <p>Generation 1</p> <p>Generation 2 – Windows</p> | Virtuelle Maschine der 2. Generation mit Betriebssystem-Datenträgertyp Basisfestplatte enthält 1 oder 2 Datenvolumes mit dem Datenträgerformat VHDX, das maximal 300 GB unterstützt. Virtuelle Computer der 2. Generation mit Linux werden nicht unterstützt. [Weitere Informationen](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)



## Optimieren der Bereitstellung

Mit den folgenden Tipps können Sie Ihre Bereitstellung optimieren und skalieren.

- **Volumegröße des Betriebssystems**: Wenn Sie einen virtuellen Computer zu Azure replizieren, muss das Volume des Betriebssystems kleiner als 1 TB sein. Falls Sie mehr als ein Volume nutzen, können Sie diese manuell auf einen anderen Datenträger verschieben, bevor Sie mit der Bereitstellung beginnen.
- **Größe des Datenträgers für Daten**: Wenn Sie zu Azure replizieren, können Sie auf einem virtuellen Computer bis zu 32 Datenträger für Daten nutzen, die jeweils maximal 1 TB groß sind. Für einen virtuellen Computer mit ~32 TB können Sie die Replikation und das Failover effektiv durchführen.
- **Beschränkungen des Wiederherstellungsplans**: Für Site Recovery ist eine Skalierung auf Tausende von virtuellen Computern möglich. Wiederherstellungspläne wurden als Modell für Anwendungen entwickelt, für die das Failover zusammen erfolgen soll. Die Anzahl von Computern in einem Wiederherstellungsplan wird daher auf 50 beschränkt.
- **Beschränkungen für Azure-Dienste**: Jedes Azure-Abonnement verfügt über eine Reihe von standardmäßigen Beschränkungen für Kerne, Clouddienste usw. Wir empfehlen Ihnen, ein Testfailover auszuführen, um die Verfügbarkeit von Ressourcen in Ihrem Abonnement zu überprüfen. Sie können diese Beschränkungen über den Azure-Support ändern.
- **Kapazitätsplanung**: Erfahren Sie mehr über die [Kapazitätsplanung](site-recovery-capacity-planner.md) für Site Recovery.
- **Replikationsbandbreite**: Beachten Sie Folgendes, falls die Replikationsbandbreite knapp ist:
	- **ExpressRoute**: Für Site Recovery können Azure ExpressRoute und WAN-Optimierer wie Riverbed verwendet werden. [Erfahren Sie mehr](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) zu ExpressRoute.
	- **Replikationsdatenverkehr**: Site Recovery führt eine intelligente anfängliche Replikation durch, bei der nur Datenblöcke verwendet werden, und nicht die gesamte VHD. Während der laufenden Replikation werden nur Änderungen repliziert.
	- **Netzwerkdatenverkehr**: Sie können Netzwerkdatenverkehr steuern, der für die Replikation verwendet wird, indem Sie [Windows-Dienstqualität](https://technet.microsoft.com/library/hh967468.aspx) mit einer Richtlinie nutzen, die auf der IP-Zieladresse und dem Port basiert. Falls Sie die Replikation zu Azure Site Recovery mit dem Azure Backup-Agent durchführen, können Sie die Drosselung für den Agent konfigurieren. [Weitere Informationen](https://support.microsoft.com/kb/3056159)
- **RTO**: Wenn Sie die Recovery Time Objective (RTO) messen möchten, die mit Site Recovery zu erwarten ist, sollten Sie ein Testfailover ausführen und die Site Recovery-Aufträge anzeigen, um zu analysieren, wie lange die Durchführung der Vorgänge dauert. Falls Sie das Failover zu Azure nutzen, erzielen Sie den besten RTO-Wert, wenn Sie alle manuellen Aktionen automatisieren. Integrieren Sie hierfür die Azure-Automatisierung und Wiederherstellungspläne.
- **RPO**: Site Recovery unterstützt beim Replizieren zu Azure eine nahezu synchrone Recovery Point Objective (RPO). So wird für eine ausreichende Bandbreite zwischen Ihrem Rechenzentrum und Azure gesorgt.





## Nächste Schritte

Nachdem Sie die allgemeinen Bereitstellungsanforderungen kennengelernt und miteinander verglichen haben, können Sie die ausführlichen Voraussetzungen lesen und mit der Bereitstellung der einzelnen Szenarien beginnen.

- [Repliziert virtuelle VMware-Computer zu Azure](site-recovery-vmware-to-azure-classic.md)
- [Replizieren physischer Server in Azure](site-recovery-vmware-to-azure-classic.md)
- [Replizieren von Hyper-V-Servern in VMM-Clouds in Azure](site-recovery-vmm-to-azure.md)
- [Replizieren virtueller Hyper-V-Maschinen (ohne VMM) in Azure](site-recovery-hyper-v-site-to-azure.md)
- [Replizieren von Hyper-V-VMs an einem sekundären Standort](site-recovery-vmm-to-vmm.md)
- [Replizieren von Hyper-V-VMs an einem sekundären Standort mit SAN](site-recovery-vmm-san.md)
- [Replizieren von Hyper-V-VMs mit einem einzelnen VMM-Server](site-recovery-single-vmm.md)

<!---HONumber=AcomDC_0406_2016-->