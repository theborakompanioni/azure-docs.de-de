<properties
	pageTitle="Replizieren von virtuellen VMware-Computern und physischen Servern nach Azure mithilfe von Azure Site Recovery im Azure-Portal | Microsoft Azure"
	description="Beschreibt, wie Azure Site Recovery bereitgestellt wird, um die Replikation, das Failover und die Wiederherstellung von lokalen virtuellen VMware-Computern und physischen Windows-/Linux-Servern auf Azure mithilfe des Azure-Portals zu orchestrieren."
	services="site-recovery"
	documentationCenter=""
	authors="rayne-wiselman"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="site-recovery"
	ms.workload="backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/12/2016"
	ms.author="raynew"/>

# Replizieren von virtuellen VMware-Computern und physischen Computern nach Azure mithilfe von Azure Site Recovery mit dem Azure-Portal

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmware-to-azure.md)
- [Azure – klassisch](site-recovery-vmware-to-azure-classic.md)
- [Azure – klassisch (Vorgängerversion)](site-recovery-vmware-to-azure-classic-legacy.md)

Willkommen bei Azure Site Recovery! Verwenden Sie die Informationen in diesem Artikel, wenn Sie lokale virtuelle VMware-Computer oder physische Windows/Linux-Server mithilfe von Azure Site Recovery im Azure-Portal nach Azure replizieren möchten.

> [AZURE.NOTE] Azure verfügt über zwei unterschiedliche [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen: die Azure Resource Manager-Bereitstellung (ARM) und die klassische Bereitstellung. Azure verfügt auch über zwei Portale: das klassische Azure-Portal mit Unterstützung des klassischen Bereitstellungsmodells und das Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle.

Site Recovery im Azure-Portal verfügt über eine Reihe neuer Features:

- Die Dienste Azure Backup und Azure Site Recovery wurden in einem Recovery Services-Tresor kombiniert, damit Sie die Geschäftskontinuität und Notfallwiederherstellung (business continuity and disaster recovery, BCDR) zentral einrichten und verwalten können. Im einheitlichen Dashboard können Sie Vorgänge über Ihre lokalen Standorte und die öffentliche Azure-Cloud hinweg überwachen und verwalten.
- Benutzer mit Azure-Abonnements, die über das Programm für Cloudlösungsanbieter (Cloud Solution Provider, CSP) bereitgestellt wurden, können Site Recovery-Vorgänge jetzt im Azure-Portal verwalten.
- Mit Site Recovery im Azure-Portal können Computer in ARM-Speicherkonten repliziert werden. Bei einem Failover erstellt Site Recovery ARM-basierte VMs in Azure.
- Für Site Recovery wird die Replikation in klassischen Speicherkonten weiterhin unterstützt. Beim Failover erstellt Site Recovery VMs mit dem klassischen Modell.

Nachdem Sie diesen Artikel gelesen haben, können Sie unten im Disqus-Kommentarbereich Ihre Kommentare eingeben. Im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) können Sie technische Fragen stellen.

## Übersicht

Organisationen benötigen eine Strategie für die Geschäftskontinuität und Notfallwiederherstellung (BCDR), mit der bestimmt wird, wie Apps, Workloads und Daten bei geplanten und ungeplanten Ausfällen verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können. Bei Ihrer BCDR-Strategie sollten Sie darauf achten, dass Unternehmensdaten geschützt sind und wiederhergestellt werden können. Stellen Sie außerdem sicher, dass Workloads auch nach dem Eintreten eines Notfalls ständig verfügbar sind.

Site Recovery ist ein Azure-Dienst, der einen Beitrag zu Ihrer BCDR-Strategie leistet, indem die Replikation von lokalen physischen Servern und virtuellen Maschinen in die Cloud (Azure) oder in ein sekundäres Rechenzentrum orchestriert wird. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Weitere Informationen finden Sie unter [Was ist Azure Site Recovery?](site-recovery-overview.md).

Dieser Artikel enthält alle Informationen, die Sie zum Replizieren von lokalen VMware-VMs und physischen Windows-/Linux-Servern nach Azure benötigen. Er enthält eine Übersicht über die Architektur, Informationen zur Planung und Bereitstellungsschritte zum Konfigurieren von Azure, lokalen Servern, Replikationseinstellungen und der Kapazitätsplanung. Nachdem Sie die Infrastruktur eingerichtet haben, können Sie die Replikation auf den zu schützenden Computern aktivieren und überprüfen, ob das Failover funktioniert.

## Vorteile für Unternehmen

- Site Recovery ermöglicht den externen Schutz von geschäftlichen Workloads und Anwendungen, die auf VMware-VMs und auf physischen Servern ausgeführt werden.
- Das Recovery Services-Portal ist ein zentraler Ort zum Einrichten, Verwalten und Überwachen von Replikation, Failover und Wiederherstellung.
- Site Recovery kann automatisch VMware-VMs ermitteln, die zu vSphere-Hosts hinzugefügt wurden.
- Sie können leicht Failover von Ihrer lokalen Infrastruktur auf Azure und Failbacks (Wiederherstellungen) von Azure auf VMware-VM-Server an Ihrem lokalen Standort durchführen.
- Sie können Multi-VM aktivieren und Replikationsgruppen erstellen, damit Anwendungsworkloads gleichzeitig repliziert werden, die auf mehrere Computer und Ebenen aufgeteilt sind. Alle Computer in einer Replikationsgruppe verfügen beim Failover über ausfallsichere und anwendungskonsistente Wiederherstellungspunkte. Beim Failover können Sie mehrere Computer in Wiederherstellungsplänen sammeln, damit Failover für Workloads von Anwendungen mit mehreren Ebenen gemeinsam erfolgen.

## Unterstützte Betriebssysteme

### Windows (nur 64 Bit)
- Windows Server 2008 R2 SP1+
- Windows Server 2012
- Windows Server 2012 R2

### Linux (nur 64 Bit)
- Red Hat Enterprise Linux 6.7, 7.1, 7.2
- CentOS 6.5, 6.6, 6.7, 7.0, 7.1, 7.2
- Oracle Enterprise Linux 6.4, 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird
- SUSE Linux Enterprise Server 11 SP3

## Szenarioarchitektur

Dies sind die Szenariokomponenten:

- **Konfigurationsserver**: Ein lokaler Computer, der die Kommunikation koordiniert und die Datenreplikations- und Wiederherstellungsvorgänge verwaltet. Auf diesem Computer führen Sie eine einzelne Setupdatei aus, um den Konfigurationsserver und diese zusätzlichen Komponenten zu installieren:
	- **Prozessserver**: Fungiert als Replikationsgateway. Er empfängt Replikationsdaten aus geschützten Quellcomputern, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage. Außerdem wickelt er die Pushinstallation des Mobilitätsdiensts auf geschützten Computern ab und führt die automatische Ermittlung von virtuellen VMware-Computern durch. Auf dem Konfigurationsserver ist der Standardprozessserver installiert. Sie können zusätzliche eigenständige Prozessserver bereitstellen, um Ihre Bereitstellung zu skalieren.
	- **Masterzielserver**: Verarbeitet die Replikationsdaten während des Failbacks von Azure.

- **Mobilitätsdienst**: Diese Komponente wird auf jedem Computer (VMware-VM oder physischer Server) bereitgestellt, den Sie nach Azure replizieren möchten. Er erfasst die Datenschreibvorgänge auf dem Computer und sendet sie an den Prozessserver.
- **Azure**: Sie müssen keine virtuellen Azure-Computer für die Replikation und das Failover auf Azure erstellen. Sie benötigen ein Azure-Abonnement, ein Azure-Speicherkonto zum Speichern replizierter Daten und ein virtuelles Azure-Netzwerk, damit Azure-VMs nach dem Failover mit einem Netzwerk verbunden sind. Das Speicherkonto und das Netzwerk müssen sich in derselben Region wie der Recovery Services-Tresor befinden.
- **Failback**: Wenn Sie nach einem Failover bereit sind, ein Failback von Azure auf den lokalen Standort auszuführen, müssen Sie eine Azure-VM als temporären Prozessserver erstellen. Nach Abschluss des Failbacks können Sie diese löschen. Für das Failback benötigen Sie auch eine VPN- (oder Azure ExpressRoute-) Verbindung zwischen dem lokalen Standort und dem Azure-Netzwerk, in dem sich Ihre Azure-VMs befinden. Wenn der Failback-Datenverkehr hoch ist, müssen Sie möglicherweise auch einen dedizierten Masterzielserver-Computer lokal einrichten. Für geringeren Datenverkehr kann der auf dem Konfigurationsserver ausgeführte Standard-Masterzielserver verwendet werden.


Die Grafik zeigt, wie diese Komponenten zusammenwirken.

