---
title: "Voraussetzungen für die Replikation in Azure mithilfe von Azure Site Recovery | Microsoft Docs"
description: "Lernen Sie die Voraussetzungen für die Replikation von virtuellen und physischen Computern in Azure mithilfe des Azure Site Recovery-Diensts kennen."
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
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: ccc2b53e0824042c0f07b9fe63e8777aa68c6dc1
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---

#  <a name="prerequisites-for-replication-from-on-premises-to-azure-by-using-site-recovery"></a>Voraussetzungen für die Replikation von lokalen Standorten nach Azure mithilfe von Site Recovery

> [!div class="op_single_selector"]
> * [Replikation von Azure nach Azure](site-recovery-azure-to-azure-prereq.md)
> * [Replikation von lokal nach Azure](site-recovery-prereq.md)

Azure Site Recovery unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung durch Orchestrierung der Replikation von Azure-VMs in eine andere Azure-Region. Site Recovery repliziert auch lokale physische Server und VMs in die Cloud (Azure) oder in ein sekundäres Rechenzentrum. Wenn es an Ihrem primären Standort zu einem Ausfall kommt, können Sie ein Failover zu einem sekundären Standort durchführen, um die Verfügbarkeit von Apps und Workloads zu erhalten. Sobald wieder Normalbetrieb herrscht, können Sie das Failback zum primären Standort durchführen. Weitere Informationen zu Site Recovery finden Sie unter [Was ist Site Recovery?](site-recovery-overview.md).

In diesem Artikel fassen wir die Voraussetzungen für das Beginnen der Replikation eines lokalen Computers in Azure Site Recovery zusammen.

Am Ende dieses Artikels können Sie einen Kommentar eingeben. Im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) können Sie auch technische Fragen stellen.

## <a name="azure-requirements"></a>Anforderungen für Azure

