---
title: "Voraussetzungen für die Replikation in Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel werden die Voraussetzungen für die Replikation von virtuellen und physischen Computern in Azure mithilfe des Azure Site Recovery-Diensts zusammengefasst."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/11/2016
ms.author: rajanaki
translationtype: Human Translation
ms.sourcegitcommit: 46ea97f5c13b7befc78466fae92a737107be8f83
ms.openlocfilehash: 31cae0cf625552c0e9547d0e514c6017c6a179ef

---

#  <a name="prerequisites-for-replication-to-azure-using-azure-site-recovery"></a>Voraussetzungen für die Replikation in Azure mithilfe von Azure Site Recovery

 Site Recovery ist ein Azure-Dienst, der einen Beitrag zu Ihrer BCDR-Strategie leistet, indem die Replikation von lokalen physischen Servern und virtuellen Computern in die Cloud (Azure) oder in ein sekundäres Datencenter orchestriert wird. Wenn es an Ihrem primären Standort zu Ausfällen kommt, können Sie ein Failover zu einem sekundären Standort durchführen, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, können Sie das Failback zum primären Standort durchführen. Weitere Informationen finden Sie unter [Was ist Site Recovery?](site-recovery-overview.md)

Dieser Artikel beschreibt die Voraussetzungen für die Replikation in Azure Site Recovery.

Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="azure-requirements"></a>Anforderungen für Azure

