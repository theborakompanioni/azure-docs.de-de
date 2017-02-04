---
title: "Voraussetzungen für die Azure Site Recovery-Bereitstellung | Microsoft-Dokumentation"
description: In diesem Artikel werden die Voraussetzungen zum Einrichten der Replikation mit Azure Site Recovery beschrieben.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/04/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 92abadf68e4acbd9daae9e15dcfdce53563f6eb2
ms.openlocfilehash: a407f5ebe096db74820ff93354a636bc63894b1d

---

# <a name="site-recovery-deployment-prerequisites"></a>Voraussetzungen für die Bereitstellung von Site Recovery

Organisationen benötigen eine Strategie für die Geschäftskontinuität und Notfallwiederherstellung (BCDR), mit der bestimmt wird, wie Apps, Workloads und Daten bei geplanten und ungeplanten Ausfällen verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können. Site Recovery ist ein Azure-Dienst, der einen Beitrag zu Ihrer BCDR-Strategie leistet, indem die Replikation von lokalen physischen Servern und virtuellen Computern in die Cloud (Azure) oder in ein sekundäres Datencenter orchestriert wird. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Weitere Informationen finden Sie unter [Was ist Site Recovery?](site-recovery-overview.md)

In diesem Artikel werden die Voraussetzungen für die Bereitstellung von Site Recovery-Replikationsszenarien zusammengefasst.  

Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.



## <a name="azure-deployment-models"></a>Azure-Bereitstellungsmodelle