**Anforderung** | **Details**
--- | ---
**Azure-Konto** | Ein [Microsoft Azure](http://azure.microsoft.com/)-Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung.
**Site Recovery-Dienst** | Weitere Informationen zu den Preisen von Azure Site Recovery finden Sie auf der Seite [Site Recovery: Preise](https://azure.microsoft.com/pricing/details/site-recovery/). |
**Azure Storage (in englischer Sprache)** | Sie benötigen ein Azure Storage-Konto, um replizierte Daten zu speichern. Das Speicherkonto muss sich in derselben Region wie der Azure Recovery Services-Tresor befinden. Azure-VMs werden erstellt, sobald ein Failover erfolgt.<br/><br/> Richten Sie abhängig davon, welches Ressourcenmodell Sie für das Failover von Azure-VMs verwenden möchten, mithilfe des [Azure Resource Manager-Bereitstellungsmodells](../storage/common/storage-create-storage-account.md) oder [klassischen Bereitstellungsmodells](../storage/common/storage-create-storage-account.md) ein Konto ein.<br/><br/>Sie können [georedundanten Speicher](../storage/common/storage-redundancy.md#geo-redundant-storage) oder lokal redundanten Speicher verwenden. Wir empfehlen georedundanten Speicher. Dank georedundantem Speicher sind Ihre Daten geschützt, wenn es zu einem regionalen Ausfall kommt oder die primäre Region nicht wiederhergestellt werden kann.<br/><br/> Sie können ein standardmäßiges Azure-Speicherkonto oder Azure Storage Premium verwenden. [Storage Premium](https://docs.microsoft.com/azure/storage/storage-premium-storage) wird in der Regel für VMs gewählt, die eine konstant hohe E/A-Leistung und kurze Wartezeit benötigen. Mit Storage Premium kann eine VM E/A-intensive Workloads hosten. Bei Verwendung von Storage Premium für replizierte Daten benötigen Sie auch ein Standard-Speicherkonto. Ein Standard-Speicherkonto dient zum Speichern von Replikationsprotokollen, in denen laufende Änderungen an lokalen Daten erfasst werden.<br/><br/>
**Speichereinschränkungen** | Speicherkonten, die Sie in Site Recovery verwenden, können Sie nicht in eine andere Ressourcengruppe oder ein anderes Abonnement verschieben, um sie in diesem zu nutzen.<br/><br/> Die Replikation von Storage Premium-Konten in den Regionen „Indien, Mitte“ und „Indien, Süden“ wird derzeit nicht unterstützt.
**Azure-Netzwerk** | Sie benötigen ein Azure-Netzwerk, mit dem Azure-VMs nach dem Failover eine Verbindung herstellen. Das Azure-Netzwerk muss sich in derselben Region wie der Recovery Services-Tresor befinden.<br/><br/> Sie können im Azure-Portal mithilfe des [Ressourcen-Manager-Bereitstellungsmodells](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) oder [klassischen Bereitstellungsmodells](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ein Azure-Netzwerk erstellen.<br/><br/> Wenn Sie von System Center Virtual Machine Manager (VMM) zu Azure replizieren, müssen Sie die Netzwerkzuordnung zwischen VMM-VM-Netzwerken und Azure-Netzwerken einrichten. Dadurch wird sichergestellt, dass sich Azure-VMs nach dem Failover mit entsprechenden Netzwerken verbinden.
**Netzwerkeinschränkungen** | Netzwerkkonten, die Sie in Site Recovery verwenden, können Sie nicht in eine andere Ressourcengruppe oder ein anderes Abonnement verschieben, um sie in diesem zu nutzen.
**Netzwerkzuordnung** | Wenn Sie Microsoft Hyper-V-VMs in VMM-Clouds replizieren, müssen Sie die Netzwerkzuordnung einrichten. Dadurch wird sichergestellt, dass sich nach dem Failover erstellte Azure-VMs mit entsprechenden Netzwerken verbinden.

> [!NOTE]
> In den folgenden Abschnitten werden die Voraussetzungen für verschiedene Komponenten in der Kundenumgebung beschrieben. Weitere Informationen zur Unterstützung bestimmter Konfigurationen finden Sie in der [Unterstützungsmatrix](site-recovery-support-matrix.md).
>

## <a name="disaster-recovery-of-vmware-vms-or-physical-windows-or-linux-servers-to-azure"></a>Notfallwiederherstellung von VMware-VMs oder physischer Windows- und Linux-Server in Azure
Die folgenden Komponenten sind für die Notfallwiederherstellung von VMware-VMs oder physischen Windows- oder Linux-Servern erforderlich. Diese gelten zusätzlich zu denen, die unter [Azure-Anforderungen](#azure-requirements) beschrieben sind.


### <a name="configuration-server-or-additional-process-server"></a>Konfigurationsserver oder zusätzlicher Prozessserver

Richten Sie einen lokalen Computer als Konfigurationsserver zum Orchestrieren der Kommunikation zwischen dem lokalen Standort und Azure ein. Der lokale Computer verwaltet auch die Datenreplikation. <br/></br>

*   **Voraussetzungen für den VMware vCenter- oder vSphere-Host**

    | **Komponente** | **Anforderungen** |
    | --- | --- |
    | **vSphere** | Sie benötigen mindestens einen VMware vSphere-Hypervisor.<br/><br/>Auf den Hypervisoren muss die vSphere-Version 6.0, 5.5 oder 5.1 mit den neuesten Updates ausgeführt werden.<br/><br/>Ihre vSphere-Hosts und vCenter-Server sollten sich im gleichen Netzwerk befinden wie der Prozessserver. Dabei handelt es sich um das Netzwerk, in dem sich der Konfigurationsserver befindet, es sei denn, Sie haben einen dedizierten Prozessserver eingerichtet. |
    | **vCenter** | Es wird empfohlen, dass Sie einen VMware vCenter-Server zum Verwalten Ihrer vSphere-Hosts bereitstellen. Auf diesem Server muss vCenter 6.0 oder 5.5 mit den neuesten Updates ausgeführt werden.<br/><br/>**Einschränkung**: Die Replikation zwischen Instanzen von vCenter vMotion wird von Site Recovery nicht unterstützt. Storage DRS und Storage vMotion werden auch nach einem erneuten Schutzvorgang nicht auf einer Masterziel-VM unterstützt.||

* **Voraussetzungen für replizierte Computer**

    | **Komponente** | **Anforderungen** |
    | --- | --- |
    | **Lokale Computer** (VMware-VMs) | Auf replizierten VMs müssen VMware-Tools installiert sein und ausgeführt werden.<br/><br/> VMs müssen die [Azure-Voraussetzungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) zum Erstellen einer Azure-VM erfüllen.<br/><br/>Die Datenträgerkapazität auf jedem geschützten Computer darf nicht höher als 1.023 GB sein. <br/><br/>Für die Komponenteninstallation sind mindestens 2 GB verfügbarer Speicherplatz auf dem Installationslaufwerk erforderlich.<br/><br/>Wenn Sie Multi-VM-Konsistenz aktivieren möchten, muss Port 20004 in der lokalen Firewall der VM geöffnet sein.<br/><br/>Computernamen dürfen 1 bis 63 Zeichen enthalten (Buchstaben, Zahlen und Bindestriche sind zulässig). Der Name muss mit einem Buchstaben oder einer Zahl beginnen und mit einem Buchstaben oder einer Zahl enden. <br/><br/>Sie können den Azure-Namen ändern, nachdem Sie die Replikation für einen Computer aktiviert haben.<br/><br/> |
    | **Windows-Computer** (physisch oder VMware) | Auf dem Computer muss eines der folgenden unterstützten 64-Bit-Betriebssysteme ausgeführt werden: <br/>Windows Server 2012 R2<br/>Windows Server 2012<br/>Windows Server 2008 R2 mit SP1 oder höher<br/><br/> Das Betriebssystem muss auf Laufwerk „C“ installiert sein. Bei dem Betriebssystem-Datenträger muss es sich um einen nicht dynamischen Windows-Basisdatenträger handeln. Der normale Datenträger kann dynamisch sein.<br/><br/>|
    | **Linux-Computer** (physisch oder VMware) | Auf dem Computer muss eines der folgenden unterstützten 64-Bit-Betriebssysteme ausgeführt werden: <br/>- Red Hat Enterprise Linux: 5.2 nach 5.11, 6.1 nach 6.9, 7.0 nach 7.3<br/>- CentOS: 5.2 nach 5.11, 6.1 nach 6.9, 7.0 nach 7.3<br/>Ubuntu 14.04 LTS Server (eine Liste der unterstützten Kernelversionen für Ubuntu finden Sie unter [Unterstützte Betriebssysteme](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions))<br/>- Ubuntu 16.04 LTS Server (eine Liste der unterstützten Kernelversionen für Ubuntu finden Sie unter [Unterstützte Betriebssysteme](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions))<br/>-  Debian 7 oder Debian 8<br/>Oracle Enterprise Linux 6.5 oder 6.4 mit ausgeführtem Red Hat-kompatiblem Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3)<br/>SUSE Linux Enterprise Server 11 SP4 oder SUSE Linux Enterprise Server 11 SP3<br/><br/>Ihre Dateien des Typs „/etc/hosts“ auf geschützten Computern müssen Einträge enthalten, die den Namen des lokalen Hosts den IP-Adressen aller Netzwerkkarten zuordnen.<br/><br/>Wenn Sie nach einem Failover mit einem SSH-Client (Secure Shell) eine Verbindung mit einer Azure-VM herstellen möchten, auf der Linux ausgeführt wird, vergewissern Sie sich, dass der SSH-Dienst auf dem geschützten Computer auf automatischen Start beim Systemstart festgelegt ist. Stellen Sie außerdem sicher, dass Firewallregeln eine SSH-Verbindung mit dem geschützten Computer zulassen.<br/><br/>Hostname, Bereitstellungspunkte, Gerätenamen und Linux-Systempfade und -Dateinamen (beispielsweise „/etc/“ oder „/usr“) dürfen nur auf Englisch angegeben werden.<br/><br/>Die folgenden Verzeichnisse (sofern als separate Partitionen oder Dateisysteme eingerichtet) müssen sich auf demselben Datenträger (dem Datenträger mit dem Betriebssystem) auf dem Quellserver befinden:<br/>- / (root)<br/>- /boot<br/>- /usr<br/>- /usr/local<br/>- /var<br/>- /etc<br/><br/>Derzeit werden XFS v5-Features, z.B. Metadatenprüfsumme, von Site Recovery in XFS-Dateisystemen nicht unterstützt. Stellen Sie sicher, dass Ihre XFS-Dateisysteme keine v5-Features verwenden. Mit dem Hilfsprogramm „xfs_info“ können Sie den XFS-Superblock für die Partition überprüfen. Wenn **ftype** auf **1** festgelegt ist, werden Features von XFS v5 verwendet.<br/><br/>Auf Servern mit Red Hat Enterprise Linux 7 und CentOS 7 muss das Hilfsprogramm „lsof“ installiert und verfügbar sein.<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-to-azure-no-vmm"></a>Notfallwiederherstellung von Hyper-V-VMs in Azure (ohne VMM)

Die folgenden Komponenten sind für die Notfallwiederherstellung von Hyper-V-VMs in VMM-Clouds erforderlich. Diese gelten zusätzlich zu denen, die unter [Azure-Anforderungen](#azure-requirements) beschrieben sind.

| **Voraussetzung** | **Details** |
| --- | --- |
| **Hyper-V-Host** |Auf mindestens einem lokaler Server muss Windows Server 2012 R2 (mit den neuesten Updates und aktivierter Hyper-V-Rolle) oder Microsoft Hyper-V Server 2012 R2 ausgeführt werden.<br/><br/>Hyper-V-Server müssen über eine oder mehrere VMs verfügen.<br/><br/>Hyper-V-Server müssen entweder direkt oder über einen Proxy mit dem Internet verbunden sein.<br/><br/>Auf Hyper-V-Servern müssen die Korrekturen installiert sein, die im Knowledge Base-Artikel [2961977](https://support.microsoft.com/kb/2961977) beschrieben sind.
|**Anbieter und Agent**| Während der Bereitstellung von Site Recovery installieren Sie den Azure Site Recovery-Anbieter. Bei der Anbieterinstallation wird auch der Azure Recovery Services-Agent auf allen Hyper-V-Servern installiert, auf denen VMs ausgeführt werden, die Sie schützen möchten. <br/><br/>Alle Hyper-V-Server in einem Site Recovery-Tresor müssen über die gleichen Anbieter- und Agent-Versionen verfügen.<br/><br/>Der Anbieter muss über das Internet eine Verbindung mit Site Recovery herstellen. Datenverkehr kann direkt oder über einen Proxy gesendet werden. HTTPS-basierte Proxys werden nicht unterstützt. Der Proxyserver muss Zugriff auf die folgenden URLs zulassen:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/>Wenn Sie auf dem Server über Firewallregeln verfügen, die auf IP-Adressen basieren, stellen Sie sicher, dass die Regeln die Kommunikation mit Azure zulassen.<br/><br/> Lassen Sie die [IP-Bereiche für Azure-Rechenzentrum](https://www.microsoft.com/download/confirmation.aspx?id=41653) und den HTTPS-Port 443 zu.<br/><br/> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für den Westen der USA (für die Zugriffsteuerung und Identitätsverwaltung) zu.


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-azure"></a>Notfallwiederherstellung von Hyper-V-VMs in VMM-Clouds in Azure

Die folgenden Komponenten sind für die Notfallwiederherstellung von Hyper-V-VMs in VMM-Clouds erforderlich. Diese gelten zusätzlich zu denen, die unter [Azure-Anforderungen](#azure-requirements) beschrieben sind.

| **Voraussetzung** | **Details** |
| --- | --- |
| **Virtual Machine Manager** |Sie benötigen mindestens einen VMM-Server, der unter System Center 2012 R2 oder höher ausgeführt wird. Für jeden VMM-Server muss mindestens eine Cloud konfiguriert sein. <br/><br/>Eine Cloud benötigt Folgendes:<br/>Eine oder mehrere VMM-Hostgruppen.<br/>Mindestens einen Hyper-V-Hostserver oder Cluster in jeder Hostgruppe<br/><br/>Weitere Informationen zum Einrichten von VMM-Clouds finden Sie unter [How to Create a Cloud in VMM 2012](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx) (Erstellen einer Cloud in VMM 2012). |
| **Hyper-V** |Auf Hyper-V-Hostservern muss mindestens Windows Server 2012 R2 mit der Hyper-V-Rolle oder Microsoft Hyper-V Server 2012 R2 ausgeführt werden. Die neuesten Updates müssen installiert sein.<br/><br/> Hyper-V-Server müssen über eine oder mehrere VMs verfügen.<br/><br/> Ein Hyper-V-Hostserver oder -Cluster, der zu replizierende VMs enthält, muss in einer VMM-Cloud verwaltet werden.<br/><br/>Hyper-V-Server müssen entweder direkt oder über einen Proxy mit dem Internet verbunden sein.<br/><br/>Auf Hyper-V-Servern müssen die Korrekturen installiert sein, die im Knowledge Base-Artikel [2961977](https://support.microsoft.com/kb/2961977) beschrieben sind.<br/><br/>Hyper-V-Hostserver benötigen Internetzugriff für die Datenreplikation in Azure. |
| **Anbieter und Agent** |Während der Azure Site Recovery-Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf dem VMM-Server. Installieren Sie den Azure Recovery Services-Agent auf Hyper-V-Hosts. Anbieter und Agent müssen direkt, über das Internet oder mittels eines Proxys eine Verbindung mit Azure herstellen. Ein HTTPS-basierter Proxy wird nicht unterstützt. Der Proxyserver auf dem VMM-Server und auf Hyper-V-Hosts muss Zugriff auf folgende URLs zulassen: <br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Wenn Sie auf dem VMM-Server über Firewallregeln verfügen, die auf IP-Adressen basieren, stellen Sie sicher, dass die Regeln die Kommunikation mit Azure zulassen.<br/><br/> Lassen Sie die [IP-Bereiche für Azure-Rechenzentrum](https://www.microsoft.com/download/confirmation.aspx?id=41653) und den HTTPS-Port 443 zu.<br/><br/>Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für den Westen der USA (für die Zugriffsteuerung und Identitätsverwaltung) zu.<br/><br/> |

### <a name="replicated-machine-prerequisites"></a>Voraussetzungen für replizierte Computer

| **Komponente** | **Details** |
| --- | --- |
| **Geschützte VMs** | Site Recovery unterstützt alle Betriebssysteme, die von [Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx) unterstützt werden.<br/><br/>VMs müssen die [Azure-Voraussetzungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) zum Erstellen einer Azure-VM erfüllen. Computernamen dürfen 1 bis 63 Zeichen enthalten (Buchstaben, Zahlen und Bindestriche sind zulässig). Der Name muss mit einem Buchstaben oder einer Zahl beginnen und mit einem Buchstaben oder einer Zahl enden. <br/><br/>Sie können den VM-Namen ändern, nachdem Sie die Replikation für die VM aktiviert haben. <br/><br/> Die Datenträgerkapazität auf jedem geschützten Computer darf nicht höher als 1.023 GB sein. Ein virtueller Computer kann bis zu 16 Datenträger haben (also bis zu 16 TB).<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-a-customer-owned-site"></a>Notfallwiederherstellung von Hyper-V-VMs in VMM-Clouds an einem Kundenstandort

Die folgenden Komponenten sind für die Notfallwiederherstellung von Hyper-V-VMs in VMM-Clouds an einem Kundenstandort erforderlich. Diese gelten zusätzlich zu denen, die unter [Azure-Anforderungen](#azure-requirements) beschrieben sind.

| **Komponente** | **Details** |
| --- | --- |
| **Virtual Machine Manager** |  Wir empfehlen Ihnen, am primären und am sekundären Standort je einen VMM-Server bereitzustellen.<br/><br/> Sie können [auf einem einzelnen VMM-Server cloudübergreifend replizieren](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment). Zur Replikation zwischen Clouds auf einem einzelnen VMM-Server müssen mindestens zwei Clouds auf dem VMM-Server konfiguriert sein.<br/><br/> Auf den VMM-Servern muss mindestens System Center 2012 SP1 mit den neuesten Updates ausgeführt werden.<br/><br/> Jeder VMM-Server muss über mindestens eine Cloud verfügen. Für alle Clouds muss das Hyper-V-Kapazitätsprofil eingerichtet worden sein. <br/><br/>Die Clouds müssen mindestens eine VMM-Hostgruppe enthalten. Weitere Informationen zum Einrichten von VMM-Clouds finden Sie unter [Vorbereiten für Azure Site Recovery-Bereitstellung](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric). |
| **Hyper-V** | Auf den Hyper-V-Servern muss mindestens Windows Server 2012 mit aktivierter Hyper-V-Rolle ausgeführt werden, und die neuesten Updates müssen installiert sein.<br/><br/> Hyper-V-Server müssen über eine oder mehrere VMs verfügen.<br/><br/>  Hyper-V-Hostserver müssen sich in Hostgruppen in der primären und in der sekundären VMM-Cloud befinden.<br/><br/> Wenn Sie Hyper-V in einem Cluster unter Windows Server 2012 R2 ausführen, wird empfohlen, dass Sie das im Knowledge Base-Artikel [2961977](https://support.microsoft.com/kb/2961977) beschriebene Update installieren.<br/><br/> Wenn Sie Hyper-V in einem Cluster unter Windows Server 2012 ausführen und über einen auf statischen IP-Adressen basierenden Cluster verfügen, wird der Clusterbroker nicht automatisch erstellt. Sie müssen den Clusterbroker manuell konfigurieren. Weitere Informationen zum Clusterbroker finden Sie unter [Configure Replica Broker Role (Cluster to Cluster Replication)](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx) (Konfigurieren der Replikatbrokerrolle [Cluster-zu-Cluster-Replikation]). |
| **Anbieter** | Während der Site Recovery-Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf VMM-Servern. Der Anbieter kommuniziert mit Azure Site Recovery über HTTPS (Port 443), um die Replikation zu orchestrieren. Die Datenreplikation erfolgt über das LAN oder eine VPN-Verbindung zwischen dem primären und sekundären Hyper-V-Server.<br/><br/> Der auf dem VMM-Server ausgeführte Anbieter benötigt Zugriff auf die folgenden URLs:<br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Der Site Recovery-Anbieter muss die Firewallkommunikation der VMM-Server mit den [IP-Bereichen für Azure-Rechenzentren](https://www.microsoft.com/download/confirmation.aspx?id=41653) und das HTTPS-Protokoll (Port 443) zulassen. |


## <a name="url-access"></a>URL-Zugriff
Diese URLs müssen auf VMware-, VMM- und Hyper-V-Hostservern verfügbar sein.

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