**Anforderung** | **Details**
--- | ---
**Azure-Konto** | Ein [Microsoft Azure](http://azure.microsoft.com/) -Konto.<br/><br/> Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung.
**Site Recovery-Dienst** | [Erfahren Sie mehr](https://azure.microsoft.com/pricing/details/site-recovery/) über die Preise für Site Recovery. |
**Azure-Speicher** | Sie benötigen ein Azure-Speicherkonto zum Speichern von replizierten Daten. Das Konto muss sich in der gleichen Region wie der Recovery Services-Tresor befinden. Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden erstellt, wenn ein Failover durchgeführt wird.<br/><br/> Richten Sie ein Konto im [Resource Manager-Modell](../storage/storage-create-storage-account.md) oder im [klassischen Modell](../storage/storage-create-storage-account-classic-portal.md) ein (je nachdem, welches Ressourcenmodell Sie nach dem Failover für virtuelle Azure-Computer verwenden möchten).<br></br>Sie können [GRS](../storage/storage-redundancy.md#geo-redundant-storage)- oder LRS-Speicher verwenden. Wir empfehlen Ihnen die Verwendung von GRS, damit Resilienz für die Daten besteht, wenn es zu einem regionalen Ausfall kommt oder wenn die primäre Region nicht wiederhergestellt werden kann.<br/><br/> Wenn Sie VMware-VMs oder physische Server im Azure-Portal replizieren, wird Storage Premium unterstützt. [Premiumspeicher](https://docs.microsoft.com/en-us/azure/storage/storage-premium-storage) wird normalerweise für virtuelle Computer verwendet, die eine konsistent hohe E/A-Leistung und geringe Latenz zum Hosten von E/A-intensiven Workloads erfordern. Bei Verwendung von Premium-Speicher für replizierte Daten benötigen Sie auch ein Standardspeicherkonto zum Speichern von Replikationsprotokollen, in denen laufende Änderungen lokaler Daten erfasst werden.<br/><br/>
**Speichereinschränkungen** | Speicherkonten, die in Site Recovery verwendet werden, können nicht über Ressourcengruppen hinweg innerhalb von Abonnements oder über verschiedene Abonnements hinweg verschoben werden.<br/><br/> Die Replikation von Storage Premium-Konten in den Regionen „Indien, Mitte“ und „Indien, Süden“ wird zurzeit nicht unterstützt.
**Azure-Netzwerk** | Sie benötigen ein Azure-Netzwerk, mit dem Azure-VMs nach dem Failover eine Verbindung herstellen. Es muss sich in der gleichen Region wie der Recovery Services-Tresor befinden.<br/><br/> Im Azure-Portal können Sie Netzwerke im [Resource Manager-Modell](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) oder im [klassischen Modell](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) erstellen.<br/><br/> Wenn Sie von VMM zu Azure replizieren, richten Sie eine [Netzwerkzuordnung](site-recovery-network-mapping.md) zwischen VMM-VM-Netzwerken und Azure-Netzwerken ein, um sicherzustellen, dass sich Azure-VMs nach dem Failover mit entsprechenden Netzwerken verknüpfen.
**Netzwerkeinschränkungen** | Netzwerkkonten, die in Site Recovery verwendet werden, können nicht über Ressourcengruppen hinweg innerhalb von Abonnements oder über verschiedene Abonnements hinweg verschoben werden.
**Netzwerkzuordnung** | Wenn Sie Hyper-V-VMs in VMM-Clouds replizieren, müssen Sie die [Netzwerkzuordnung](site-recovery-network-mapping.md) so einrichten, dass für Azure-VMs eine Verbindung mit geeigneten Netzwerken hergestellt wird, wenn sie nach dem Failover erstellt werden.

>[!NOTE]
>Die folgenden Abschnitte beschreiben die Voraussetzungen für die verschiedenen Komponenten in der Kundenumgebung. Weitere Informationen zur Unterstützung bestimmter Konfigurationen finden Sie in der [Unterstützungsmatrix](site-recovery-support-matrix.md).
>

## <a name="disaster-recovery-of-vmware-virtual-machines-or-physical-windowslinux-servers-to-azure"></a>Notfallwiederherstellung von virtuellen VMware-Computern oder physischen Windows-/Linux-Servern in Azure
Im Folgenden werden die erforderlichen Komponenten aufgeführt, die für die Notfallwiederherstellung von virtuellen VMware-Computern oder physischen Windows-/Linux-Servern zusätzlich zu den [oben](#Azure requirements) genannten benötigt werden.


1. **Konfigurationsserver oder zusätzliche Prozessserver**: Sie müssen einen lokalen Computer als Konfigurationsserver einrichten, um die Kommunikation zwischen dem lokalen Standort und Azure zu koordinieren und die Datenreplikation zu verwalten. <br></br>

2. **VMware vCenter-/vSphere-Host**

| **Komponente** | **Anforderungen** |
| --- | --- |
| **vSphere** | Mindestens einen VMware vSphere-Hypervisor.<br/><br/>Auf den Hypervisoren muss vSphere in einer der Versionen 6.0, 5.5 oder 5.1 mit den neuesten Updates ausgeführt werden.<br/><br/>Ihre vSphere-Hosts und vCenter-Server sollten sich im gleichen Netzwerk befinden wie der Prozessserver (dabei handelt es sich um das Netzwerk, in dem sich der Konfigurationsserver befindet, es sei denn, Sie haben einen dedizierten Prozessserver eingerichtet). |
| **vCenter** | Es wird empfohlen, dass Sie einen VMware vCenter-Server zum Verwalten Ihrer vSphere-Hosts bereitstellen. Auf diesem Server muss vCenter 6.0 oder 5.5 mit den neuesten Updates ausgeführt werden.<br/><br/>**Einschränkung**: Site Recovery unterstützt die neuen Features von vCenter und vSphere 6.0 nicht. Hierzu zählen etwa Cross vCenter vMotion, virtuelle Volumes und Storage DRS. Site Recovery unterstützt nur Features, die auch in Version 5.5 verfügbar waren.||

3. **Voraussetzungen für replizierte Computer**


| **Komponente** | **Anforderungen** |
| --- | --- |
| **Lokal (VMware-VMs)** | Auf replizierten VMs sollten VMware-Tools installiert sein und ausgeführt werden.<br/><br/> VMs müssen die [Voraussetzungen für Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements) zum Erstellen von Azure-VMs erfüllen.<br/><br/>Die individuelle Datenträgerkapazität auf geschützten Computern sollte nicht mehr als 1023 GB betragen. <br/><br/>Für die Komponenteninstallation sind mindestens 2 GB verfügbarer Speicherplatz auf dem Installationslaufwerk erforderlich.<br/><br/>**Port 20004** sollte auf der lokalen VM-Firewall geöffnet werden, wenn Sie Multi-VM-Konsistenz aktivieren möchten.<br/><br/>Computernamen dürfen zwischen 1 und 63 Zeichen enthalten (Buchstaben, Zahlen und Bindestriche). Der Name muss mit einem Buchstaben oder einer Zahl beginnen und mit einem Buchstaben oder einer Zahl enden. Sie können den Azure-Namen ändern, nachdem Sie die Replikation für einen Computer aktiviert haben.<br/><br/> |
| **Windows-Computer (physisch oder VMware)** | Auf dem Computer muss ein unterstütztes 64-Bit-Betriebssystem ausgeführt werden: Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 mit SP1 oder höher.<br/><br/> Das Betriebssystem sollte auf dem Laufwerk C:\ installiert werden. Bei dem Betriebssystem-Datenträger sollte es sich um einen Windows-Basisdatenträger anstelle eines dynamischen Datenträgers handeln. Der Datenträger kann dynamisch sein.<br/><br/>|
| **Linux-Computer** (physisch oder VMware) | Sie benötigen ein unterstütztes 64-Bit-Betriebssystem: Red Hat Enterprise Linux 6.7,7.1,7.2; Centos 6.5, 6.6, 6.7, 7.0, 7.1, 7.2; Oracle Enterprise Linux 6.4, 6.5, auf dem der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird, SUSE Linux Enterprise Server 11 SP3.<br/><br/>Dateien vom Typ „/etc/hosts“ auf geschützten Computern müssen Einträge enthalten, die den Namen des lokalen Hosts den IP-Adressen aller Netzwerkkarten zuordnen.<br/><br/>Wenn Sie nach einem Failover eine Verbindung mit virtuellen Azure-Computern unter Linux über einen Secure Shell-Client (SSH) herstellen möchten, stellen Sie sicher, dass der Secure Shell-Dienst auf dem geschützten Computer beim Systemstart automatisch gestartet wird und dass die Firewallregeln eine SSH-Verbindung zulassen.<br/><br/>Der Hostname, Bereitstellungspunkte, Gerätenamen und Linux-Systempfade und -Dateinamen (z. B./etc/, /usr) sollten nur englische Zeichen umfassen.<br/><br/>

## <a name="disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-vmm"></a>Notfallwiederherstellung von Hyper-V-VMs in Azure (ohne VMM)

Im Folgenden werden die erforderlichen Komponenten aufgeführt, die für die Notfallwiederherstellung von virtuellen Hyper-V-Computern in VMM-Clouds zusätzlich zu den [oben](#Azure requirements) genannten benötigt werden.

| **Voraussetzung** | **Details** |
| --- | --- |
| **Hyper-V-Host** |Mindestens ein lokaler Server, auf dem Windows Server 2012 R2 mit den aktuellsten Updates und aktivierter Hyper-V-Rolle oder Microsoft Hyper-V Server 2012 R2 ausgeführt wird.<br></br>Der Hyper-V-Server muss mindestens einen virtuellen Computer enthalten.<br></br>Hyper-V-Server müssen entweder direkt oder über einen Proxy mit dem Internet verbunden sein.<br></br>Auf Hyper-V-Servern sollten die in [KB2961977](https://support.microsoft.com/en-us/kb/2961977) erwähnten Fixes installiert sein.
|**Anbieter und Agent**| Während der Bereitstellung von Azure Site Recovery installieren Sie den Azure Site Recovery-Anbieter. Die Anbieterinstallation installiert den Azure Recovery Services-Agent außerdem auf jedem Hyper-V-Server, der über zu schützende virtuelle Computer verfügt. <br></br>Alle Hyper-V-Server in einem Site Recovery-Tresor sollten über die gleichen Anbieter- und Agent-Versionen verfügen.<br></br>Der Anbieter muss über das Internet eine Verbindung mit Azure Site Recovery herstellen. Datenverkehr kann direkt oder über einen Proxy gesendet werden. Beachten Sie, dass HTTPS-basierte Proxys nicht unterstützt werden. Der Proxyserver sollte den Zugriff auf Folgendes zulassen: <br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/>Wenn Sie auf dem Server über Firewallregeln verfügen, die auf IP-Adressen basieren, sollten Sie sicherstellen, dass die Regeln die Kommunikation mit Azure zulassen.<br></br> Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.<br></br> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.


## <a name="disaster-recovery-of-hyper-v-virtual-machines-in-vmm-clouds-to-azure"></a>Notfallwiederherstellung von virtuellen Hyper-V-Computern in VMM-Clouds in Azure

Im Folgenden werden die erforderlichen Komponenten aufgeführt, die für die Notfallwiederherstellung von virtuellen Hyper-V-Computern in VMM-Clouds zusätzlich zu den [oben](#Azure requirements) genannten benötigt werden.

| **Voraussetzung** | **Details** |
| --- | --- |
| **VMM** |Mindestens ein VMM-Server unter **System Center 2012 R2 oder höher**. Für jeden VMM-Server sollte mindestens eine Cloud konfiguriert sein. <br></br>Eine Cloud sollte Folgendes enthalten<br>– Eine oder mehrere VMM-Hostgruppen.<br/>– Einen oder mehrere Hyper-V-Hostserver oder Cluster in jeder Hostgruppe.<br/><br/>[hier](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx) zum Einrichten von VMM-Clouds. |
| **Hyper-V** |Auf Hyper-V-Hostservern muss mindestens **Windows Server 2012 R2** mit Hyper-V-Rolle oder **Microsoft Hyper-V Server 2012 R2** ausgeführt werden, und die neuesten Updates müssen installiert sein.<br/><br/> Ein Hyper-V-Server muss mindestens einen virtuellen Computer enthalten.<br/><br/> Ein Hyper-V-Hostserver oder -Cluster, der zu replizierende VMs enthält, muss in einer VMM-Cloud verwaltet werden.<br/><br/>Hyper-V-Server müssen entweder direkt oder über einen Proxy mit dem Internet verbunden sein.<br/><br/>Auf Hyper-V-Servern sollten die in [Artikel&29619;77](https://support.microsoft.com/kb/2961977) genannten Fehlerbehebungen installiert sein.<br/><br/>Hyper-V-Hostserver benötigen Internetzugriff für die Datenreplikation in Azure. |
| **Anbieter und Agent** |Während der Azure Site Recovery-Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf dem VMM-Server und den Recovery Services-Agent auf Hyper-V-Hosts. Anbieter und Agent müssen direkt oder über einen Proxy eine Internetverbindung mit Azure herstellen. Ein HTTPS-basierter Proxy wird nicht unterstützt. Der Proxyserver auf dem VMM-Server und auf Hyper-V-Hosts sollte den Zugriff auf folgende URLs zulassen: <br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Wenn Sie auf dem VMM-Server über Firewallregeln verfügen, die auf IP-Adressen basieren, sollten Sie sicherstellen, dass die Regeln die Kommunikation mit Azure zulassen.<br/><br/> Lassen Sie die [IP-Bereiche für Azure-Rechenzentren](https://www.microsoft.com/download/confirmation.aspx?id=41653) und den HTTPS-Port (443) zu.<br/><br/> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.<br/><br/> |

### <a name="replicated-machine-prerequisites"></a>Voraussetzungen für replizierte Computer
| **Komponente** | **Details** |
| --- | --- |
| **Geschützte VMs** | Site Recovery unterstützt alle Betriebssysteme, die von [Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx) unterstützt werden.<br></br>VMs müssen die [Voraussetzungen für Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements) zum Erstellen von Azure-VMs erfüllen. Computernamen dürfen zwischen 1 und 63 Zeichen enthalten (Buchstaben, Zahlen und Bindestriche). Der Name muss mit einem Buchstaben oder einer Zahl beginnen und mit einem Buchstaben oder einer Zahl enden. <br></br>Sie können den Namen ändern, nachdem Sie die Replikation für die VM aktiviert haben. <br/><br/> Die Kapazität der einzelnen Datenträger auf geschützten Computern darf maximal 1.023 GB betragen. Ein virtueller Computer kann bis zu 16 Datenträger haben (also bis zu 16 TB).<br/><br/>


## <a name="disaster-recovery-of-hyper-v-virtual-machines-in-vmm-clouds-to-a-customer-owned-site"></a>Notfallwiederherstellung von virtuellen Hyper-V-Computern in VMM-Clouds an einem Kundenstandort

Im Folgenden werden die erforderlichen Komponenten aufgeführt, die für die Notfallwiederherstellung von virtuellen Hyper-V-Computern in VMM-Clouds an einem Kundenstandort zusätzlich zu den [oben](#Azure requirements) genannten benötigt werden.

| **Komponenten** | **Details** |
| --- | --- |
| **VMM** |  Wir empfehlen Ihnen, am primären und am sekundären Standort je einen VMM-Server bereitzustellen.<br/><br/> Sie können [auf einem einzelnen VMM-Server cloudübergreifend replizieren](site-recovery-single-vmm.md). Zu diesem Zweck benötigen Sie mindestens zwei auf dem VMM-Server konfigurierte Clouds.<br/><br/> Auf den VMM-Servern muss **mindestens System Center 2012 SP1** mit den neuesten Updates ausgeführt werden.<br/><br/> Jeder VMM-Server muss über mindestens eine Cloud verfügen. Für alle Clouds muss das Hyper-V-Kapazitätsprofil eingerichtet worden sein. <br/><br/>Die Clouds müssen mindestens eine VMM-Hostgruppe enthalten. [hier](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric) zum Einrichten von VMM-Clouds.<br/><br/> VMM-Server benötigen Zugriff auf das Internet. |
| **Hyper-V** | Auf den Hyper-V-Servern muss **mindestens Windows Server 2012 mit der Hyper-V-Rolle** ausgeführt werden, und die neuesten Updates müssen installiert sein.<br/><br/> Ein Hyper-V-Server muss mindestens einen virtuellen Computer enthalten.<br/><br/>  Hyper-V-Hostserver müssen sich in Hostgruppen in der primären und in der sekundären VMM-Cloud befinden.<br/><br/> Wenn Sie Hyper-V in einem Cluster unter Windows Server 2012 R2 ausführen, müssen Sie das [Update 2961977](https://support.microsoft.com/kb/2961977) installieren.<br/><br/> Wenn Sie Hyper-V in einem Cluster unter Windows Server 2012 ausführen, beachten Sie, dass der Clusterbroker nicht automatisch erstellt wird, falls Sie über einen auf statischen IP-Adressen basierenden Cluster verfügen. Sie müssen den Clusterbroker manuell konfigurieren. [Weitere Informationen](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx) |
| **Anbieter** | Während der Site Recovery-Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf VMM-Servern. Der Anbieter kommuniziert mit Site Recovery über HTTPS 443, um die Replikation zu orchestrieren. Die Datenreplikation erfolgt über das LAN oder eine VPN-Verbindung zwischen dem primären und sekundären Hyper-V-Server.<br/><br/> Der auf dem VMM-Server ausgeführte Anbieter benötigt Zugriff auf diese URLs:<br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Ermöglichen Sie die Firewallkommunikation der VMM-Server mit den [IP-Bereichen des Azure-Rechenzentrums (in englischer Sprache)](https://www.microsoft.com/download/confirmation.aspx?id=41653), und lassen Sie das HTTPS-Protokoll (443) zu. |



<!--HONumber=Jan17_HO2-->


