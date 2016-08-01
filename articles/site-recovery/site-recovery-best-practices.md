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
	ms.date="07/06/2016"
	ms.author="raynew"/>

# Vorbereiten der Azure Site Recovery-Bereitstellung

Lesen Sie diesen Artikel, um eine allgemeine Übersicht über die Bereitstellungsanforderungen für jedes vom Azure Site Recovery-Dienst unterstützte Datenreplikationsszenario zu erhalten. Lesen Sie zunächst die allgemeinen Anforderungen für jedes Szenario, und folgen Sie dann den Links zu speziellen Bereitstellungsdetails für jede Bereitstellung.

Nach der Lektüre dieses Artikels können Sie Kommentare oder Fragen am Ende des Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## Übersicht

Organisationen benötigen eine Strategie für die Geschäftskontinuität und Notfallwiederherstellung (BCDR), mit der bestimmt wird, wie Apps, Workloads und Daten bei geplanten und ungeplanten Ausfällen verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können. Bei Ihrer BCDR-Strategie sollten Sie darauf achten, dass Unternehmensdaten geschützt sind und wiederhergestellt werden können. Stellen Sie außerdem sicher, dass Workloads auch nach dem Eintreten eines Notfalls ständig verfügbar sind.

Site Recovery ist ein Azure-Dienst, der einen Beitrag zu Ihrer BCDR-Strategie leistet, indem die Replikation von lokalen physischen Servern und virtuellen Maschinen in die Cloud (Azure) oder in ein sekundäres Rechenzentrum orchestriert wird. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Weitere Informationen finden Sie unter [Was ist Site Recovery?](site-recovery-overview.md).

## Wählen Sie Ihr Bereitstellungsmodell