Azure verfügt über zwei unterschiedliche [Bereitstellungsmodelle](../azure-resource-manager/resource-manager-deployment-model.md) zum Erstellen und Verwenden von Ressourcen: das Azure Resource Manager-Modell und das klassische Modell. Außerdem verfügt Azure über zwei Portale: das [klassische Azure-Portal](https://manage.windowsazure.com/) mit Unterstützung des klassischen Bereitstellungsmodells und das [Azure-Portal](https://ms.portal.azure.com/) mit Unterstützung für beide Bereitstellungsmodelle.

Neue Site Recovery-Szenarios sollten im [Azure-Portal](https://portal.azure.com) bereitgestellt werden. Dieses Portal bietet neue Features und eine verbesserte Bereitstellungserfahrung. Tresore können im klassischen Portal verwaltet, jedoch keine neuen erstellt werden.


## <a name="azure-account-requirements"></a>Azure-Kontoanforderungen

**Anforderung** | **Details**
--- | --- 
**Azure-Konto** | Ein [Microsoft Azure](http://azure.microsoft.com/) -Konto.<br/><br/> Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung. [Erfahren Sie mehr](https://azure.microsoft.com/pricing/details/site-recovery/) über die Preise für Site Recovery.


## <a name="azure-storage-requirements"></a>Azure-Speicheranforderungen

Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden erstellt, wenn ein Failover durchgeführt wird.

**Anforderung** | **Details**
--- | --- 
[Azure-Speicherkonto](../storage/storage-introduction.md) | Sie können [GRS](../storage/storage-redundancy.md#geo-redundant-storage)- oder LRS-Speicher verwenden.<br/><br/> Wir empfehlen Ihnen die Verwendung von GRS, damit Resilienz für die Daten besteht, wenn es zu einem regionalen Ausfall kommt oder wenn die primäre Region nicht wiederhergestellt werden kann. [Weitere Informationen](https://docs.microsoft.com/en-us/azure/storage/storage-redundancy)
**Azure-Portal** | Im Azure-Portal können Sie Resource Manager-Speicher oder ein klassisches Speicherkonto verwenden.
**Storage Premium** | [Premium-Speicher](https://docs.microsoft.com/en-us/azure/storage/storage-premium-storage) wird unterstützt, wenn Sie VMware-VMs oder physische Server im Azure-Portal nach Azure replizieren.<br/><br/> Premium-Speicher wird normalerweise für virtuelle Computer verwendet, die eine konsistent hohe E/A-Leistung und geringe Latenz zum Hosten von E/A-intensiven Workloads erfordern.<br/><br/> Bei Verwendung von Premium-Speicher benötigen Sie auch ein Standardspeicherkonto zum Speichern von Replikationsprotokollen, in denen laufende Änderungen lokaler Daten erfasst werden.
**Speichereinschränkungen** | GRS wird nur im klassischen Portal unterstützt.<br/><br/> Im Azure-Portal erstellte Speicherkonten können nur innerhalb von Ressourcengruppen verschoben werden.<br/><br/> Die Replikation von Storage Premium-Konten in den Regionen „Indien, Mitte“ und „Indien, Süden“ wird zurzeit nicht unterstützt.

## <a name="azure-network-requirements"></a>Azure-Netzwerkanforderungen

Sie benötigen ein Azure-Netzwerk, mit dem nach dem Failover erstellte virtuelle Azure-Computer eine Verbindung herstellen.

**Anforderung** | **Details**
--- | ---
**Netzwerkregion** | Das Netzwerk muss sich in der gleichen Region wie der Tresor befinden.
**Azure-Portal** | Sie können ein Resource Manager-Netzwerk oder ein klassisches Netzwerk im Azure-Portal verwenden.
**Netzwerkzuordnung** | Wenn Sie von VMM zu Azure replizieren, richten Sie eine [Netzwerkzuordnung](site-recovery-network-mapping.md) zwischen VMM-VM-Netzwerken und Azure-Netzwerken ein, um sicherzustellen, dass sich Azure-VMs nach dem Failover mit entsprechenden Netzwerken verknüpfen.


## <a name="vmware-replication-requirements-to-azure"></a>Anforderungen für die VMware-Replikation (nach Azure)

**Komponente** | **Anforderung**
--- | ---
**Konfigurationsserver** | Ein lokaler, physischer oder virtueller Computer, auf dem Windows Server 2012 R2 ausgeführt wird. Alle lokalen Site Recovery-Komponenten werden auf diesem Computer installiert.<br/><br/>Für die VM-Replikation von VMware wird empfohlen, den Server als hoch verfügbare VMware-VM bereitzustellen. <br/><br/>Wenn der Server eine VMware-VM ist, sollte der Netzwerkadaptertyp VMXNET3 lauten. Wenn Sie einen anderen Netzwerkadaptertyp verwenden, müssen Sie auf dem vSphere 5.5-Server ein [VMware-Update](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1) installieren.<br/><br/>Der Server muss über eine statische IP-Adresse verfügen.<br/><br/>Der Server darf kein Domänencontroller sein.<br/><br/>Der Hostname des Servers darf höchstens 15 Zeichen umfassen.<br/><br/>Das Betriebssystem darf nur auf Englisch sein.<br/><br/> Installieren des VMware vSphere PowerCLI 6.0- Servers<br/><br/>Der Konfigurationsserver benötigt Zugriff auf das Internet. Folgender ausgehender Zugriff ist erforderlich:<br/><br/>Temporärer Zugriff auf HTTP 80 während des Setups von Site Recovery-Komponenten (zum Herunterladen von MySQL)<br/><br/>Dauerhafter ausgehender Zugriff auf HTTPS 443 für die Replikationsverwaltung<br/><br/>Dauerhafter ausgehender Zugriff auf HTTPS 9443 für den Replikationsdatenverkehr (dieser Port kann geändert werden)<br/><br/> Lassen Sie diese URL für den MySQL-Download zu: ``http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi``<br/><br/> Der Server benötigt außerdem Zugriff auf [diese URLs](#requirements-for-url-access).
**Prozessserver** | Wird standardmäßig auf dem Konfigurationsserver installiert.<br/><br/> Fungiert als Replikationsgateway. Sie empfängt Replikationsdaten aus geschützten Quellcomputern, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet die Daten an den Azure-Speicher. Außerdem wickelt er die Pushinstallation des Mobilitätsdiensts auf geschützten Computern ab und führt die automatische Ermittlung von virtuellen VMware-Computern durch. Bei zunehmender Größe der Bereitstellung können Sie zusätzlich separate dedizierte Prozessserver hinzufügen, um steigende Mengen von Replikationsdatenverkehr zu bewältigen.
**Masterzielserver** | Wird standardmäßig auf dem Konfigurationsserver installiert. Verarbeitet die Replikationsdaten während des Failbacks von Azure.
**vSphere-Hosts** | Mindestens einen VMware vSphere-Hypervisor.<br/><br/>Auf den Hypervisoren muss vSphere in einer der Versionen 6.0, 5.5 oder 5.1 mit den neuesten Updates ausgeführt werden.<br/><br/>Ihre vSphere-Hosts und vCenter-Server sollten sich im gleichen Netzwerk befinden wie der Prozessserver (dabei handelt es sich um das Netzwerk, in dem sich der Konfigurationsserver befindet, es sei denn, Sie haben einen dedizierten Prozessserver eingerichtet). 
**vCenter-Server** | Es wird empfohlen, dass Sie einen VMware vCenter-Server zum Verwalten Ihrer vSphere-Hosts bereitstellen. Auf diesem Server muss vCenter 6.0 oder 5.5 mit den neuesten Updates ausgeführt werden.<br/><br/>**Einschränkung**: Site Recovery unterstützt die neuen Features von vCenter und vSphere 6.0 nicht. Hierzu zählen etwa Cross vCenter vMotion, virtuelle Volumes und Storage DRS. Site Recovery unterstützt nur Features, die auch in Version 5.5 verfügbar waren.

### <a name="vmware-vm-requirements-to-azure"></a>VMware-VM-Anforderungen (nach Azure)

**Komponente** | **Anforderung**
--- | ---
**Virtuelle VMware-Computer** | Auf replizierten VMs sollten VMware-Tools installiert sein und ausgeführt werden.<br/><br/> VMs müssen die [Voraussetzungen für Azure](site-recovery-support-matrix.md#support-for-azure-vms) zum Erstellen von Azure-VMs erfüllen.<br/><br/>Die Kapazität der einzelnen Datenträger auf geschützten Computern darf maximal 1.023 GB betragen. Ein virtueller Computer kann bis zu 64 Datenträger haben (also bis zu 64 TB). <br/><br/>Für die Komponenteninstallation sind mindestens 2 GB verfügbarer Speicherplatz auf dem Installationslaufwerk erforderlich.<br/><br/>**Einschränkung**: Der Schutz von VMs mit verschlüsselten Festplatten wird nicht unterstützt.<br/><br/>**Einschränkung**: Gastcluster mit freigegebenen Datenträgern werden nicht unterstützt.<br/><br/>**Port 20004** sollte auf der lokalen VM-Firewall geöffnet werden, wenn Sie Multi-VM-Konsistenz aktivieren möchten.<br/><br/>Computer mit Unified Extensible Firmware Interface (UEFI)/Extensible Firmware Interface (EFI) werden nicht unterstützt.<br/><br/>Computernamen dürfen zwischen 1 und 63 Zeichen enthalten (Buchstaben, Zahlen und Bindestriche). Der Name muss mit einem Buchstaben oder einer Zahl beginnen und mit einem Buchstaben oder einer Zahl enden. Sie können den Azure-Namen ändern, nachdem Sie die Replikation für einen Computer aktiviert haben.<br/><br/>Wenn auf dem virtuellen Quellcomputer NIC-Teamvorgänge ausgeführt wurden, wird er nach dem Failover zu Azure in eine einzelne NIC konvertiert.<br/><br/>Wenn geschützte virtuelle Computer über einen iSCSI-Datenträger verfügen, konvertiert Site Recovery die iSCSI-Datenträger der geschützten VMs in eine VHD-Datei, wenn ein Failover der VM zu Azure durchgeführt wird. Wenn das iSCSI-Ziel von der Azure-VM erreicht werden kann, wird eine Verbindung hergestellt. Dadurch sind für ihn effektiv zwei Datenträger sichtbar – der VHD-Datenträger auf der Azure-VM und der iSCSI-Quelldatenträger. In diesem Fall müssen Sie die Verbindung mit dem iSCSI-Ziel trennen, das auf der Azure-VM erscheint.
**VMs unter Windows** | Auf Windows-Computern sollte ein [unterstütztes](site-recovery-support-matrix.md#support-for-replicated-machines) 64-Bit-Betriebssystem ausgeführt werden.<br/><br/> Das Betriebssystem sollte auf dem Laufwerk C:\ installiert werden.<br/><br/> Bei dem Betriebssystem-Datenträger sollte es sich um einen Windows-Basisdatenträger anstelle eines dynamischen Datenträgers handeln. Der Datenträger kann dynamisch sein.<br/><br/> Site Recovery unterstützt virtuelle Computer mit einem RDM-Datenträger. Beim Failback wird der RDM-Datenträger von Site Recovery wiederverwendet, sofern die ursprüngliche Quell-VM und der RDM-Datenträger verfügbar sind. Wenn sie nicht verfügbar sind, erstellt Site Recovery beim Failback für jeden Datenträger eine neue VMDK-Datei.
**VMs mit Linux** | Auf Linux-Computern sollte ein [unterstütztes Betriebssystem](site-recovery-support-matrix.md#support-for-replicated-machines) ausgeführt werden.<br/><br/> Dateien vom Typ „/etc/hosts“ auf geschützten Computern müssen Einträge enthalten, die den Namen des lokalen Hosts den IP-Adressen aller Netzwerkkarten zuordnen.<br/><br/> Der Hostname, Bereitstellungspunkte, Gerätenamen und Linux-Systempfade und -Dateinamen (z. B./etc/, /usr) sollten nur englische Zeichen umfassen.<br/><br/> Speicher muss [unterstützt](site-recovery-support-matrix.md#support-for-replicated-machines) werden.<br/><br/> Site Recovery unterstützt virtuelle Computer mit einem RDM-Datenträger.  Beim Failback für Linux wird der RDM-Datenträger nicht wiederverwendet. Anstelle dessen erstellt Site Recovery für jeden entsprechenden RDM-Datenträger eine neue VMDK-Datei.<br/><br/> Stellen Sie sicher, dass Sie die Einstellung „disk.enableUUID=true“ in den Konfigurationsparametern der VM in VMware festlegen. Erstellen Sie den Eintrag, wenn er nicht vorhanden ist. Er ist erforderlich, um der VMDK-Datei eine konsistente UUID bereitzustellen, damit sie ordnungsgemäß bereitgestellt wird. Durch das Hinzufügen dieser Einstellung wird außerdem sichergestellt, dass während des Failbacks nur Deltaänderungen zurück an den lokalen Standort übertragen werden, und keine vollständige Replikation.



## <a name="physical-servers-replication-requirements-to-azure"></a>Anforderungen für die Replikation physischer Server (nach Azure)

**Komponente** | **Anforderung**
--- | ---
**Konfigurationsserver** | Ein lokaler, physischer oder virtueller Computer, auf dem Windows Server 2012 R2 ausgeführt wird. Alle lokalen Site Recovery-Komponenten werden auf diesem Computer installiert.<br/><br/> Wenn Sie physische Computer replizieren, kann der Computer ein physischer Server sein.<br/><br/>  Der Konfigurationsserver benötigt Zugriff auf das Internet. Folgender ausgehender Zugriff ist erforderlich:<br/><br/> Temporärer Zugriff auf HTTP 80 während des Setups von Site Recovery-Komponenten (zum Herunterladen von MySQL)<br/><br/>Dauerhafter ausgehender Zugriff auf HTTPS 443 für die Replikationsverwaltung<br/><br/>Dauerhafter ausgehender Zugriff auf HTTPS 9443 für den Replikationsdatenverkehr (dieser Port kann geändert werden)<br/><br/> Lassen Sie diese URL für den MySQL-Download zu: ``http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi``<br/><br/> Der Server benötigt außerdem Zugriff auf [diese URLs](#requirements-for-url-access).
**Failback** | Das Failback von Azure erfolgt immer auf VMware-VMs. Dies gilt auch, wenn das Failover für einen physischen Server durchgeführt wird. Wenn Sie den Konfigurationsserver nicht als VMware-VM bereitstellen, müssen Sie eine VMware-VM einrichten, um Failbackdatenverkehr vor dem Failback zu empfangen.

### <a name="physical-machine-requirements-to-azure"></a>Anforderungen für physische Computer (nach Azure)

**Komponente** | **Anforderung**
--- | ---
**Physische Server, auf denen Windows ausgeführt wird** | Auf Windows-Computern sollte ein [unterstütztes](site-recovery-support-matrix.md#support-for-replicated-machines) 64-Bit-Betriebssystem ausgeführt werden.<br/><br/> Das Betriebssystem sollte auf dem Laufwerk C:\ installiert werden.<br/><br/> Bei dem Betriebssystem-Datenträger sollte es sich um einen Windows-Basisdatenträger anstelle eines dynamischen Datenträgers handeln. Der Datenträger kann dynamisch sein.<br/><br/> Site Recovery unterstützt virtuelle Computer mit einem RDM-Datenträger. Beim Failback wird der RDM-Datenträger von Site Recovery wiederverwendet, sofern die ursprüngliche Quell-VM und der RDM-Datenträger verfügbar sind. Wenn sie nicht verfügbar sind, erstellt Site Recovery beim Failback für jeden Datenträger eine neue VMDK-Datei.
**Physische Server, auf denen Linux ausgeführt wird** | Auf Linux-Computern sollte ein [unterstütztes Betriebssystem](site-recovery-support-matrix.md#support-for-replicated-machines) ausgeführt werden.<br/><br/> Dateien vom Typ „/etc/hosts“ auf geschützten Computern müssen Einträge enthalten, die den Namen des lokalen Hosts den IP-Adressen aller Netzwerkkarten zuordnen.<br/><br/> Der Hostname, Bereitstellungspunkte, Gerätenamen und Linux-Systempfade und -Dateinamen (z. B./etc/, /usr) sollten nur englische Zeichen umfassen.<br/><br/> Speicher muss [unterstützt](site-recovery-support-matrix.md#support-for-replicated-machines) werden.

## <a name="hyper-v-replication-requirements-to-azure"></a>Anforderungen für die Hyper-V-Replikation (nach Azure)

**Komponente** | **Anforderung**
--- | ---
**VMM (optional)** | Optional können Sie virtuelle Computer auf in VMM-Clouds verwalteten Hyper-V-Hosts replizieren.<br/><br/> Wenn Sie VMM nicht verwenden, fassen Sie einen oder mehrere Hyper-V-Hosts oder -Cluster in Hyper-V-Sites zusammen und richten die Replikation für einen Standort ein.<br/><br/> Wenn Sie VMM verwenden, sollte darauf System Center 2012 R2 ausgeführt werden.<br/><br/> Wenn Sie VMM verwenden, sollte für jeden VMM-Server mindestens eine Cloud konfiguriert sein. Eine Cloud sollte eine oder mehrere VMM-Hostgruppen sowie einen oder mehrere Hyper-V-Hostserver bzw. -Cluster in jeder Hostgruppe enthalten.<br/><br/> Der VMM-Server sollte mit dem Internet verbunden sein, und auf die [erforderlichen URLs](#requirements-for-url-access) zugreifen können.<br/><br/> Wenn Sie auf dem VMM-Server über Firewallregeln verfügen, die auf IP-Adressen basieren, sollten Sie sicherstellen, dass die Regeln die Kommunikation mit Azure zulassen.<br/><br/> Lassen Sie die [IP-Bereiche für Azure-Rechenzentren](https://www.microsoft.com/download/confirmation.aspx?id=41653) und den HTTPS-Port (443) zu.<br/><br/> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ zu.
**Hyper-V-Hosts** | Auf Hyper-V-Hostservern muss mindestens **Windows Server 2012 R2** mit der Hyper-V-Rolle oder **Microsoft Hyper-V Server 2012 R2** ausgeführt werden, und die neuesten Updates müssen installiert sein.<br/><br/> Ein Hyper-V-Server muss mindestens einen virtuellen Computer enthalten.<br/><br/>Hyper-V-Server müssen entweder direkt oder über einen Proxy mit dem Internet verbunden sein.<br/><br/>Auf Hyper-V-Servern sollten die in [Artikel 2961977](https://support.microsoft.com/kb/2961977) genannten Fehlerbehebungen installiert sein.<br/><br/>Hyper-V-Hostserver benötigen Internetzugriff für die Datenreplikation nach Azure sowie Zugriff auf die [erforderlichen URLs](#requirements-for-url-access). Lassen Sie außerdem die [IP-Bereiche für das Azure-Rechenzentrum](https://www.microsoft.com/download/confirmation.aspx?id=41653) und den HTTPS-Port (443) zu.<br/><br/> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ zu.<br/><br/> Wenn sich Hyper-V-Hosts in VMM-Clouds befinden, muss die in [KB 2961977](https://support.microsoft.com/en-us/kb/2961977) beschriebene Lösung installiert sein.

### <a name="hyper-v-vm-requirements-to-azure"></a>Hyper-V-VM-Anforderungen (nach Azure)

**Komponente** | **Anforderung**
--- | ---
**VM-Compliance** |Stellen Sie vor dem Durchführen des Failovers für eine VM sicher, dass der Name, der dem virtuellen Azure-Computer zugewiesen wird, die [Voraussetzungen für Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllt. Sie können den Namen ändern, nachdem Sie die Replikation für die VM aktiviert haben.
**Datenträger** | Die individuelle Datenträgerkapazität auf geschützten Computern sollte nicht mehr als 1023 GB betragen. Ein virtueller Computer kann bis zu 64 Datenträger haben (also bis zu 64 TB).<br/><br/> Gastcluster mit freigegebenen Datenträgern werden nicht unterstützt.
**UEFI** | Das Starten über Unified Extensible Firmware Interface (UEFI)/Extensible Firmware Interface (EFI) wird nicht unterstützt.
**NIC-Teaming** | Wenn auf dem virtuellen Quellcomputer NIC-Teamvorgänge ausgeführt wurden, wird er nach dem Failover zu Azure in eine einzelne NIC konvertiert.
**Linux (statisch)** | Das Schützen von Hyper-V-VMs, auf denen Linux mit einer statischen IP-Adresse ausgeführt wird, wird nicht unterstützt.



## <a name="vmwarephysical-servers-replication-requirements-secondary-site"></a>Anforderungen für die Replikation physischer/VMware-Server (sekundärer Standort)

Dieses Szenario wird durch das Produkt InMage behandelt, das Sie aus dem Site Recovery-Tresor herunterladen können, wenn Sie VMware-VMs oder physische Server an einen sekundären Standort replizieren möchten. Beachten Sie, dass dieses Szenario nicht im Azure-Portal verfügbar ist.

**Komponente** | **Anforderung**
--- | ---
**Primärer Standort** | Sie benötigen einen Prozessserver für Zwischenspeicherung, Komprimierung und Datenoptimierung. Dieser Server behandelt auch die Pushinstallation des einheitlichen Agents, der auf jedem zu replizierenden Computer erforderlich ist.<br/><br/> Wenn Sie VMware-VMs replizieren möchten, benötigen Sie einen oder mehrere VMware ESX/ESXi-Server und optional einen VMware vCenter-Server.
**Sekundärer Standort** | Sie benötigen einen am sekundären Standort eingerichteten Konfigurationsserver, um Ihre Umgebung zu konfigurieren und zu verwalten.<br/><br/> Ein Masterzielserver wird am sekundären Standort installiert, um die replizierten Daten zu speichern.<br/><br/> Ein vContinuum-Server wird standardmäßig auf dem Masterzielserver installiert und bietet eine Konsole zum Verwalten und Überwachen Ihrer Umgebung.

### <a name="vmware-vmphysical-machine-requirements-secondary-site"></a>Anforderungen für VMware-VM/physischen Computer (sekundärer Standort)

Überprüfen Sie die Anforderungen in der [InMage-Supportmatrix](https://aka.ms/asr-scout-cm).


## <a name="hyper-v-replication-requirements-secondary-site"></a>Anforderungen für die Hyper-V-Replikation (sekundärer Standort)

**Komponente** | **Anforderung**
--- | ---
**VMM** | Der Hyper-V-Host muss in System Center VMM-Clouds verwaltet werden, um Hyper-V-VMs an einen sekundären Standort zu replizieren. <br/><br/> Unter VMM muss mindestens System Center 2012 SP1 mit den neuesten Updates ausgeführt werden.<br/><br/> Wir empfehlen Ihnen, am primären und am sekundären Standort je einen VMM-Server bereitzustellen. Sie können Replikationen zwischen [verschiedenen Clouds auf dem gleichen VMM-Server](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-single-vmm) durchführen, allerdings müssen in diesem Szenario einige Einstellungen manuell konfiguriert werden.<br/><br/> Der VMM-Server sollte mit dem Internet verbunden sein, und auf die [erforderlichen URLs](#requirements-for-url-access) zugreifen können.<br/><br/> Wenn Sie auf dem VMM-Server über Firewallregeln verfügen, die auf IP-Adressen basieren, sollten Sie sicherstellen, dass die Regeln die Kommunikation mit Azure zulassen.<br/><br/> Lassen Sie die [IP-Bereiche für Azure-Rechenzentren](https://www.microsoft.com/download/confirmation.aspx?id=41653) und den HTTPS-Port (443) zu.<br/><br/> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ zu.
**Hyper-V** | Auf den Hyper-V-Servern muss mindestens Windows Server 2012 mit der Hyper-V-Rolle ausgeführt werden, und die neuesten Updates müssen installiert sein.<br/><br/> Der Hyper-V-Server muss mindestens einen virtuellen Computer enthalten.<br/><br/> Hyper-V-Hosts sollten sich in Hostgruppen auf dem primären und dem sekundären VMM-Server befinden.<br/><br/> Wenn Sie Hyper-V in einem Cluster unter Windows Server 2012 R2 ausführen, installieren Sie das Update [2961977](https://support.microsoft.com/kb/2961977). In einem Hyper-V-Cluster unter Windows Server 2012 wird der Clusterbroker nicht automatisch erstellt, falls Sie über einen auf statischen IP-Adressen basierenden Cluster verfügen. [Weitere Informationen](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx) zur manuellen Konfiguration.

### <a name="hyper-v-vm-requirements-secondary-site"></a>Hyper-V-VM-Anforderungen (sekundärer Standort)

**Komponente** | **Details**
--- | ---
**VMM-Cloud** | Virtuelle Computer müssen sich auf dem Hyper-V-Host in VMM-Clouds befinden.






## <a name="url-access-requirements"></a>URL-Zugriffsanforderungen

Diese URLs sollten von VMware-, VMM- und Hyper-V-Hostservern verfügbar sein.

**URL** | **VMM zu VMM** | **VMM zu Azure** | **Hyper-V in Azure** | **VMware zu Azure** 
--- | --- | --- | --- | ---
``*.accesscontrol.windows.net`` | Zulassen | Zulassen | Zulassen | Zulassen
``*.backup.windowsazure.com`` | Nicht erforderlich | Zulassen | Zulassen | Zulassen
``*.hypervrecoverymanager.windowsazure.com`` | Zulassen | Zulassen | Zulassen | Zulassen
``*.store.core.windows.net`` | Zulassen | Zulassen | Zulassen | Zulassen
``*.blob.core.windows.net`` | Nicht erforderlich | Zulassen | Zulassen | Zulassen
``https://www.msftncsi.com/ncsi.txt`` | Zulassen | Zulassen | Zulassen | Zulassen
``https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi`` | Nicht erforderlich | Nicht erforderlich | Nicht erforderlich | Für SQL-Download zulassen
``time.windows.com`` | Zulassen | Zulassen | Zulassen | Zulassen
``time.nist.gov`` | Zulassen | Zulassen | Zulassen | Zulassen

## <a name="failover-requirements"></a>Failoveranforderungen

**Szenario** | **Test** | **Geplant** | **Ungeplant**
--- | --- | --- | ---
VMware zu Azure | Unterstützt | Nicht unterstützt | Unterstützt
Physisch nach Azure | Unterstützt | Nicht unterstützt | Unterstützt
Hyper-V (VMM) nach Azure | Unterstützt | Unterstützt | Unterstützt
Hyper-V (ohne VMM) nach Azure | Unterstützt | Unterstützt | Nicht unterstützt
Hyper-V (primärer VMM/Cloud) nach sekundärem Standort | Unterstützt | Unterstützt | Unterstützt

## <a name="failback-requirements"></a>Failbackanforderungen

**Szenario** | **Test** | **Geplant** | **Ungeplant**
--- | --- | --- | ---
**Azure nach VMware** | Nicht unterstützt | Nicht unterstützt | Unterstützt
**Azure nach physisch** | Nicht unterstützt | Nicht unterstützt | Nur nach VMware unterstützt
**Azure nach VMM** | Nicht unterstützt | Unterstützt | Nicht unterstützt
**Azure nach Hyper-V (ohne VMM)** | Nicht unterstützt | Unterstützt | Nicht unterstützt
**Hyper-V (sekundärer VMM/Cloud) nach primärem Standort** | Unterstützt | Unterstützt | Unterstützt

### <a name="failback-from-azure-to-vmware-vms"></a>Failback von Azure nach VMware-VMs

**Komponente** | **Details**
--- | ---
** Temporärer Prozessserver in Azure** | Wenn Sie nach einem Failover ein Failback aus Azure durchführen möchten, müssen Sie eine als Prozessserver konfigurierte Azure-VM für die Replikation aus Azure einrichten. Nach Beendigung des Failbacks können Sie diese VM löschen.
**VPN-Verbindung** | Für das Failback benötigen Sie eine VPN-Verbindung (oder Azure ExpressRoute) zwischen dem Azure-Netzwerk und dem lokalen Standort.
**Separater lokaler Masterzielserver** | Der lokale Masterzielserver verarbeitet das Failback. Der Masterzielserver wird standardmäßig auf dem Verwaltungsserver installiert, aber wenn Sie Failbacks für größere Mengen von Datenverkehr durchführen, sollten Sie für diese Zwecke einen separaten lokalen Masterzielserver einrichten.



### <a name="failback-from-azure-to-physical-machines"></a>Failback von Azure auf physische Computer

**Komponente** | **Details**
--- | ---
**Failback vom Typ „physisch zu physisch“** | Dies wird nicht unterstützt. Dies bedeutet, dass ein Failback auf eine VMware-VM erforderlich ist, wenn Sie für physische Server ein Failover zu Azure mit anschließendem Failback durchführen möchten. Ein Failback zu einem physischen Server ist nicht möglich.
**VM** | Sie benötigen eine lokale VMware-VM, auf die Sie das Failback ausführen können.
**Konfigurationsserver** | Wenn Sie den lokalen Konfigurationsserver nicht als VMware-VM bereitstellen, müssen Sie einen separaten Masterzielserver als VMware-VM einrichten. Dies ist erforderlich, weil der Masterzielserver mit dem VMware-Speicher interagiert und diesem Speicher zugeordnet wird, um die Datenträger auf einer VMware-VM wiederherzustellen.



## <a name="deployment-optimization"></a>Optimierung der Bereitstellung

Mit den folgenden Tipps können Sie Ihre Bereitstellung optimieren und skalieren.

- **Volumegröße des Betriebssystems**: Wenn Sie einen virtuellen Computer zu Azure replizieren, muss das Volume des Betriebssystems kleiner als 1 TB sein. Falls Sie mehr als ein Volume nutzen, können Sie diese manuell auf einen anderen Datenträger verschieben, bevor Sie mit der Bereitstellung beginnen.
- **Größe des Datenträgers**: Wenn Sie zu Azure replizieren, können Sie auf einem virtuellen Computer bis zu 64 Datenträger für Daten nutzen, die jeweils maximal 1 TB groß sind. Für einen virtuellen Computer mit ~64 TB können Sie die Replikation und das Failover effektiv durchführen.
- **Einschränkungen des Wiederherstellungsplans**: Für Site Recovery ist eine Skalierung auf Tausende von virtuellen Computern möglich. Wiederherstellungspläne wurden als Modell für Anwendungen entwickelt, für die das Failover zusammen erfolgen soll. Die Anzahl von Computern in einem Wiederherstellungsplan wird daher auf 50 beschränkt.
- **Einschränkungen für Azure-Dienste**: Jedes Azure-Abonnement weist eine Reihe von standardmäßigen Einschränkungen in Bezug auf Kerne, Clouddienste usw. auf. Wir empfehlen Ihnen, ein Testfailover auszuführen, um die Verfügbarkeit von Ressourcen in Ihrem Abonnement zu überprüfen. Sie können diese Beschränkungen über den Azure-Support ändern.
- **Kapazitätsplanung**: Erfahren Sie mehr über die [Kapazitätsplanung](site-recovery-capacity-planner.md) für Site Recovery.
- **Replikationsbandbreite**: Beachten Sie Folgendes, falls die Replikationsbandbreite knapp ist:
- **ExpressRoute**: Für Site Recovery können Azure ExpressRoute und WAN-Optimierer wie Riverbed verwendet werden. [Erfahren Sie mehr](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) zu ExpressRoute.
- **Replikationsdatenverkehr**: Site Recovery führt eine intelligente anfängliche Replikation durch, bei der nur Datenblöcke verwendet werden, und nicht die gesamte VHD. Während der laufenden Replikation werden nur Änderungen repliziert.
- **Netzwerkdatenverkehr**: Sie können Netzwerkdatenverkehr steuern, der für die Replikation verwendet wird, indem Sie [Windows-Dienstqualität](https://technet.microsoft.com/library/hh967468.aspx) mit einer Richtlinie nutzen, die auf der IP-Zieladresse und dem Port basiert.  Falls Sie die Replikation in Azure Site Recovery mit dem Azure Backup-Agent durchführen, können Sie die Drosselung für diesen Agent konfigurieren. [Weitere Informationen](https://support.microsoft.com/kb/3056159)
- **RTO**: Um die Recovery Time Objective (RTO) zu messen, die mit Site Recovery zu erwarten ist, sollten Sie ein Testfailover ausführen und die Site Recovery-Aufträge anzeigen, um zu analysieren, wie lange die Durchführung der Vorgänge dauert. Falls Sie das Failover zu Azure nutzen, erzielen Sie den besten RTO-Wert, wenn Sie alle manuellen Aktionen automatisieren. Integrieren Sie hierfür die Azure-Automatisierung und Wiederherstellungspläne.
- **RPO**: Site Recovery unterstützt beim Replizieren zu Azure eine nahezu synchrone Recovery Point Objective (RPO). Dies setzt ausreichende Bandbreite zwischen Ihrem Rechenzentrum und Azure voraus.



## <a name="next-steps"></a>Nächste Schritte
Lesen Sie nach der Überprüfung der allgemeinen Bereitstellungsanforderungen die detaillierten Voraussetzungen, und stellen Sie ein Szenario bereit.

* [Repliziert virtuelle VMware-Computer zu Azure](site-recovery-vmware-to-azure.md)
* [Replizieren physischer Server in Azure](site-recovery-vmware-to-azure.md)
* [Replizieren von Hyper-V-Servern in VMM-Clouds in Azure](site-recovery-vmm-to-azure.md)
* [Replizieren virtueller Hyper-V-Maschinen (ohne VMM) in Azure](site-recovery-hyper-v-site-to-azure.md)
* [Replizieren von Hyper-V-VMs an einem sekundären Standort](site-recovery-vmm-to-vmm.md)
* [Replizieren von Hyper-V-VMs an einem sekundären Standort mit SAN](site-recovery-vmm-san.md)
* [Replizieren von Hyper-V-VMs mit einem einzelnen VMM-Server](site-recovery-single-vmm.md)



<!--HONumber=Dec16_HO1-->


