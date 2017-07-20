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
ms.date: 06/23/2017
ms.author: rajanaki
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: 1ed0bfe4f1b77db00dc858f010f72e084e77039e
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---

#  <a name="prerequisites-for-replication-from-on-premises-to-azure-using-azure-site-recovery"></a>Voraussetzungen für die Replikation von lokalen Standorten nach Azure mithilfe von Azure Site Recovery

> [!div class="op_single_selector"]
> * [Replikation von Azure nach Azure](site-recovery-azure-to-azure-prereq.md)
> * [Replikation von lokal nach Azure](site-recovery-prereq.md)

Azure Site Recovery ist ein Dienst, der einen Beitrag zu Ihrer Strategie für die Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR) leistet, indem er die Replikation von virtuellen Azure-Computern in eine andere Azure-Region bzw. von lokalen physischen Servern und virtuellen Computern in die Cloud (Azure) oder in ein sekundäres Rechenzentrum orchestriert. Wenn es an Ihrem primären Standort zu Ausfällen kommt, können Sie ein Failover zu einem sekundären Standort durchführen, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, können Sie das Failback zum primären Standort durchführen. Weitere Informationen zu Site Recovery finden Sie unter [Was ist Site Recovery?](site-recovery-overview.md).

Dieser Artikel fasst die Voraussetzungen für die Replikation von lokalen Standorten nach Azure mit Site Recovery zusammen.

Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="azure-requirements"></a>Anforderungen für Azure