![Architektur](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Abbildung 1: VMware/physisch zu Azure**

## Voraussetzungen für Azure

In Azure benötigen Sie für die Bereitstellung dieses Szenarios Folgendes:

**Voraussetzung** | **Details**
--- | ---
**Azure-Konto**| Sie benötigen ein [Microsoft Azure](http://azure.microsoft.com/)-Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung. Weitere Informationen zu den Preisen für Site Recovery erhalten Sie [hier](https://azure.microsoft.com/pricing/details/site-recovery/).
**Azure-Speicher** | Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden erstellt, wenn ein Failover durchgeführt wird. <br/><br/>Zum Speichern von Daten benötigen Sie ein Standard- oder Premium-Speicherkonto in derselben Region wie der Recovery Services-Tresor.<br/><br/>Sie können ein LRS- oder GRS-Speicherkonto verwenden. Wir empfehlen Ihnen die Verwendung von GRS, damit Resilienz für die Daten besteht, wenn es zu einem regionalen Ausfall kommt oder wenn die primäre Region nicht wiederhergestellt werden kann. [Weitere Informationen](../storage/storage-redundancy.md).<br/><br/> [Storage Premium](../storage/storage-premium-storage.md) wird normalerweise für virtuelle Computer verwendet, die eine konsistent hohe E/A-Leistung und geringe Latenz zum Hosten von E/A-intensiven Workloads erfordern.<br/><br/> Wenn Sie ein Premium-Konto zum Speichern replizierter Daten verwenden möchten, benötigen Sie außerdem ein Standardspeicherkonto zum Speichern von Replikationsprotokollen, in denen laufende Änderungen lokaler Daten erfasst werden.<br/><br/> Beachten Sie, dass im Azure-Portal erstellte Speicherkonten nicht über Ressourcengruppen hinweg verschoben werden können. Darüber hinaus wird der Schutz von Premium-Speicherkonten in den Regionen „Indien, Mitte“ und „Indien, Süden“ zurzeit nicht unterstützt.<br/><br/> [Erfahren Sie mehr](../storage/storage-introduction.md) über Azure Storage.
**Azure-Netzwerk** | Sie benötigen ein virtuelles Azure-Netzwerk, mit dem die virtuellen Azure-Computer eine Verbindung herstellen, wenn ein Failover stattfindet. Das virtuelle Azure-Netzwerk muss sich in derselben Region wie der Recovery Services-Tresor befinden.
**Failback von Azure** | Sie benötigen einen temporären Prozessserver, der als virtueller Azure-Computer eingerichtet ist. Sie können diesen erstellen, wenn Sie bereit für das Failback sind, und ihn nach Abschluss des Failbacks wieder löschen.<br/><br/> Für das Failback benötigen Sie außerdem eine VPN-Verbindung (oder Azure ExpressRoute) zwischen dem Azure-Netzwerk und dem lokalen Standort.

## Voraussetzungen für den Konfigurationsserver

Sie richten einen lokalen Computer als Konfigurationsserver ein.

**Voraussetzung** | **Details**
--- | ---
**Konfigurationsserver**| Sie benötigen einen lokalen physischen oder virtuellen Computer, auf dem Windows Server 2012 R2 ausgeführt wird. Alle lokalen Site Recovery-Komponenten werden auf diesem Computer installiert.<br/><br/>Für die VMware-VM-Replikation wird empfohlen, dass Sie diesen Server als hoch verfügbaren virtuellen VMware-Computer bereitstellen. Wenn Sie physische Computer replizieren, kann der Computer ein physischer Server sein.<br/><br/> Das Failback von Azure auf den lokalen Standort erfolgt immer zu virtuellen VMware-Computern, unabhängig davon, ob das Failover für virtuelle Computer oder für physische Server durchgeführt wurde. Wenn Sie den Konfigurationsserver nicht als VMware-VM bereitstellen, müssen Sie einen separaten Masterzielserver als VMware-VM einrichten, die den Failback-Datenverkehr empfängt.<br/><br/>Wenn der Server eine VMware-VM ist, sollte der Netzwerkkartentyp VMXNET3 sein. Wenn Sie einen anderen Netzwerkkartentyp verwenden, müssen Sie ein [VMware-Update](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1) auf dem Server mit vSphere 5.5 installieren.<br/><br/>Der Server muss eine statische IP-Adresse besitzen.<br/><br/>Der Server darf kein Domänencontroller sein.<br/><br/>Der Hostname des Servers darf maximal 15 Zeichen umfassen.<br/><br/>Es darf nur ein englischsprachiges Betriebssystem verwendet werden.<br/><br/> Sie müssen VMware vSphere PowerCLI 6.0 auf dem Konfigurationsserver installieren.<br/><br/>Der Konfigurationsserver benötigt Internetzugriff. Der ausgehende Zugriff muss wie folgt eingerichtet sein:<br/><br/>Temporärer Zugriff auf HTTP 80 während des Setups der Site Recovery-Komponenten (zum Herunterladen von MySQL)<br/><br/>Kontinuierlicher ausgehender Zugriff auf HTTPS 443 für die Replikationsverwaltung<br/><br/>Kontinuierlicher ausgehender Zugriff auf HTTPS 9443 für den Replikationsdatenverkehr (Port kann geändert werden)<br/><br/>Der Server benötigt außerdem Zugriff auf die folgenden URLs, damit er eine Verbindung mit Azure herstellen kann: *.hypervrecoverymanager.windowsazure.com; *.accesscontrol.windows.net; *.backup.windowsazure.com; *.blob.core.windows.net; *.store.core.windows.net<br/><br/>Wenn Sie über IP-Adressen-basierte Firewallregeln auf dem Server verfügen, stellen Sie sicher, dass die Regeln die Kommunikation mit Azure zulassen. Sie müssen die [IP-Bereiche des Azure-Rechenzentrums](https://www.microsoft.com/download/confirmation.aspx?id=41653) und das HTTPS-Protokoll (443) zulassen.<br/><br/>Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für „USA, Westen“ zu.<br/><br/>Lassen Sie die folgende URL für den MySQL-Download zu: .http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi


## Voraussetzungen für den VMware vCenter-/vSphere-Host

**Voraussetzung** | **Details**
--- | ---
**vSphere**| Sie benötigen mindestens einen VMware vSphere-Hypervisor.<br/><br/>Hypervisoren müssen vSphere Version 6.0, 5.5 oder 5.1 mit den neuesten Updates verwenden.<br/><br/>Ihre vSphere-Hosts und vCenter-Server sollten sich im gleichen Netzwerk befinden wie der Prozessserver. (Dabei handelt es sich um das Netzwerk, in dem sich der Konfigurationsserver befindet, es sei denn, Sie haben einen dedizierten Prozessserver eingerichtet.)
**vCenter** | Es wird empfohlen, dass Sie einen VMware vCenter-Server zum Verwalten Ihrer vSphere-Hosts bereitstellen. Auf diesem muss Version 6.0 oder 5.5 von vCenter mit den neuesten Updates ausgeführt werden.<br/><br/>Beachten Sie, dass Site Recovery neue Features von vCenter und vSphere 6.0 nicht unterstützt. Hierzu zählen etwa Cross vCenter vMotion, virtuelle Volumes und Storage DRS. Site Recovery unterstützt nur Features, die auch in Version 5.5 verfügbar waren.


## Voraussetzungen für geschützte Computer

**Voraussetzung** | **Details**
--- | ---
**Lokale VMware-VMs** | Auf virtuellen VMware-Computern, die Sie schützen möchten, müssen VMware-Tools installiert sein und ausgeführt werden.<br/><br/> Computer, die Sie schützen möchten, müssen die [Azure-Voraussetzungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) für das Erstellen von Azure-VMs erfüllen.<br/><br/>Die Kapazität der einzelnen Datenträger auf geschützten Computern darf maximal 1.023 GB betragen. Ein virtueller Computer kann bis zu 64 Datenträger haben (also bis zu 64 TB). <br/><br/>Der Schutz virtueller Computer mit verschlüsselten Festplatten (sowohl Stammdatenträger als auch Datenträger mit Daten) wird nicht unterstützt.

Gastcluster mit freigegebenen Datenträgern werden nicht unterstützt.<br/><br/>**Port 20004** sollte auf den geschützten virtuellen Computern offengelassen werden, wenn Sie **Anwendungskonsistenz** aktivieren möchten.

Startvorgänge mit Unified Extensible Firmware Interface (UEFI)/Extensible Firmware Interface (EFI) werden nicht unterstützt.<br/><br/>Computernamen dürfen 1 bis 63 Zeichen enthalten (Buchstaben, Ziffern und Bindestriche). Der Name muss mit einem Buchstaben oder einer Zahl beginnen und mit einem Buchstaben oder einer Zahl enden. Nachdem Sie die Replikation für einen Computer aktiviert haben, können Sie den Azure-Namen ändern.<br/><br/>Verfügt der virtuelle Quellcomputer über einen NIC-Teamvorgang, wird dieser nach dem Failover auf Azure in eine einzelne NIC konvertiert.<br/><br/>Wenn geschützte VMs über einen iSCSI-Datenträger verfügen, konvertiert Site Recovery den iSCSI-Datenträger des geschützten virtuellen Computers in eine VHD-Datei, wenn für den virtuellen Computer ein Failover auf Azure ausgeführt wird. Wenn das iSCSI-Ziel vom virtuellen Azure-Computer erreicht werden kann, stellt dieser eine Verbindung mit diesem Ziel her. Dadurch sind für ihn effektiv zwei Datenträger sichtbar – der VHD-Datenträger auf dem virtuellen Azure-Computer und der iSCSI-Quelldatenträger. In diesem Fall müssen Sie die Verbindung mit dem iSCSI-Ziel trennen, das auf der Azure-VM erscheint. **Windows-Computer (physische oder VMware-Computer)** | Auf dem Computer muss ein unterstütztes 64-Bit-Betriebssystem ausgeführt werden: Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 mit SP1 oder höher.<br/><br/> Das Betriebssystem sollte auf dem Laufwerk C:\\ installiert werden. Bei dem Betriebssystem-Datenträger sollte es sich um einen Windows-Basisdatenträger anstelle eines dynamischen Datenträgers handeln. Der Datenträger kann dynamisch sein.<br/><br/>Site Recovery unterstützt VMs mit einem RDM-Datenträger. Beim Failback wird der RDM-Datenträger von Site Recovery wiederverwendet, sofern die ursprüngliche Quell-VM und der RDM-Datenträger verfügbar sind. Wenn sie nicht verfügbar sind, erstellt Site Recovery während des Failbacks eine neue VMDK-Datei für jeden Datenträger. **Linux-Computer** | Sie benötigen ein unterstütztes 64-Bit-Betriebssystem: Red Hat Enterprise Linux 6.7, 7.1, 7.2, CentOS 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, Oracle Enterprise Linux 6.4, 6.5 (entweder mit dem Red Hat-kompatiblen Kernel oder mit UEK3 [Unbreakable Enterprise Kernel Release 3]), SUSE Linux Enterprise Server 11 SP3.<br/><br/>Dateien unter „/etc/hosts“ auf geschützten Computern müssen Einträge enthalten, die den Namen des lokalen Hosts jeweils IP-Adressen zuordnen, die allen Netzwerkkarten zugeordnet sind.<br/><br/>Wenn Sie nach einem Failover über einen Secure Shell-Client (SSH) eine Verbindung mit einem virtuellen Azure-Computer unter Linux herstellen möchten, stellen Sie sicher, dass der Secure Shell-Dienst auf dem geschützten Computer beim Systemstart automatisch gestartet wird und dass die Firewallregeln eine SSH-Verbindung mit ihm zulassen.<br/><br/>Der Hostname, die Bereitstellungspunkte, die Gerätenamen sowie die Linux-Systempfade und -Dateinamen (etwa „/etc/“ oder „/usr“) dürfen nur in englischer Sprache angegeben werden.<br/><br/>Der Schutz kann für Linux-Computer nur mit dem folgenden Speicher aktiviert werden: Dateisystem (EXT3, ETX4, ReiserFS, XFS), Multipath-Softwaregeräte-Mapper (multipath), Volume-Manager (LVM2). Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.<br/><br/>Site Recovery unterstützt virtuelle Computer mit einem RDM-Datenträger. Beim Failback für Linux wird der RDM-Datenträger nicht wiederverwendet. Stattdessen wird eine neue VMDK-Datei für jeden entsprechenden RDM-Datenträger erstellt.<br/><br/>Achten Sie darauf, die Einstellung „disk.enableUUID=true“ in den Konfigurationsparametern für die VM in VMware festzulegen. Erstellen Sie den Eintrag, wenn er nicht vorhanden ist. Er ist erforderlich, um der VMDK-Datei eine konsistente UUID bereitzustellen, damit sie ordnungsgemäß bereitgestellt wird. Durch das Hinzufügen dieser Einstellung wird außerdem sichergestellt, dass während des Failbacks nur Deltaänderungen zurück an den lokalen Standort übertragen werden, und keine vollständige Replikation. **Mobilitätsdienst** | **Windows**: Für eine automatische Übertragung des Mobilitätsdiensts mittels Push auf virtuelle Windows-Computer müssen Sie ein Administratorkonto (lokaler Administrator auf dem Windows-Computer) angeben, damit der Prozessserver eine Pushinstallation durchführen kann.<br/><br/> **Linux**: Für eine automatische Übertragung des Mobilitätsdiensts mittels Push auf virtuelle Linux-Computer müssen Sie ein Konto erstellen, das vom Prozessserver zum Durchführen einer Pushinstallation verwendet werden kann.<br/><br/> Standardmäßig werden alle Datenträger auf einem Computer repliziert. Der Mobilitätsdienst muss vor der Aktivierung der Replikation manuell auf dem Computer installiert werden, um [einen Datenträger von der Replikation auszuschließen](#exclude-disks-from-replication).

## Vorbereiten der Bereitstellung

Für die Vorbereitung der Bereitstellung benötigen Sie Folgendes:

1. [Richten Sie ein Azure-Netzwerk ein](#set-up-an-azure-network), in dem nach dem Failover virtuelle Azure-Computer hochgefahren werden. Zusätzlich müssen Sie für das Failback eine VPN-Verbindung (oder Azure ExpressRoute) zwischen dem Azure-Netzwerk und Ihrem lokalen Standort einrichten.
2. [Richten Sie ein Azure-Speicherkonto ein](#set-up-an-azure-storage-account), das für replizierte Daten verwendet werden kann.
3. [Bereiten Sie ein Konto auf dem vCenter-Server oder den vSphere-Hosts vor](#prepare-an-account-for-automatic-discovery), damit Site Recovery automatisch virtuelle VMware-Computer erkennen kann, die hinzugefügt werden.
4. [Bereiten Sie den Konfigurationsserver vor](#prepare-the-configuration-server), um sicherzustellen, dass er auf die erforderlichen URLs zugreifen kann, und installieren Sie vSphere PowerCLI 6.0.


### Einrichten eines Azure-Netzwerks

- Das Netzwerk sollte sich in derselben Azure-Region wie das Netzwerk befinden, in dem Sie den Recovery Services-Tresor bereitstellen.
- Richten Sie das Azure-Netzwerk im [ARM-Modus](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) oder im [klassischen Modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ein (je nachdem, welches Ressourcenmodell Sie für virtuelle Azure-Computer nach dem Failover verwenden möchten).
- Für das Failback von Azure auf Ihren lokalen VMware-Standort benötigen Sie eine VPN-Verbindung (oder eine Azure ExpressRoute-Verbindung) zwischen dem Azure-Netzwerk, in dem sich die replizierten Azure-VMs befinden, und dem lokalen Netzwerk, in dem sich der Konfigurationsserver befindet.
- [Informieren Sie sich](../vpn-gateway/vpn-gateway-site-to-site-create.md) über die unterstützten Bereitstellungsmodelle für VPN-Standort-zu-Standort-Verbindungen und über das [Einrichten einer Verbindung](../vpn-gateway/vpn-gateway-site-to-site-create.md#create-your-virtual-network).
- Alternativ dazu können Sie [Azure ExpressRoute](../expressroute/expressroute-introduction.md) einrichten. [Informieren Sie sich](../expressroute/expressroute-howto-vnet-portal-classic.md) über das Einrichten eines Azure-Netzwerks mit ExpressRoute.

> [AZURE.NOTE] [Migration of networks](../resource-group-move-resources.md) zwischen Ressourcengruppen im gleichen Abonnement oder zwischen verschiedenen Abonnements wird für Netzwerke nicht unterstützt, die für die Site Recovery-Bereitstellung verwendet werden.

### Einrichten eines Azure-Speicherkontos

- Sie benötigen ein Standard- oder Premium-Azure-Speicherkonto zum Speichern von Daten, die nach Azure repliziert werden. Das Konto muss sich in derselben Region wie der Recovery Services-Tresor befinden. Richten Sie ein Konto im [ARM-Modus](../storage/storage-create-storage-account.md) oder im [klassischen Modus](../storage/storage-create-storage-account-classic-portal.md) ein (je nachdem, welches Ressourcenmodell Sie für virtuelle Azure-Computer nach dem Failover verwenden möchten).
- Wenn Sie ein Premium-Konto für replizierte Daten verwenden, müssen Sie ein zusätzliches Standardkonto zum Speichern von Replikationsprotokollen erstellen, in denen laufende Änderungen lokaler Daten erfasst werden.

> [AZURE.NOTE] [Migration of storage accounts](../resource-group-move-resources.md) zwischen Ressourcengruppen im gleichen Abonnement oder zwischen verschiedenen Abonnements wird für Speicherkonten nicht unterstützt, die für die Site Recovery-Bereitstellung verwendet werden.

### Vorbereiten eines Kontos für die automatische Ermittlung

Der Site Recovery-Prozessserver kann virtuelle VMware-Computer auf vSphere-Hosts oder einem vCenter-Server automatisch erkennen, der Hosts verwaltet. Site Recovery benötigt für die Ausführung der automatischen Ermittlung Anmeldeinformationen, mit denen auf den VMware-Server zugegriffen werden kann. Wenn Sie nur physische Computer replizieren, ist dieser Abschnitt nicht relevant.

1. Erstellen Sie auf der vCenter-Ebene eine Rolle (etwa „Azure\_Site\_Recovery“) mit den [erforderlichen Berechtigungen](#vmware-account-permissions), um für die automatische Ermittlung ein dediziertes Konto zu verwenden.
2. Erstellen Sie einen neuen Benutzer auf dem vSphere-Host oder dem vCenter-Server und weisen Sie die Rolle dem Benutzer zu. Später teilen Sie Site Recovery diese Anmeldeinformationen mit, damit die automatische Ermittlung ausgeführt werden kann.

	>[AZURE.NOTE] Ein vCenter-Benutzerkonto mit einer schreibgeschützten Rolle kann das Failover ausführen, geschützte Quellcomputer jedoch nicht herunterfahren. Wenn Sie diese Computer herunterfahren möchten, benötigen Sie die Rolle [Azure\_Site\_Recovery](#vmware-account-permissions). Wenn Sie nur virtuelle Computer (VMs) von VMware zu Azure migrieren und kein Failback benötigen, ist die schreibgeschützte Rolle ausreichend.

### Vorbereiten des Konfigurationsservers

1.	Stellen Sie sicher, dass der Computer, den Sie für den Konfigurationsserver verwenden die [Voraussetzungen](#configuration-server-prerequisites) erfüllt. Stellen Sie insbesondere sicher, dass der Computer mit diesen Einstellungen mit dem Internet verbunden ist:

	- Erlauben Sie den Zugriff auf diese URLs: *.hypervrecoverymanager.windowsazure.com; *.accesscontrol.windows.net; *.backup.windowsazure.com; *.blob.core.windows.net; *.store.core.windows.net
	- Lassen Sie zum Herunterladen von MySQL den Zugriff auf [http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi](http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi) zu.
	- Lassen Sie mithilfe der [IP-Adressbereiche des Azure-Rechenzentrums](https://www.microsoft.com/download/confirmation.aspx?id=41653) und des HTTPS-Protokolls (443) die Firewallkommunikation mit Azure zu.

2.	Laden Sie [VMware vSphere PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) herunter, und installieren Sie es auf dem Konfigurationsserver. (Derzeit werden keine anderen PowerCLI-Versionen unterstützt, auch R-Releases von Version 6.0 nicht).


## Erstellen eines Recovery Services-Tresors

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Neu** > **Verwaltung** > **Sicherung und Site Recovery (OMS)**. Alternativ können Sie auf **Durchsuchen** > **Recovery Services-Tresor** > **Hinzufügen** klicken.

	![Neuer Tresor](./media/site-recovery-vmware-to-azure/new-vault3.png)

3. Geben Sie unter **Name** einen Anzeigenamen an, mit dem der Tresor identifiziert wird. Wenn Sie mehrere Abonnements haben, müssen Sie ein Abonnement auswählen.
4. [Erstellen Sie eine neue Ressourcengruppe](../resource-group-template-deploy-portal.md), oder wählen Sie eine vorhandene Ressourcengruppe aus. Geben Sie eine Azure-Region an. Computer werden in dieser Region repliziert. Beachten Sie, dass sich für Site Recovery verwendete Azure-Speicher und -Netzwerke in derselben Region befinden müssen. Sie finden eine Liste der unterstützten Regionen unter Geografische Verfügbarkeit auf der Seite [Azure Site Recovery – Preisübersicht](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Wenn Sie über das Dashboard schnell auf den Tresor zugreifen möchten, klicken Sie auf **An Dashboard anheften** und anschließend auf **Erstellen**.

	![Neuer Tresor](./media/site-recovery-vmware-to-azure/new-vault-settings.png)

Der neue Tresor wird unter **Dashboard** > **Alle Ressourcen** und auf dem Hauptblatt **Recovery Services-Tresore** angezeigt.

## Erste Schritte

Site Recovery stellt eine Benutzeroberfläche für die ersten Schritte bereit, damit Sie so schnell wie möglich loslegen können. Sie überprüft Voraussetzungen und führt Sie durch die Schritte zur Bereitstellung von Site Recovery.

Sie wählen den Typ der Computer aus, die Sie replizieren möchten, und geben an, wohin sie repliziert werden sollen. Sie richten die Infrastruktur ein, einschließlich lokaler Server, Azure-Einstellungen, Richtlinien für die Replikation und der Kapazitätsplanung. Nachdem Ihre Infrastruktur eingerichtet wurde, aktivieren Sie die Replikation für VMs und physische Server. Sie können anschließend Failover für bestimmte Computer ausführen oder Wiederherstellungspläne erstellen, um das Failover für mehrere Computer durchzuführen.

Beginnen Sie mit den ersten Schritten, indem Sie auswählen, wie Sie Site Recovery bereitstellen möchten. Der Ablauf der ersten Schritte kann je nach Replikationsanforderungen leicht variieren.


## Schritt 1: Auswählen der Schutzziele

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.

1. Wählen Sie auf dem Blatt **Recovery Services-Tresore** Ihren Tresor aus, und klicken Sie auf **Einstellungen**.
2. Klicken Sie unter **Einstellungen** > **Erste Schritte** auf **Site Recovery** > **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Schutzziel**.

	![Ziele wählen](./media/site-recovery-vmware-to-azure/choose-goals.png)

3. Wählen Sie unter **Schutzziel** die Option **In Azure** und dann **Ja, mit Hyper-V** aus. Klicken Sie dann auf **OK**.

	![Ziele wählen](./media/site-recovery-vmware-to-azure/choose-goals2.png)


## Schritt 2: Einrichten der Quellumgebung

Richten Sie den Konfigurationsserver ein, und registrieren Sie ihn im Recovery Services-Tresor. Wenn Sie virtuelle VMware-Computer replizieren, geben Sie das VMware-Konto an, das Sie für die automatische Ermittlung verwenden.

1. Klicken Sie auf **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Quelle**. Falls Sie über keinen Konfigurationsserver verfügen, klicken Sie unter **Quelle vorbereiten** auf **+Konfigurationsserver**, um einen Konfigurationsserver hinzuzufügen.

	![Quelle einrichten](./media/site-recovery-vmware-to-azure/set-source1.png)

2. Überprüfen Sie auf dem Blatt **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver** angezeigt wird.
3. Überprüfen Sie vor dem Einrichten des Konfigurationsservers die [Voraussetzungen](#configuration-server-prerequisites). Stellen Sie insbesondere sicher, dass der Computer auf die erforderlichen URLs zugreifen kann.
4.	Laden Sie die Installationsdatei für das einheitliche Setup von Site Recovery herunter.
5.	Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen diesen beim Ausführen des einheitlichen Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

	![Quelle einrichten](./media/site-recovery-vmware-to-azure/set-source2.png)

6.	Führen Sie auf dem Computer, den Sie als Konfigurationsserver verwenden, das einheitliche Setup aus, um den Konfigurationsserver, den Prozessserver und den Masterzielserver zu installieren.


### Ausführen des einheitlichen Setups von Site Recovery

1.	Führen Sie die Installationsdatei für das einheitliche Setup aus.
2.	Wählen Sie in **Vorbereitung** die Option **Konfigurationsserver und Prozessserver installieren** aus.

	![Vorbereitung](./media/site-recovery-vmware-to-azure/combined-wiz1.png)

3. Klicken Sie unter **Drittanbietersoftware-Lizenz** auf **Ich stimme zu**, um MySQL herunterzuladen und zu installieren.

	![Drittanbietersoftware](./media/site-recovery-vmware-to-azure/combined-wiz105.PNG)

4. Navigieren Sie unter **Registrierung** zu dem Registrierungsschlüssel, den Sie aus dem Tresor heruntergeladen haben, und wählen Sie ihn aus.

	![Registrierung](./media/site-recovery-vmware-to-azure/combined-wiz3.png)

5. Geben Sie unter **Interneteinstellungen** an, wie der auf dem Konfigurationsserver ausgeführte Anbieter eine Internetverbindung mit Azure Site Recovery herstellen soll.

	- Wenn die Verbindung über den derzeit auf dem Computer eingerichteten Proxy hergestellt werden soll, wählen Sie **Mit vorhandenen Proxyeinstellungen verbinden** aus.
	- Wenn der Anbieter eine direkte Verbindung herstellt, wählen Sie **Ohne Proxy direkt verbinden** aus.
	- Falls für den vorhandenen Proxy eine Authentifizierung erforderlich ist oder Sie für die Anbieterverbindung einen benutzerdefinierten Proxy verwenden möchten, wählen Sie **Mit benutzerdefinierten Proxyeinstellungen verbinden** aus.
		- Bei einem benutzerdefinierten Proxy müssen Sie die Adresse, den Port und Anmeldeinformationen eingeben.
		- Bei Verwendung eines Proxys sollten Sie die unter [Voraussetzungen](#configuration-server-prerequisites) beschriebenen URLs bereits zugelassen haben.

	![Firewall](./media/site-recovery-vmware-to-azure/combined-wiz4.png)

6. Bei der **Voraussetzungsüberprüfung** führt das Setup eine Überprüfung durch, um sicherzustellen, dass die Installation ausgeführt werden kann. Falls beim **Überprüfen der Synchronisierung der globalen Zeit** eine Warnung angezeigt wird, stellen Sie sicher, dass die Zeit der Systemuhr (Einstellungen für **Datum und Uhrzeit**) mit der Zeitzone übereinstimmt.

	![Voraussetzungen](./media/site-recovery-vmware-to-azure/combined-wiz5.png)

7. Erstellen Sie unter **MySQL-Konfiguration** Anmeldeinformationen, um sich bei der MySQL-Serverinstanz anzumelden, die installiert wird.

	![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz6.png)

8. Wählen Sie unter **Umgebungsdetails** aus, ob Sie virtuelle VMware-Computer replizieren möchten. Wenn dies der Fall ist, überprüft das Setup, ob PowerCLI 6.0 installiert ist.

	![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz7.png)

9. Wählen Sie unter **Installationsspeicherort** aus, wo die Binärdateien installiert werden sollen und wo der Cache gespeichert werden soll. Sie können ein Laufwerk auswählen, auf dem mindestens 5 GB Speicherplatz verfügbar ist, wir raten Ihnen jedoch zu einem Cachelaufwerk mit mindestens 600 GB freiem Speicherplatz.

	![Installationspfad](./media/site-recovery-vmware-to-azure/combined-wiz8.png)

10. Geben Sie unter **Netzwerkauswahl** den Listener (Netzwerkkarte und SSL-Port) an, über den der Konfigurationsserver Replikationsdaten sendet und empfängt. Sie können den Standardport (9443) ändern. Zusätzlich zu diesem Port wird Port 443 von einem Webserver verwendet, der Replikationsvorgänge orchestriert. 443 sollte nicht zum Empfangen von Replikationsdatenverkehr verwendet werden.


	![Netzwerkauswahl](./media/site-recovery-vmware-to-azure/combined-wiz9.png)



11.  Lesen Sie die unter **Zusammenfassung** angezeigten Informationen, und klicken Sie auf **Installieren**. Nach Abschluss der Installation wird eine Passphrase generiert. Diese benötigen Sie bei der Aktivierung der Replikation, kopieren Sie sie daher und bewahren Sie sie an einem sicheren Ort auf.

	![Zusammenfassung](./media/site-recovery-vmware-to-azure/combined-wiz10.png)

12.  Nach Abschluss der Registrierung wird der Server auf dem Blatt **Einstellungen** > **Server** im Tresor angezeigt.



#### Ausführen des Setupprogramms über die Befehlszeile

Sie können den Konfigurationsserver über die Befehlszeile einrichten:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Führen Sie nach Abschluss der Installation Folgendes durch, um die Registrierung abzuschließen:

1. Starten Sie über das Windows-Startmenü die Anwendung „Microsoft Azure Recovery Services Shell“.
2. Führen Sie im daraufhin geöffneten Befehlsfenster den folgenden Satz an Befehlen aus, um die Proxyservereinstellungen zu konfigurieren.

		PS C:\Windows\System32> $pwd = ConvertTo-SecureString -String ProxyUserPassword
		PS C:\Windows\System32> Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
		PS C:\Windows\System32> net stop obengine.exe

Parameter:

- /ServerMode: Obligatorisch. Gibt an, ob sowohl der Konfigurations- als auch der Prozessserver oder nur der Prozessserver installiert werden soll. Eingabewerte: CS, PS.
- InstallLocation: Obligatorisch. Der Ordner, in den die Komponenten installiert werden.
- /MySQLCredsFilePath. Obligatorisch. Der Dateipfad, unter dem die MySQL Server-Anmeldeinformationen gespeichert werden. Die Datei sollte folgendes Format aufweisen:
	- [MySQLCredentials]
	- MySQLRootPassword = "<Kennwort>"
	- MySQLUserPassword = "<Kennwort>"
- /VaultCredsFilePath. Obligatorisch. Der Speicherort der Datei mit den Anmeldeinformationen für den Tresor.
- /EnvType. Obligatorisch. Der Installationstyp. Werte: VMware, NonVMware
- /PSIP und /CSIP. Obligatorisch. Die IP-Adressen des Prozessservers und des Konfigurationsservers.
- /PassphraseFilePath. Obligatorisch. Der Speicherort der Datei mit der Passphrase.
- /BypassProxy. Optional. Gibt an, dass der Konfigurationsserver die Verbindung mit Azure ohne Proxy herstellt.
- /ProxySettingsFilePath. Optional. Proxyeinstellungen (der standardmäßige Proxy erfordert eine Authentifizierung oder einen benutzerdefinierten Proxy). Die Datei sollte folgendes Format aufweisen:
	- [ProxySettings]
	- ProxyAuthentication = "Yes/No"
	- Proxy IP = "IP Address>"
	- ProxyPort = "<Port>"
	- ProxyUserName = "<Benutzername>"
	- ProxyPassword = "<Kennwort>"
- DataTransferSecurePort. Optional. Die Portnummer, die für Replikationsdaten verwendet werden soll.
- SkipSpaceCheck. Optional. Überspringt die Überprüfung des Speicherplatzes für Cache.
- AcceptThirdpartyEULA. Obligatorisch. Das Flag impliziert die Akzeptanz von Drittanbieterlizenzbedingungen.
- ShowThirdpartyEULA. Obligatorisch. Zeigt Drittanbieterlizenzbedingungen an. Alle anderen Parameter werden ignoriert, wenn sie als Eingabe bereitgestellt werden.

### Fügen Sie das VMware-Konto hinzu, das für die automatische Ermittlung verwendet wird.

 Im Zuge der Bereitstellungsvorbereitung sollten Sie [ein VMware-Konto erstellt](#prepare-an-account-for-automatic-discovery) haben, das von Site Recovery für die automatische Ermittlung verwendet werden kann. Fügen Sie dieses Konto wie folgt hinzu:

1. Öffnen Sie **CSPSConfigtool.exe**. Das Tool steht als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner „[INSTALLATIONSPFAD]\\home\\svsystems\\bin“.
2. Klicken Sie auf **Konten verwalten** > **Konto hinzufügen**.

	![Konto hinzufügen](./media/site-recovery-vmware-to-azure/credentials1.png)

3. Fügen Sie unter **Kontodetails** das Konto hinzu, das für die automatische Ermittlung verwendet werden soll. Beachten Sie, dass es 15 Minuten oder länger dauern kann, bis der Kontoname im Portal angezeigt wird. Klicken Sie zur sofortigen Aktualisierung auf **Konfigurationsserver** > „Servername“ > **Server aktualisieren**.

	![Details](./media/site-recovery-vmware-to-azure/credentials2.png)

### Verbinden mit vSphere-Hosts und vCenter-Servern

Wenn Sie virtuelle VMware-Computer replizieren, stellen Sie eine Verbindung mit den vSphere-Hosts und vCenter-Servern her.

1. Stellen Sie sicher, dass der Konfigurationsserver über Netzwerkzugriff auf die vSphere-Hosts und vCenter-Server verfügt.
2. Klicken Sie auf **Infrastruktur vorbereiten** > **Quelle**. Wählen Sie unter **Quelle vorbereiten** den Konfigurationsserver aus, und klicken Sie auf **+vCenter**, um einen vSphere-Host oder einen vCenter-Server hinzuzufügen.
3. Geben Sie unter **vCenter hinzufügen** einen Anzeigenamen für den vSphere-Host oder den vCenter-Server an, und geben Sie die IP-Adresse oder den FQDN des Servers an. Belassen Sie den Port als Port 443, es sei denn Ihre VMware-Server sind so konfiguriert, dass sie auf Anforderungen auf einem anderen Port lauschen können. Wählen Sie anschließend das Konto aus, das für die Verbindung mit dem VMware-Server verwendet wird. Klicken Sie auf **OK**.

	![VMware](./media/site-recovery-vmware-to-azure/vmware-server.png)

	>[AZURE.NOTE] Wenn Sie den vCenter-Server oder den vSphere-Host mit einem Konto hinzufügen, das auf dem vCenter- oder Hostserver keine Administratorrechte besitzt, müssen Sie sicherstellen, dass für das Konto folgende Berechtigungen aktiviert sind: Datencenter, Datenspeicher, Ordner, Host, Netzwerk, Ressource, virtueller Computer, vSphere Distributed Switch. Außerdem benötigt der vCenter-Server die Anzeigeberechtigung für den Speicher.

Site Recovery stellt mithilfe der angegebenen Einstellungen eine Verbindung mit VMware-Servern her und ermittelt VMs.

## Schritt 3: Einrichten der Zielumgebung

Stellen Sie sicher, dass Sie über ein Speicherkonto für die Replikation und über ein Azure-Netzwerk verfügen, mit dem Azure-VMs nach dem Failover eine Verbindung herstellen.

1.	Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das gewünschte Azure-Abonnement aus.
2.	Geben Sie das Bereitstellungsmodell an, das Sie nach einem Failover für VMs verwenden möchten.
3.	Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

	![Ziel](./media/site-recovery-vmware-to-azure/gs-target.png)

4.	Wenn Sie noch kein Speicherkonto erstellt haben und dies per ARM nachholen möchten, können Sie auf **+Speicherkonto** klicken, um diesen Vorgang direkt durchzuführen. Geben Sie auf dem Blatt **Speicherkonto erstellen** einen Kontonamen, einen Typ, ein Abonnement und einen Standort an. Das Konto sollte sich in derselben Region wie der Recovery Services-Tresor befinden.

	![Speicher](./media/site-recovery-vmware-to-azure/gs-createstorage.png)

	Beachten Sie Folgendes:

	- Wenn Sie ein Speicherkonto mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das Azure-Portal. [Weitere Informationen](../storage/storage-create-storage-account-classic-portal.md)
	- Wenn Sie ein Storage Premium-Konto für replizierte Daten verwenden, müssen Sie ein weiteres Standardspeicherkonto zum Speichern von Replikationsprotokollen einrichten, mit denen laufende Änderungen lokaler Daten erfasst werden.
	
	> [AZURE.NOTE] Der Schutz von Premium-Speicherkonten in den Regionen „Indien, Mitte“ und „Indien, Süden“ wird zurzeit nicht unterstützt.

4.	Wählen Sie ein Azure-Netzwerk aus. Wenn Sie noch kein Netzwerk erstellt haben und dies per ARM nachholen möchten, klicken Sie auf **+Netzwerk**, um diesen Vorgang direkt durchzuführen. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** einen Netzwerknamen, einen Adressbereich, Subnetzdetails, ein Abonnement und einen Standort an. Das Netzwerk sollte sich an demselben Standort wie der Recovery Services-Tresor befinden.

	![Netzwerk](./media/site-recovery-vmware-to-azure/gs-createnetwork.png)

	Wenn Sie ein Netzwerk mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das Azure-Portal. [Weitere Informationen](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

## Schritt 4: Einrichten der Replikationseinstellungen

1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Erstellen und zuordnen**.
2. Geben Sie unter **Richtlinie erstellen und zuordnen** einen Richtliniennamen an.
3. Geben Sie unter **RPO-Schwellenwert** den RPO-Grenzwert an. Warnungen werden generiert, wenn die fortlaufende Replikation diesen Grenzwert überschreitet.
5. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte in Stunden an. Geschützte Computer können innerhalb eines Zeitfensters an einem beliebigen Punkt wiederhergestellt werden. Für nach Storage Premium replizierte Computer wird eine Aufbewahrungsdauer von bis zu 24 Stunden unterstützt.
6. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit** an, wie häufig (in Minuten) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten.
7. Wenn Sie eine Replikationsrichtlinie erstellen, wird für das Failback standardmäßig eine passende Richtlinie automatisch erstellt. Bei Verwendung der Replikationsrichtlinie **rep-policy** lautet die Failbackrichtlinie beispielsweise **rep-policy-failback**. Diese Richtlinie wird erst verwendet, wenn Sie ein Failback initiieren.
8. Klicken Sie auf **OK**, um die Richtlinie zu erstellen.

	![Replikationsrichtlinie](./media/site-recovery-vmware-to-azure/gs-replication2.png)

9. Wenn Sie eine neue Richtlinie erstellen, wird sie dem Konfigurationsserver automatisch zugeordnet. Klicken Sie auf **OK**.

	![Replikationsrichtlinie](./media/site-recovery-vmware-to-azure/gs-replication3.png)


## Schritt 5: Kapazitätsplanung

Nachdem Sie nun Ihre grundlegende Infrastruktur eingerichtet haben, können Sie sich mit der Kapazitätsplanung beschäftigen und ermitteln, ob Sie zusätzliche Ressourcen benötigen.

Site Recovery verfügt über einen Capacity Planner, der Sie dabei unterstützt, die richtigen Ressourcen für Ihre Quellumgebung, die Site Recovery-Komponenten, das Netzwerk und den Speicher zuzuordnen. Sie können den Planer im Schnellmodus für Schätzungen ausführen, die auf einer durchschnittlichen Anzahl von VMs, Datenträgern und Speicher basieren, oder im ausführlichen Modus, in dem Sie Zahlen auf Workloadebene eingeben. Führen Sie Folgendes aus, bevor Sie beginnen:

- Sammeln Sie Informationen zu Ihrer Replikationsumgebung, z.B. VMs, Datenträger pro VM und Speicher pro Datenträger.
- Schätzen Sie die tägliche Änderungsrate für replizierte Daten. Sie können hierfür die [Appliance für die vSphere-Kapazitätsplanung](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) verwenden.

1.	Klicken Sie auf **Herunterladen**, um das Tool herunterzuladen, und führen Sie es anschließend aus. Lesen Sie sich den [zum Tool gehörigen Artikel](site-recovery-capacity-planner.md) durch.
2.	Wählen Sie anschließend unter **Haben Sie die Kapazitätsplanung abgeschlossen?** die Option **Ja** aus.

	![Kapazitätsplanung](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)

In der folgenden Tabelle wird eine Reihe von Punkten erfasst, die Ihnen bei der Kapazitätsplanung für dieses Szenario helfen können.


**Komponente** | **Details**
--- | --- | ---
**Replikation** | **Maximale tägliche Änderungsrate**: Ein geschützter Computer kann nur einen einzigen Prozessserver verwenden, und ein einzelner Prozessserver kann eine tägliche Änderungsrate von bis zu 2 TB verarbeiten. Daher beträgt die maximale Datenänderungsrate pro Tag, die für einen geschützten Computer unterstützt wird, 2 TB.<br/><br/> **Maximaler Durchsatz**: Ein replizierter Computer kann zu einem einzigen Speicherkonto in Azure gehören. Ein Standardspeicherkonto kann maximal 20.000 Anforderungen pro Sekunde verarbeiten, und es wird empfohlen, dass Sie die Anzahl von IOPS über einen Quellcomputer hinweg bei 20.000 halten. Wenn Ihr Quellcomputer z. B. 5 Datenträger hat und jeder Datenträger 120 IOPS (mit einer Größe von 8 KB) für die Quelle generiert, wird das Azure-Limit von 500 IOPS pro Datenträger eingehalten. Erforderliche Anzahl von Speicherkonten = Quell-IOPS insgesamt/20000.
**Konfigurationsserver** | Der Konfigurationsserver muss in der Lage sein, die tägliche Änderungsrate für alle auf geschützten Computern ausgeführten Workloads zu bewältigen, und er muss über genügend Bandbreite verfügen, um Daten kontinuierlich in den Azure-Speicher zu replizieren.<br/><br/> Als bewährte Methode wird empfohlen, den Konfigurationsserver im selben Netzwerk und LAN-Segment zu platzieren, in dem sich auch die zu schützenden Computer befinden. Er kann in einem anderen Netzwerk platziert werden, aber die zu schützenden Computer sollten für ihn über L3-Netzwerksichtbarkeit verfügen.<br/><br/> Die empfohlenen Größen für den Konfigurationsserver werden in der nachstehenden Tabelle zusammengefasst.
**Prozessserver** | Der erste Prozessserver wird standardmäßig auf dem Konfigurationsserver installiert. Sie können zusätzliche Prozessserver bereitstellen, um Ihre Umgebung zu skalieren. Beachten Sie Folgendes:<br/><br/> Der Prozessserver empfängt Replikationsdaten von geschützten Computern und optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung, bevor er sie an Azure sendet. Der Prozessservercomputer muss über genügend Ressourcen zum Ausführen dieser Aufgaben verfügen.<br/><br/> Der Prozessserver verwendet einen datenträgerbasierten Cache. Wir empfehlen die Verwendung eines separaten Cachedatenträgers mit einer Größe von mindestens 600 GB, um Datenänderungen bei einem Netzwerkengpass oder -ausfall zwischenspeichern und verarbeiten zu können.

### Empfohlene Größen für den Konfigurationsserver

**CPU** | **Arbeitsspeicher** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer**
--- | --- | --- | --- | ---
8 vCPUs (2 Sockets * 4 Kerne mit 2,5 GHz) | 16 GB | 300 GB | 500 GB oder weniger | Weniger als 100 Computer replizieren.
12 vCPUs (2 Sockets * 6 Kerne mit 2,5 GHz) | 18 GB | 600 GB | 500 GB bis 1 TB | Zwischen 100 und 150 Computer replizieren.
16 vCPUs (2 Sockets * 8 Kerne mit 2,5 GHz) | 32 GB | 1 TB | 1 TB bis 2 TB | Zwischen 150 und 200 Computer replizieren.
Bereitstellen eines weiteren Prozessservers | | | > 2 TB | Stellen Sie zusätzliche Prozessserver bereit, wenn Sie mehr als 200 Computer replizieren oder wenn die tägliche Änderungsrate 2 TB überschreitet.

Hierbei gilt:

- Jeder Quellcomputer ist mit 3 Datenträgern von jeweils 100 GB konfiguriert.
- Bei den Benchmarkingmessungen für den Cachedatenträger wurde ein Speicher aus 8 SAS-Laufwerken mit 10.000 U/min und RAID 10 verwendet.

### Empfohlene Größen für den Prozessserver

Wenn Sie mehr als 200 Computer schützen müssen oder die tägliche Änderungsrate größer als 2 TB ist, können Sie zusätzliche Prozessserver zum Verarbeiten der Replikationslast hinzufügen. Zum horizontalen Hochskalieren haben Sie folgende Möglichkeiten:

- Erhöhen Sie die Anzahl von Konfigurationsservern. Beispielsweise können Sie mit zwei Konfigurationsservern bis zu 400 Computer schützen.
- Fügen Sie zusätzliche Prozessserver hinzu, und verwenden Sie diese Prozessserver anstelle des Konfigurationsservers (oder als Ergänzung zu diesem) für die Abwicklung des Datenverkehrs.

Die folgende Tabelle beschreibt dieses Szenario:

- Sie planen nicht, den Konfigurationsserver als Prozessserver zu verwenden.
- Sie haben einen zusätzlichen Prozessserver eingerichtet.
- Sie haben geschützte virtuelle Computer so konfiguriert, dass sie den zusätzlichen Prozessserver verwenden.
- Jeder geschützte Quellcomputer ist mit drei Datenträgern von jeweils 100 GB konfiguriert.

**Konfigurationsserver** | **Zusätzlicher Prozessserver**| **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer**
--- | --- | --- | --- | ---
8 vCPUs (2 Sockets * 4 Kerne mit 2,5 GHz), 16 GB Speicher | 4 vCPUs (2 Sockets * 2 Kerne mit 2,5 GHz), 8 GB Speicher | 300 GB | 250 GB oder weniger | Bis zu 85 Computer replizieren.
8 vCPUs (2 Sockets * 4 Kerne mit 2,5 GHz), 16 GB Speicher | 8 vCPUs (2 Sockets * 4 Kerne mit 2,5 GHz), 12 GB Speicher | 600 GB | 250 GB bis 1 TB | Zwischen 85 und 150 Computer replizieren.
12 vCPUs (2 Sockets * 6 Kerne mit 2,5 GHz), 18 GB Speicher | 12 vCPUs (2 Sockets * 6 Kerne mit 2,5 GHz), 24 GB Speicher | 1 TB | 1 TB bis 2 TB | Zwischen 150 und 225 Computer replizieren.


Wie Sie Ihre Server skalieren, hängt davon ab, ob Sie das zentrale Hochskalieren oder das horizontale Hochskalieren als Modell bevorzugen. Beim zentralen Hochskalieren stellen Sie wenige besonders leistungsstarke Konfigurations- und Prozessserver bereit. Beim horizontalen Hochskalieren stellen Sie mehr Server mit geringeren Ressourcen bereit. Ein Beispiel: Wenn Sie 220 Computer schützen müssen, können Sie sich für eine der beiden folgenden Optionen entscheiden:

- Richten Sie den Konfigurationsserver mit 12 vCPU und 18 GB Arbeitsspeicher und einen zusätzlichen Prozessserver mit 12 vCPU und 24 GB Arbeitsspeicher ein, und konfigurieren Sie die geschützten Computer so, dass sie nur den zusätzlichen Prozessserver verwenden.
- Alternativ können Sie zwei Konfigurationsserver (2 x 8vCPU, 16 GB RAM) und zwei zusätzliche Prozessserver (1 x 8vCPU und 1 x 4vCPU zur Handhabung von 135 + 85 (220) Computern) konfigurieren. Auch hier konfigurieren Sie die geschützten Computer so, dass sie nur die zusätzlichen Prozessserver verwenden.

[Befolgen Sie diese Anweisungen](#deploy-additional-process-servers), um einen zusätzlichen Prozessserver einzurichten.

### Aspekte der Netzwerkbandbreite

Sie können den Capacity Planner verwenden, um die Bandbreite zu berechnen, die Sie für die Replikation benötigen (erste Replikation und dann Deltareplikation). Sie haben verschiedene Möglichkeiten, um die verwendete Bandbreite für die Replikation zu steuern:

- **Bandbreite drosseln**: VMware-Datenverkehr, der nach Azure repliziert wird, wird über einen speziellen Prozessserver geleitet. Sie können die Bandbreite auf den Computern, die als Prozessserver ausgeführt werden, einschränken.
- **Bandbreite beeinflussen**: Die für die Replikation genutzte Bandbreite lässt sich mithilfe einiger Registrierungsschlüssel beeinflussen:
	- Der Registrierungswert **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows Azure Backup\\UploadThreadsPerVM** gibt die Anzahl von Threads an, die für die Datenübertragung (erste Replikation oder Deltareplikation) eines Datenträgers verwendet werden. Bei einem höheren Wert wird die Netzwerkbandbreite für die Replikation erhöht.
	- Der Registrierungswert **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows Azure Backup\\DownloadThreadsPerVM** gibt die Anzahl von Threads an, die für die Datenübertragung während eines Failbacks verwendet werden.

#### Bandbreite einschränken

1. Öffnen Sie das Microsoft Azure Backup-MMC-Snap-In auf dem Computer, der als Prozessserver fungiert. Standardmäßig ist auf dem Desktop oder unter „C:\\Programme\\Microsoft Azure Recovery Services Agent\\bin\\wabadmin“ eine Verknüpfung für Microsoft Azure Backup verfügbar.
2. Klicken Sie im Snap-In auf **Eigenschaften ändern**.

	![Bandbreite einschränken](./media/site-recovery-vmware-to-azure/throttle1.png)

3. Wählen Sie auf der Registerkarte **Drosselung** die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren**, und legen Sie die Grenzwerte für Geschäftszeiten und arbeitsfreie Zeiten fest. Der gültige Bereich reicht von 512 KBit/s bis 102 MBit/s.

	![Bandbreite einschränken](./media/site-recovery-vmware-to-azure/throttle2.png)

Sie können auch das Cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) verwenden, um die Drosselung festzulegen. Hier ein Beispiel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** gibt an, dass keine Drosselung erforderlich ist.


#### Netzwerkbandbreite beeinflussen

1. Navigieren Sie in der Registrierung zu **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows Azure Backup\\Replication**.
	- Um die Bandbreite für den Datenverkehr auf einem replizierenden Datenträger zu beeinflussen, ändern Sie den Wert für **UploadThreadsPerVM**, oder erstellen Sie den Schlüssel, falls dieser nicht vorhanden ist.
	- Um die Bandbreite für den Failbackdatenverkehr von Azure zu beeinflussen, ändern Sie den Wert für **DownloadThreadsPerVM**.
2. Der Standardwert ist 4. In einem absichtlich mit großen Reserven ausgestatteten Netzwerk müssen die Standardwerte dieser Registrierungsschlüssel geändert werden. Der maximale Wert beträgt 32. Überwachen Sie den Datenverkehr, um den Wert zu optimieren.

## Schritt 6: Replizieren von Anwendungen

Stellen Sie sicher, dass Computer, die Sie replizieren möchten, für die Installation des Mobilitätsdiensts vorbereitet sind, und aktivieren Sie anschließend die Replikation.

### Installieren des Mobilitätsdiensts

Der erste Schritt beim Aktivieren des Schutzes für virtuelle Computer und physische Server besteht im Installieren des Mobilitätsdiensts. Dazu haben Sie mehrere Möglichkeiten:

- **Serverpushvorgänge verarbeiten**: Wenn Sie die Replikation auf einem Computer aktivieren, übertragen Sie die Mobilitätsdienstkomponente mittels Push vom Prozessserver, und installieren Sie sie. Beachten Sie, dass die Pushinstallation nicht auftritt, wenn auf den Computern bereits eine aktuelle Version der Komponente ausgeführt wird.
- **Pushvorgang in Ihrem Unternehmen**: Installieren Sie die Komponente automatisch mithilfe des in Ihrem Unternehmen verwendeten Pushprozesses, z.B. WSUS (Windows Server Update Services) oder System Center Configuration Manager. Richten Sie zuvor den Konfigurationsserver ein.
- **Manuelle Installation**: Installieren Sie die Komponente manuell auf jedem Computer, den Sie replizieren möchten. Richten Sie zuvor den Konfigurationsserver ein.


#### Vorbereiten des automatischen Pushs auf Windows-Computern

Im Folgenden wird beschrieben, wie Sie Windows-Computer vorbereiten, damit der Prozessserver automatisch den Mobilitätsdienst darauf installieren kann.

1.  Erstellen Sie ein Konto, mit dem der Prozessserver auf den Computer zugreifen kann. Das Konto muss über Administratorrechte (lokal oder für die Domäne) verfügen, und es wird nur für die Pushinstallation verwendet.

	>[AZURE.NOTE] Wenn Sie kein Domänenkonto verwenden, müssen Sie die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren. Zu diesem Zweck fügen Sie in der Registrierung unter „HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Policies\\System“ den DWORD-Eintrag „LocalAccountTokenFilterPolicy“ mit dem Wert 1 hinzu. Um den Registrierungseintrag von einer Befehlszeilenschnittstelle aus hinzuzufügen, geben Sie **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** ein.

2.  Wählen Sie in der Windows-Firewall des Computers, den Sie schützen möchten, **Eine App oder ein Feature durch die Windows-Firewall zulassen** aus. Aktivieren Sie **Datei- und Druckerfreigabe** und **Windows-Verwaltungsinstrumentation**. Für Computer, die einer Domäne angehören, können Sie die Firewalleinstellungen mit einem Gruppenrichtlinienobjekt konfigurieren.

	![Firewalleinstellungen](./media/site-recovery-vmware-to-azure/mobility1.png)

2. Fügen Sie das von Ihnen erstellte Konto hinzu:

	- Öffnen Sie **cspsconfigtool**. Das Tool steht als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner „[INSTALLATIONSPFAD]\\home\\svsystems\\bin“.
	- Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
	- Fügen Sie das von Ihnen erstellte Konto hinzu. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie die Replikation für einen Computer aktivieren.


#### Vorbereiten des automatischen Pushs auf Linux-Servern

1.	Vergewissern Sie sich, dass der zu schützende Linux-Computer unterstützt wird, wie unter [Voraussetzungen für geschützte Computer](#protected-machine-prerequisites) beschrieben. Stellen Sie sicher, dass zwischen dem Linux-Computer und dem Prozessserver eine Netzwerkkonnektivität besteht.

2.	Erstellen Sie ein Konto, mit dem der Prozessserver auf den Computer zugreifen kann. Das Konto sollte ein Root-Benutzer auf dem Linux-Quellserver sein und wird nur für die Pushinstallation verwendet.

	- Öffnen Sie **cspsconfigtool**. Das Tool steht als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner „[INSTALLATIONSPFAD]\\home\\svsystems\\bin“.
	- Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
	- Fügen Sie das von Ihnen erstellte Konto hinzu. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie die Replikation für einen Computer aktivieren.

3.	Vergewissern Sie sich, dass die Datei „/etc/hosts“ auf dem Linux-Quellserver Einträge enthält, die den Namen des lokalen Hosts den IP-Adressen zuordnen, die allen Netzwerkkarten zugewiesen sind.
4.	Installieren Sie die neuesten „openssh“-, „openssh-server“- und „openssl“-Pakete auf dem Computer, den Sie replizieren möchten.
5.	Stellen Sie sicher, dass SSH auf Port 22 aktiviert ist und ausgeführt wird.
6.	Aktivieren Sie das SFTP-Subsystem und die Kennwortauthentifizierung in der Datei „sshd\_config“ wie folgt:

	- Melden Sie sich als Root-Benutzer an.
	- Suchen Sie in der Datei „/etc/ssh/sshd\_config“ die Zeile, die mit **PasswordAuthentication** beginnt.
	- Heben Sie die Auskommentierung der Zeile auf, und ändern Sie den Wert von **no** in **yes**.
	- Suchen Sie die Zeile, die mit **Subsystem** beginnt, und heben Sie die Auskommentierung der Zeile auf.

		![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)


#### Manuelles Installieren des Mobilitätsdiensts

Die Installationsprogramme stehen auf dem Prozessserver unter **C:\\Programme (x86)\\Microsoft Azure Site Recovery\\home\\svsystems\\pushinstallsvc\\repository** zur Verfügung.

Quellbetriebssystem | Installationsdatei für den Mobilitätsdienst
--- | ---
Windows Server (nur 64 Bit) | Microsoft-ASR\_UA\_9.*.0.0_Windows_* release.exe
CentOS 6.4, 6.5, 6.6 (nur 64 Bit) | Microsoft-ASR\_UA\_9.*.0.0\_RHEL6-64\_*release.tar.gz
SUSE Linux Enterprise Server 11 SP3 (nur 64 Bit) | Microsoft-ASR\_UA\_9.*.0.0\_SLES11-SP3-64\_*release.tar.gz
Oracle Enterprise Linux 6.4, 6.5 (nur 64 Bit) | Microsoft-ASR\_UA\_9.*.0.0\_OL6-64\_*release.tar.gz


#### Manuelles Installieren auf einem Windows-Server


1. Laden Sie das entsprechende Installationsprogramm herunter, und führen Sie es aus.
2. Wählen Sie unter **Vorbereitung** die Option **Mobilitätsdienst** aus.

	![Mobilitätsdienst](./media/site-recovery-vmware-to-azure/mobility3.png)

3. Geben Sie unter **Konfigurationsserverdetails** die IP-Adresse des Konfigurationsservers und die Passphrase ein, die beim Ausführen des einheitlichen Setups generiert wurde. Sie können die Passphrase abrufen, indem Sie auf dem Konfigurationsserver **<Site Recovery-Installationsordner>\\home\\sysystems\\bin\\genpassphrase.exe –v** ausführen.

	![Mobilitätsdienst](./media/site-recovery-vmware-to-azure/mobility6.png)

4. Behalten Sie unter **Installationsort** die Standardeinstellung bei, und klicken Sie auf **Weiter**, um mit der Installation zu beginnen.
5. Überwachen Sie unter **Installationsstatus** die Installation, und starten Sie den Computer neu, wenn Sie dazu aufgefordert werden. Nach der Installation des Diensts kann es etwa 15 Minuten dauern, bis der Status im Portal aktualisiert wird.

Sie können die Installation auch über die Befehlszeile ausführen:

UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installationsverzeichnis>] [/CSIP <IP-Adresse des Konfigurationsservers für die Registrierung>] [/PassphraseFilePath <Pfad der Passphrase-Datei>] [/LogFilePath <Pfad der Protokolldatei>]

Hierbei gilt:

- /Role: Obligatorisch. Gibt an, ob der Mobilitätsdienst installiert werden soll.
- /InstallLocation: Obligatorisch. Gibt an, wo der Dienst installiert werden soll.
- /PassphraseFilePath: Obligatorisch. Die Passphrase des Konfigurationsservers.
- /LogFilePath: Obligatorisch. Der Speicherort für die Setupprotokolldateien.

#### Manuelles Deinstallieren des Mobilitätsdiensts

Der Mobilitätsdienst kann mithilfe der Option „Software“ in der Systemsteuerung oder über die Befehlszeile deinstalliert werden.

Der Befehl zum Deinstallieren des Mobilitätsdiensts mithilfe der Befehlszeile lautet:

	MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}


#### Manuelles Installieren auf einem Linux-Server

1. Kopieren Sie anhand der obigen Tabelle das geeignete TAR-Archiv auf den zu replizierenden Linux-Computer.
2. Öffnen Sie ein Shellprogramm, und extrahieren Sie das gezippte TAR-Archiv in einen lokalen Pfad, indem Sie Folgendes ausführen: `tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Erstellen Sie in dem lokalen Verzeichnis, in das Sie den Inhalt des TAR-Archivs extrahiert haben, die Datei „passphrase.txt“. Hierzu kopieren Sie die Passphrase aus „C:\\ProgramData\\Microsoft Azure Site Recovery\\private\\connection.passphrase“ auf dem Konfigurationsserver und speichern sie dann in „passphrase.txt“, indem Sie in der Shell *`echo <passphrase> >passphrase.txt`* ausführen.
4. Installieren Sie den Mobilitätsdienst, indem Sie *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`* ausführen.
5. Geben Sie die interne IP-Adresse des Konfigurationsservers an, und stellen Sie sicher, dass der Port 443 ausgewählt ist. Nach der Installation des Diensts kann es etwa 15 Minuten dauern, bis der Status im Portal aktualisiert wird.

**Sie können die Installation auch über die Befehlszeile ausführen**:

1. Kopieren Sie die Passphrase aus „C:\\Programme (x86)\\InMage Systems\\private\\connection“ auf dem Konfigurationsserver, und speichern Sie sie in „passphrase.txt“ auf dem Konfigurationsserver. Führen Sie anschließend die aufgeführten Befehle aus. In unserem Beispiel wird für den Konfigurationsserver die IP-Adresse 104.40.75.37 verwendet, und der HTTPS-Port sollte 443 sein:

So installieren Sie den Dienst auf einem Produktionsserver:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

So installieren Sie den Dienst auf dem Masterzielserver:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


### Replikation aktivieren

#### Vorbereitung

Wenn Sie VMware-VMs replizieren, beachten Sie Folgendes:

- Virtuelle VMware-Computer werden alle 15 Minuten ermittelt, und es kann 15 Minuten oder länger dauern, bis sie nach der Ermittlung im Portal angezeigt werden. Ebenso kann Ermittlung 15 Minuten oder länger dauern, wenn Sie einen neuen vCenter-Server oder vSphere-Host hinzufügen.
- Es kann auch 15 Minuten oder länger dauern, bis Umgebungsänderungen auf dem virtuellen Computer (z.B. die Installation von VMware-Tools) im Portal aktualisiert werden.
- Sie können den Zeitpunkt der letzten Ermittlung für die virtuellen VMware-Computer Sie auf dem Blatt **Konfigurationsserver** im Feld **Letzter Kontakt um** für den vCenter-Server oder den vSphere-Host überprüfen.
- Wenn Sie Computer für die Replikation hinzufügen möchten, ohne auf die planmäßige Ermittlung zu warten, markieren Sie den Konfigurationsserver (klicken Sie nicht darauf), und klicken Sie auf die Schaltfläche **Aktualisieren**.
- Wenn Sie die Replikation aktivieren, installiert der Prozessserver den Mobilitätsdienst automatisch auf dem Computer, falls dieser vorbereitet ist.

#### Ausschließen von Datenträgern von der Replikation

Wenn Sie die Replikation aktivieren, werden standardmäßig alle Datenträger auf einem Computer repliziert. Sie können Datenträger von der Replikation ausschließen. Beispielsweise möchten Sie möglicherweise nicht, dass Datenträger mit temporären Daten oder Daten, die bei jedem Neustart des Computers oder der Anwendung aktualisiert werden (z.B. pagefile.sys oder SQL Server tempdb), repliziert werden. Wenn Sie Datenträger ausschließen möchten, beachten Sie Folgendes:

- Sie können nur Datenträger ausschließen, auf denen der Mobilitätsdienst bereits installiert ist. Sie müssen [den Mobilitätsdienst manuell installieren](#install-the-mobility-service-manually), da dieser nur nach Aktivierung der Replikation mithilfe des Pushmechanismus installiert wird.
- Nur Basisdatenträger können von der Replikation ausgeschlossen werden. Betriebssystem- oder dynamische Datenträger können nicht ausgeschlossen werden.
- Nach Aktivierung der Replikation können Sie keine Datenträger für die Replikation hinzufügen oder entfernen. Wenn Sie einen Datenträger hinzufügen oder entfernen möchten, müssen Sie den Schutz für den Computer deaktivieren und anschließend wieder aktivieren.
- Wenn Sie einen Datenträger ausschließen, der für den Betrieb einer Anwendung erforderlich ist, müssen Sie ihn nach dem Failover auf Azure manuell in Azure erstellen, damit die replizierte Anwendung ausgeführt werden kann. Alternativ könnten Sie Azure Automation in einen Wiederherstellungsplan integrieren, um den Datenträger während des Failovers des Computers zu erstellen.
- Für Datenträger, die Sie manuell in Azure erstellen, wird ein Failback ausgeführt. Wenn Sie beispielsweise ein Failover für drei Datenträger ausführen und zwei direkt in Azure erstellen, wird für alle fünf ein Failback ausgeführt. Sie können keine Datenträger von einem Failback ausschließen, die manuell erstellt wurden.

**Aktivieren Sie die Replikation jetzt wie folgt**:

1. Klicken Sie auf **Schritt 2: Anwendung replizieren** > **Quelle**. Klicken Sie nach der erstmaligen Aktivierung der Replikation im Tresor auf **+Replizieren**, um die Replikation für weitere Computer zu aktivieren.
2. Wählen Sie auf dem Blatt **Quelle** > **Quelle** den Konfigurationsserver aus.
3. Wählen Sie unter **Computertyp** die Option **Virtuelle Computer** oder **Physische Computer** aus.
4. Wählen Sie unter **vCenter/vSphere-Hypervisor** den vCenter-Server aus, der den vSphere-Host verwaltet, oder wählen Sie den Host aus. Wenn Sie physische Computer replizieren, ist diese Einstellung nicht relevant.
5. Wählen Sie den Prozessserver aus. Wenn Sie keine zusätzlichen Prozessserver erstellt haben, ist dies der Name des Konfigurationsservers. Klicken Sie dann auf **OK**.

	![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. Wählen Sie unter **Ziel** das Tresorabonnement und unter **Bereitstellungsmodell nach dem Failover** das Modell (klassisches Modell oder Resource Manager-Modell) aus, das Sie in Azure nach einem Failover verwenden möchten.
7. Wählen Sie das Azure-Speicherkonto aus, das Sie für die Replikation von Daten verwenden. Beachten Sie Folgendes:

	- Sie können ein Premium- oder Standardspeicherkonto auswählen. Wenn Sie ein Premium-Konto auswählen, müssen Sie für fortlaufende Replikationsprotokolle ein zusätzliches Standardspeicherkonto angeben. Die Konten müssen sich in derselben Region wie der Recovery Services-Tresor befinden.
	- Wenn Sie ein anderes Speicherkonto als Ihre bereits vorhandenen Speicherkonten verwenden möchten, können Sie ein [Speicherkonto erstellen](#set-up-an-azure-storage-account). Klicken Sie zum Erstellen eines Speicherkontos mit dem ARM-Modell auf **Neu erstellen**. Wenn Sie ein Speicherkonto mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das [Azure-Portal](../storage/storage-create-storage-account-classic-portal.md).

8. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem Azure-VMs eine Verbindung herstellen, wenn sie nach einem Failover erstellt werden. Das Netzwerk muss sich in derselben Region wie der Recovery Services-Tresor befinden. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen. Wenn Sie über kein Netzwerk verfügen, müssen Sie [ein Netzwerk erstellen](#set-up-an-azure-network). Klicken Sie zum Erstellen eines Netzwerks mit dem ARM-Modell auf **Neu erstellen**. Wenn Sie ein Netzwerk mit dem klassischen Modell erstellen möchten, verwenden Sie das [Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Wählen Sie, falls zutreffend, ein Subnetz aus. Klicken Sie dann auf **OK**.

	![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-replication3.png)

9. Klicken Sie auf **Virtuelle Computer** > **Virtuelle Computer auswählen**, und wählen Sie die Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.

	![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-replication5.png)

10. Wählen Sie unter **Eigenschaften** > **Eigenschaften konfigurieren** das Konto aus, das der Prozessserver zum automatischen Installieren des Mobilitätsdiensts auf dem Computer verwenden soll. Standardmäßig werden alle Datenträger repliziert. Klicken Sie auf **Alle Datenträger**, und entfernen Sie alle Datenträger, die Sie nicht replizieren möchten. Klicken Sie dann auf **OK**. Sie können später weitere Eigenschaften festlegen.

	![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-replication6.png)

11. Überprüfen Sie, ob unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren** die richtige Replikationsrichtlinie ausgewählt ist. Sie können die Replikationsrichtlinieneinstellungen unter **Einstellungen** > **Replikationsrichtlinien** > Richtlinienname > **Einstellungen bearbeiten** ändern. Die Änderungen, die Sie an einer Richtlinie vornehmen, werden auf die Replikation und auf neue Computer angewendet.

12. Aktivieren Sie **Multi-VM-Konsistenz**, wenn Sie Computer in einer Replikationsgruppe zusammenfassen möchten, und geben Sie einen Namen für die Gruppe an. Klicken Sie dann auf **OK**. Beachten Sie Folgendes:

	- Die Computer in einer Replikationsgruppe werden gemeinsam repliziert und verfügen beim Failover über gemeinsame ausfallsichere und anwendungskonsistente Wiederherstellungspunkte.
	- Es wird empfohlen, dass Sie virtuelle Computer und physische Server zusammenfassen, damit sie Ihre Workloads widerspiegeln. Das Aktivieren von Multi-VM-Konsistenz kann sich auf die Leistung der Workload auswirken und sollte nur verwendet werden, wenn Computer die gleiche Workload ausführen und Sie Konsistenz benötigen.

	![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-replication7.png)

13. Klicken Sie auf **Replikation aktivieren**. Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.

> [AZURE.NOTE] Wenn der Computer für die Pushinstallation vorbereitet ist, wird die Mobilitätsdienstkomponente installiert, wenn der Schutz aktiviert wird. Nachdem die Komponente auf dem Computer installiert wurde, wird ein Schutzauftrag gestartet. Er führt zu einem Fehler. Nach dem Fehler müssen Sie jeden Computer manuell neu starten. Nach dem Neustart wird der Schutzauftrag erneut gestartet, und die erste Replikation wird ausgeführt.

### Anzeigen und Verwalten von VM-Eigenschaften

Es wird empfohlen, dass Sie die Eigenschaften des Quellcomputers überprüfen. Beachten Sie, dass der Name des virtuellen Azure-Computers die [Anforderungen für virtuelle Azure-Computer](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllen muss.

1. Klicken Sie auf **Einstellungen** > **Replizierte Elemente**, und wählen Sie den Computer aus. Das Blatt **Zusammenfassung** enthält Informationen zu den Einstellungen und zum Status der Computer.

2. Unter **Eigenschaften** können Sie die Informationen zur Replikation und zum Failover für den virtuellen Computer anzeigen.

	![Replikation aktivieren](./media/site-recovery-vmware-to-azure/test-failover2.png)

3. Unter **Compute und Netzwerk** > **Compute-Eigenschaften** können Sie den Namen und die Zielgröße des virtuellen Azure-Computers angeben. Ändern Sie ggf. den Namen, damit er die Azure-Anforderungen erfüllt. Sie können auch Informationen zum Zielnetzwerk, zum Subnetz sowie zu der IP-Adresse anzeigen und hinzufügen, die der Azure-VM zugewiesen wird. Beachten Sie Folgendes:

	- Sie können die Ziel-IP-Adresse festlegen. Wenn Sie keine Adresse angeben, wird für den Computer, für den das Failover durchgeführt wurde, DHCP verwendet. Wenn Sie eine Adresse festlegen, die beim Failover nicht verfügbar ist, funktioniert das Failover nicht. Dieselbe Ziel-IP-Adresse kann für das Testfailover verwendet werden, wenn die Adresse im Testfailover-Netzwerk verfügbar ist.
	- Die Anzahl der Netzwerkkarten hängt von der Größe ab, die Sie für den virtuellen Zielcomputer angeben. Hierbei gilt Folgendes:

		- Wenn die Anzahl der Netzwerkkarten des Quellcomputers maximal der Anzahl der Netzwerkkarten entspricht, die für die Größe des Zielcomputers zulässig ist, hat der Zielcomputer die gleiche Anzahl von Netzwerkkarten wie der Quellcomputer.
		- Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputer die maximal zulässige Anzahl für die Größe des Zielcomputers übersteigt, wird die Anzahl verwendet, die maximal für die Größe des Zielcomputers zulässig ist.
		- Ein Beispiel: Wenn ein Quellcomputer zwei Netzwerkkarten besitzt und der Zielcomputer aufgrund seiner Größe vier Netzwerkkarten unterstützt, erhält der Zielcomputer zwei Netzwerkkarten. Wenn der Quellcomputer dagegen zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe nur einen Adapter unterstützt, erhält der Zielcomputer nur einen Adapter.
	- Wenn die VM über mehrere Netzwerkkarten verfügt, werden alle mit dem gleichen Netzwerk verbunden.

	![Replikation aktivieren](./media/site-recovery-vmware-to-azure/test-failover4.png)

4. Unter **Datenträger** werden das Betriebssystem und die Datenträger auf der VM angezeigt, die repliziert wird.


## Schritt 7: Testen der Bereitstellung

Zum Testen der Bereitstellung können Sie ein Test-Failover für einen einzelnen virtuellen Computer durchführen. Alternativ können Sie einen Wiederherstellungsplan erstellen, der mehrere virtuelle Computer enthält.


### Vorbereiten des Failovers

- Zum Ausführen eines Testfailovers empfehlen wir Folgendes: Erstellen Sie ein neues Azure-Zielnetzwerk, das von Ihrem Azure-Produktionsnetzwerk isoliert ist (Standardverhalten bei der Erstellung eines neuen Netzwerks in Azure). [Erfahren Sie mehr](site-recovery-failover.md#run-a-test-failover) über die Ausführung von Testfailovervorgängen.
- Installieren Sie den Azure-Agent auf dem geschützten Computer, um für das Failover zu Azure die beste Leistung zu erzielen. Hierdurch wird der Startvorgang beschleunigt und die Problembehandlung vereinfacht. Installieren Sie den Agent für [Linux](https://github.com/Azure/WALinuxAgent) oder [Windows](http://go.microsoft.com/fwlink/?LinkID=394789).
- Zum vollständigen Testen der Bereitstellung benötigen Sie eine entsprechende Infrastruktur, damit der replizierte Computer wie erwartet funktioniert. Wenn Sie Active Directory und DNS testen möchten, können Sie einen virtuellen Computer als Domänencontroller mit DNS erstellen und per Azure Site Recovery zu Azure replizieren. Weitere Informationen finden Sie unter [Überlegungen zum Testfailover für Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Stellen Sie sicher, dass der Konfigurationsserver ausgeführt wird. Andernfalls schlägt das Failover fehl.
- Wenn Sie Datenträger von der Replikation ausgeschlossen haben, müssen Sie diese Datenträger nach einem Failover in Azure möglicherweise manuell erstellen, damit die Anwendung wie erwartet ausgeführt wird.
- Beachten Sie Folgendes, wenn Sie anstelle eines Testfailovers ein ungeplantes Failover durchführen möchten:

	- Falls möglich, sollten Sie primäre Computer herunterfahren, bevor Sie ein ungeplantes Failover ausführen. Dadurch wird sichergestellt, dass die Quell- und Replikatcomputer nicht gleichzeitig ausgeführt werden. Wenn Sie virtuelle VMware-Computer replizieren, können Sie festlegen, dass Site Recovery bestmöglichst versuchen soll, die Quellcomputer herunterzufahren. Dies funktioniert abhängig vom Zustand am primären Standort. Wenn Sie physische Server replizieren, bietet Site Recovery diese Möglichkeit nicht.
	- Beim Durchführen eines ungeplanten Failovers wird die Datenreplikation der primären Computer beendet. Das heißt, dass nach dem Beginn des Failovers keine Datenänderungen mehr übertragen werden. Außerdem wird ein ungeplantes Failover, das auf einem Wiederherstellungsplan basiert, auch dann bis zum Ende durchgeführt, wenn ein Fehler auftritt.

### Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Gehen Sie wie folgt vor, wenn Sie die Verbindung mit Azure-VMs nach dem Failover per RDP herstellen möchten:

**Auf dem lokalen Computer vor dem Failover**:

- Aktivieren Sie für den Zugriff über das Internet RDP. Stellen Sie sicher, dass TCP- und UDP-Regeln für **Öffentlich** hinzugefügt werden, und stellen Sie sicher, dass RDP unter **Windows-Firewall** > **Zugelassene Apps und Features** für alle Profile zugelassen ist.
- Aktivieren Sie für den Zugriff über eine Standort-zu-Standort-Verbindung RDP auf dem Computer, und stellen Sie sicher, dass RDP unter **Windows-Firewall** > **Zugelassene Apps und Features** für die Netzwerke vom Typ **Domäne** und **Privat** zugelassen ist.
- Installieren Sie den [Azure-VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) auf dem lokalen Computer.
- [Installieren Sie den Mobilitätsdienst manuell](#install-the-mobility-service-manually) auf Computern, anstatt den Prozessserver zur automatischen Pushinstallation des Diensts zu verwenden. Der Grund dafür ist, dass die Pushinstallation nur durchgeführt wird, nachdem der Computer für die Replikation aktiviert wurde.
- Achten Sie darauf, dass die SAN-Richtlinie des Betriebssystems auf OnlineAll festgelegt ist. [Weitere Informationen](https://support.microsoft.com/kb/3031135)
- Deaktivieren Sie den IPSec-Dienst, bevor Sie das Failover durchführen.

**Auf der Azure-VM nach einem Failover**:

- Fügen Sie einen öffentlichen Endpunkt für das RDP-Protokoll (Port 3389) hinzu, und geben Sie die Anmeldeinformationen für die Anmeldung an.
- Stellen Sie sicher, dass keine Domänenrichtlinien vorhanden sind, die das Verbinden mit einem virtuellen Computer über eine öffentliche Adresse verhindern.
- Versuchen Sie, eine Verbindung herzustellen. Wenn Sie keine Verbindung herstellen können, sollten Sie überprüfen, ob der virtuelle Computer ausgeführt wird. Weitere Tipps zur Problembehandlung finden Sie in [diesem Artikel](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


Gehen Sie wie folgt vor, wenn Sie nach dem Failover auf eine Azure-VM mit Linux über einen Secure Shell-Client (ssh) zugreifen möchten:

**Auf dem lokalen Computer vor dem Failover**:

- Stellen Sie sicher, dass der Secure Shell-Dienst auf der Azure-VM so festgelegt ist, dass er beim Systemstart automatisch gestartet wird.
- Überprüfen Sie, ob die Firewallregeln eine SSH-Verbindung damit zulassen.

**Auf der Azure-VM nach einem Failover**:

- In den Netzwerksicherheitsgruppen-Regeln auf der VM nach dem Failover und für das Azure-Subnetz, mit dem die VM verbunden ist, müssen eingehende Verbindungen zum SSH-Port zulässig sein.
- Es sollte ein öffentlicher Endpunkt erstellt werden, um eingehende Verbindungen für den SSH-Port (standardmäßig TCP-Port 22) zuzulassen.
- Wenn auf die VM über eine VPN-Verbindung (Express Route- oder Standort-zu-Standort-VPN-Verbindung) zugegriffen wird, kann der Client verwendet werden, um per SSH eine direkte Verbindung mit der VM herzustellen.

**Auf dem virtuellen Azure-Computer mit Windows/Linux nach einem Failover**:

Wenn dem virtuellen Computer oder dem Subnetz, dem der Computer angehört, eine Netzwerksicherheitsgruppe zugeordnet ist, vergewissern Sie sich, dass die Netzwerksicherheitsgruppe über eine ausgehende Regel verfügt, die HTTP/HTTPS zulässt. Vergewissern Sie sich außerdem, dass das DNS des Netzwerks, das als Ziel für das Failover des virtuellen Computers fungiert, korrekt konfiguriert ist. Andernfalls kann ein Failover-Timeout mit dem Fehler „PreFailoverWorkflow task WaitForScriptExecutionTask timed out“ (Timeout bei der PreFailoverWorkflow-Aufgabe „WaitForScriptExecutionTask“) auftreten. Ausführlichere Informationen finden Sie im [Leitfaden zur Überwachung und Problembehandlung](site-recovery-monitoring-and-troubleshooting.md#recovery) im Abschnitt zur Wiederherstellung.

## Ausführen eines Testfailovers

1. Klicken Sie zum Durchführen eines Failovers für einen einzelnen Computer unter **Einstellungen** > **Replizierte Elemente** auf den virtuellen Computer und dann auf das Symbol **+Testfailover**.

	![Testfailover](./media/site-recovery-vmware-to-azure/test-failover1.png)

2. Klicken Sie zum Durchführen eines Failovers für einen Wiederherstellungsplan unter **Einstellungen** > **Wiederherstellungspläne** mit der rechten Maustaste auf den Plan, und klicken Sie dann auf **Testfailover**. [Befolgen Sie diese Anweisungen](site-recovery-create-recovery-plans.md) zum Erstellen eines Wiederherstellungsplans.

3. Wählen Sie unter **Testfailover** das Azure-Netzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden.
4. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt verfolgen, indem Sie auf den virtuellen Computer klicken, um die Eigenschaften zu öffnen. Alternativ können Sie unter „Tresorname“ > **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** auf den Auftrag **Testfailover** klicken.
5. Gehen Sie wie folgt vor, wenn das Failover den Status **Test abschließen** erreicht hat:

	1. Zeigen Sie den virtuellen Replikatcomputer im Azure-Portal an. Prüfen Sie, ob der virtuelle Computer erfolgreich startet.
	2. Wenn Sie den Zugriff auf virtuelle Computer aus Ihrem lokalen Netzwerk eingerichtet haben, können Sie eine Remotedesktopverbindung mit dem virtuellen Computer herstellen.
	3. Klicken Sie auf **Test abschließen**, um den Test abzuschließen.

		![Testfailover](./media/site-recovery-vmware-to-azure/test-failover6.png)


	4. Klicken Sie auf **Notizen**, um alle Beobachtungen im Zusammenhang mit dem Test-Failover aufzuzeichnen und zu speichern.
	5. Klicken Sie auf **Das Test-Failover ist abgeschlossen.**, um eine automatische Bereinigung der Testumgebung durchzuführen. Nach Abschluss dieses Vorgangs wird für das Testfailover der Status **Abgeschlossen** angezeigt.
	6.  An diesem Punkt werden alle Elemente oder VMs gelöscht, die von Site Recovery während des Testfailovers automatisch erstellt wurden. Alle weiteren Elemente, die Sie für das Testfailover erstellt haben, werden nicht gelöscht.

	> [AZURE.NOTE] Sollte ein Testfailover länger als zwei Wochen ausgeführt werden, wird sein Abschluss erzwungen.


6. Nach Abschluss des Failovers sollte der Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt werden. Stellen Sie sicher, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.
7. Wenn Sie die [Vorbereitung für Verbindungen nach dem Failover](#prepare-to-connect-to-azure-vms-after-failover) durchgeführt haben, sollten Sie die Verbindung mit der Azure-VM herstellen können.

## Überwachen der Bereitstellung

Hier wird beschrieben, wie Sie die Konfigurationseinstellungen, den Status und die Integrität für die Site Recovery-Bereitstellung überwachen können:

1. Klicken Sie auf den Tresornamen, um auf das Dashboard **Zusammenfassung** zuzugreifen. In diesem Dashboard werden Site Recovery-Aufträge, Replikationsstatus, Wiederherstellungspläne, Serverzustand und Ereignisse angezeigt. Sie können die Zusammenfassung so anpassen, dass die für Sie nützlichsten Kacheln und Layouts angezeigt werden, z.B. der Status anderer Site Recovery- und Backup-Tresore.<br> ![Zusammenfassung](./media/site-recovery-vmware-to-azure/essentials.png)

2. Auf der Kachel **Integrität** können Sie Standortserver (VMM- oder Konfigurationsserver), bei denen Probleme auftreten, sowie die von Site Recovery in den letzten 24 Stunden ausgelösten Ereignisse überwachen.
3. Sie können die Replikation über die Kacheln **Replizierte Elemente**, **Wiederherstellungspläne** und **Site Recovery-Aufträge** verwalten und überwachen. Detailinformationen zu Aufträgen können Sie unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** anzeigen.


## Bereitstellen zusätzlicher Prozessserver

Wenn Sie Ihre Bereitstellung über 200 Quellcomputer oder eine gesamte tägliche Änderungsrate von 2 TB hinaus horizontal hochskalieren müssen, benötigen Sie zusätzliche Prozessserver zur Bewältigung des Datenverkehrsaufkommens.

Überprüfen Sie die [für Prozessserver empfohlenen Größen](#size-recommendations-for-the-process-server), und befolgen Sie anschließend diese Anweisungen, um den Prozessserver einzurichten. Nachdem Sie den Prozessserver eingerichtet haben, migrieren Sie Quellcomputer, damit diese ihn verwenden.

### Installieren eines zusätzlichen Prozessservers

1. Klicken Sie unter **Einstellungen** > **Site Recovery-Server** auf den Konfigurationsserver und anschließend auf **Prozessserver**.

	![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/migrate-ps1.png)

2. Klicken Sie unter **Servertyp** auf **Prozessserver (lokal)**.

	![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

3. Laden Sie die Datei für das einheitliche Setup von Site Recovery herunter, führen Sie sie zum Installieren des Prozessservers aus, und registrieren Sie sie im Tresor.
4. Wählen Sie unter **Vorbereitung** die Option **Weitere Prozessserver zum horizontalen Hochskalieren der Bereitstellung hinzufügen** aus.
5. Schließen Sie den Assistenten genau wie beim [Einrichten](#step-2-set-up-the-source-environment) des Konfigurationsservers ab.

	![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/add-ps1.png)

6. Geben Sie unter **Konfigurationsserverdetails** die IP-Adresse des Konfigurationsservers und die Passphrase ein. Um die Passphrase abzurufen, führen Sie auf dem Konfigurationsserver **<Site Recovery-Installationsordner>\\home\\sysystems\\bin\\genpassphrase.exe –n** aus.

	![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/add-ps2.png)

### Migrieren der Computer für die Verwendung des neuen Prozessservers

1. Klicken Sie unter **Einstellungen** > **Site Recovery-Server** auf den Konfigurationsserver, und erweitern Sie anschließend **Prozessserver**.

	![Prozessserver aktualisieren](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

2. Klicken Sie mit der rechten Maustaste auf den aktuell verwendeten Prozessserver, und klicken Sie auf **Wechseln**.

	![Prozessserver aktualisieren](./media/site-recovery-vmware-to-azure/migrate-ps3.png)

3. Wählen Sie unter **Zielprozessserver auswählen** den neuen Prozessserver aus, den Sie verwenden möchten, und wählen Sie anschließend die virtuellen Computer aus, für die der neue Prozessserver zuständig sein soll. Klicken Sie auf das Informationssymbol, um Informationen über den Server zu erhalten. Der durchschnittliche zum Replizieren jedes ausgewählten virtuellen Computers auf den neuen Prozessserver benötigte Speicherplatz wird angezeigt, um Sie bei Lastenentscheidungen zu unterstützen. Klicken Sie auf das Häkchen, um mit dem Replizieren auf den neuen Prozessserver zu beginnen.

## VMware-Kontoberechtigungen

Der Prozessserver kann virtuelle Computer auf einem vCenter-Server automatisch ermitteln. Zum Ausführen der automatischen Ermittlung müssen Sie [eine Rolle definieren (Azure\_Site\_Recovery)](#prepare-an-account-for-automatic-discovery), um Site Recovery den Zugriff auf den VMware-Server zu ermöglichen. Beachten Sie: Wenn Sie VMware-Computer nur zu Azure migrieren müssen und kein Failback von Azure auszuführen brauchen, können Sie eine nur zum Lesezugriff berechtigende schreibgeschützte Rolle definieren – dies ist ausreichend. Die erforderlichen Rollenberechtigungen werden in der folgenden Tabelle zusammengefasst.

**Rolle** | **Details** | **Berechtigungen**
--- | --- | ---
Rolle „Azure\_Site\_Recovery“ | Ermittlung von virtuellen VMware-Computern |Weisen Sie die folgenden Berechtigungen für den vCenter-Server zu:<br/><br/>Datastore -> Allocate space, Browse datastore, Low level file operations, Remove file, Update virtual machine files<br/><br/>Network -> Network assign<br/><br/>Resource -> Assign virtual machine to resource pool, Migrate powered off virtual machine, Migrate powered on virtual machine<br/><br/>Tasks -> Create task, update task<br/><br/>Virtual machine -> Configuration<br/><br/>Virtual machine -> Interact -> Answer question, Device connection, Configure CD media, Configure floppy media, Power off, Power on, VMware tools install<br/><br/>Virtual machine -> Inventory -> Create, Register, Unregister<br/><br/>Virtual machine -> Provisioning -> Allow virtual machine download, Allow virtual machine files upload<br/><br/>Virtual machine -> Snapshots -> Remove snapshots
vCenter-Benutzerrolle | Ermittlung von virtuellen VMware-Computern/Failover ohne Herunterfahren der Quell-VM | Weisen Sie die folgenden Berechtigungen für den vCenter-Server zu:<br/><br/>Data Center object –> Propagate to Child Object, role=Read-only <br/><br/>Der Benutzer wird auf Rechenzentrumsebene zugewiesen und hat daher Zugriff auf alle Objekte im Rechenzentrum. Wenn Sie den Zugriff einschränken möchten, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, virtuelle Computer und Netzwerke) die Rolle **No access** mit **Propagate to child object** zu.
vCenter-Benutzerrolle | Failover und Failback | Weisen Sie die folgenden Berechtigungen für den vCenter-Server zu:<br/><br/>Datacenter object – Propagate to child object, role=Azure\_Site\_Recovery<br/><br/>Der Benutzer wird auf Rechenzentrumsebene zugewiesen und hat daher Zugriff auf alle Objekte im Rechenzentrum. Wenn Sie den Zugriff einschränken möchten, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, virtuelle Computer und Netzwerke) die Rolle **No access** mit **Propagate to child object** zu.  
## Nächste Schritte

- [Informieren](site-recovery-failover.md) Sie sich über die unterschiedlichen Failoverarten.
- [Erfahren Sie mehr zum Failback](site-recovery-failback-azure-to-vmware.md) von unter Azure ausgeführten Computern auf die lokale Umgebung.

## Hinweise und Informationen zu Drittanbietersoftware

Do Not Translate or Localize

The software and firmware running in the Microsoft product or service is based on or incorporates material from the projects listed below (collectively, “Third Party Code”). Microsoft is the not original author of the Third Party Code. The original copyright notice and license, under which Microsoft received such Third Party Code, are set forth below.

The information in Section A is regarding Third Party Code components from the projects listed below. Such licenses and information are provided for informational purposes only. This Third Party Code is being relicensed to you by Microsoft under Microsoft's software licensing terms for the Microsoft product or service.

The information in Section B is regarding Third Party Code components that are being made available to you by Microsoft under the original licensing terms.

The complete file may be found on the [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft reserves all rights not expressly granted herein, whether by implication, estoppel or otherwise.

<!---HONumber=AcomDC_0921_2016-->