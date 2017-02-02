---
title: Replizieren von virtuellen VMware-Computern und physischen Servern zu Azure mithilfe von Azure Site Recovery im Azure-Portal | Microsoft Docs
description: Beschreibt, wie Azure Site Recovery bereitgestellt wird, um die Replikation, das Failover und die Wiederherstellung von lokalen virtuellen VMware-Computern und physischen Windows-/Linux-Servern auf Azure mithilfe des Azure-Portals zu orchestrieren.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 1268d29b0d9c4368f62918758836a73c757c0c8d
ms.openlocfilehash: 20ffa261ef17847a665e7c83defeb19e9029fb63


---
# <a name="replicate-vmware-virtual-machines-and-physical-machines-to-azure-with-azure-site-recovery-using-the-azure-portal"></a>Replizieren von virtuellen VMware-Computern und physischen Computern nach Azure mithilfe von Azure Site Recovery mit dem Azure-Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](site-recovery-vmware-to-azure.md)
> * [Klassisches Azure](site-recovery-vmware-to-azure-classic.md)



Willkommen beim Azure Site Recovery-Dienst!

Der Azure Site Recovery-Dienst unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR). Site Recovery dient zum Orchestrieren der Replikation von lokalen physischen Servern und virtuellen Computern in die Cloud (Azure) oder in ein sekundäres Datencenter. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Weitere Informationen finden Sie unter [Was ist Azure Site Recovery?](site-recovery-overview.md)

Dieser Artikel beschreibt die Replikation lokaler virtueller VMware-Computer oder physischer Windows-/Linux-Server zu Azure mithilfe von Azure Site Recovery im Azure-Portal.

Nachdem Sie diesen Artikel gelesen haben, können Sie unten im Disqus-Kommentarbereich Ihre Kommentare eingeben. Im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)können Sie technische Fragen stellen.

## <a name="quick-summary"></a>Kurze Zusammenfassung
Für eine vollständige Bereitstellung empfiehlt es sich dringend, alle Schritte in diesem Artikel auszuführen. Sollten Sie wenig Zeit haben, finden Sie hier eine kurze Zusammenfassung.

| **Bereich** | **Details** |
| --- | --- |
| **Bereitstellungsszenario** |Replizieren von VMware-VMs oder physischen Servern (Windows/Linux) zu Azure mithilfe des Azure-Portals |
| **Lokale Anforderungen** |Lokaler Computer mit Konfigurationsserver, Prozessserver, Masterzielserver<br/><br/> Konfigurationsserver mit einer Internetverbindung und Zugriff (direkt oder über einen Proxy) auf bestimmte URLs [Details](#configuration-server-or-additional-process-server-prerequisites) |
| **Anforderungen für Azure** |Azure-Konto<br/><br/> Recovery Services-Tresor <br/><br/> LRS- oder GRS-Speicherkonto in der Tresorregion<br/><br/> Premium- oder Standard-Speicherkonto<br/><br/> Virtuelles Azure-Netzwerk in der Tresorregion. [Details](#azure-prerequisites) |
| **Azure-Einschränkungen** |Bei Verwendung von GRS benötigen Sie ein weiteres LRS-Konto für die Protokollierung.<br/><br/> Im Azure-Portal erstellte Speicherkonten können nur innerhalb von Ressourcengruppen verschoben werden.<br/><br/> Die Replikation von Premium-Speicherkonten in den Regionen „Indien, Mitte“ und „Indien, Süden“ wird zurzeit nicht unterstützt. |
| **Windows-Replikation** |Windows 64-Bit auf VMware-VMs oder physischen Servern:<br/><br/> Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit mindestens SP1. [Details](#replicated-machine-prerequisites) |
| **Linux-Replikation** |Linux auf VMware-VMs oder physischen Servern:<br/><br/>Red Hat Enterprise Linux 6.7, 7.1, 7.2<br/><br/> CentOS 6.5, 6.6, 6.7, 7.0, 7.1, 7.2<br/><br/> Oracle Enterprise Linux 6.4, 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird<br/><br/> SUSE Linux Enterprise Server 11 SP3. [Details](#replicated-machine-prerequisites) |
| **Agent** |Der Mobilitätsdienstagent wird auf jedem replizierten Computer installiert.<br/><br/> Manuelles Installieren oder Pushinstallation vom Prozessserver. [Details](#install-the-mobility-service) |
| **Replikationsanforderungen** |Replizierte Computer müssen den [Azure-Voraussetzungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) entsprechen.<br/><br/> VMs mit verschlüsselten Festplatten können nicht repliziert werden.<br/><br/> Gastcluster mit freigegebenen Datenträgern werden nicht unterstützt.<br/><br/> Sie können bestimmte Basisdatenträger von der Replikation ausschließen, jedoch keine Betriebssysteme oder dynamischen Datenträger.<br/><br/> Für Windows-Computer sollte sich der Betriebssystemdatenträger auf Laufwerk C befinden und nicht dynamisch sein. [Weitere Informationen](#replicated-machine-prerequisites) |
| **VMware-Anforderungen** | Ein oder mehrere VMware vSphere-Server (6.0, 5.5 oder 5.1 mit neuesten Updates). Sie sollten sich im selben Netzwerk wie der Konfigurationsserver (oder Prozessserver, wenn Sie ihn dediziert eingerichtet haben) befinden.<br/><br/> Wir empfehlen einen vCenter-Server zum Verwalten von vSphere-Hosts (Version 6.0 oder 5.5 mit neuesten Updates) |
| **VMware-Einschränkungen** |Site Recovery unterstützt neue Features von vCenter und vSphere 6.0 nicht. Hierzu zählen etwa Cross vCenter vMotion, Virtual Volumes und Storage DRS. Es werden nur Features unterstützt, die auch in Version 5.5 verfügbar waren. |
| **Bereitstellungsschritte** | **1)** Vorbereiten von Azure (Abonnement, Speicher, Netzwerk) -> **2)** Lokal vorbereiten (Konfigurationsservercomputer, VMware-Konto) -> **3)** Erstellen des Recovery Services-Tresors -> **4)** Einrichten des Konfigurationsservers -> **5)**Konfigurieren der Replikationseinstellungen - > **6)** Vorbereiten der Bereitstellung des Mobilitätsdienstagents -> **7)** Aktivieren der Replikation -> **8)** Testen der Replikation und Failover. |
| **Failback** | Failback nur für VMware, auch wenn Sie physische Server replizieren<br/><br/> Sie benötigen ein VPN oder Azure Express Route zwischen Azure und dem primären Standort.<br/><br/> Sie benötigen einen temporären Prozessserver, der als Azure-VM eingerichtet ist. Sie können diesen erstellen, wenn Sie für das Failback bereit sind, und ihn nach Abschluss des Failbacks wieder löschen. |

## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery im Azure-Portal
Azure verfügt über zwei verschiedene [Bereitstellungsmodelle](../azure-resource-manager/resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen – die Azure Resource Manager- und die klassische Bereitstellung. Azure verfügt zudem über zwei Portale – das klassische Azure-Portal und das Azure-Portal.

Dieser Artikel beschreibt die Bereitstellung im Azure-Portal, die neue Features bereitstellt und eine optimierte Bereitstellungserfahrung ermöglicht. Das klassische Portal kann verwendet werden, um bestehende Tresore zu verwalten. Sie können keine neuen Tresore mithilfe des klassischen Azure-Portals erstellen. 


## <a name="site-recovery-in-your-business"></a>Site Recovery in Ihrem Unternehmen
Organisationen benötigen eine BCDR-Strategie, die bestimmt, wie Apps und Daten bei geplanten und ungeplanten Ausfällen verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können. Site Recovery bietet folgende Vorteile:

* Gibt den externen Schutz von geschäftlichen Workloads an, die auf VMware-VMs und auf physischen Servern ausgeführt werden.
* Gibt einen einzelnen Speicherort zum Einrichten, Verwalten und Überwachen von Replikation, Failover und Wiederherstellung an.
* Ermittelt VMware-VMs automatisch, die zu vSphere-Hosts hinzugefügt wurden.
* Einfache Failovers von Ihrer lokalen Infrastruktur zu Azure und ein Failback (Wiederherstellung) von Azure zu VMware-VM-Server an Ihrem lokalen Standort.
* Aktivieren von Replikation und Failover, damit Anwendungsworkloads, die auf mehrere Computer aufgeteilt sind, gleichzeitig repliziert werden. Sie können mehrere Computer in Wiederherstellungsplänen sammeln, damit Failover für Workloads von Anwendungen mit mehreren Ebenen gemeinsam erfolgen.

## <a name="scenario-architecture"></a>Szenarioarchitektur
Dies sind die Szenariokomponenten:

- **Konfigurationsserver**: Ein lokaler Computer, der die Kommunikation koordiniert und die Datenreplikations- und Wiederherstellungsvorgänge verwaltet. Führen Sie das einheitliche Setup auf diesem Computer aus, um den Konfigurationsserver und diese zusätzlichen Komponenten zu installieren:
 - **Prozessserver**: Fungiert als Replikationsgateway. Er empfängt Replikationsdaten aus geschützten Quellcomputern, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage. Außerdem wickelt er die Pushinstallation des Mobilitätsdiensts auf geschützten Computern ab und führt die automatische Ermittlung von virtuellen VMware-Computern durch. Auf dem Konfigurationsserver ist der Standardprozessserver installiert. Sie können zusätzliche eigenständige Prozessserver bereitstellen, um Ihre Bereitstellung zu skalieren.
 - **Masterzielserver**: Verarbeitet die Replikationsdaten während des Failbacks von Azure.
- **Mobilitätsdienst**: Diese Komponente wird auf jedem Computer (VMware-VM oder physischer Server) bereitgestellt, den Sie nach Azure replizieren möchten. Er erfasst die Datenschreibvorgänge auf dem Computer und sendet sie an den Prozessserver.
- **Azure**: Sie müssen keine virtuellen Azure-Computer für die Replikation und das Failover auf Azure erstellen.  Sie benötigen ein Azure-Abonnement, ein Azure-Speicherkonto zum Speichern replizierter Daten und ein virtuelles Azure-Netzwerk, damit Azure-VMs nach dem Failover mit einem Netzwerk verbunden sind. Das Speicherkonto und das Netzwerk müssen sich in derselben Region wie der Recovery Services-Tresor befinden.
- **Failback**: Für das Failback benötigen Sie eine Reihe von Komponenten:
 - Temporäre Prozessserver: Sie benötigen eine Azure-VM als temporären Prozessserver. Nach Abschluss des Failbacks können Sie diese löschen.
 - VPN: Sie benötigen eine VPN-Verbindung (oder Azure ExpressRoute-Verbindung) zwischen Ihrem lokalen Standort und dem Azure-Netzwerk, in dem sich Ihre Azure-VMs befinden.
 - Masterzielserver: Wenn der Failback-Datenverkehr hoch ist, müssen Sie möglicherweise einen dedizierten Masterzielserver-Computer lokal einrichten. Für geringeren Datenverkehr kann der auf dem Konfigurationsserver ausgeführte Standardmasterzielserver verwendet werden.

Die Grafik zeigt, wie diese Komponenten zusammenwirken.

![Architektur](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Abbildung 1: VMware/physisch zu Azure**

## <a name="azure-prerequisites"></a>Voraussetzungen für Azure
In Azure benötigen Sie Folgendes:

| **Komponente** | **Anforderung** |
| --- | --- |
| **Azure-Konto** |Sie benötigen ein [Microsoft Azure](http://azure.microsoft.com/) -Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung. [hier](https://azure.microsoft.com/pricing/details/site-recovery/) . |
| **Azure-Speicher** |Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden erstellt, wenn ein Failover durchgeführt wird. <br/><br/>Zum Speichern von Daten benötigen Sie ein Premium- oder Standard-Speicherkonto in derselben Region wie der Recovery Services-Tresor.<br/><br/>Sie können ein LRS- oder GRS-Speicherkonto verwenden. Wir empfehlen Ihnen die Verwendung von GRS, damit Resilienz für die Daten besteht, wenn es zu einem regionalen Ausfall kommt oder wenn die primäre Region nicht wiederhergestellt werden kann. [detaillierte Kapazitätsplanung](../storage/storage-redundancy.md)<br/><br/> [Premiumspeicher](../storage/storage-premium-storage.md) wird normalerweise für virtuelle Computer verwendet, die eine konsistent hohe E/A-Leistung und geringe Latenz zum Hosten von E/A-intensiven Workloads erfordern.<br/><br/> Wenn Sie ein Premiumkonto zum Speichern replizierter Daten verwenden möchten, benötigen Sie außerdem ein Standard-Speicherkonto zum Speichern von Replikationsprotokollen, in denen laufende Änderungen lokaler Daten erfasst werden.<br/><br/> **Einschränkung**: Speicherkonten, die im Azure-Portal erstellt wurden, können nicht über Ressourcengruppen hinweg verschoben werden.<br/><br/> **Einschränkung**: Die Replikation von Premium-Speicherkonten in den Regionen „Indien, Mitte“ und „Indien, Süden“ wird zurzeit nicht unterstützt.<br/><br/> [Erfahren Sie mehr](../storage/storage-introduction.md) zu Azure Storage. |
| **Azure-Netzwerk** |Sie benötigen ein virtuelles Azure-Netzwerk, mit dem die Azure-VMs eine Verbindung herstellen, wenn ein Failover stattfindet. Das virtuelle Azure-Netzwerk muss sich in derselben Region wie der Recovery Services-Tresor befinden. |
| **Failback von Azure** |Sie benötigen einen temporären Prozessserver, der als virtueller Azure-Computer eingerichtet ist. Sie können diesen erstellen, wenn Sie für das Failback bereit sind, und ihn nach Abschluss des Failbacks wieder löschen.<br/><br/> Für das Failback benötigen Sie eine VPN-Verbindung (oder Azure ExpressRoute-Verbindung) zwischen dem Azure-Netzwerk und dem lokalen Standort. |

## <a name="configuration-server-or-additional-process-server-prerequisites"></a>Voraussetzungen für Konfigurationsserver oder zusätzliche Prozessserver
Richten Sie einen lokalen Computer als Konfigurationsserver ein.

> [!NOTE]
> Wenn Sie Prozessserver für eine höhere Kapazität erweitern möchten, sind die erforderlichen Komponenten für zusätzliche Prozessserver identisch mit den Voraussetzungen für Konfigurationsserver.
>

| **Komponente** | **Anforderung** |
| --- | --- |
| **Konfigurationsserver** |Ein lokaler, physischer oder virtueller Computer, auf dem Windows Server 2012 R2 ausgeführt wird. Alle lokalen Site Recovery-Komponenten werden auf diesem Computer installiert.<br/><br/>Für die VM-Replikation von VMware wird empfohlen, den Server als hoch verfügbare VMware-VM bereitzustellen. Wenn Sie physische Computer replizieren, kann der Computer ein physischer Server sein.<br/><br/> Das Failback von Azure erfolgt immer auf VMware-VMs. Dies gilt auch, wenn das Failover für einen physischen Server durchgeführt wird. Wenn Sie den Konfigurationsserver nicht als VMware-VM bereitstellen, müssen Sie einen separaten Masterzielserver als VMware-VM einrichten, um Failbackdatenverkehr vor dem Failback zu empfangen.<br/><br/>Wenn der Server eine VMware-VM ist, sollte der Netzwerkadaptertyp VMXNET3 lauten. Wenn Sie einen anderen Netzwerkadaptertyp verwenden, müssen Sie auf dem vSphere 5.5-Server ein [VMware-Update](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1) installieren.<br/><br/>Der Server muss über eine statische IP-Adresse verfügen.<br/><br/>Der Server darf kein Domänencontroller sein.<br/><br/>Der Hostname des Servers darf höchstens 15 Zeichen umfassen.<br/><br/>Das Betriebssystem darf nur auf Englisch sein.<br/><br/> Installieren des VMware vSphere PowerCLI 6.0- Servers<br/><br/>Der Konfigurationsserver benötigt Zugriff auf das Internet. Folgender ausgehender Zugriff ist erforderlich:<br/><br/>Temporärer Zugriff auf HTTP 80 während des Setups von Site Recovery-Komponenten (zum Herunterladen von MySQL)<br/><br/>Dauerhafter ausgehender Zugriff auf HTTPS 443 für die Replikationsverwaltung<br/><br/>Dauerhafter ausgehender Zugriff auf HTTPS 9443 für den Replikationsdatenverkehr (dieser Port kann geändert werden)<br/><br/>Der Server benötigt außerdem Zugriff auf die folgenden URLs, sodass eine Verbindung zu Azure hergestellt werden kann: ``*.accesscontrol.windows.net``<br/><br/> ``*.backup.windowsazure.com``<br/><br/> ``*.hypervrecoverymanager.windowsazure.com``<br/><br/> ``*.store.core.windows.net``<br/><br/> ``*.blob.core.windows.net``<br/><br/> ``https://www.msftncsi.com/ncsi.txt``<br/><br/> ``time.windows.com``<br/><br/> ``time.nist.gov``<br/><br/> Wenn Sie auf dem Server über Firewallregeln verfügen, die auf IP-Adressen basieren, sollten Sie sicherstellen, dass die Regeln die Kommunikation mit Azure zulassen.<br/><br/> Lassen Sie die [IP-Bereiche für Azure-Rechenzentren (in englischer Sprache)](https://www.microsoft.com/download/confirmation.aspx?id=41653) und das HTTPS-Portprotokoll (443) zu.<br/><br/>Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ zu.<br/><br/>Lassen Sie diese URL für den MySQL-Download zu: ``http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi`` |

## <a name="vmware-vcentervsphere-host-prerequisites"></a>Voraussetzungen für den VMware vCenter-/vSphere-Host
| **Komponente** | **Anforderungen** |
| --- | --- |
| **vSphere** | Mindestens einen VMware vSphere-Hypervisor.<br/><br/>Auf den Hypervisoren muss vSphere in einer der Versionen 6.0, 5.5 oder 5.1 mit den neuesten Updates ausgeführt werden.<br/><br/>Ihre vSphere-Hosts und vCenter-Server sollten sich im gleichen Netzwerk befinden wie der Prozessserver (dabei handelt es sich um das Netzwerk, in dem sich der Konfigurationsserver befindet, es sei denn, Sie haben einen dedizierten Prozessserver eingerichtet). |
| **vCenter** | Es wird empfohlen, dass Sie einen VMware vCenter-Server zum Verwalten Ihrer vSphere-Hosts bereitstellen. Auf diesem Server muss vCenter 6.0 oder 5.5 mit den neuesten Updates ausgeführt werden.<br/><br/>**Einschränkung**: Site Recovery unterstützt die neuen Features von vCenter und vSphere 6.0 nicht. Hierzu zählen etwa Cross vCenter vMotion, virtuelle Volumes und Storage DRS. Site Recovery unterstützt nur Features, die auch in Version 5.5 verfügbar waren. |

## <a name="replicated-machine-prerequisites"></a>Voraussetzungen für replizierte Computer
| **Komponente** | **Anforderungen** |
| --- | --- |
| **Lokal (VMware-VMs)** | Auf replizierten VMs sollten VMware-Tools installiert sein und ausgeführt werden.<br/><br/> VMs müssen die [Voraussetzungen für Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements) zum Erstellen von Azure-VMs erfüllen.<br/><br/>Die Kapazität der einzelnen Datenträger auf geschützten Computern darf maximal 1.023 GB betragen. Ein virtueller Computer kann bis zu 64 Datenträger haben (also bis zu 64 TB). <br/><br/>Für die Komponenteninstallation sind mindestens 2 GB verfügbarer Speicherplatz auf dem Installationslaufwerk erforderlich.<br/><br/>**Einschränkung**: Der Schutz von VMs mit verschlüsselten Festplatten wird nicht unterstützt.<br/><br/>**Einschränkung**: Gastcluster mit freigegebenen Datenträgern werden nicht unterstützt.<br/><br/>**Port 20004** sollte auf der lokalen VM-Firewall geöffnet werden, wenn Sie Multi-VM-Konsistenz aktivieren möchten.<br/><br/>Computer mit Unified Extensible Firmware Interface (UEFI)/Extensible Firmware Interface (EFI) werden nicht unterstützt.<br/><br/>Computernamen dürfen zwischen 1 und 63 Zeichen enthalten (Buchstaben, Zahlen und Bindestriche). Der Name muss mit einem Buchstaben oder einer Zahl beginnen und mit einem Buchstaben oder einer Zahl enden. Sie können den Azure-Namen ändern, nachdem Sie die Replikation für einen Computer aktiviert haben.<br/><br/>Wenn auf dem virtuellen Quellcomputer NIC-Teamvorgänge ausgeführt wurden, wird er nach dem Failover zu Azure in eine einzelne NIC konvertiert.<br/><br/>Wenn geschützte virtuelle Computer über einen iSCSI-Datenträger verfügen, konvertiert Site Recovery die iSCSI-Datenträger der geschützten VMs in eine VHD-Datei, wenn ein Failover der VM zu Azure durchgeführt wird. Wenn das iSCSI-Ziel von der Azure-VM erreicht werden kann, wird eine Verbindung hergestellt. Dadurch sind für ihn effektiv zwei Datenträger sichtbar – der VHD-Datenträger auf der Azure-VM und der iSCSI-Quelldatenträger. In diesem Fall müssen Sie die Verbindung mit dem iSCSI-Ziel trennen, das auf der Azure-VM erscheint. |
| **Windows-Computer (physisch oder VMware)** | Auf dem Computer muss ein unterstütztes 64-Bit-Betriebssystem ausgeführt werden: Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 mit SP1 oder höher.<br/><br/> Das Betriebssystem sollte auf dem Laufwerk C:\ installiert werden. Bei dem Betriebssystem-Datenträger sollte es sich um einen Windows-Basisdatenträger anstelle eines dynamischen Datenträgers handeln. Der Datenträger kann dynamisch sein.<br/><br/>Site Recovery unterstützt virtuelle Computer mit einem RDM-Datenträger. Beim Failback wird der RDM-Datenträger von Site Recovery wiederverwendet, sofern die ursprüngliche Quell-VM und der RDM-Datenträger verfügbar sind. Wenn sie nicht verfügbar sind, erstellt Site Recovery beim Failback für jeden Datenträger eine neue VMDK-Datei. |
| **Linux-Computer** (physisch oder VMware) | Sie benötigen ein unterstütztes 64-Bit-Betriebssystem: Red Hat Enterprise Linux 6.7,7.1,7.2; Centos 6.5, 6.6, 6.7, 7.0, 7.1, 7.2; Oracle Enterprise Linux 6.4, 6.5, auf dem der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird, SUSE Linux Enterprise Server 11 SP3.<br/><br/>Dateien vom Typ „/etc/hosts“ auf geschützten Computern müssen Einträge enthalten, die den Namen des lokalen Hosts den IP-Adressen aller Netzwerkkarten zuordnen.<br/><br/>Wenn Sie nach einem Failover eine Verbindung mit virtuellen Azure-Computern unter Linux über einen Secure Shell-Client (SSH) herstellen möchten, stellen Sie sicher, dass der Secure Shell-Dienst auf dem geschützten Computer beim Systemstart automatisch gestartet wird und dass die Firewallregeln eine SSH-Verbindung zulassen.<br/><br/>Der Hostname, Bereitstellungspunkte, Gerätenamen und Linux-Systempfade und -Dateinamen (z. B./etc/, /usr) sollten nur englische Zeichen umfassen.<br/><br/>Der Schutz kann nur für Linux-Computer mit folgendem Speicher aktiviert werden: Dateisystem (EXT3, ETX4, ReiserFS, XFS), Multipath-Softwaregeräte-Mapper (multipath), Volume-Manager (LVM2). Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.<br/><br/>Site Recovery unterstützt virtuelle Computer mit einem RDM-Datenträger.  Beim Failback für Linux wird der RDM-Datenträger nicht wiederverwendet. Anstelle dessen erstellt Site Recovery für jeden entsprechenden RDM-Datenträger eine neue VMDK-Datei.<br/><br/>Stellen Sie sicher, dass Sie die Einstellung „disk.enableUUID=true“ in den Konfigurationsparametern der VM in VMware festlegen. Erstellen Sie den Eintrag, wenn er nicht vorhanden ist. Er ist erforderlich, um der VMDK-Datei eine konsistente UUID bereitzustellen, damit sie ordnungsgemäß bereitgestellt wird. Durch das Hinzufügen dieser Einstellung wird außerdem sichergestellt, dass während des Failbacks nur Deltaänderungen zurück an den lokalen Standort übertragen werden, und keine vollständige Replikation. |
| **Mobilitätsdienst** | **Windows**: Für eine automatische Übertragung des Mobilitätsdiensts mittels Push auf Windows-VMs müssen Sie ein Administratorkonto (lokaler Administrator auf dem Windows-Computer) angeben, damit der Prozessserver eine Pushinstallation durchführen kann.<br/><br/>**Linux:** Für eine automatische Übertragung des Mobilitätsdiensts mittels Push auf virtuelle Linux-Computer müssen Sie ein Konto erstellen, das vom Prozessserver zum Durchführen einer Pushinstallation verwendet werden kann.<br/><br/> Standardmäßig werden alle Datenträger auf einem Computer repliziert. Der Mobilitätsdienst muss vor der Aktivierung der Replikation manuell auf dem Computer installiert werden, um einen [Datenträger von der Replikation auszuschließen](#exclude-disks-from-replication).<br/> |

## <a name="prepare-for-deployment"></a>Vorbereiten der Bereitstellung
Für die Vorbereitung der Bereitstellung benötigen Sie Folgendes:

1. [Richten Sie ein Azure-Netzwerk ein](#set-up-an-azure-network) , in dem sich Azure-VMs befinden, wenn sie nach dem Failover erstellt werden. Zusätzlich müssen Sie für das Failback eine VPN-Verbindung (oder Azure ExpressRoute-Verbindung) zwischen dem Azure-Netzwerk und Ihrem lokalen Standort einrichten.
2. [Richten Sie ein Azure-Speicherkonto ein](#set-up-an-azure-storage-account) , das für replizierte Daten verwendet werden kann.
3. [Vorbereiten eines Kontos](#prepare-an-account-for-automatic-discovery) auf dem vCenter-Server oder den vSphere-Hosts, damit Site Recovery automatisch VMware-VMs erkennen kann, die hinzugefügt werden.
4. [Vorbereiten des Konfigurationsservers](#prepare-the-configuration-server), um sicherzustellen, dass er auf die erforderlichen URLs zugreifen und vSphere PowerCLI 6.0 installieren kann.

### <a name="set-up-an-azure-network"></a>Richten Sie ein Azure-Netzwerk ein
* Das Netzwerk sollte sich in derselben Azure-Region befinden, wie das Netzwerk, in dem Sie den Recovery Services-Tresor bereitstellen.
* Richten Sie das Azure-Netzwerk im [Resource Manager-Modus](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) oder im [klassischen Modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ein (je nachdem, welches Ressourcenmodell Sie für virtuelle Azure-Computer nach dem Failover verwenden möchten).
* Für das Failback von Azure auf Ihren lokalen VMware-Standort benötigen Sie eine VPN-Verbindung (oder eine Azure ExpressRoute-Verbindung) zwischen dem Azure-Netzwerk, in dem sich die replizierten Azure-VMs befinden, und dem lokalen Netzwerk, in dem sich der Konfigurationsserver befindet.
* [Informieren Sie sich](../vpn-gateway/vpn-gateway-site-to-site-create.md) über die unterstützten Bereitstellungsmodelle für Site-to-Site-VPN-Verbindungen und über das [Einrichten einer Verbindung](../vpn-gateway/vpn-gateway-site-to-site-create.md#CreateVNet).
* Alternativ dazu können Sie [Azure ExpressRoute](../expressroute/expressroute-introduction.md)einrichten. [Informieren Sie sich](../expressroute/expressroute-howto-vnet-portal-classic.md) über das Einrichten eines Azure-Netzwerks mit ExpressRoute.

> [!NOTE]
> Für Netzwerke, die für die Site Recovery-Bereitstellung verwendet werden, wird die [Migration von Netzwerken](../azure-resource-manager/resource-group-move-resources.md) zwischen Ressourcengruppen im selben Abonnement oder zwischen verschiedenen Abonnements nicht unterstützt.
>
>

### <a name="set-up-an-azure-storage-account"></a>Richten Sie ein Azure-Speicherkonto ein
* Sie benötigen ein Standard- oder Premium-Azure-Speicherkonto zum Speichern von Daten, die nach Azure repliziert werden. Das Konto muss sich in derselben Region wie der Recovery Services-Tresor befinden. Richten Sie ein Konto im [Resource Manager-Modus](../storage/storage-create-storage-account.md) oder im [klassischen Modus](../storage/storage-create-storage-account-classic-portal.md) ein, je nachdem, welches Ressourcenmodell Sie für Azure-VMs nach dem Failover verwenden möchten.
* Wenn Sie ein Premium-Konto für replizierte Daten verwenden, müssen Sie ein zusätzliches Standardkonto zum Speichern von Replikationsprotokollen erstellen, in denen laufende Änderungen lokaler Daten erfasst werden.  

> [!NOTE]
> [Die Migration von Speicherkonten](../azure-resource-manager/resource-group-move-resources.md) zwischen Ressourcengruppen im gleichen Abonnement oder zwischen verschiedenen Abonnements wird für Speicherkonten nicht unterstützt, die für die Site Recovery-Bereitstellung verwendet werden.
>
>

### <a name="prepare-an-account-for-automatic-discovery"></a>Vorbereiten eines Kontos für die automatische Ermittlung
Der Site Recovery-Prozessserver kann virtuelle VMware-Computer auf vSphere-Hosts oder einem vCenter-Server automatisch erkennen, der Hosts verwaltet. Site Recovery benötigt für die Ausführung der automatischen Ermittlung Anmeldeinformationen, mit denen auf den VMware-Server zugegriffen werden kann. Wenn Sie nur physische Computer replizieren, ist dieser Abschnitt nicht relevant.

1. Erstellen Sie auf der vCenter-Ebene eine Rolle (etwa „Azure_Site_Recovery“) mit den [erforderlichen Berechtigungen](#vmware-account-permissions), um für die automatische Ermittlung ein dediziertes Konto zu verwenden.
2. Erstellen Sie einen neuen Benutzer auf dem vSphere-Host oder dem vCenter-Server und weisen Sie die Rolle dem Benutzer zu. Später teilen Sie Site Recovery diese Anmeldeinformationen mit, damit die automatische Ermittlung ausgeführt werden kann.

   > [!NOTE]
   > Ein vCenter-Benutzerkonto mit einer schreibgeschützten Rolle kann das Failover ausführen, geschützte Quellcomputer jedoch nicht herunterfahren. Wenn Sie diese Computer herunterfahren möchten, benötigen Sie die Rolle [Azure_Site_Recovery](#vmware-account-permissions). Wenn Sie nur virtuelle Computer (VMs) von VMware zu Azure migrieren und kein Failback benötigen, ist die schreibgeschützte Rolle ausreichend.
   >
   >

### <a name="prepare-the-configuration-server"></a>Bereiten Sie den Konfigurationsserver vor
1. Stellen Sie sicher, dass der Computer, den Sie für den Konfigurationsserver verwenden die [Voraussetzungen](#configuration-server-prerequisites)erfüllt. Stellen Sie insbesondere sicher, dass der Computer mit diesen Einstellungen mit dem Internet verbunden ist:

   * Ermöglichen des Zugriffs auf diese URLs: ``*.hypervrecoverymanager.windowsazure.com``; ``*.accesscontrol.windows.net``; ``*.backup.windowsazure.com``; ``*.blob.core.windows.net``; ``*.store.core.windows.net``
   * Lassen Sie zum Herunterladen von MySQL den Zugriff auf [http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi](http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi) zu.
   * Lassen Sie mithilfe der [IP-Adressbereiche des Azure-Rechenzentrums](https://www.microsoft.com/download/confirmation.aspx?id=41653) und des HTTPS-Protokolls (443) die Firewallkommunikation mit Azure zu.
2. Laden Sie [VMware vSphere PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) herunter, und installieren Sie es auf dem Konfigurationsserver. (Derzeit werden keine anderen PowerCLI-Versionen unterstützt, auch R-Releases von Version 6.0 nicht).

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Neu** > **Verwaltung** > **Backup und Site Recovery (OMS)**. Sie können auch auf **Durchsuchen** > **Recovery Services-Tresor** > **Hinzufügen** klicken.

    ![Neuer Tresor](./media/site-recovery-vmware-to-azure/new-vault3.png)
3. Geben Sie unter **Name**einen Anzeigenamen an, mit dem der Tresor identifiziert wird. Wenn Sie mehrere Abonnements haben, müssen Sie ein Abonnement auswählen.
4. [Erstellen Sie eine neue Ressourcengruppe](../azure-resource-manager/resource-group-template-deploy-portal.md) , oder wählen Sie eine vorhandene Ressourcengruppe aus. Geben Sie eine Azure-Region an. Computer werden in dieser Region repliziert. Beachten Sie, dass sich für Site Recovery verwendete Azure-Speicher und -Netzwerke in derselben Region befinden müssen. Sie finden eine Liste der unterstützten Regionen unter Geografische Verfügbarkeit auf der Seite [Azure Site Recovery – Preisübersicht](https://azure.microsoft.com/pricing/details/site-recovery/)
5. Wenn Sie über das Dashboard schnell auf den Tresor zugreifen möchten, klicken Sie auf **An Dashboard anheften** und anschließend auf **Erstellen**.

    ![Neuer Tresor](./media/site-recovery-vmware-to-azure/new-vault-settings.png)

Der neue Tresor wird unter **Dashboard** > **Alle Ressourcen** und auf dem Hauptblatt **Recovery Services-Tresore** angezeigt.

## <a name="getting-started"></a>Erste Schritte
Site Recovery stellt eine Benutzeroberfläche für die ersten Schritte bereit, damit Sie so schnell wie möglich loslegen können. Sie überprüft Voraussetzungen und führt Sie durch die Schritte zur Bereitstellung von Site Recovery.

Sie wählen den Typ der Computer aus, die Sie replizieren möchten, und geben an, wonach sie repliziert werden sollen. Sie richten die Infrastruktur ein, einschließlich lokaler Server, Azure-Einstellungen, Richtlinien für die Replikation und der Kapazitätsplanung. Nachdem Ihre Infrastruktur eingerichtet wurde, aktivieren Sie die Replikation für VMs und physische Server. Sie können anschließend Failover für bestimmte Computer ausführen oder Wiederherstellungspläne erstellen, um das Failover für mehrere Computer durchzuführen.

Beginnen Sie mit den ersten Schritten, indem Sie auswählen, wie Sie Site Recovery bereitstellen möchten. Der Ablauf der ersten Schritte kann je nach Replikationsanforderungen leicht variieren.

## <a name="step-1-choose-your-protection-goals"></a>Schritt 1: Auswählen der Schutzziele
Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.

1. Wählen Sie auf dem Blatt **Recovery Services-Tresore** Ihren Tresor aus, und klicken Sie auf **Einstellungen**.
2. Klicken Sie unter **Einstellungen** > **Erste Schritte** auf **Site Recovery** > **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Schutzziel**.

    ![Ziele wählen](./media/site-recovery-vmware-to-azure/choose-goals.png)
3. Wählen Sie unter **Schutzziel** die Option **To Azure** (Zu Azure) und dann **Yes, with VMware vSphere Hypervisor** (Ja, mit VMware vSphere-Hypervisor) aus. Klicken Sie dann auf **OK**.

    ![Ziele wählen](./media/site-recovery-vmware-to-azure/choose-goals2.png)

## <a name="step-2-set-up-the-source-environment"></a>Schritt 2: Einrichten der Quellumgebung
Richten Sie den Konfigurationsserver ein, und registrieren Sie ihn im Recovery Services-Tresor. Wenn Sie virtuelle VMware-Computer replizieren, geben Sie das VMware-Konto an, das Sie für die automatische Ermittlung verwenden.

1. Klicken Sie auf **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Quelle**. Falls Sie über keinen Konfigurationsserver verfügen, klicken Sie unter **Quelle vorbereiten** auf **+Konfigurationsserver**, um einen Konfigurationsserver hinzuzufügen.

    ![Quelle einrichten](./media/site-recovery-vmware-to-azure/set-source1.png)
2. Überprüfen Sie auf dem Blatt **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver** angezeigt wird.
3. Überprüfen Sie vor dem Einrichten des Konfigurationsservers die [Voraussetzungen](#configuration-server-prerequisites). Stellen Sie insbesondere sicher, dass der Computer auf die erforderlichen URLs zugreifen kann.
4. Laden Sie die Installationsdatei für das einheitliche Setup von Site Recovery herunter.
5. Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen diesen beim Ausführen des einheitlichen Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

   ![Quelle einrichten](./media/site-recovery-vmware-to-azure/set-source2.png)
6. Führen Sie auf dem Computer, den Sie als Konfigurationsserver verwenden, das einheitliche Setup aus, um den Konfigurationsserver, den Prozessserver und den Masterzielserver zu installieren.

### <a name="run-site-recovery-unified-setup"></a>Ausführen des einheitlichen Setups von Site Recovery
1. Führen Sie die Installationsdatei für das einheitliche Setup aus.
2. Wählen Sie unter **Vorbereitung** die Option **Install the configuration server and process server** (Konfigurationsserver und Prozessserver installieren) aus.

   ![Vorbereitung](./media/site-recovery-vmware-to-azure/combined-wiz1.png)
3. Klicken Sie unter **Third-Party Software License** (Drittanbietersoftwarelizenz) auf **I Accept** (Ich stimme zu), um MySQL herunterzuladen und zu installieren.

    ![Drittanbietersoftware](./media/site-recovery-vmware-to-azure/combined-wiz105.PNG)
4. Navigieren Sie unter **Registrierung** zu dem Registrierungsschlüssel, den Sie aus dem Tresor heruntergeladen haben, und wählen Sie ihn aus.

    ![Registrierung](./media/site-recovery-vmware-to-azure/combined-wiz3.png)
5. Geben Sie unter **Internet Settings** (Interneteinstellungen) an, wie der auf dem Konfigurationsserver ausgeführte Anbieter eine Internetverbindung mit Azure Site Recovery herstellen soll.

   * Wenn die Verbindung über den derzeit auf dem Computer eingerichteten Proxy hergestellt werden soll, wählen Sie **Connect with existing proxy settings** (Mit vorhandenen Proxyeinstellungen verbinden) aus.
   * Wenn der Anbieter eine direkte Verbindung herstellen soll, wählen Sie **Connect directly without a proxy** (Ohne Proxy direkt verbinden) aus.
   * Falls für den vorhandenen Proxy eine Authentifizierung erforderlich ist oder Sie für die Anbieterverbindung einen benutzerdefinierten Proxy verwenden möchten, wählen Sie **Mit benutzerdefinierten Proxyeinstellungen verbinden**aus.

     * Bei Verwendung eines benutzerdefinierten Proxys müssen Sie die Adresse, den Port und die Anmeldeinformationen eingeben.
     * Bei Verwendung eines Proxys sollten Sie die unter [Voraussetzungen](#configuration-server-prerequisites) beschriebenen URLs bereits zugelassen haben.

     ![Firewall](./media/site-recovery-vmware-to-azure/combined-wiz4.png)
6. Bei der **Voraussetzungsüberprüfung** führt das Setup eine Überprüfung durch, um sicherzustellen, dass die Installation ausgeführt werden kann. Falls beim **Überprüfen der Synchronisierung der globalen Zeit** eine Warnung angezeigt wird, stellen Sie sicher, dass die Zeit der Systemuhr (Einstellungen für **Datum und Uhrzeit**) mit der Zeitzone übereinstimmt.

    ![Voraussetzungen](./media/site-recovery-vmware-to-azure/combined-wiz5.png)
7. Erstellen Sie unter **MySQL Configuration** (MySQL-Konfiguration) Anmeldeinformationen für die Anmeldung bei der MySQL-Serverinstanz, die installiert wird.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz6.png)
8. Wählen Sie unter **Umgebungsdetails** aus, ob Sie VMware-VMs replizieren werden. Wenn dies der Fall ist, überprüft das Setup, ob PowerCLI 6.0 installiert ist.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz7.png)
9. Wählen Sie unter **Install Location** (Installationsspeicherort) aus, wo die Binärdateien installiert werden sollen und wo der Cache gespeichert werden soll. Sie können ein Laufwerk auswählen, auf dem mindestens 5 GB Speicherplatz verfügbar ist, wir raten Ihnen jedoch zu einem Cachelaufwerk mit mindestens 600 GB freiem Speicherplatz.

    ![Installationsspeicherort](./media/site-recovery-vmware-to-azure/combined-wiz8.png)
10. Geben Sie unter **Network Selection** (Netzwerkauswahl) den Listener (Netzwerkadapter und SSL-Port) an, über den der Konfigurationsserver Replikationsdaten senden und empfangen wird. Sie können den Standardport (9443) ändern. Zusätzlich zu diesem Port wird Port 443 von einem Webserver verwendet, der Replikationsvorgänge orchestriert. 443 sollte nicht zum Empfangen von Replikationsdatenverkehr verwendet werden.

    ![Netzwerkauswahl](./media/site-recovery-vmware-to-azure/combined-wiz9.png)



1. Lesen Sie die unter **Summary** (Zusammenfassung) angezeigten Informationen und klicken Sie auf **Install** (Installieren). Nach Abschluss der Installation wird eine Passphrase generiert. Sie benötigen diese bei der Aktivierung der Replikation. Kopieren Sie sie daher, und bewahren Sie sie an einem sicheren Ort auf.

   ![Zusammenfassung](./media/site-recovery-vmware-to-azure/combined-wiz10.png)
2. Nach Abschluss der Registrierung wird der Server auf dem Blatt **Einstellungen** > **Server** im Tresor angezeigt.

#### <a name="run-setup-from-the-command-line"></a>Ausführen des Setupprogramms über die Befehlszeile
Sie können den Konfigurationsserver über die Befehlszeile einrichten:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parameter:

* /ServerMode: Obligatorisch. Gibt an, ob sowohl der Konfigurations- als auch der Prozessserver oder nur der Prozessserver installiert werden soll. Eingabewerte: CS, PS.
* InstallLocation: Obligatorisch. Der Ordner, in den die Komponenten installiert werden.
* /MySQLCredsFilePath. Obligatorisch. Der Dateipfad, unter dem die MySQL Server-Anmeldeinformationen gespeichert werden. Die Datei sollte folgendes Format aufweisen:
  * [MySQLCredentials]
  * MySQLRootPassword = "<Password>"
  * MySQLUserPassword = "<Password>"
* /VaultCredsFilePath. Obligatorisch. Der Speicherort der Datei mit den Anmeldeinformationen für den Tresor.
* /EnvType. Obligatorisch. Der Installationstyp. Werte: VMware, NonVMware
* /PSIP und /CSIP. Obligatorisch. Die IP-Adressen des Prozessservers und des Konfigurationsservers.
* /PassphraseFilePath. Obligatorisch. Der Speicherort der Datei mit der Passphrase.
* /BypassProxy. Optional. Gibt an, dass der Konfigurationsserver die Verbindung mit Azure ohne Proxy herstellt.
* /ProxySettingsFilePath. Optional. Proxyeinstellungen (der standardmäßige Proxy erfordert eine Authentifizierung oder einen benutzerdefinierten Proxy). Die Datei sollte folgendes Format aufweisen:
  * [ProxySettings]
  * ProxyAuthentication = "Yes/No"
  * Proxy IP = "IP Address>"
  * ProxyPort = "<Port>"
  * ProxyUserName="<User Name>"
  * ProxyPassword="<Password>"
* DataTransferSecurePort. Optional. Die Portnummer, die für Replikationsdaten verwendet werden soll.
* SkipSpaceCheck. Optional. Überspringt die Überprüfung des Speicherplatzes für Cache.
* AcceptThirdpartyEULA. Obligatorisch. Das Flag impliziert die Akzeptanz von Drittanbieterlizenzbedingungen.
* ShowThirdpartyEULA. Obligatorisch. Zeigt Drittanbieterlizenzbedingungen an. Alle anderen Parameter werden ignoriert, wenn sie als Eingabe bereitgestellt werden.

### <a name="add-the-vmware-account-used-for-automatic-discovery"></a>Fügen Sie das VMware-Konto hinzu, das für die automatische Ermittlung verwendet wird.
 Im Zuge der Bereitstellungsvorbereitung sollten Sie [ein VMware-Konto erstellt](#prepare-an-account-for-automatic-discovery) haben, das von Site Recovery für die automatische Ermittlung verwendet werden kann. Fügen Sie dieses Konto wie folgt hinzu:

1. Öffnen Sie **CSPSConfigtool.exe**. Das Tool steht als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner „[INSTALLATIONSPFAD]\home\svsystems\bin“.
2. Klicken Sie auf **Konten verwalten** > **Konto hinzufügen**.

    ![Konto hinzufügen](./media/site-recovery-vmware-to-azure/credentials1.png)
3. Fügen Sie unter **Account Details** (Kontodetails) das Konto hinzu, das für die automatische Ermittlung verwendet werden wird. Beachten Sie, dass es 15 Minuten oder länger dauern kann, bis der Kontoname im Portal angezeigt wird. Klicken Sie zur sofortigen Aktualisierung auf **Konfigurationsserver** > Servername > **Server aktualisieren**.

    ![Details](./media/site-recovery-vmware-to-azure/credentials2.png)

### <a name="connect-to-vsphere-hosts-and-vcenter-servers"></a>Verbinden mit vSphere-Hosts und vCenter-Servern
Wenn Sie VMware-VMs replizieren, stellen Sie eine Verbindung mit den vSphere-Hosts und vCenter-Servern her.

1. Stellen Sie sicher, dass der Konfigurationsserver über Netzwerkzugriff auf die vSphere-Hosts und vCenter-Server verfügt.
2. Klicken Sie auf **Infrastruktur vorbereiten** > **Quelle**. Wählen Sie unter **Quelle vorbereiten** den Konfigurationsserver aus, und klicken Sie auf **+vCenter**, um einen vSphere-Host oder einen vCenter-Server hinzuzufügen.
3. Geben Sie unter **Add vCenter** (vCenter hinzufügen) einen Anzeigenamen für den vSphere-Host oder den vCenter-Server an, und geben Sie die IP-Adresse oder den FQDN des Servers an. Belassen Sie den Port als Port 443, es sei denn Ihre VMware-Server sind so konfiguriert, dass sie auf Anforderungen auf einem anderen Port lauschen können. Wählen Sie anschließend das Konto aus, das für die Verbindung mit dem VMware-Server verwendet wird. Klicken Sie auf **OK**.

    ![VMware](./media/site-recovery-vmware-to-azure/vmware-server.png)

   > [!NOTE]
   > Wenn Sie den vCenter-Server oder den vSphere-Host mit einem Konto hinzufügen, das auf dem vCenter- oder Hostserver keine Administratorrechte besitzt, müssen Sie sicherstellen, dass für das Konto folgende Berechtigungen aktiviert sind: Rechenzentrum, Datenspeicher, Ordner, Host, Netzwerk, Ressource, virtueller Computer, vSphere Distributed Switch. Außerdem benötigt der vCenter-Server die Anzeigeberechtigung für den Speicher.
   >
   >

Site Recovery stellt mithilfe der angegebenen Einstellungen eine Verbindung mit VMware-Servern her und ermittelt VMs.

## <a name="step-3-set-up-the-target-environment"></a>Schritt 3: Einrichten der Zielumgebung
Stellen Sie sicher, dass Sie über ein Speicherkonto für die Replikation und über ein Azure-Netzwerk verfügen, mit dem Azure-VMs nach dem Failover eine Verbindung herstellen.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das gewünschte Azure-Abonnement aus.
2. Geben Sie das Bereitstellungsmodell an, das Sie nach einem Failover für VMs verwenden möchten.
3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

   ![Ziel](./media/site-recovery-vmware-to-azure/gs-target.png)
4. Wenn Sie noch kein Speicherkonto erstellt haben und dies per Resource Manager nachholen möchten, können Sie auf **+Speicherkonto** klicken, um diesen Vorgang direkt durchzuführen.  Geben Sie auf dem Blatt **Speicherkonto erstellen** einen Kontonamen, einen Typ, ein Abonnement und einen Standort an. Das Konto sollte sich in derselben Region wie der Recovery Services-Tresor befinden.

   ![Speicher](./media/site-recovery-vmware-to-azure/gs-createstorage.png)

   Beachten Sie Folgendes:

   * Wenn Sie ein Speicherkonto mit dem klassischen Modell erstellen möchten, verwenden Sie das Azure-Portal. [Weitere Informationen](../storage/storage-create-storage-account-classic-portal.md)
   * Wenn Sie ein Storage Premium-Konto für replizierte Daten verwenden, müssen Sie ein weiteres Standardspeicherkonto zum Speichern von Replikationsprotokollen einrichten, mit denen laufende Änderungen lokaler Daten erfasst werden.

   > [!NOTE]
   > Der Schutz von Premium-Speicherkonten in den Regionen „Indien, Mitte“ und „Indien, Süden“ wird zurzeit nicht unterstützt.
   >
   >
5. Wählen Sie ein Azure-Netzwerk aus. Wenn Sie noch kein Netzwerk erstellt haben und dies mit Resource Manager nachholen möchten, klicken Sie auf **+Network** (+Netzwerk), um diesen Vorgang direkt durchzuführen. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** einen Netzwerknamen, einen Adressbereich, Subnetzdetails, ein Abonnement und einen Standort an. Das Netzwerk sollte sich an demselben Standort wie der Recovery Services-Tresor befinden.

   ![Netzwerk](./media/site-recovery-vmware-to-azure/gs-createnetwork.png)

   Falls Sie ein Netzwerk mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das Azure-Portal. [detaillierte Kapazitätsplanung](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)

## <a name="step-4-set-up-replication-settings"></a>Schritt 4: Einrichten der Replikationseinstellungen
1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Create and Associate** (+Erstellen und Zuordnen).
2. Geben Sie unter **Richtlinie erstellen und zuordnen**einen Richtliniennamen an.
3. Geben Sie unter **RPO-Schwellenwert** den RPO-Grenzwert an. Warnungen werden generiert, wenn die fortlaufende Replikation diesen Grenzwert überschreitet.
4. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts**das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte in Stunden an. Geschützte Computer können innerhalb eines Zeitfensters an einem beliebigen Punkt wiederhergestellt werden. Für nach Storage Premium replizierte Computer wird eine Aufbewahrungsdauer von bis zu 24 Stunden unterstützt.
5. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit**an, wie häufig (in Minuten) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten.
6. Wenn Sie eine Replikationsrichtlinie erstellen, wird für das Failback standardmäßig eine passende Richtlinie automatisch erstellt. Bei Verwendung der Replikationsrichtlinie **rep-policy** lautet die Failbackrichtlinie beispielsweise **rep-policy-failback**. Diese Richtlinie wird erst verwendet, wenn Sie ein Failback initiieren.  
7. Klicken Sie auf **OK**, um die Richtlinie zu erstellen.

    ![Replikationsrichtlinie](./media/site-recovery-vmware-to-azure/gs-replication2.png)
8. Wenn Sie eine neue Richtlinie erstellen, wird sie dem Konfigurationsserver automatisch zugeordnet. Klicken Sie auf **OK**.

    ![Replikationsrichtlinie](./media/site-recovery-vmware-to-azure/gs-replication3.png)

## <a name="step-5-capacity-planning"></a>Schritt 5: Kapazitätsplanung
Nachdem Sie nun Ihre grundlegende Infrastruktur eingerichtet haben, können Sie sich mit der Kapazitätsplanung beschäftigen und ermitteln, ob Sie zusätzliche Ressourcen benötigen.

Site Recovery verfügt über einen Capacity Planner, der Sie dabei unterstützt, die richtigen Ressourcen für Ihre Quellumgebung, die Site Recovery-Komponenten, das Netzwerk und den Speicher zuzuordnen. Sie können den Planer im Schnellmodus für Schätzungen ausführen, die auf einer durchschnittlichen Anzahl von VMs, Datenträgern und Speicher basieren, oder im ausführlichen Modus, in dem Sie Zahlen auf Workloadebene eingeben. Führen Sie Folgendes aus, bevor Sie beginnen:

* Sammeln Sie Informationen zu Ihrer Replikationsumgebung, z.B. VMs, Datenträger pro VM und Speicher pro Datenträger.
* Schätzen Sie die tägliche Änderungsrate für replizierte Daten. Sie können hierfür die [Appliance für die vSphere-Kapazitätsplanung](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) verwenden.

1. Klicken Sie auf **Herunterladen**, um das Tool herunterzuladen und es anschließend auszuführen. [entsprechenden Artikel](site-recovery-capacity-planner.md) durch.
2. Wählen Sie anschließend **Ja** in **Haben Sie die Kapazitätsplanung abgeschlossen?** aus.

   ![Kapazitätsplanung](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)

Die folgende Tabelle unterstützt Sie bei der Kapazitätsplanung.

| **Komponente** | **Details** |
| --- | --- | --- |
| **Replikation** |**Maximale tägliche Änderungsrate:** Ein geschützter Computer kann nur einen einzigen Prozessserver verwenden, und ein einzelner Prozessserver kann eine tägliche Änderungsrate von bis zu 2 TB verarbeiten. Daher beträgt die maximale Datenänderungsrate pro Tag, die für einen geschützten Computer unterstützt wird, 2 TB.<br/><br/> **Maximaler Durchsatz:** Ein replizierter Computer kann zu einem einzigen Speicherkonto in Azure gehören. Ein Standardspeicherkonto kann maximal 20.000 Anforderungen pro Sekunde verarbeiten, und es wird empfohlen, dass Sie die Anzahl von IOPS über einen Quellcomputer hinweg bei 20.000 halten. Wenn Ihr Quellcomputer z. B. 5 Datenträger hat und jeder Datenträger 120 IOPS (mit einer Größe von 8 KB) für die Quelle generiert, wird das Azure-Limit von 500 IOPS pro Datenträger eingehalten. Erforderliche Anzahl von Speicherkonten = Quell-IOPS insgesamt/20000. |
| **Konfigurationsserver** |Der Konfigurationsserver muss in der Lage sein, die tägliche Änderungsrate für alle auf geschützten Computern ausgeführten Workloads zu bewältigen, und er muss über genügend Bandbreite verfügen, um Daten kontinuierlich in den Azure-Speicher zu replizieren.<br/><br/> Als bewährte Methode wird empfohlen, den Konfigurationsserver im selben Netzwerk und LAN-Segment zu platzieren, in dem sich auch die zu schützenden Computer befinden. Er kann in einem anderen Netzwerk platziert werden, aber die zu schützenden Computer sollten für ihn über L3-Netzwerksichtbarkeit verfügen.<br/><br/> Die empfohlenen Größen für den Konfigurationsserver werden in der nachstehenden Tabelle zusammengefasst. |
| **Prozessserver** |Der erste Prozessserver wird standardmäßig auf dem Konfigurationsserver installiert. Sie können zusätzliche Prozessserver bereitstellen, um Ihre Umgebung zu skalieren. Beachten Sie Folgendes:<br/><br/> Der Prozessserver empfängt Replikationsdaten von geschützten Computern und optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung, bevor er sie an Azure sendet. Der Prozessservercomputer muss über genügend Ressourcen zum Ausführen dieser Aufgaben verfügen.<br/><br/> Der Prozessserver verwendet einen datenträgerbasierten Cache. Wir empfehlen die Verwendung eines separaten Cachedatenträgers mit einer Größe von mindestens 600 GB, um Datenänderungen bei einem Netzwerkengpass oder -ausfall zwischenspeichern und verarbeiten zu können. |

### <a name="size-recommendations-for-the-configuration-server"></a>Empfohlene Größen für den Konfigurationsserver
| **CPU** | **Arbeitsspeicher** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 Sockets * 4 Kerne @ 2,5 GHz) |16 GB |300 GB |500 GB oder weniger |Weniger als 100 Computer replizieren. |
| 12 vCPUs (2 Sockets * 6 Kerne @ 2,5 GHz) |18 GB |600 GB |500 GB bis 1 TB |Zwischen 100 und 150 Computer replizieren. |
| 16 vCPUs (2 Sockets * 8 Kerne @ 2,5 GHz) |32 GB |1 TB |1 TB bis 2 TB |Zwischen 150 und 200 Computer replizieren. |
| Bereitstellen eines weiteren Prozessservers | | |> 2 TB |Stellen Sie zusätzliche Prozessserver bereit, wenn Sie mehr als 200 Computer replizieren oder wenn die tägliche Änderungsrate 2 TB überschreitet. |

Hierbei gilt:

* Jeder Quellcomputer ist mit 3 Datenträgern von jeweils 100 GB konfiguriert.
* Bei den Benchmarkingmessungen für den Cachedatenträger wurde ein Speicher aus 8 SAS-Laufwerken mit 10.000 U/min und RAID 10 verwendet.

### <a name="size-recommendations-for-the-process-server"></a>Empfohlene Größen für den Prozessserver
Wenn Sie mehr als 200 Computer schützen müssen oder die tägliche Änderungsrate größer als 2 TB ist, können Sie zusätzliche Prozessserver zum Verarbeiten der Replikationslast hinzufügen. Zum horizontalen Hochskalieren haben Sie folgende Möglichkeiten:

* Erhöhen Sie die Anzahl von Konfigurationsservern. Beispielsweise können Sie mit zwei Konfigurationsservern bis zu 400 Computer schützen.
* Fügen Sie zusätzliche Prozessserver hinzu, und verwenden Sie diese anstelle des Konfigurationsservers (oder als Ergänzung zu diesem) für die Abwicklung des Datenverkehrs.

Die folgende Tabelle beschreibt dieses Szenario:

* Sie planen nicht, den Konfigurationsserver als Prozessserver zu verwenden.
* Sie haben einen zusätzlichen Prozessserver eingerichtet.
* Sie haben geschützte virtuelle Computer so konfiguriert, dass sie den zusätzlichen Prozessserver verwenden.
* Jeder geschützte Quellcomputer ist mit drei Datenträgern von jeweils 100 GB konfiguriert.

| **Konfigurationsserver** | **Zusätzlicher Prozessserver** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 Sockets * 4 Kerne @ 2,5 GHz), 16 GB Speicher |4 vCPUs (2 Sockets * 2 Kerne @ 2,5 GHz), 8 GB Speicher |300 GB |250 GB oder weniger |Bis zu 85 Computer replizieren. |
| 8 vCPUs (2 Sockets * 4 Kerne @ 2,5 GHz), 16 GB Speicher |8 vCPUs (2 Sockets * 4 Kerne @ 2,5 GHz), 12 GB Speicher |600 GB |250 GB bis 1 TB |Zwischen 85 und 150 Computer replizieren. |
| 12 vCPUs (2 Sockets * 6 Kerne @ 2,5 GHz), 18 GB Speicher |12 vCPUs (2 Sockets * 6 Kerne @ 2,5 GHz), 24 GB Speicher |1 TB |1 TB bis 2 TB |Zwischen 150 und 225 Computer replizieren. |

Wie Sie Ihre Server skalieren, hängt davon ab, ob Sie das zentrale Hochskalieren oder das horizontale Hochskalieren als Modell bevorzugen.  Beim zentralen Hochskalieren stellen Sie wenige besonders leistungsstarke Konfigurations- und Prozessserver bereit. Beim horizontalen Hochskalieren stellen Sie mehr Server mit geringeren Ressourcen bereit. Wenn Sie zum Beispiel 220 Computer schützen müssen, können Sie sich für eine der beiden folgenden Optionen entscheiden:

* Richten Sie den Konfigurationsserver mit 12 vCPU und 18 GB Arbeitsspeicher und einen zusätzlichen Prozessserver mit 12 vCPU und 24 GB Arbeitsspeicher ein, und konfigurieren Sie die geschützten Computer so, dass sie nur den zusätzlichen Prozessserver verwenden.
* Alternativ können Sie zwei Konfigurationsserver (2 x 8vCPU, 16 GB RAM) und zwei zusätzliche Prozessserver (1 x 8vCPU und 1 x 4vCPU zur Handhabung von 135 + 85 (220) Computern) konfigurieren und geschützte Computer so konfigurieren, dass sie nur die zusätzlichen Prozessserver verwenden.

[Befolgen Sie diese Anweisungen](#deploy-additional-process-servers) , um einen zusätzlichen Prozessserver einzurichten.

### <a name="network-bandwidth-considerations"></a>Aspekte der Netzwerkbandbreite
Sie können den Capacity Planner verwenden, um die Bandbreite zu berechnen, die Sie für die Replikation benötigen (erste Replikation und dann Deltareplikation). Zur Steuerung der für die Replikation verwendeten Bandbreite stehen Ihnen verschiedene Optionen zur Verfügung:

* **Bandbreite drosseln**: VMware-Datenverkehr, der nach Azure repliziert wird, wird über einen speziellen Prozessserver geleitet. Sie können die Bandbreite auf den Computern, die als Prozessserver ausgeführt werden, einschränken.
* **Bandbreite beeinflussen**: Die für die Replikation genutzte Bandbreite lässt sich mithilfe einiger Registrierungsschlüssel beeinflussen:
  * Der Registrierungswert **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** gibt die Anzahl von Threads an, die für die Datenübertragung (erste Replikation oder Deltareplikation) eines Datenträgers verwendet werden. Bei einem höheren Wert wird die Netzwerkbandbreite für die Replikation erhöht.
  * Der Registrierungswert **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** gibt die Anzahl von Threads an, die für die Datenübertragung während eines Failbacks verwendet werden.

#### <a name="throttle-bandwidth"></a>Bandbreite drosseln
1. Öffnen Sie das Microsoft Azure Backup-MMC-Snap-In auf dem Computer, der als Prozessserver fungiert. Standardmäßig ist auf dem Desktop oder unter „C:\Programme\Microsoft Azure Recovery Services Agent\bin\wabadmin“ eine Verknüpfung für Microsoft Azure Backup verfügbar.
2. Klicken Sie im Snap-In auf **Eigenschaften ändern**.

    ![Bandbreite einschränken](./media/site-recovery-vmware-to-azure/throttle1.png)
3. Wählen Sie auf der Registerkarte **Drosselung** die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren** aus, und legen Sie die Grenzwerte für Geschäftszeiten und arbeitsfreie Zeiten fest. Der gültige Bereich reicht von 512 KBit/s bis 102 MBit/s.

    ![Bandbreite drosseln](./media/site-recovery-vmware-to-azure/throttle2.png)

Sie können auch das Cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) verwenden, um die Drosselung festzulegen. Hier ein Beispiel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** gibt an, dass keine Drosselung erforderlich ist.

#### <a name="influence-network-bandwidth"></a>Netzwerkbandbreite beeinflussen
1. Navigieren Sie in der Registrierung zu **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Um die Bandbreite für den Datenverkehr auf einem replizierenden Datenträger zu beeinflussen, ändern Sie den Wert für **UploadThreadsPerVM**, oder erstellen Sie den Schlüssel, falls dieser nicht vorhanden ist.
   * Um die Bandbreite für den Failbackdatenverkehr von Azure zu beeinflussen, ändern Sie den Wert für **DownloadThreadsPerVM**.
2. Der Standardwert ist 4. In einem absichtlich mit großen Reserven ausgestatteten Netzwerk müssen die Standardwerte dieser Registrierungsschlüssel geändert werden. Der maximale Wert beträgt 32. Überwachen Sie den Datenverkehr, um den Wert zu optimieren.

## <a name="step-6-replicate-applications"></a>Schritt 6: Replizieren von Anwendungen
Stellen Sie sicher, dass Computer, die Sie replizieren möchten, für die Installation des Mobilitätsdiensts vorbereitet sind, und aktivieren Sie anschließend die Replikation.

### <a name="install-the-mobility-service"></a>Installieren des Mobilitätsdiensts
Der erste Schritt beim Aktivieren des Schutzes für virtuelle Computer und physische Server besteht im Installieren des Mobilitätsdiensts. Dazu haben Sie mehrere Möglichkeiten:

* **Serverpushvorgänge verarbeiten**: Wenn Sie die Replikation auf einem Computer aktivieren, übertragen Sie die Mobilitätsdienstkomponente mittels Push vom Prozessserver, und installieren Sie sie. Beachten Sie, dass die Pushinstallation nicht auftritt, wenn auf den Computern bereits eine aktuelle Version der Komponente ausgeführt wird.
* **Pushvorgang im Unternehmen**: Installieren Sie die Komponente automatisch mithilfe des in Ihrem Unternehmen verwendeten Pushprozesses, z.B. WSUS (Windows Server Update Services) oder System Center Configuration Manager oder [Azure Automation und Desired State Configuration](site-recovery-automate-mobility-service-install.md). Richten Sie zuvor den Konfigurationsserver ein.
* **Manuelle Installation**: Installieren Sie die Komponente manuell auf jedem Computer, den Sie replizieren möchten. Richten Sie zuvor den Konfigurationsserver ein.

#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Vorbereiten des automatischen Pushs auf Windows-Computern
Im Folgenden wird beschrieben, wie Sie Windows-Computer vorbereiten, damit der Prozessserver automatisch den Mobilitätsdienst darauf installieren kann.

1. Erstellen Sie ein Konto, mit dem der Prozessserver auf den Computer zugreifen kann. Das Konto muss über Administratorrechte (lokal oder für die Domäne) verfügen und es wird nur für die Pushinstallation verwendet.

   > [!NOTE]
   > Wenn Sie kein Domänenkonto verwenden, müssen Sie die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren. Zu diesem Zweck fügen Sie in der Registrierung unter „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System“ den DWORD-Eintrag „LocalAccountTokenFilterPolicy“ mit dem Wert 1 hinzu. Um den Registrierungseintrag von einer Befehlszeilenschnittstelle aus hinzuzufügen, geben Sie **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**ein.
   >
   >
2. Wählen Sie in der Windows-Firewall des Computers, den Sie schützen möchten, **Eine App oder ein Feature durch die Windows-Firewall zulassen**aus. Aktivieren Sie **Datei- und Druckerfreigabe** und **Windows-Verwaltungsinstrumentation**. Für Computer, die einer Domäne angehören, können Sie die Firewalleinstellungen mit einem Gruppenrichtlinienobjekt konfigurieren.

   ![Firewalleinstellungen](./media/site-recovery-vmware-to-azure/mobility1.png)
3. Fügen Sie das von Ihnen erstellte Konto hinzu:

   * Öffnen Sie **cspsconfigtool**. Es steht als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner [INSTALLATIONSORT]\home\svsystems\bin.
   * Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
   * Fügen Sie das von Ihnen erstellte Konto hinzu. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie die Replikation für einen Computer aktivieren.

#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Vorbereiten des automatischen Pushs auf Linux-Servern
1. Vergewissern Sie sich, dass der zu schützende Linux-Computer unterstützt wird, wie unter [Voraussetzungen für geschützte Computer](#protected-machine-prerequisites)beschrieben. Stellen Sie sicher, dass zwischen dem Linux-Computer und dem Prozessserver eine Netzwerkkonnektivität besteht.
2. Erstellen Sie ein Konto, mit dem der Prozessserver auf den Computer zugreifen kann. Das Konto sollte ein Root-Benutzer auf dem Linux-Quellserver sein und wird nur für die Pushinstallation verwendet.

   * Öffnen Sie **cspsconfigtool**. Es steht als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner [INSTALLATIONSORT]\home\svsystems\bin.
   * Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
   * Fügen Sie das von Ihnen erstellte Konto hinzu. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie die Replikation für einen Computer aktivieren.
3. Vergewissern Sie sich, dass die Datei „/etc/hosts“ auf dem Linux-Quellserver Einträge enthält, die den Namen des lokalen Hosts den IP-Adressen zuordnen, die allen Netzwerkkarten zugewiesen sind.
4. Installieren Sie die neuesten „openssh“-, „openssh-server“- und „openssl“-Pakete auf dem Computer, den Sie replizieren möchten.
5. Stellen Sie sicher, dass SSH auf Port 22 aktiviert ist und ausgeführt wird.
6. Aktivieren Sie das SFTP-Subsystem und die Kennwortauthentifizierung in der Datei „sshd_config“ wie folgt:

   * Melden Sie sich als Root-Benutzer an.
   * Suchen Sie in der Datei „/etc/ssh/sshd_config“ die Zeile, die mit **PasswordAuthentication** beginnt.
   * Heben Sie die Auskommentierung der Zeile auf, und ändern Sie den Wert von **no** in **yes**.
   * Suchen Sie die Zeile, die mit **Subsystem** beginnt, und heben Sie die Auskommentierung der Zeile auf.

     ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)

### <a name="install-the-mobility-service-manually"></a>Manuelles Installieren des Mobilitätsdiensts
Die Installationsprogramme stehen auf dem Konfigurationsserver unter **C:\Programme (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository** zur Verfügung.

| Quellbetriebssystem | Installationsdatei für den Mobilitätsdienst |
| --- | --- |
| Windows Server (nur 64 Bit) |Microsoft-ASR_UA_9.*.0.0_Windows_* release.exe |
| CentOS 6.4, 6.5, 6.6 (nur 64 Bit) |Microsoft-ASR_UA_9.*.0.0_RHEL6-64_*release.tar.gz |
| SUSE Linux Enterprise Server 11 SP3 (nur 64 Bit) |Microsoft-ASR_UA_9.*.0.0_SLES11-SP3-64_*release.tar.gz |
| Oracle Enterprise Linux 6.4, 6.5 (nur 64 Bit) |Microsoft-ASR_UA_9.*.0.0_OL6-64_*release.tar.gz |

#### <a name="install-mobility-service-on-a-windows-server"></a>Installieren des Mobilitätsdiensts auf einem Windows Server
1. Laden Sie das entsprechende Installationsprogramm herunter, und führen Sie es aus.
2. Wählen Sie unter **Before you begin** (Vorbereitung) die Option **Mobility service** (Mobilitätsdienst) aus.

    ![Mobilitätsdienst](./media/site-recovery-vmware-to-azure/mobility3.png)
3. Geben Sie unter **Configuration Server Details** (Konfigurationsserverdetails) die IP-Adresse des Konfigurationsservers und die Passphrase ein, die beim Ausführen des einheitlichen Setups generiert wurden. Sie können die Passphrase abrufen, indem Sie auf dem Konfigurationsserver **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe –v** ausführen.

    ![Mobilitätsdienst](./media/site-recovery-vmware-to-azure/mobility6.png)
4. Behalten Sie unter **Install Location** (Installationsort) die Standardeinstellung bei und klicken Sie auf **Next** (Weiter), um mit der Installation zu beginnen.
5. Überwachen Sie die Installation unter **Installation Progress** (Installationsstatus), und starten Sie den Computer neu, wenn Sie dazu aufgefordert werden. Nach der Installation des Diensts kann es etwa 15 Minuten dauern, bis der Status im Portal aktualisiert wird.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Installieren des Mobilitätsdiensts auf einem Windows-Server über die Eingabeaufforderung
1. Kopieren Sie das Installationsprogramm in einen lokalen Ordner (z.B. C:\Temp) auf dem Server, den Sie schützen möchten. Das Installationsprogramm befindet sich auf dem Konfigurationsserver unter **[Installationsort]\home\svsystems\pushinstallsvc\repository**. Das Paket für Windows-Betriebssysteme verfügt über einen Namen, der etwa „Microsoft-ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe“ lautet.
2. Benennen Sie diese Datei in „MobilitySvcInstaller.exe“ um.
3. Führen Sie den folgenden Befehl aus, um das MSI-Installationsprogramm zu extrahieren:

    ``C:\> cd C:\Tempww
    ``C:\Temp > MobilitySvcInstaller.exe/q / /xC:\Temp\Extracted``
    ``C:\Temp > cd extrahiert``
    ``C:\Temp\Extracted > UnifiedAgent.exe/Role "Agent" /CSEndpoint "IP-Adresse des Konfigurationsservers"/PassphraseFilePath <Full path to the passphrase file>``

##### <a name="full-command-line-syntax"></a>Vollständige Befehlszeilensyntax
    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parameter**

* **/Role:** Obligatorisch. Gibt an, ob der Mobilitätsdienst installiert werden soll. Eingabewerte Agent|MasterTarget
* **/InstallLocation:** Obligatorisch. Gibt an, wo der Dienst installiert werden soll.
* **/PassphraseFilePath:** Obligatorisch. Die Passphrase des Konfigurationsservers.
* **/LogFilePath:** Obligatorisch. Der Speicherort, an dem die Dateien des Installationsprotokolls erstellt werden sollen.

#### <a name="uninstall-the-mobility-service-manually"></a>Manuelles Deinstallieren des Mobilitätsdiensts
Der Mobilitätsdienst kann über „Software“ in der Systemsteuerung oder mithilfe der Befehlszeilenanweisung MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} deinstalliert werden.

#### <a name="install-the-mobility-service-on-a-linux-server"></a>Installieren des Mobilitätsdiensts auf einem Linux-Server
1. Kopieren Sie anhand der obigen Tabelle das geeignete TAR-Archiv auf den zu replizierenden Linux-Computer.
2. Öffnen Sie ein Shellprogramm, und extrahieren Sie das gezippte TAR-Archiv in einen lokalen Pfad, indem Sie Folgendes ausführen: `tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Erstellen Sie in dem lokalen Verzeichnis, in das Sie den Inhalt des TAR-Archivs extrahiert haben, die Datei „passphrase.txt“. Hierzu kopieren Sie die Passphrase aus „C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase“ auf dem Konfigurationsserver, und speichern Sie sie dann in „passphrase.txt“, indem Sie *`echo <passphrase> >passphrase.txt`* in der Shell ausführen.
4. Installieren Sie den Mobilitätsdienst, indem Sie *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*ausführen.
5. Geben Sie die interne IP-Adresse des Konfigurationsservers an, und stellen Sie sicher, dass der Port 443 ausgewählt ist. Nach der Installation des Diensts kann es etwa 15 Minuten dauern, bis der Status im Portal aktualisiert wird.

**Sie können die Installation auch über die Befehlszeile ausführen**:

Kopieren Sie die Passphrase aus „C:\Programme (x86)\InMage Systems\private\connection“ auf dem Konfigurationsserver, und speichern Sie sie in „passphrase.txt“ auf dem Konfigurationsserver. Führen Sie anschließend die aufgeführten Befehle aus. In unserem Beispiel wird für den Konfigurationsserver die IP-Adresse 104.40.75.37 verwendet, und der HTTPS-Port sollte 443 sein:


So installieren Sie den Dienst auf einem Produktionsserver:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

So installieren Sie den Dienst auf dem Masterzielserver:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


### <a name="enable-replication"></a>Replikation aktivieren
#### <a name="before-you-start"></a>Vorbereitung
Wenn Sie VMware-VMs replizieren, beachten Sie Folgendes:

* VMware-VMs werden alle 15 Minuten ermittelt. Es kann 15 Minuten oder länger dauern, bis sie nach der Ermittlung im Portal angezeigt werden. Ebenso kann Ermittlung 15 Minuten oder länger dauern, wenn Sie einen neuen vCenter-Server oder vSphere-Host hinzufügen.
* Es kann auch 15 Minuten oder länger dauern, bis Umgebungsänderungen auf dem virtuellen Computer (z.B. die Installation von VMware-Tools) im Portal aktualisiert werden.
* Sie können den Zeitpunkt der letzten Ermittlung für die VMware-VMs im Feld für den vCenter-Server oder den vSphere-Host **Last Contact At** (Letzter Kontakt um) im Blatt **Configuration Servers** (Konfigurationsserver) überprüfen.
* Wenn Sie Computer für die Replikation hinzufügen möchten, ohne auf die planmäßige Ermittlung zu warten, markieren Sie den Konfigurationsserver (nicht darauf klicken), und klicken Sie auf die Schaltfläche **Refresh** (Aktualisieren).
* Wenn Sie die Replikation aktivieren, installiert der Prozessserver den Mobilitätsdienst automatisch auf dem Computer, falls dieser vorbereitet ist.

#### <a name="exclude-disks-from-replication"></a>Ausschließen von Datenträgern von der Replikation
Wenn Sie die Replikation aktivieren, werden standardmäßig alle Datenträger auf einem Computer repliziert. Sie können Datenträger von der Replikation ausschließen. Beispielsweise möchten Sie möglicherweise nicht, dass Datenträger mit temporären Daten oder Daten, die bei jedem Neustart des Computers oder der Anwendung aktualisiert werden (z.B. pagefile.sys oder SQL Server tempdb), repliziert werden. Beachten Sie Folgendes:

* Sie können nur Datenträger ausschließen, auf denen der Mobilitätsdienst bereits installiert ist. Sie müssen [den Mobilitätsdienst manuell installieren](#install-the-mobility-service-manually), da dieser nur nach Aktivierung der Replikation mithilfe des Pushmechanismus installiert wird.
* Nur Basisdatenträger können von der Replikation ausgeschlossen werden. Betriebssystem- oder dynamische Datenträger können nicht ausgeschlossen werden.
* Nach Aktivierung der Replikation können Sie keine Datenträger für die Replikation hinzufügen oder entfernen. Wenn Sie einen Datenträger hinzufügen oder entfernen möchten, müssen Sie den Schutz für den Computer deaktivieren und anschließend erneut aktivieren.
* Wenn Sie einen Datenträger ausschließen, der für den Betrieb einer Anwendung erforderlich ist, müssen Sie ihn nach dem Failover auf Azure manuell in Azure erstellen, damit die replizierte Anwendung ausgeführt werden kann. Alternativ können Sie Azure Automation in einen Wiederherstellungsplan integrieren, um den Datenträger während des Failovers des Computers zu erstellen.
* Virtueller Windows-Computer: Für Datenträger, die Sie manuell in Azure erstellen, wird kein Failback ausgeführt. Wenn Sie also beispielsweise ein Failover für drei Datenträger ausführen und zwei Datenträger direkt auf dem virtuellen Azure-Computer erstellen, wird nur für die drei Datenträger, für die das Failover ausgeführt wurde, ein Failback ausgeführt. Manuell erstellte Datenträger können nicht in das Failback oder in den erneuten Schutz von lokal auf Azure einbezogen werden.
* Virtueller Linux-Computer: Für Datenträger, die Sie manuell in Azure erstellen, wird ein Failback ausgeführt. Wenn Sie beispielsweise ein Failover für drei Datenträger ausführen und zwei direkt in Azure erstellen, wird für alle fünf ein Failback ausgeführt. Sie können keine Datenträger von einem Failback ausschließen, die manuell erstellt wurden.

**Aktivieren Sie die Replikation jetzt wie folgt**:

1. Klicken Sie auf **Schritt 2: Replizieren Sie die Anwendung** > **Quelle**. Klicken Sie nach der erstmaligen Aktivierung der Replikation im Tresor auf **+Replizieren**, um die Replikation für weitere Computer zu aktivieren.
2. Wählen Sie auf dem Blatt **Quelle** > **Quelle** den Konfigurationsserver aus.
3. Wählen Sie unter **Computertyp** die Option **Virtuelle Computer** oder **Physische Computer** aus.
4. Wählen Sie unter **vCenter-/vSphere-Hypervisor** den vCenter-Server aus, der den vSphere-Host verwaltet, oder wählen Sie den Host aus. Wenn Sie physische Computer replizieren, ist diese Einstellung nicht relevant.
5. Wählen Sie den Prozessserver aus. Wenn Sie keine zusätzlichen Prozessserver erstellt haben, ist dies der Name des Konfigurationsservers. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. Wählen Sie unter **Ziel** das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die virtuellen Computer erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die virtuellen Computer verwendet werden soll, für die ein Failover durchgeführt wurde (klassisch oder Resource Manager).


7. Wählen Sie das Azure-Speicherkonto aus, das Sie für die Replikation von Daten verwenden möchten. Beachten Sie Folgendes:

   * Sie können ein Premium- oder Standardspeicherkonto auswählen. Wenn Sie ein Premium-Konto auswählen, müssen Sie für fortlaufende Replikationsprotokolle ein zusätzliches Standard-Speicherkonto angeben. Die Konten müssen sich in derselben Region wie der Recovery Services-Tresor befinden.
   * Falls Sie keines der bereits vorhandenen Speicherkonten verwenden möchten, können Sie ein [Speicherkonto erstellen](#set-up-an-azure-storage-account). Klicken Sie zum Erstellen eines Speicherkontos mit dem Resource Manager auf **Neu erstellen**. Wenn Sie ein Speicherkonto mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das [Azure-Portal](../storage/storage-create-storage-account-classic-portal.md).
8. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem Azure-VMs eine Verbindung herstellen, wenn sie nach einem Failover erstellt werden. Das Netzwerk muss sich in derselben Region wie der Recovery Services-Tresor befinden. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen. Wenn Sie über kein Netzwerk verfügen, müssen Sie ein [Netzwerk erstellen](#set-up-an-azure-network). Klicken Sie zum Erstellen eines Netzwerks mit dem Resource Manager auf **Neu erstellen**. Falls Sie ein Netzwerk mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das [Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Wählen Sie, falls zutreffend, ein Subnetz aus. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-rep3.png)
9. Klicken Sie auf **Virtuelle Computer** > **Virtuelle Computer auswählen**, und wählen Sie die Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-replication5.png)
10. Geben Sie unter **Eigenschaften** > **Eigenschaften konfigurieren**das Konto aus, das der Prozessserver zum automatischen Installieren des Mobilitätsdiensts auf dem Computer verwenden soll. Standardmäßig werden alle Datenträger repliziert. Klicken Sie auf **Alle Datenträger** , und entfernen Sie alle Datenträger, die Sie nicht replizieren möchten. Klicken Sie dann auf **OK**. Sie können später weitere Eigenschaften festlegen.

    ![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-replication6.png)
11. Überprüfen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren**, ob die richtige Replikationsrichtlinie ausgewählt ist. Sie können die Replikationsrichtlinieneinstellungen unter **Einstellungen** > **Replikationsrichtlinien** > Richtlinienname > **Einstellungen bearbeiten** ändern. Die Änderungen, die Sie an einer Richtlinie vornehmen, werden auf die Replikation und auf neue Computer angewendet.
12. Aktivieren Sie **Multi-VM-Konsistenz** , wenn Sie Computer in einer Replikationsgruppe zusammenfassen möchten, und geben Sie einen Namen für die Gruppe an. Klicken Sie dann auf **OK**. Beachten Sie Folgendes:

    * Die Computer in einer Replikationsgruppe werden gemeinsam repliziert und verfügen beim Failover über gemeinsame ausfallsichere und anwendungskonsistente Wiederherstellungspunkte.
    * Es wird empfohlen, dass Sie virtuelle Computer und physische Server zusammenfassen, damit sie Ihre Workloads widerspiegeln. Das Aktivieren von Multi-VM-Konsistenz kann sich auf die Leistung der Workload auswirken und sollte nur verwendet werden, wenn Computer die gleiche Workload ausführen und Sie Konsistenz benötigen.

    ![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-replication7.png)
13. Klicken Sie auf **Replikation aktivieren**. Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.

> [!NOTE]
> Wenn der Computer für die Pushinstallation vorbereitet ist, wird die Mobilitätsdienstkomponente installiert, wenn der Schutz aktiviert wird. Nachdem die Komponente auf dem Computer installiert wurde, wird ein Schutzauftrag gestartet. Er führt zu einem Fehler. Nach dem Fehler müssen Sie jeden Computer manuell neu starten. Nach dem Neustart wird der Schutzauftrag erneut gestartet, und die erste Replikation wird ausgeführt.
>
>

### <a name="view-and-manage-vm-properties"></a>Anzeigen und Verwalten von VM-Eigenschaften
Es wird empfohlen, dass Sie die Eigenschaften des Quellcomputers überprüfen. Beachten Sie, dass der Name des virtuellen Azure-Computers die [Anforderungen für virtuelle Azure-Computer](site-recovery-best-practices.md#azure-virtual-machine-requirements)erfüllen muss.

1. Klicken Sie auf **Einstellungen** > **Replizierte Elemente**, und wählen Sie den Computer aus. Das Blatt **Zusammenfassung** enthält Informationen zu den Einstellungen und zum Status der Computer.
2. Unter **Eigenschaften** können Sie die Informationen zur Replikation und zum Failover für den virtuellen Computer anzeigen.

    ![Replikation aktivieren](./media/site-recovery-vmware-to-azure/test-failover2.png)
3. Unter **Compute und Netzwerk** > **Compute-Eigenschaften** können Sie den Namen und die Zielgröße des virtuellen Azure-Computers angeben. Ändern Sie ggf. den Namen, damit er die Azure-Anforderungen erfüllt.
   Sie können auch Informationen zum Zielnetzwerk, zum Subnetz sowie zu der IP-Adresse anzeigen und hinzufügen, die der Azure-VM zugewiesen wird. Beachten Sie Folgendes:

   * Sie können die Ziel-IP-Adresse festlegen. Wenn Sie keine Adresse angeben, wird für den Computer, für den das Failover durchgeführt wurde, DHCP verwendet. Wenn Sie eine Adresse festlegen, die beim Failover nicht verfügbar ist, funktioniert das Failover nicht. Dieselbe Ziel-IP-Adresse kann für das Testfailover verwendet werden, wenn die Adresse im Testfailover-Netzwerk verfügbar ist.
   * Die Anzahl der Netzwerkkarten hängt von der Größe ab, die Sie für den virtuellen Zielcomputer angeben. Hierbei gilt Folgendes:

     * Wenn die Anzahl der Netzwerkkarten des Quellcomputers maximal der Anzahl der Netzwerkkarten entspricht, die für die Größe des Zielcomputers zulässig ist, hat der Zielcomputer die gleiche Anzahl von Netzwerkkarten wie der Quellcomputer.
     * Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputer die maximal zulässige Anzahl für die Größe des Zielcomputers übersteigt, wird die Anzahl verwendet, die maximal für die Größe des Zielcomputers zulässig ist.
     * Ein Beispiel: Wenn ein Quellcomputer zwei Netzwerkkarten besitzt und der Zielcomputer aufgrund seiner Größe vier Netzwerkkarten unterstützt, erhält der Zielcomputer zwei Netzwerkkarten. Wenn der Quellcomputer dagegen zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe nur einen Adapter unterstützt, erhält der Zielcomputer nur einen Adapter.     
   * Wenn der virtuelle Computer über mehrere Netzwerkadapter verfügt, werden alle mit dem gleichen Netzwerk verbunden.
   * Wenn der virtuelle Computer über mehrere Netzwerkadapter verfügt, wird der erste, der in der Liste angezeigt wird, zum *Standard*-Netzwerkadapter im virtuellen Azure-Computer.

     ![Replikation aktivieren](./media/site-recovery-vmware-to-azure/test-failover4.png)
4. Unter **Datenträger** werden das Betriebssystem und die Datenträger auf der VM angezeigt, die repliziert werden.

## <a name="step-7-test-the-deployment"></a>Schritt 7: Testen der Bereitstellung
Zum Testen der Bereitstellung können Sie ein Test-Failover für einen einzelnen virtuellen Computer durchführen. Alternativ können Sie einen Wiederherstellungsplan erstellen, der mehrere virtuelle Computer enthält.

### <a name="prepare-for-failover"></a>Vorbereiten des Failovers
* Zum Ausführen eines Testfailovers empfehlen wir Folgendes: Erstellen Sie ein neues Azure-Zielnetzwerk, das von Ihrem Azure-Produktionsnetzwerk isoliert ist (Standardverhalten bei der Erstellung eines neuen Netzwerks in Azure). [Erfahren Sie mehr](site-recovery-failover.md#run-a-test-failover) über die Ausführung von Testfailovervorgängen.
* Installieren Sie den Azure-Agent auf dem geschützten Computer, um für das Failover zu Azure die beste Leistung zu erzielen. Hierdurch wird der Startvorgang beschleunigt und die Problembehandlung vereinfacht. Installieren Sie den Agent für [Linux](https://github.com/Azure/WALinuxAgent) oder [Windows](http://go.microsoft.com/fwlink/?LinkID=394789).
* Zum vollständigen Testen der Bereitstellung benötigen Sie eine entsprechende Infrastruktur, damit der replizierte Computer wie erwartet funktioniert. Wenn Sie Active Directory und DNS testen möchten, können Sie einen virtuellen Computer als Domänencontroller mit DNS erstellen und per Azure Site Recovery zu Azure replizieren. Weitere Informationen finden Sie unter [Überlegungen zum Testfailover für Active Directory](site-recovery-active-directory.md#test-failover-considerations).
* Stellen Sie sicher, dass der Konfigurationsserver ausgeführt wird. Andernfalls schlägt das Failover fehl.
* Wenn Sie Datenträger von der Replikation ausgeschlossen haben, müssen Sie diese Datenträger nach einem Failover in Azure möglicherweise manuell erstellen, damit die Anwendung wie erwartet ausgeführt wird.
* Wenn Sie anstelle eines Testfailovers ein ungeplantes Failover durchführen möchten, beachten Sie Folgendes:

  * Fahren Sie primäre Computer nach Möglichkeit herunter, bevor Sie ein ungeplantes Failover ausführen. Dadurch wird sichergestellt, dass die Quell- und Replikatcomputer nicht gleichzeitig ausgeführt werden. Wenn Sie VMware-VMs replizieren, können Sie festlegen, dass Site Recovery bestmöglich versuchen soll, die Quellcomputer herunterzufahren. Dies funktioniert abhängig vom Zustand am primären Standort. Wenn Sie physische Server replizieren, bietet Site Recovery diese Möglichkeit nicht.
  * Beim Durchführen eines ungeplanten Failovers wird die Datenreplikation der primären Computer beendet. Das heißt, dass nach dem Beginn des Failovers keine Datenänderungen mehr übertragen werden. Außerdem wird ein ungeplantes Failover, das auf einem Wiederherstellungsplan basiert, auch dann bis zum Ende durchgeführt, wenn ein Fehler auftritt.

## <a name="failover"></a>Failover
Nach Abschluss der ersten Replikation für Ihre Computer könne Sie bei Bedarf Failover auslösen. Site Recovery unterstützt verschiedene Arten von Failovern: Testfailover, geplantes Failover und ungeplantes Failover.
Lesen Sie die [weiteren Informationen](site-recovery-failover.md) zu den unterschiedlichen Typen von Failovern und die ausführlichen Beschreibungen dazu, wann und wie sie durchgeführt werden sollten.

> [!NOTE]
> Falls Sie virtuelle Computer zu Azure migrieren möchten, empfehlen wir Ihnen dringend, ein [geplantes Failover](site-recovery-failover.md#run-an-unplanned-failover) durchzuführen, um die virtuellen Computer zu Azure zu migrieren. Nachdem die migrierte Anwendung in Azure per Testfailover überprüft wurde, können Sie die Schritte unter [Vollständige Migration](#Complete-migration-of-your-virtual-machines-to-Azure) ausführen, um die Migration Ihrer virtuellen Computer durchzuführen. Sie müssen keinen Commit- oder Löschvorgang durchführen. Mit „Migration abschließen“ wird die Migration abgeschlossen, der Schutz für den virtuellen Computer beendet und die Azure Site Recovery-Gebührenberechnung für den Computer beendet.
>
>

### <a name="run-an-unplanned-failover"></a>Durchführen eines ungeplanten Failovers
Hier erfahren Sie, wie Sie ein ungeplantes Failover für einen Wiederherstellungsplan durchführen. Alternativ können Sie das Failover über die Registerkarte „Virtuelle Computer“ auch für einen einzelnen virtuellen Computer durchführen. Vergewissern Sie sich zunächst, dass bei allen virtuellen Computern, für die ein Failover durchgeführt werden soll, die erste Replikation abgeschlossen ist.

1. Wählen Sie **Wiederherstellungspläne > <Name des Wiederherstellungsplans>**.
2. Klicken Sie auf dem Blatt „Wiederherstellungsplan“ auf **Ungeplantes Failover**.
3. Wählen Sie auf der Seite **Ungeplantes Failover** den Quell- und Zielort aus.
4. Wählen Sie **Virtuelle Computer herunterfahren und die aktuellen Daten synchronisieren** aus, damit Site Recovery die geschützten virtuellen Computer herunterfährt und die Daten synchronisiert, sodass das Failover mit der neuesten Version der Daten erfolgt.
5. Nach dem Failover weist der Status der virtuellen Computer darauf hin, dass ein Commit aussteht.  Klicken Sie auf **Commit**, um ein Commit für das Failover auszuführen.

[Weitere Informationen](site-recovery-failover.md#run-an-unplanned-failover)

## <a name="complete-migration-of-your-virtual-machines-to-azure"></a>Abschließen der Migration für Ihre virtuellen Computer zu Azure
> [!NOTE]
> Die folgenden Schritte gelten nur, wenn Sie virtuelle Computer zu Azure migrieren.
>
>

1. Führen Sie ein ungeplantes Failover wie [hier](site-recovery-failover.md#run-an-unplanned-failover) beschrieben durch.
2. Klicken Sie unter **Einstellungen > Replizierte Elemente** mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Migration abschließen**.

    ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)
3. Klicken Sie auf **OK**, um die Migration abzuschließen. Sie können den Status verfolgen, indem Sie auf die VM klicken, um die Eigenschaften zu öffnen. Alternativ dazu können Sie unter **Einstellungen > Site Recovery-Aufträge** den Auftrag „Migration abschließen“ verwenden.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover
Gehen Sie wie folgt vor, wenn Sie die Verbindung mit Azure-VMs nach dem Failover per RDP herstellen möchten:

**Auf dem lokalen Computer vor dem Failover**:

* Aktivieren Sie für den Zugriff über das Internet RDP. Stellen Sie sicher, dass TCP- und UDP-Regeln für **Öffentlich** hinzugefügt werden, und stellen Sie sicher, dass RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für alle Profile zugelassen ist.
* Aktivieren Sie für den Zugriff über eine Site-to-Site-Verbindung RDP auf dem Computer, und stellen Sie sicher, dass RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für die Netzwerke vom Typ **Domäne** und **Privat** zugelassen ist.
* Installieren Sie den [Azure-VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) auf dem lokalen Computer.
* [Installieren Sie den Mobilitätsdienst manuell](#install-the-mobility-service-manually) auf Computern, anstatt den Prozessserver zur automatischen Pushinstallation des Diensts zu verwenden. Dies ist erforderlich, da die Pushinstallation nur durchgeführt wird, nachdem der Computer für die Replikation aktiviert wurde.
* Achten Sie darauf, dass die SAN-Richtlinie des Betriebssystems auf OnlineAll festgelegt ist. [Weitere Informationen](https://support.microsoft.com/kb/3031135)
* Deaktivieren Sie den IPSec-Dienst, bevor Sie das Failover durchführen.

**Auf der Azure-VM nach einem Failover**:

* Fügen Sie einen öffentlichen Endpunkt für das RDP-Protokoll (Port 3389) hinzu, und geben Sie die Anmeldeinformationen für die Anmeldung an.
* Stellen Sie sicher, dass keine Domänenrichtlinien vorhanden sind, die das Verbinden mit einem virtuellen Computer über eine öffentliche Adresse verhindern.
* Versuchen Sie, eine Verbindung herzustellen. Wenn Sie keine Verbindung herstellen können, sollten Sie überprüfen, ob die VM ausgeführt wird. Weitere Tipps zur Problembehandlung finden Sie in [diesem Artikel](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Gehen Sie wie folgt vor, wenn Sie nach dem Failover auf eine Azure-VM mit Linux über einen Secure Shell-Client (ssh) zugreifen möchten:

**Auf dem lokalen Computer vor dem Failover**:

* Stellen Sie sicher, dass der Secure Shell-Dienst auf der Azure-VM so festgelegt ist, dass er beim Systemstart automatisch gestartet wird.
* Überprüfen Sie, ob die Firewallregeln eine SSH-Verbindung damit zulassen.

**Auf der Azure-VM nach einem Failover**:

* In den Netzwerksicherheitsgruppen-Regeln auf dem virtuellen Computer nach dem Failover und für das Azure-Subnetz, mit dem der virtuelle Computer verbunden ist, müssen eingehende Verbindungen über den SSH-Port zulässig sein.
* Es sollte ein öffentlicher Endpunkt erstellt werden, um eingehende Verbindungen für den SSH-Port (standardmäßig TCP-Port 22) zuzulassen.
* Wenn auf den virtuellen Computer über eine VPN-Verbindung (ExpressRoute- oder Site-to-Site-VPN-Verbindung) zugegriffen wird, kann der Client verwendet werden, um per SSH eine direkte Verbindung mit dem virtuellen Computer herzustellen.

**Auf dem virtuellen Azure-Computer mit Windows/Linux nach einem Failover**:

Wenn Sie über eine Netzwerksicherheitsgruppe verfügen, die der VM oder dem VM-Subnetz zugeordnet ist, dann stellen Sie sicher, dass die Gruppe über eine ausgehende Regel zum Zulassen von HTTP/HTTPS verfügt. Vergewissern Sie sich außerdem, dass das DNS des Netzwerks, das als Ziel für das Failover der VM fungiert, korrekt konfiguriert ist. Andernfalls kann ein Failover-Timeout mit dem Fehler „PreFailoverWorkflow task WaitForScriptExecutionTask timed out“ (Timeout bei der PreFailoverWorkflow-Aufgabe „WaitForScriptExecutionTask“) auftreten. [detaillierte Kapazitätsplanung](site-recovery-monitoring-and-troubleshooting.md#recovery)

## <a name="run-a-test-failover"></a>Ausführen eines Testfailovers
1. Klicken Sie zum Durchführen eines Failovers für einen einzelnen Computer unter **Einstellungen** > **Replizierte Elemente** auf den virtuellen Computer und dann auf das Symbol **+Testfailover**.

    ![Testfailover](./media/site-recovery-vmware-to-azure/test-failover1.png)
2. Klicken Sie zum Durchführen eines Failovers für einen Wiederherstellungsplan unter **Einstellungen** > **Wiederherstellungspläne** mit der rechten Maustaste auf den Plan, und klicken Sie dann auf **Testfailover**. Eine Anleitung zum Erstellen eines Wiederherstellungsplans finden Sie [hier](site-recovery-create-recovery-plans.md).
3. Wählen Sie unter **Testfailover** das Azure-Netzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden.
4. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt verfolgen, indem Sie auf den virtuellen Computer klicken, um die Eigenschaften zu öffnen. Alternativ können Sie unter „Tresorname“ > **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** auf den Auftrag **Testfailover** klicken.
5. Gehen Sie wie folgt vor, wenn das Failover den Status **Test abschließen** erreicht hat:

   1. Zeigen Sie den virtuellen Replikatcomputer im Azure-Portal an. Prüfen Sie, ob der virtuelle Computer erfolgreich startet.
   2. Wenn Sie den Zugriff auf virtuelle Computer über Ihr lokales Netzwerk eingerichtet haben, können Sie eine Remotedesktopverbindung mit dem virtuellen Computer herstellen.
   3. Klicken Sie auf **Test abschließen**, um den Test abzuschließen.

       ![Testfailover](./media/site-recovery-vmware-to-azure/test-failover6.png)
   4. Klicken Sie auf **Notizen**, um alle Beobachtungen im Zusammenhang mit dem Test-Failover zu erfassen und zu speichern.
   5. Klicken Sie auf **The test failover is complete** (Das Testfailover ist abgeschlossen), um eine automatische Bereinigung der Testumgebung durchzuführen. Nach Abschluss dieses Vorgangs wird für das Testfailover der Status **Abgeschlossen** angezeigt.
   6. An diesem Punkt werden alle Elemente oder VMs gelöscht, die von Site Recovery während des Testfailovers automatisch erstellt wurden. Alle weiteren Elemente, die Sie für das Testfailover erstellt haben, werden nicht gelöscht.

      > [!NOTE]
      > Sollte ein Testfailover länger als zwei Wochen ausgeführt werden, wird sein Abschluss erzwungen.
      >
      >
6. Nach Abschluss des Failovers sollte der Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt werden. Stellen Sie sicher, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.
7. Wenn Sie die [Vorbereitung für Verbindungen nach dem Failover](#prepare-to-connect-to-azure-vms-after-failover) durchgeführt haben, sollten Sie eine Verbindung mit dem virtuellen Azure-Computer herstellen können.

## <a name="monitor-your-deployment"></a>Überwachen der Bereitstellung
Hier wird beschrieben, wie Sie die Konfigurationseinstellungen, den Status und die Integrität für die Site Recovery-Bereitstellung überwachen können:

1. Klicken Sie auf den Tresornamen, um auf das Dashboard **Zusammenfassung** zuzugreifen. In diesem Dashboard werden Site Recovery-Aufträge, Replikationsstatus, Wiederherstellungspläne, Serverzustand und Ereignisse angezeigt.  Sie können die Einstellungen so anpassen, dass die nützlichsten Kacheln und Layouts angezeigt werden, einschließlich des Status der Tresore.<br>
   ![Zusammenfassung](./media/site-recovery-vmware-to-azure/essentials.png)
2. Auf der Kachel **Integrität** können Sie Probleme mit Standortservern (VMM- oder Konfigurationsserver) sowie die Ereignisse überwachen, die von Site Recovery in den letzten 24 Stunden ausgelöst wurden.
3. Über die Kacheln **Replizierte Elemente**, **Wiederherstellungspläne** und **Site Recovery-Aufträge** können Sie die Replikation verwalten und überwachen. Genauere Details zu Aufträgen können Sie unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** anzeigen.

## <a name="deploy-additional-process-servers"></a>Bereitstellen zusätzlicher Prozessserver
Wenn Sie Ihre Bereitstellung über 200 Quellcomputer oder eine gesamte tägliche Änderungsrate von 2 TB hinaus horizontal hochskalieren müssen, benötigen Sie zusätzliche Prozessserver zur Bewältigung des Datenverkehrsaufkommens.

Überprüfen Sie die [für Prozessserver empfohlenen Größen](#size-recommendations-for-the-process-server), und befolgen Sie anschließend diese Anweisungen, um den Prozessserver einzurichten. Nachdem Sie den Prozessserver eingerichtet haben, migrieren Sie Quellcomputer, damit diese ihn verwenden können.

### <a name="install-an-additional-process-server"></a>Installieren eines zusätzlichen Prozessservers
1. Klicken Sie unter **Einstellungen** > **Site Recovery servers** (Site Recovery-Server) auf den Konfigurationsserver und anschließend auf **Prozessserver**.

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/migrate-ps1.png)
2. Klicken Sie unter **Servertyp** auf **Process server (on-premises)** (Prozessserver (lokal)).

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
3. Laden Sie die Datei für das einheitliche Setup von Site Recovery herunter, führen Sie sie zum Installieren des Prozessservers aus, und registrieren Sie sie im Tresor.
4. Wählen Sie unter **Before you begin** die Option **Add additional process servers to scale out deployment** (Weitere Prozessserver zum horizontalen Hochskalieren der Bereitstellung hinzufügen) aus.
5. Schließen Sie den Assistenten genau wie beim [Einrichten](#step-2-set-up-the-source-environment) des Konfigurationsservers ab.

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/add-ps1.png)
6. Geben Sie unter **Configuration Server Details** die IP-Adresse des Konfigurationsservers und die Passphrase an. Führen Sie **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe –n** auf dem Konfigurationsserver aus, um die Passphrase abzurufen.

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migrieren der Computer für die Verwendung des neuen Prozessservers
1. Klicken Sie unter **Einstellungen** > **Site Recovery servers** auf den Konfigurationsserver, und erweitern Sie anschließend **Prozessserver**.

    ![Prozessserver aktualisieren](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. Klicken Sie mit der rechten Maustaste auf den aktuell verwendeten Prozessserver, und klicken Sie auf **Wechseln**.

    ![Prozessserver aktualisieren](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. Wählen Sie unter **Zielprozessserver auswählen** den neuen Prozessserver aus, den Sie verwenden möchten, und wählen Sie anschließend die virtuellen Computer aus, für die der neue Prozessserver zuständig sein wird. Klicken Sie auf das Informationssymbol, um Informationen über den Server zu erhalten. Der durchschnittliche zum Replizieren jedes ausgewählten virtuellen Computers auf den neuen Prozessserver benötigte Speicherplatz wird angezeigt, um Sie bei Lastenentscheidungen zu unterstützen. Klicken Sie auf das Häkchen, um mit dem Replizieren auf den neuen Prozessserver zu beginnen.

## <a name="vmware-account-permissions"></a>VMware-Kontoberechtigungen
Site Recovery benötigt Zugriff auf VMware-Konten für Failover und Failback und damit der Prozessserver VMs automatisch auf einem vCenter-Server ermitteln kann. Die erforderlichen Rollenberechtigungen werden in der folgenden Tabelle zusammengefasst.

* Wenn Sie nur VMs und Failover zu Azure ohne ein Failback (Migration) ermitteln möchten, können Sie ein Konto ohne Schreibzugriff erstellen.
* Für Failover und Failback empfehlen wir die Erstellung einer VMware-Rolle (Azure_Site_Recovery) mit den erforderlichen Berechtigungen und anschließend eine Zuweisung der Rolle zu einem VMware-Benutzer oder einer VMware-Gruppe.

| **Task** | **Rollentyp** | **Berechtigungen** | **Details** |
| --- | --- | --- | --- |
| VMware-Ermittlung<br/><br/> Failover zu Azure ohne Herunterfahren der Quell-VM (dies ist nützlich für die Migration, für die kein Failback ausgeführt wird) |Schreibgeschützter VMware-Benutzer |Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt) |Der Benutzer wird auf Rechenzentrumsebene zugewiesen und hat daher Zugriff auf alle Objekte im Rechenzentrum.<br/><br/> Wenn Sie den Zugriff einschränken möchten, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle „No access“ (Kein Zugriff) mit „Propagate to child object“ (Auf untergeordnetes Objekt übertragen) zu. |
| Failover und Failback |VMware-Benutzer<br/><br/> Dieser Benutzer muss Vorgänge wie das Erstellen und Entfernen von Datenträgern und das Einschalten von VMs usw. durchführen können.<br/><br/> Wir empfehlen, dass Sie eine Rolle (Azure_Site_Recovery) mit den erforderlichen Berechtigungen erstellen und sie dann einem VMware-Benutzer oder einer VMware-Gruppe zuweisen |Rechenzentrumsobjekt -> An untergeordnetes Objekt weitergeben, Rolle=Azure_Site_Recovery<br/><br/> Datenspeicher -> Speicherplatz zuordnen, Datenspeicher durchsuchen, Low-Level-Dateivorgänge, Datei entfernen, Dateien virtueller Computer aktualisieren<br/><br/> Netzwerk -> Netzwerk zuweisen<br/><br/> Ressource -> Zuweisen der VM zu einem Ressourcenpool, ausgeschaltete VM migrieren, eingeschaltete VM migrieren<br/><br/> Tasks (Aufgaben) -> Create task (Aufgabe erstellen), update task (Aufgabe aktualisieren)<br/><br/> Virtueller Computer -> Konfiguration<br/><br/> Virtueller Computer -> Interagieren -> Frage beantworten, Geräteverbindung, CD-Medien konfigurieren, Diskettenmedien konfigurieren, Ausschalten, Einschalten, VMware-Tools installieren<br/><br/> Virtueller Computer -> Inventar -> Erstellen, Registrieren, Registrierung aufheben<br/><br/> Virtueller Computer -> Bereitstellung -> Download virtueller Computer zulassen, Upload von Dateien virtueller Computer zulassen<br/><br/> Virtual machine -> Snapshots -> Remove snapshots |Der Benutzer wird auf Rechenzentrumsebene zugewiesen und hat daher Zugriff auf alle Objekte im Rechenzentrum.<br/><br/> Wenn Sie den Zugriff einschränken möchten, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle „No access“ (Kein Zugriff) mit „Propagate to child object“ (Auf untergeordnetes Objekt übertragen) zu. |

## <a name="next-steps"></a>Nächste Schritte
* [Informieren](site-recovery-failover.md) Sie sich über die unterschiedlichen Failoverarten.
* [Erfahren Sie mehr über das Failback](site-recovery-failback-azure-to-vmware.md), um Failbacks durchzuführen und Azure-VMs zurück auf den lokalen primären Standort zu replizieren.

## <a name="third-party-software-notices-and-information"></a>Hinweise und Informationen zu Drittanbietersoftware
Do Not Translate or Localize

The software and firmware running in the Microsoft product or service is based on or incorporates material from the projects listed below (collectively, “Third Party Code”).  Microsoft is the not original author of the Third Party Code.  The original copyright notice and license, under which Microsoft received such Third Party Code, are set forth below.

The information in Section A is regarding Third Party Code components from the projects listed below. Such licenses and information are provided for informational purposes only.  This Third Party Code is being relicensed to you by Microsoft under Microsoft's software licensing terms for the Microsoft product or service.  

The information in Section B is regarding Third Party Code components that are being made available to you by Microsoft under the original licensing terms.

The complete file may be found on the [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft reserves all rights not expressly granted herein, whether by implication, estoppel or otherwise.



<!--HONumber=Dec16_HO2-->