**Anforderung** | **Details**
--- | ---
**Azure-Konto** | Ein [Microsoft Azure](http://azure.microsoft.com/) -Konto.<br/><br/> Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung.
**Site Recovery-Dienst** | Weitere Informationen zu den Preisen von Site Recovery finden Sie unter [Site Recovery-Preise](https://azure.microsoft.com/pricing/details/site-recovery/). |
**Azure-Speicher** | Sie benötigen ein Azure-Speicherkonto, um die replizierten Daten zu speichern. Das Konto muss sich in der gleichen Region befinden wie der Recovery Services-Tresor. Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden erstellt, wenn ein Failover durchgeführt wird.<br/><br/> Richten Sie im [Azure Resource Manager-Modell](../storage/storage-create-storage-account.md) oder im [klassischen Modell](../storage/storage-create-storage-account-classic-portal.md) ein Konto ein – je nachdem, welches Ressourcenmodell Sie nach dem Failover für virtuelle Azure-Computer verwenden möchten.<br/><br/>Sie können [georedundanten Speicher](../storage/storage-redundancy.md#geo-redundant-storage) oder lokal redundanten Speicher verwenden. Wir empfehlen die Verwendung von georedundantem Speicher, damit Resilienz für die Daten besteht, wenn es zu einem regionalen Ausfall kommt oder wenn die primäre Region nicht wiederhergestellt werden kann.<br/><br/> Sie können Storage Standard oder Storage Premium verwenden. [Storage Premium](https://docs.microsoft.com/azure/storage/storage-premium-storage) wird normalerweise für virtuelle Computer verwendet, die eine konsistent hohe E/A-Leistung und geringe Latenz zum Hosten von E/A-intensiven Workloads erfordern. Bei Verwendung von Premium-Speicher für replizierte Daten benötigen Sie auch ein Standardspeicherkonto zum Speichern von Replikationsprotokollen, in denen laufende Änderungen lokaler Daten erfasst werden.<br/><br/>
**Speichereinschränkungen** | Speicherkonten, die in Site Recovery verwendet werden, können nicht über Ressourcengruppen hinweg, innerhalb von Abonnements oder über verschiedene Abonnements hinweg verschoben werden.<br/><br/> Die Replikation von Storage Premium-Konten in den Regionen „Indien, Mitte“ und „Indien, Süden“ wird zurzeit nicht unterstützt.
**Azure-Netzwerk** | Sie benötigen ein Azure-Netzwerk, mit dem virtuelle Azure-Computer nach dem Failover eine Verbindung herstellen. Dieses muss sich in der gleichen Region befinden wie der Recovery Services-Tresor.<br/><br/> Im Azure-Portal können Sie Netzwerke im [Resource Manager-Modell](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) oder im [klassischen Modell](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) erstellen.<br/><br/> Bei der Replikation von System Center Virtual Machine Manager in Azure richten Sie eine Netzwerkzuordnung zwischen Virtual Machine Manager-VM-Netzwerken und Azure-Netzwerken ein, um sicherzustellen, dass virtuelle Azure-Computer nach dem Failover eine Verbindung mit entsprechenden Netzwerken herstellen.
**Netzwerkeinschränkungen** | Netzwerkkonten, die in Site Recovery verwendet werden, können nicht über Ressourcengruppen hinweg, innerhalb von Abonnements oder über verschiedene Abonnements hinweg verschoben werden.
**Netzwerkzuordnung** | Wenn Sie virtuelle Hyper-V-Computer in Virtual Machine Manager-Clouds replizieren, müssen Sie die Netzwerkzuordnung so einrichten, dass für virtuelle Azure-Computer eine Verbindung mit geeigneten Netzwerken hergestellt wird, wenn sie nach dem Failover erstellt werden.

>[!NOTE]
>In den folgenden Abschnitten werden die Voraussetzungen für verschiedene Komponenten in der Kundenumgebung beschrieben. Weitere Informationen zur Unterstützung bestimmter Konfigurationen finden Sie in der [Unterstützungsmatrix](site-recovery-support-matrix.md).
>

## <a name="disaster-recovery-of-vmware-virtual-machines-or-physical-windows-or-linux-servers-to-azure"></a>Notfallwiederherstellung virtueller VMware-Computer oder physischer Windows-/Linux-Server in Azure
Im Anschluss werden die Komponenten aufgeführt, die zusätzlich zu den genannten [Anforderungen für Azure](#Azure requirements) für die Notfallwiederherstellung virtueller VMware-Computer oder physischer Windows-/Linux-Server benötigt werden.


### <a name="configuration-server-or-additional-process-server-you-will-need-to-set-up-an-on-premises-machine-as-the-configuration-server-to-coordinate-communications-between-the-on-premises-site-and-azure-and-to-manage-data-replication-brbr"></a>**Konfigurationsserver oder zusätzliche Prozessserver**: Sie müssen einen lokalen Computer als Konfigurationsserver einrichten, um die Kommunikation zwischen dem lokalen Standort und Azure zu koordinieren und die Datenreplikation zu verwalten. <br></br>

1. **VMware vCenter oder vSphere-Host**

| **Komponente** | **Anforderungen** |
| --- | --- |
| **vSphere** | Mindestens einen VMware vSphere-Hypervisor.<br/><br/>Auf den Hypervisoren muss die vSphere-Version 6.0, 5.5 oder 5.1 mit den neuesten Updates ausgeführt werden.<br/><br/>Ihre vSphere-Hosts und vCenter-Server sollten sich im gleichen Netzwerk befinden wie der Prozessserver. Dabei handelt es sich um das Netzwerk, in dem sich der Konfigurationsserver befindet (es sei denn, Sie haben einen dedizierten Prozessserver eingerichtet). |
| **vCenter** | Es wird empfohlen, dass Sie einen VMware vCenter-Server zum Verwalten Ihrer vSphere-Hosts bereitstellen. Auf diesem Server muss vCenter 6.0 oder 5.5 mit den neuesten Updates ausgeführt werden.<br/><br/>**Einschränkung**: Die Sitewiederherstellung unterstützt kein übergreifendes vCenter vMotion. Storage DRS und Storage vMotion werden auch nach einem erneuten Schutzvorgang nicht auf virtuellen Hauptzielcomputern unterstützt.||

1. **Voraussetzungen für replizierte Computer**


| **Komponente** | **Anforderungen** |
| --- | --- |
| **Lokal** (virtuelle VMware-Computer) | Auf replizierten VMs sollten VMware-Tools installiert sein und ausgeführt werden.<br/><br/> VMs müssen die [Voraussetzungen für Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) zum Erstellen von Azure-VMs erfüllen.<br/><br/>Die Kapazität der einzelnen Datenträger auf geschützten Computern darf maximal 1.023 GB betragen. <br/><br/>Für die Komponenteninstallation sind mindestens 2 GB verfügbarer Speicherplatz auf dem Installationslaufwerk erforderlich.<br/><br/>Port 20004 muss für die lokale Firewall des virtuellen Computers geöffnet werden, wenn Sie Multi-VM-Konsistenz aktivieren möchten.<br/><br/>Computernamen dürfen zwischen 1 und 63 Zeichen enthalten (Buchstaben, Zahlen und Bindestriche). Der Name muss mit einem Buchstaben oder einer Zahl beginnen und mit einem Buchstaben oder einer Zahl enden. Sie können den Azure-Namen ändern, nachdem Sie die Replikation für einen Computer aktiviert haben.<br/><br/> |
| **Windows-Computer** (physisch oder VMware) | Auf dem Computer muss ein unterstütztes 64-Bit-Betriebssystem ausgeführt werden: Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 mit SP1 oder höher.<br/><br/> Das Betriebssystem muss auf dem Laufwerk „C“ installiert sein. Bei dem Betriebssystem-Datenträger muss es sich um einen nicht dynamischen Windows-Basisdatenträger handeln. Der normale Datenträger kann dynamisch sein.<br/><br/>|
| **Linux-Computer** (physisch oder VMware) | Sie benötigen ein unterstütztes 64-Bit-Betriebssystem: Red Hat Enterprise Linux 6.7, 6.8, 7.1 oder 7.2, CentOS 6.5, 6.6, 6.7, 6.8, 7.0, 7.1 oder 7.2, Ubuntu 14.04 LTS Server (eine Liste der unterstützten Kernelversionen bei Ubuntu finden Sie unter [unterstützte Betriebssysteme](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)), Oracle Enterprise Linux 6.4 oder 6.5 (unter dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird), SUSE Linux Enterprise Server 11 SP3, SUSE Linux Enterprise Server 11 SP4.<br/><br/>Ihre Dateien vom Typ „/etc/hosts“ auf geschützten Computern müssen Einträge enthalten, die den Namen des lokalen Hosts den IP-Adressen aller Netzwerkkarten zuordnen.<br/><br/>Wenn Sie nach einem Failover über einen Secure Shell-Client (SSH) eine Verbindung mit virtuellen Azure-Computern unter Linux herstellen möchten, stellen Sie sicher, dass der Secure Shell-Dienst auf dem geschützten Computer beim Systemstart automatisch gestartet wird und dass die Firewallregeln eine SSH-Verbindung zulassen.<br/><br/>Hostname, Bereitstellungspunkte, Gerätenamen und Linux-Systempfade/-Dateinamen (beispielsweise „/etc/“ oder „/usr“) dürfen nur auf Englisch angegeben werden.<br/><br/>Die folgenden Verzeichnisse (sofern als separate Partitionen/Dateisysteme eingerichtet) müssen sich auf demselben Datenträger (dem Datenträger mit dem Betriebssystem) auf dem Quellserver befinden: / (root), /boot, /usr, /usr/local, /var, /etc<br/><br/>XFS v5-Features, z. B. die Metadatenprüfsumme, werden derzeit von ASR nicht auf XFS-Dateisystemen unterstützt. Stellen Sie sicher, dass Ihre XFS-Dateisysteme keine v5-Features verwenden. Mit dem Hilfsprogramm „xfs_info“ können Sie den XFS-Superblock für die Partition überprüfen. Wenn ftype auf 1 festgelegt ist, werden Features von XFS v5 verwendet.<br/><br/>Auf Servern mit Red Hat Enterprise Linux 7 und CentOS 7 muss das Hilfsprogramm „lsof“ installiert und verfügbar sein.<br/><br/>


## <a name="disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager"></a>Notfallwiederherstellung virtueller Hyper-V-Computer in Azure (ohne Virtual Machine Manager)

Im Anschluss werden die Komponenten aufgeführt, die zusätzlich zu den genannten [Anforderungen für Azure](#Azure requirements) für die Notfallwiederherstellung virtueller Hyper-V-Computer in Virtual Machine Manager-Clouds benötigt werden.

| **Voraussetzung** | **Details** |
| --- | --- |
| **Hyper-V-Host** |Mindestens ein lokaler Server, auf dem Windows Server 2012 R2 (mit den neuesten Updates und aktivierter Hyper-V-Rolle) oder Microsoft Hyper-V Server 2012 R2 ausgeführt wird.<br/><br/>Die Hyper-V-Server müssen mindestens einen virtuellen Computer enthalten.<br/><br/>Hyper-V-Server müssen entweder direkt oder über einen Proxy mit dem Internet verbunden sein.<br/><br/>Auf Hyper-V-Servern sollten die in [KB2961977](https://support.microsoft.com/kb/2961977) erwähnten Fixes installiert sein.
|**Anbieter und Agent**| Im Rahmen der Bereitstellung von Azure Site Recovery installieren Sie den Azure Site Recovery-Anbieter. Bei der Anbieterinstallation wird auch der Azure Recovery Services-Agent auf jedem Hyper-V-Server installiert, der über zu schützende virtuelle Computer verfügt. <br/><br/>Alle Hyper-V-Server in einem Site Recovery-Tresor sollten über die gleichen Anbieter- und Agent-Versionen verfügen.<br/><br/>Der Anbieter muss über das Internet eine Verbindung mit Azure Site Recovery herstellen. Datenverkehr kann direkt oder über einen Proxy gesendet werden. HTTPS-basierte Proxys werden nicht unterstützt. Der Proxyserver sollte den Zugriff auf Folgendes zulassen: <br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/>Wenn Sie auf dem Server über Firewallregeln verfügen, die auf IP-Adressen basieren, stellen Sie sicher, dass die Regeln die Kommunikation mit Azure zulassen.<br/><br/> Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Datencenter) und den HTTPS-Port (443) zu.<br/><br/> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für den Westen der USA (für die Zugriffsteuerung und Identitätsverwaltung) zu.


## <a name="disaster-recovery-of-hyper-v-virtual-machines-in-virtual-machine-manager-clouds-to-azure"></a>Notfallwiederherstellung virtueller Hyper-V-Computer in Virtual Machine Manager-Clouds in Azure

Im Anschluss werden die Komponenten aufgeführt, die zusätzlich zu den genannten [Anforderungen für Azure](#Azure requirements) für die Notfallwiederherstellung virtueller Hyper-V-Computer in Virtual Machine Manager-Clouds benötigt werden.

| **Voraussetzung** | **Details** |
| --- | --- |
| **Virtual Machine Manager** |Mindestens ein Virtual Machine Manager-Server unter **System Center 2012 R2 oder höher**. Für jeden Virtual Machine Manager-Server muss mindestens eine Cloud konfiguriert sein. <br/><br/>Eine Cloud sollte Folgendes enthalten:<br>- Mindestens eine Virtual Machine Manager-Hostgruppe<br/>- Mindestens einen Hyper-V-Hostserver oder Cluster in jeder Hostgruppe<br/><br/>Weitere Informationen zum Einrichten von Virtual Machine Manager-Clouds finden Sie unter [How to Create a Cloud in VMM 2012](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx) (Erstellen einer Cloud in VMM 2012). |
| **Hyper-V** |Auf Hyper-V-Hostservern muss mindestens Windows Server 2012 R2 mit Hyper-V-Rolle oder Microsoft Hyper-V Server 2012 R2 ausgeführt werden, und die neuesten Updates müssen installiert sein.<br/><br/> Ein Hyper-V-Server muss mindestens einen virtuellen Computer enthalten.<br/><br/> Ein Hyper-V-Hostserver oder -Cluster, der zu replizierende virtuelle Computer enthält, muss in einer Virtual Machine Manager-Cloud verwaltet werden.<br/><br/>Hyper-V-Server müssen entweder direkt oder über einen Proxy mit dem Internet verbunden sein.<br/><br/>Auf Hyper-V-Servern müssen die im Artikel [2961977](https://support.microsoft.com/kb/2961977) genannten Fehlerbehebungen installiert sein.<br/><br/>Hyper-V-Hostserver benötigen Internetzugriff für die Datenreplikation in Azure. |
| **Anbieter und Agent** |Im Rahmen der Azure Site Recovery-Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf dem Virtual Machine Manager-Server und den Recovery Services-Agent auf Hyper-V-Hosts. Anbieter und Agent müssen direkt oder über einen Proxy eine Internetverbindung mit Azure herstellen. Ein HTTPS-basierter Proxy wird nicht unterstützt. Der Proxyserver auf dem Virtual Machine Manager-Server und auf Hyper-V-Hosts muss den Zugriff auf Folgendes ermöglichen: <br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Wenn Sie auf dem Virtual Machine Manager-Server über Firewallregeln verfügen, die auf IP-Adressen basieren, stellen Sie sicher, dass die Regeln die Kommunikation mit Azure zulassen.<br/><br/> Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Datencenter) und den HTTPS-Port (443) zu.<br/><br/>Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für den Westen der USA (für die Zugriffsteuerung und Identitätsverwaltung) zu.<br/><br/> |

### <a name="replicated-machine-prerequisites"></a>Voraussetzungen für replizierte Computer
| **Komponente** | **Details** |
| --- | --- |
| **Geschützte VMs** | Site Recovery unterstützt alle Betriebssysteme, die von [Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx) unterstützt werden.<br/><br/>VMs müssen die [Voraussetzungen für Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) zum Erstellen von Azure-VMs erfüllen. Computernamen dürfen zwischen 1 und 63 Zeichen enthalten (Buchstaben, Zahlen und Bindestriche). Der Name muss mit einem Buchstaben oder einer Zahl beginnen und mit einem Buchstaben oder einer Zahl enden. <br/><br/>Sie können den Namen ändern, nachdem Sie die Replikation für die VM aktiviert haben. <br/><br/> Die Kapazität der einzelnen Datenträger auf geschützten Computern darf maximal 1.023 GB betragen. Ein virtueller Computer kann bis zu 16 Datenträger haben (also bis zu 16 TB).<br/><br/>


## <a name="disaster-recovery-of-hyper-v-virtual-machines-in-virtual-machine-manager-clouds-to-a-customer-owned-site"></a>Notfallwiederherstellung virtueller Hyper-V-Computer in Virtual Machine Manager-Clouds an einem Standort des Kunden

Im Anschluss werden die Komponenten aufgeführt, die zusätzlich zu den genannten [Anforderungen für Azure](#Azure requirements) für die Notfallwiederherstellung virtueller Hyper-V-Computer in Virtual Machine Manager-Clouds an einem Standort des Kunden benötigt werden.

| **Komponenten** | **Details** |
| --- | --- |
| **Virtual Machine Manager** |  Es empfiehlt sich, einen Virtual Machine Manager-Server am primären Standort und einen Virtual Machine Manager-Server am sekundären Standort bereitzustellen.<br/><br/> Sie können [auf einem einzelnen VMM-Server cloudübergreifend replizieren](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment). Hierzu müssen auf dem Virtual Machine Manager-Server mindestens zwei Clouds konfiguriert sein.<br/><br/> Auf den Virtual Machine Manager-Servern muss mindestens System Center 2012 SP1 mit den neuesten Updates ausgeführt werden.<br/><br/> Jeder Virtual Machine Manager-Server muss über mindestens eine Cloud verfügen. Für alle Clouds muss das Hyper-V-Kapazitätsprofil eingerichtet worden sein. <br/><br/>Die Clouds müssen mindestens eine Virtual Machine Manager-Hostgruppe enthalten. Weitere Informationen zum Einrichten von Virtual Machine Manager-Clouds finden Sie unter [Vorbereiten für Azure Site Recovery-Bereitstellung](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric). |
| **Hyper-V** | Auf den Hyper-V-Servern muss mindestens Windows Server 2012 mit der Hyper-V-Rolle ausgeführt werden, und die neuesten Updates müssen installiert sein.<br/><br/> Ein Hyper-V-Server muss mindestens einen virtuellen Computer enthalten.<br/><br/>  Hyper-V-Hostserver müssen sich in Hostgruppen in der primären und in der sekundären VMM-Cloud befinden.<br/><br/> Wenn Sie Hyper-V in einem Cluster unter Windows Server 2012 R2 ausführen, empfiehlt es sich, das [Update 2961977](https://support.microsoft.com/kb/2961977) zu installieren.<br/><br/> Wenn Sie Hyper-V in einem Cluster unter Windows Server 2012 ausführen und über einen auf statischen IP-Adressen basierenden Cluster verfügen, wird der Clusterbroker nicht automatisch erstellt. Der Clusterbroker muss manuell konfiguriert werden. Weitere Informationen zum Clusterbroker finden Sie unter [Configure Replica Broker Role (Cluster to Cluster Replication)](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx) (Konfigurieren der Replikatbrokerrolle (Cluster-zu-Cluster-Replikation)). |
| **Anbieter** | Im Rahmen der Site Recovery-Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf Virtual Machine Manager-Servern. Der Anbieter kommuniziert mit Azure Site Recovery über HTTPS 443, um die Replikation zu orchestrieren. Die Datenreplikation erfolgt über das LAN oder eine VPN-Verbindung zwischen dem primären und sekundären Hyper-V-Server.<br/><br/> Der auf dem Virtual Machine Manager-Server ausgeführte Anbieter benötigt Zugriff auf folgende URLs:<br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Der Anbieter muss die Firewallkommunikation der Virtual Machine Manager-Server mit den [IP-Bereichen des Azure-Datencenters](https://www.microsoft.com/download/confirmation.aspx?id=41653) und das HTTPS-Protokoll (443) zulassen. |


## <a name="url-access"></a>URL-Zugriff
Diese URLs sollten von VMware-, VMM- und Hyper-V-Hostservern verfügbar sein.

|**URL** | **VMM zu VMM** | **VMM zu Azure** | **Hyper-V in Azure** | **VMware zu Azure** |
|--- | --- | --- | --- | --- |
|``*.accesscontrol.windows.net`` | Zulassen | Zulassen | Zulassen | Zulassen |
|``*.backup.windowsazure.com`` | Nicht erforderlich | Zulassen | Zulassen | Zulassen |
|``*.hypervrecoverymanager.windowsazure.com`` | Zulassen | Zulassen | Zulassen | Zulassen |
|``*.store.core.windows.net`` | Zulassen | Zulassen | Zulassen | Zulassen |
|``*.blob.core.windows.net`` | Nicht erforderlich | Zulassen | Zulassen | Zulassen |
|``https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi`` | Nicht erforderlich | Nicht erforderlich | Nicht erforderlich | Für SQL-Download zulassen |
|``time.windows.com`` | Zulassen | Zulassen | Zulassen | Zulassen|
|``time.nist.gov`` | Zulassen | Zulassen | Zulassen | ZULASSEN |