Azure verfügt über zwei unterschiedliche [Bereitstellungsmodelle](../resource-manager-deployment-model.md) zum Erstellen und Verwenden von Ressourcen: das Azure Resource Manager-Modell und das klassische Dienstverwaltungsmodell. Azure verfügt auch über zwei Portale: das [klassische Azure-Portal](https://manage.windowsazure.com/) mit Unterstützung des klassischen Bereitstellungsmodells und das [Azure-Portal](https://ms.portal.azure.com/) mit Unterstützung für beide Bereitstellungsmodelle.

Site Recovery ist sowohl im klassischen Portal als auch im Azure-Portal verfügbar. Im klassischen Azure-Portal können Sie Site Recovery mit dem klassischen Dienstverwaltungsmodell unterstützen. Im Azure-Portal können Sie das klassische Modell oder Resource Manager-Bereitstellungen unterstützen. Weitere Informationen zur Bereitstellung mit dem Azure-Portal finden Sie [hier](site-recovery-overview.md#site-recovery-in-the-azure-portal).

Wenn Sie ein Bereitstellungsmodell wählen, beachten Sie bitte Folgendes:

- Sie sollten soweit möglich das [Azure-Portal](https://portal.azure.com) und das Resource Manager-Modell verwenden.
- Site Recovery ermöglicht eine einfachere und intuitivere Einstiegserfahrung im Azure-Portal.
- Im Azure-Portal können Sie Computer sowohl zu klassischem als auch Resource Manager-Speicher in Azure replizieren. Darüber hinaus können Sie im Azure-Portal sowohl LRS- als auch GRS-Speicherkonten verwenden.
- Im Azure-Portal sind die Dienste Site Recovery und Backup in einem Recovery Services-Tresor kombiniert, sodass Sie BCDR-Dienste von einem einzigen Ort aus einrichten und verwalten können.
- Benutzer mit Azure-Abonnements, die über das Programm für Cloudlösungsanbieter (Cloud Solution Provider, CSP) bereitgestellt wurden, können Site Recovery-Vorgänge jetzt im Azure-Portal verwalten.
- Für das Replizieren virtueller VMware-Computer oder physischer Computer in Azure im Azure-Portal gibt es eine Reihe neuer Features, einschließlich der Unterstützung für Storage Premium und der Möglichkeit, bestimmte Datenträger von der Replikation auszuschließen.


## Wählen Sie Ihr Bereitstellungsszenario

**Bereitstellung** | **Details** | **Azure-Portal** | **Klassisches Portal** | **PowerShell**
---|---|---|---|---
**Replizieren von VMware-VMs in Azure** | Replizieren von VMware-VMs in Azure-Speicher | Im Azure-Portal kann ein Failover von VMs zu klassischem oder Resource Manager-Speicher durchgeführt werden.<br/><br/> Bereitstellung im [Azure-Portal](site-recovery-vmware-to-azure.md) bietet eine optimierte Bereitstellungserfahrung und eine Reihe vorteilhafter Features. | Im klassischen Azure-Portal können Sie eine Bereitstellung mit dem [erweiterten Modell](site-recovery-vmware-to-azure-classic.md) und ein Failover zum klassischen Azure-Speicher durchführen.<br/><br/> Es gibt auch ein Legacymodell, das nicht für neue Bereitstellungen verwendet werden darf. | PowerShell wird derzeit nicht unterstützt.
**Hyper-V-VMs in Azure** | Hyper-V-VMs nach Azure-Speicher. VMs können auf Hyper-V-Hosts in System Center VMM-Clouds oder ohne VMM verwaltet werden. | Im Azure-Portal können VMs ein Failover zu klassischem oder Resource Manager-Speicher durchführen.<br/><br/> Das Azure-Portal bietet eine optimierte Bereitstellungserfahrung. [Erfahren Sie mehr](site-recovery-vmm-to-azure.md) über das Replizieren von Hyper-V-VMs in VMM-Clouds. [Erfahren Sie mehr](site-recovery-hyper-v-site-to-azure.md) über das Replizieren von Hyper-V-VMs (ohne VMM).| Im klassischen Azure-Portal können Sie ein Failover von VMs zu klassischem Azure-Speicher durchführen. | PowerShell-Bereitstellung wird unterstützt.
**Physische Server nach Azure** | Replizieren physischer Windows-/Linux-Server nach Azure-Speicher | Im Azure-Portal können Server ein Failover zu klassischem oder Resource Manager-Speicher durchführen.<br/><br/> Bereitstellung im [Azure-Portal](site-recovery-vmware-to-azure.md) bietet eine optimierte Bereitstellungserfahrung und eine Reihe vorteilhafter Features. | Im klassischen Azure-Portal können Sie eine Bereitstellung mit dem [erweiterten Modell](site-recovery-vmware-to-azure-classic.md) und ein Failover zum klassischen Azure-Speicher durchführen.<br/><br/> Es gibt auch ein Legacymodell, das nicht für neue Bereitstellungen verwendet werden darf. | Die PowerShell-Bereitstellung wird derzeit nicht unterstützt.
* *VMware-VMs/physische Server an einem sekundären Standort* | Replizieren von VMware-VMs oder physischen Windows-/Linux-Servern an einem sekundären Standort. [Um mehr zu erfahren](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf), laden Sie das InMage Scout-Benutzerhandbuch herunter. | Nicht im Azure-Portal verfügbar. | Im klassischen Portal können Sie InMage Scout von einem Site Recovery-Tresor herunterladen. | Die PowerShell-Bereitstellung wird derzeit nicht unterstützt.
**Hyper-V-VMs an einem sekundären Standort** | Replizieren von Hyper-V-VMs in VMM-Clouds in einer sekundären Cloud | Im [Azure-Portal](site-recovery-vmm-to-vmm.md) können Sie Hyper-V-VMs in VMM-Clouds nur mit Hyper-V-Replikat an einem sekundären Standort replizieren. SAN wird derzeit nicht unterstützt. | Im klassischen Azure-Portal können Sie Hyper-V-VMs in VMM-Clouds mit [Hyper-V-Replikat](site-recovery-vmm-to-vmm-classic.md) oder [SAN-Replikation](site-recovery-vmm-san.md) an einem sekundären Standort replizieren. | PowerShell-Bereitstellung wird unterstützt.



## Überprüfen Sie, was Sie zur Bereitstellung benötigen

### Replikation zu Azure

**Anforderung** | **Details** 
---|---
**Azure-Konto** | Sie benötigen ein [Microsoft Azure](http://azure.microsoft.com/)-Konto.<br/><br/> Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung. [Erfahren Sie mehr](https://azure.microsoft.com/pricing/details/site-recovery/) über die Preise für Site Recovery.
**Azure-Speicher** | Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden erstellt, wenn ein Failover durchgeführt wird. Um in Azure zu replizieren, benötigen Sie ein [Azure-Speicherkonto](../storage/storage-introduction.md).<br/><br/>Wenn Sie Site Recovery im klassischen Portal bereitstellen, benötigen Sie ein oder mehrere [standardmäßige GRS-Speicherkonten](..storage/storage-redundancy.md#geo-redundant-storage).<br/><br/> Wenn Sie im Azure-Portal bereitstellen, können Sie GRS- oder LRS-Speicher verwenden.<br/><br/> Wenn Sie VMware-VMs oder physische Server im Azure-Portal replizieren, wird Storage Premium unterstützt. Beachten Sie, dass Sie bei Verwendung eines Storage Premium-Kontos auch ein Standardspeicherkonto zum Speichern von Replikationsprotokollen benötigen, in denen laufende Änderungen lokaler Daten erfasst werden. [Storage Premium](../storage/storage-premium-storage.md) wird normalerweise für virtuelle Computer verwendet, die eine konsistent hohe E/A-Leistung und geringe Wartezeit zum Hosten von E/A-intensiven Workloads erfordern.<br/><br/> Wenn Sie ein Premium-Konto zum Speichern replizierter Daten verwenden möchten, benötigen Sie außerdem ein Standardspeicherkonto zum Speichern von Replikationsprotokollen, in denen laufende Änderungen lokaler Daten erfasst werden.
**Azure-Netzwerk** | Zum Replizieren in Azure benötigen Sie ein Azure-Netzwerk, mit dem die Azure-VMs eine Verbindung herstellen, wenn sie nach einem Failover erstellt werden.<br/><br/> Wenn Sie im klassischen Portal bereitstellen, verwenden Sie ein klassisches Netzwerk. Wenn Sie im Azure-Portal bereitstellen, können Sie ein klassisches oder Resource Manager-Netzwerk verwenden.<br/><br/> Das Netzwerk muss sich in der gleichen Region wie der Tresor befinden.
**Netzwerkzuordnung (VMM zu Azure)** | Wenn Sie von VMM in Azure replizieren, stellt die [Netzwerkzuordnung](site-recovery-network-mapping.md) sicher, dass Azure-VMs nach einem Failover mit den richtigen Netzwerken verbunden werden.<br/><br/> Zum Einrichten der Netzwerkzuordnung müssen Sie die VM-Netzwerke im VMM-Portal konfigurieren.
**Lokal** | **VMware-VMs**: Sie benötigen einen lokalen Computer, auf dem Site Recovery-Komponenten ausgeführt werden, VMware vSphere-Hosts/vCenter-Server und VMs, die Sie replizieren möchten. [Weitere Informationen](site-recovery-vmware-to-azure.md#configuration-server-prerequisites)<br/><br/> **Physische Server**: Wenn Sie physische Server replizieren, benötigen Sie einen lokalen Computer, auf dem Site Recovery-Komponenten ausgeführt werden, und physische Server, die Sie replizieren möchten. [Weitere Informationen](site-recovery-vmware-to-azure.md#configuration-server-prerequisites) Wenn Sie ein [Failback](site-recovery-failback-azure-to-vmware.md) nach dem Failover zu Azure ausführen möchten, benötigen Sie eine VMware-Infrastruktur.<br/><br/> **Hyper-V-VMs**: Wenn Sie Hyper-V-VMs in VMM-Clouds replizieren möchten, benötigen Sie einen VMM-Server sowie Hyper-V-Hosts, auf denen sich VMs befinden, die Sie schützen möchten. [Weitere Informationen](site-recovery-vmm-to-azure.md#on-premises-prerequisites)<br/><br/> Wenn Sie Hyper-V-VMs ohne VMM replizieren möchten, benötigen Sie Hyper-V-Hosts, auf denen sich VMs befinden. [Weitere Informationen](site-recovery-hyper-v-site-to-azure.md#on-premises-prerequisites)
**Geschützte Computer** | Geschützte Computer, die in Azure repliziert werden sollen, müssen die unten beschriebenen [Voraussetzungen für Azure](#azure-virtual-machine-requirements) einhalten.


### Replikation an einem sekundären Standort

**Anforderung** | **Details** 
---|---
**Azure-Konto** | Sie benötigen ein [Microsoft Azure](http://azure.microsoft.com/)-Konto.<br/><br/> Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung. [Erfahren Sie mehr](https://azure.microsoft.com/pricing/details/site-recovery/) über die Preise für Site Recovery.
**Lokal** | **VMware-VMs**: Am primären Standort benötigen Sie einen Prozessserver, mit dem Replikationsdaten vor dem Senden an den sekundären Standort zwischengespeichert, komprimiert und verschlüsselt werden. Am sekundären Standort installieren Sie einen Konfigurationsserver zum Verwalten und Überwachen der Bereitstellung sowie einen Masterzielserver. Sie sollten außerdem einen vContinuum-Server zur einfacheren Verwaltung verwenden. Darüber hinaus benötigen Sie einen oder mehrere VMware vSphere-Hosts und optional einen vCenter-Server. [Erfahren Sie mehr über](site-recovery-vmware-to-vmware.md)<br/><br/> **Hyper-V-VMs in VMM-Clouds**: Sie müssen VMM-Server einrichten und Hyper-V-Hosts, die VMs enthalten, die Sie replizieren möchten. [Weitere Informationen](site-recovery-vmm-to-vmm.md#on-premises-prerequisites)
**Netzwerkzuordnung (VMM zu VMM)** | Wenn Sie von VMM zu VMM replizieren, richten Sie eine [Netzwerkzuordnung](site-recovery-network-mapping.md) ein, um sicherzustellen, dass Replikat-VMs nach einem Failover mit den entsprechenden Netzwerken verbunden und optimal auf Hyper-V-Hosts platziert werden. Zum Einrichten der Netzwerkzuordnung müssen Sie VM-Netzwerke auf Ihren VMM-Servern konfigurieren.
**Speicherzuordnung** | Wenn Sie von VMM zu VMM replizieren, können Sie die [Speicherzuordnung](site-recovery-storage-mapping.md) optional festlegen, um das Speicherziel für replizierte VMs anzugeben. Speicherzuordnung kann sowohl für Hyper-V-Replikat als auch SAN-Replikation eingerichtet werden.<br/><br/> Speicherzuordnung wird im Azure-Portal derzeit nicht unterstützt.


## Überprüfen des URL-Zugriffs

Stellen Sie sicher, dass die folgenden URLs von Servern aus zugänglich sind:

**URL** | **VMM zu VMM** | **VMM zu Azure** | **Hyper-V in Azure** | **VMware zu Azure**
---|---|---|---|---
*.accesscontrol.windows.net | Zulassen | Zulassen | Zulassen | Zulassen
*.backup.windowsazure.com | Nicht erforderlich | Zulassen | Zulassen | Zulassen
*.hypervrecoverymanager.windowsazure.com | Zulassen | Zulassen | Zulassen | Zulassen
*.store.core.windows.net | Zulassen | Zulassen | Zulassen | Zulassen
*.blob.core.windows.net | Nicht erforderlich | Zulassen | Zulassen | Zulassen
https://www.msftncsi.com/ncsi.txt | Zulassen | Zulassen | Zulassen | Zulassen
https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi | Nicht erforderlich | Nicht erforderlich | Nicht erforderlich | Zulassen

## Anforderungen für virtuelle Azure-Maschinen

Sie können Site Recovery zum Replizieren virtueller Maschinen und physischer Server bereitstellen, auf denen ein von Azure unterstütztes Betriebssystem ausgeführt wird. Dies gilt für die meisten Versionen von Windows und Linux. Sie müssen sicherstellen, dass lokale virtuelle Maschinen, die Sie schützen möchten, den Azure-Anforderungen entsprechen.


**Feature** | **Anforderungen** | **Details**
---|---|---
Hyper-V-Host | Es sollte Windows Server 2012 R2 ausgeführt werden. | Bei der Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn das Betriebssystem nicht unterstützt wird.
VMware-Hypervisor | Unterstütztes Betriebssystem | [Überprüfen der Anforderungen](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
Gastbetriebssystem | Replikation von Hyper-V in Azure: Site Recovery unterstützt alle Betriebssysteme, die [von Azure unterstützt werden](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> Replikation von VMware und physischen Servern: Lesen Sie die [Voraussetzungen](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) für Windows und Linux. | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Architektur des Gastbetriebssystems | 64 Bit | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Größe des Betriebssystem-Datenträgers | Bis zu 1.023 GB | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Anzahl von Betriebssystem-Datenträgern | 1 | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Anzahl von Datenträgern für Daten | Maximal 16 (der Höchstwert ist eine Funktion der Größe des erstellten virtuellen Computers: 16 = XL) | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Größe des VHD-Datenträgers | Maximal 1.023 GB | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Netzwerkadapter | Es werden mehrere Adapter unterstützt. |
Statische IP-Adresse | Unterstützt | Wenn für den primären virtuellen Computer eine statische IP-Adresse verwendet wird, können Sie die statische IP-Adresse für den virtuellen Computer angeben, der in Azure erstellt wird. Beachten Sie, dass statische IP-Adressen für einen virtuellen Linux-Computer unter Hyper-V nicht unterstützt werden.
iSCSI-Datenträger | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Freigegebene VHD | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Fiber-Channel-Datenträger | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Festplattenformat| VHD <br/><br/> VHDX | Obwohl VHDX in Azure derzeit nicht unterstützt wird, führt Site Recovery automatisch die Konvertierung von VHDX in VHD durch, wenn Sie das Failover auf Azure anstoßen. Wenn Sie das Failover in den lokalen Speicher durchführen, wird für die virtuellen Computer weiterhin das VHDX-Format verwendet.
Name des virtuellen Computers| Zwischen 1 und 63 Zeichen. Ist auf Buchstaben, Zahlen und Bindestriche beschränkt. Muss mit einem Buchstaben oder einer Zahl beginnen und enden. | Aktualisieren Sie den Wert in den Eigenschaften für virtuelle Computer in Site Recovery.
Typ des virtuellen Computers | <p>Generation 1</p> <p>Generation 2 – Windows</p> | Virtuelle Maschine der 2. Generation mit Betriebssystem-Datenträgertyp Basisfestplatte enthält 1 oder 2 Datenvolumes mit dem Datenträgerformat VHDX, das maximal 300 GB unterstützt. Virtuelle Computer der 2. Generation mit Linux werden nicht unterstützt. [Weitere Informationen](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)



## Optimieren der Bereitstellung

Mit den folgenden Tipps können Sie Ihre Bereitstellung optimieren und skalieren.

- **Volumegröße des Betriebssystems**: Wenn Sie einen virtuellen Computer zu Azure replizieren, muss das Volume des Betriebssystems kleiner als 1 TB sein. Falls Sie mehr als ein Volume nutzen, können Sie diese manuell auf einen anderen Datenträger verschieben, bevor Sie mit der Bereitstellung beginnen.
- **Größe des Datenträgers für Daten**: Wenn Sie zu Azure replizieren, können Sie auf einem virtuellen Computer bis zu 32 Datenträger für Daten nutzen, die jeweils maximal 1 TB groß sind. Für einen virtuellen Computer mit ~32 TB können Sie die Replikation und das Failover effektiv durchführen.
- **Beschränkungen des Wiederherstellungsplans**: Für Site Recovery ist eine Skalierung auf Tausende von virtuellen Computern möglich. Wiederherstellungspläne wurden als Modell für Anwendungen entwickelt, für die das Failover zusammen erfolgen soll. Die Anzahl von Computern in einem Wiederherstellungsplan wird daher auf 50 beschränkt.
- **Beschränkungen für Azure-Dienste**: Jedes Azure-Abonnement verfügt über eine Reihe von standardmäßigen Beschränkungen für Kerne, Clouddienste usw. Wir empfehlen Ihnen, ein Testfailover auszuführen, um die Verfügbarkeit von Ressourcen in Ihrem Abonnement zu überprüfen. Sie können diese Beschränkungen über den Azure-Support ändern.
- **Kapazitätsplanung**: Erfahren Sie mehr über die [Kapazitätsplanung](site-recovery-capacity-planner.md) für Site Recovery.
- **Replikationsbandbreite**: Beachten Sie Folgendes, falls die Replikationsbandbreite knapp ist:
	- **ExpressRoute**: Für Site Recovery können Azure ExpressRoute und WAN-Optimierer wie Riverbed verwendet werden. [Erfahren Sie mehr](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) zu ExpressRoute.
	- **Replikationsdatenverkehr**: Site Recovery führt eine intelligente anfängliche Replikation durch, bei der nur Datenblöcke verwendet werden, und nicht die gesamte VHD. Während der laufenden Replikation werden nur Änderungen repliziert.
	- **Netzwerkdatenverkehr**: Sie können Netzwerkdatenverkehr steuern, der für die Replikation verwendet wird, indem Sie [Windows-Dienstqualität](https://technet.microsoft.com/library/hh967468.aspx) mit einer Richtlinie nutzen, die auf der IP-Zieladresse und dem Port basiert. Falls Sie die Replikation in Azure Site Recovery mit dem Azure Backup-Agent durchführen, können Sie die Drosselung für diesen Agent konfigurieren. [Weitere Informationen](https://support.microsoft.com/kb/3056159)
- **RTO**: Um die Recovery Time Objective (RTO) zu messen, die mit Site Recovery zu erwarten ist, sollten Sie ein Testfailover ausführen und die Site Recovery-Aufträge anzeigen, um zu analysieren, wie lange die Durchführung der Vorgänge dauert. Falls Sie das Failover zu Azure nutzen, erzielen Sie den besten RTO-Wert, wenn Sie alle manuellen Aktionen automatisieren. Integrieren Sie hierfür die Azure-Automatisierung und Wiederherstellungspläne.
- **RPO**: Site Recovery unterstützt beim Replizieren zu Azure eine nahezu synchrone Recovery Point Objective (RPO). Dies setzt ausreichende Bandbreite zwischen Ihrem Rechenzentrum und Azure voraus.


##Dienst-URLs
Stellen Sie sicher, dass die folgenden URLs vom Server aus zugänglich sind:


**URLs** | **VMM zu VMM** | **VMM zu Azure** | **Hyper-V-Standort zu Azure** | **VMware zu Azure**
---|---|---|---|---
 *.accesscontrol.windows.net | Zugriff erforderlich | Zugriff erforderlich | Zugriff erforderlich | Zugriff erforderlich
 *.backup.windowsazure.com | | Zugriff erforderlich | Zugriff erforderlich | Zugriff erforderlich
 *.hypervrecoverymanager.windowsazure.com | Zugriff erforderlich | Zugriff erforderlich | Zugriff erforderlich | Zugriff erforderlich
 *.store.core.windows.net | Zugriff erforderlich | Zugriff erforderlich | Zugriff erforderlich | Zugriff erforderlich
 *.blob.core.windows.net | | Zugriff erforderlich | Zugriff erforderlich | Zugriff erforderlich
 https://www.msftncsi.com/ncsi.txt | Zugriff erforderlich | Zugriff erforderlich | Zugriff erforderlich | Zugriff erforderlich
 https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi | | | | Zugriff erforderlich


## Nächste Schritte

Nachdem Sie die allgemeinen Bereitstellungsanforderungen kennengelernt und miteinander verglichen haben, können Sie die ausführlichen Voraussetzungen lesen und mit der Bereitstellung der einzelnen Szenarien beginnen.

- [Repliziert virtuelle VMware-Computer zu Azure](site-recovery-vmware-to-azure-classic.md)
- [Replizieren physischer Server in Azure](site-recovery-vmware-to-azure-classic.md)
- [Replizieren von Hyper-V-Servern in VMM-Clouds in Azure](site-recovery-vmm-to-azure.md)
- [Replizieren virtueller Hyper-V-Maschinen (ohne VMM) in Azure](site-recovery-hyper-v-site-to-azure.md)
- [Replizieren von Hyper-V-VMs an einem sekundären Standort](site-recovery-vmm-to-vmm.md)
- [Replizieren von Hyper-V-VMs an einem sekundären Standort mit SAN](site-recovery-vmm-san.md)
- [Replizieren von Hyper-V-VMs mit einem einzelnen VMM-Server](site-recovery-single-vmm.md)

<!---HONumber=AcomDC_0720_2016-->