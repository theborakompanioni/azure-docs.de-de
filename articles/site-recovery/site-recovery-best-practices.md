---
title: Vorbereiten der Site Recovery-Bereitstellung | Microsoft Docs
description: Dieser Artikel beschreibt, wie Sie die Bereitstellung der Replikation mit Azure Site Recovery vorbereiten.
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
ms.date: 12/01/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 5ed46a6bd8931d3b8829a053ace12fd219f108c4
ms.openlocfilehash: 6430ae9a97888ef993cd3dd35eea765fe95bb43f

---

# <a name="prepare-for-azure-site-recovery-deployment"></a>Vorbereiten der Azure Site Recovery-Bereitstellung

Organisationen benötigen eine Strategie für die Geschäftskontinuität und Notfallwiederherstellung (BCDR), mit der bestimmt wird, wie Apps, Workloads und Daten bei geplanten und ungeplanten Ausfällen verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können. Site Recovery ist ein Azure-Dienst, der einen Beitrag zu Ihrer BCDR-Strategie leistet, indem die Replikation von lokalen physischen Servern und virtuellen Computern in die Cloud (Azure) oder in ein sekundäres Datencenter orchestriert wird. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Weitere Informationen finden Sie unter [Was ist Site Recovery?](site-recovery-overview.md)

In diesem Artikel werden die höchsten Anforderungen für jedes Site Recovery-Replikationsszenario zusammengefasst und Links zu ausführlichen exemplarischen Vorgehensweisen für die Bereitstellung bereitgestellt.  

Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.



## <a name="azure-deployment-models"></a>Azure-Bereitstellungsmodelle

Azure verfügt über zwei unterschiedliche [Bereitstellungsmodelle](../azure-resource-manager/resource-manager-deployment-model.md) zum Erstellen und Verwenden von Ressourcen: das Azure Resource Manager-Modell und das klassische Modell. Außerdem verfügt Azure über zwei Portale: das [klassische Azure-Portal](https://manage.windowsazure.com/) mit Unterstützung des klassischen Bereitstellungsmodells und das [Azure-Portal](https://ms.portal.azure.com/) mit Unterstützung für beide Bereitstellungsmodelle.

Neue Site Recovery-Szenarios sollten im [Azure-Portal](https://portal.azure.com) bereitgestellt werden. Dieses Portal bietet neue Features und eine verbesserte Bereitstellungserfahrung. Tresore können im klassischen Portal verwaltet, jedoch keine neuen erstellt werden.


## <a name="deployment-scenarios"></a>Bereitstellungsszenarien

Mit Site Recovery können Sie Folgendes replizieren.

| **Bereitstellung** | **Details** | **Azure-Portal** | **Klassisches Portal** | **PowerShell-Bereitstellung** |
| --- | --- | --- | --- | --- |
| [VMware zu Azure](site-recovery-vmware-to-azure.md) | Replizieren von lokalen VMware-VMs zu Azure Storage | Verwenden Sie Resource Manager oder die klassische Speicherung und die klassischen Netzwerke. Führen Sie ein Failover zu Resource Manager-basierten oder klassischen VMs aus. | Nur im Wartungsmodus. Neue Tresore können nicht erstellt werden. | Derzeit nicht unterstützt. |
| [Physische Server nach Azure](site-recovery-vmware-to-vmware.md) | Replizieren physischer Windows-/Linux-Server nach Azure-Speicher | Verwenden Sie Resource Manager oder die klassische Speicherung und die klassischen Netzwerke. Führen Sie ein Failover zu Resource Manager-basierten oder klassischen VMs aus. | Nur im Wartungsmodus. Neue Tresore können nicht erstellt werden.  |
| [Hyper-V-VMs in VMM-Clouds zu Azure](site-recovery-vmm-to-azure.md) | Replizieren von lokalen Hyper-V-VMs in VMM-Clouds zu Azure Storage<br/><br/> | Verwenden Sie Resource Manager oder die klassische Speicherung und die klassischen Netzwerke. Führen Sie ein Failover zu Resource Manager-basierten oder klassischen VMs aus.   | Nur im Wartungsmodus. Neue Tresore können nicht erstellt werden. | Unterstützt |
| [Hyper-V-VMs in Azure (ohne VMM)](site-recovery-hyper-v-site-to-azure.md) | Replizieren von lokalen Hyper-V-VMs zu Azure Storage | Verwenden Sie Resource Manager oder die klassische Speicherung und die klassischen Netzwerke. Führen Sie ein Failover zu Resource Manager-basierten oder klassischen VMs aus. | Nur im Wartungsmodus. Neue Tresore können nicht erstellt werden. | Unterstützt |
| VMware-VMs/physische Server zum sekundären Standort (site-recovery-vmware-to-vmware.md) | Replizieren von VMware-VMs oder physischen Windows-/Linux-Servern an einem sekundären Standort.<br/><br/> [Laden Sie das InMage Scout-Benutzerhandbuch herunter](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf). | Nicht im Azure-Portal verfügbar. | Laden Sie InMage Scout von einem Site Recovery-Tresor herunter. | Nicht unterstützt |
| [Hyper-V-VMs an einem sekundären Standort](site-recovery-vmm-to-vmm.md) | Replizieren von Hyper-V-VMs in VMM-Clouds in einer sekundären Cloud  | Die Replikation verwendet Standard-Hyper-V-Replikate. SAN wird nicht unterstützt. | Replizieren mithilfe von Hyper-V-Replikaten oder der [SAN-Replikation](site-recovery-vmm-san.md) | Unterstützt |

## <a name="requirements-for-replication-to-azure"></a>Anforderungen für die Replikation zu Azure

| **Anforderung** | **Details** |
| --- | --- |
| **Azure-Konto** | Ein [Microsoft Azure](http://azure.microsoft.com/) -Konto.<br/><br/> Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung. [hier](https://azure.microsoft.com/pricing/details/site-recovery/) . |
| **Azure-Speicher** | Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden erstellt, wenn ein Failover durchgeführt wird. Sie benötigen ein [Azure Storage-Konto](../storage/storage-introduction.md).<br/><br/>Verwenden Sie den [GRS](../storage/storage-redundancy.md#geo-redundant-storage)- oder LRS-Speicher im Azure-Portal. GRS wird nur im klassischen Portal unterstützt.<br/><br/> Wenn Sie VMware-VMs oder physische Server im Azure-Portal replizieren, wird Storage Premium unterstützt.<br/><br/>  |
| **Azure-Netzwerk** | Sie benötigen ein Azure-Netzwerk, mit dem Azure-VMs eine Verbindung herstellen können. <br/><br/> Sie können ein klassisches oder ein Resource Manager-Netzwerk im Azure-Portal verwenden.<br/><br/> Das Netzwerk muss sich in der gleichen Region wie der Tresor befinden.<br/><br/> Wenn Sie von VMM zu Azure replizieren, richten Sie eine [Netzwerkzuordnung](site-recovery-network-mapping.md) zwischen VMM-VM-Netzwerken und Azure-Netzwerken ein, um sicherzustellen, dass sich Azure-VMs nach dem Failover mit entsprechenden Netzwerken verknüpfen. |
| **Lokal** |**VMware-VMs**: Ein lokaler Computer, auf dem Site Recovery-Komponenten ausgeführt werden. Darüber hinaus benötigen Sie VMware vSphere-Hosts/vCenter-Server und VMs, die Sie replizieren möchten. [detaillierte Kapazitätsplanung](site-recovery-vmware-to-azure.md#configuration-server-or-additional-process-server-prerequisites)<br/><br/> **Physische Server**: Ein lokaler Computer, auf dem Site Recovery-Komponenten ausgeführt werden, und physische Server, die Sie replizieren möchten. [detaillierte Kapazitätsplanung](site-recovery-vmware-to-azure.md#configuration-server-or-additional-process-server-prerequisites) Wenn Sie ein [Failback](site-recovery-failback-azure-to-vmware.md) für physische Server zu Azure ausführen möchten, benötigen Sie eine VMware-Infrastruktur.<br/><br/> **Hyper-V-VMs**: Ein VMM-Server und Hyper-V-Hosts, die VMs enthalten, die Sie replizieren möchten. [detaillierte Kapazitätsplanung](site-recovery-vmm-to-azure.md#on-premises-prerequisites)<br/><br/> Sie benötigen nur Hyper-V-Hosts zum Replizieren von Hyper-V-VMs ohne VMM. [detaillierte Kapazitätsplanung](site-recovery-hyper-v-site-to-azure.md#on-premises-prerequisites) |
| **Geschützte Computer** | Geschützte Computer, die in Azure repliziert werden sollen, müssen die unten beschriebenen [Voraussetzungen für Azure](#azure-virtual-machine-requirements) einhalten. |

## <a name="requirements-for-replication-to-a-secondary-site"></a>Anforderungen für die Replikation zu einem sekundären Standort

| **Anforderung** | **Details** |
| --- | --- |
| **Azure** | Ein [Microsoft Azure](http://azure.microsoft.com/) -Konto.<br/><br/> Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung. [Preise für Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) . |
| **Lokal** |**VMware-VMs**: Sie benötigen einen Prozessserver am primären Standort, mit dem Replikationsdaten zwischengespeichert, komprimiert und verschlüsselt werden. Sie benötigen einen Konfigurationsserver am sekundären Standort zum Verwalten und Überwachen der Bereitstellung sowie einen Masterzielserver. Sie sollten außerdem einen vContinuum-Server verwenden, um die Verwaltung zu vereinfachen. Darüber hinaus benötigen Sie einen oder mehrere VMware vSphere-Hosts und optional einen vCenter-Server. [Weitere Informationen](site-recovery-vmware-to-vmware.md)<br/><br/> **Hyper-V-VMs in VMM-Clouds**: VMM-Server und Hyper-V-Hosts, die VMs enthalten, die Sie replizieren möchten [detaillierte Kapazitätsplanung](site-recovery-vmm-to-vmm.md#on-premises-prerequisites) |
| **Netzwerk (VMM zu VMM)** | Wenn Sie von VMM zu VMM replizieren, richten Sie eine [Netzwerkzuordnung](site-recovery-network-mapping.md) ein, um sicherzustellen, dass Replikat-VMs nach einem Failover mit den entsprechenden Netzwerken verknüpft und optimal auf Hyper-V-Hosts platziert werden. |

## <a name="url-access"></a>URL-Zugriff

Diese URLs sollten von VMware-, VMM- und Hyper-V-Hostservern verfügbar sein.

**URL** | **VMM zu VMM** | **VMM zu Azure** | **Hyper-V in Azure** | **VMware zu Azure** |
|--- | --- | --- | --- | ---
``*.accesscontrol.windows.net`` | Zulassen | Zulassen | Zulassen | Zulassen
``*.backup.windowsazure.com`` | Nicht erforderlich | Zulassen | Zulassen | Zulassen
``*.hypervrecoverymanager.windowsazure.com`` | Zulassen | Zulassen | Zulassen | Zulassen
``*.store.core.windows.net`` | Zulassen | Zulassen | Zulassen | Zulassen
``*.blob.core.windows.net`` | Nicht erforderlich | Zulassen | Zulassen | Zulassen
``https://www.msftncsi.com/ncsi.txt`` | Zulassen | Zulassen | Zulassen | Zulassen
``https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi`` | Nicht erforderlich | Nicht erforderlich | Nicht erforderlich | Für SQL-Download zulassen
``time.windows.com`` | Zulassen | Zulassen | Zulassen | Zulassen
``time.nist.gov`` | Zulassen | Zulassen | Zulassen | Zulassen



## <a name="azure-virtual-machine-requirements"></a>Anforderungen für virtuelle Azure-Maschinen

Sie können Site Recovery zum Replizieren virtueller Computer und physischer Server bereitstellen, auf denen ein von Azure unterstütztes Betriebssystem ausgeführt wird. Dies gilt für die meisten Versionen von Windows und Linux. Sie müssen sicherstellen, dass lokale virtuelle Computer, die Sie replizieren möchten, den Azure-Anforderungen entsprechen.

**Feature** | **Anforderungen** | **Details**
--- | --- | ---
**Hyper-V-Host** | Sollte Windows Server 2012 R2 oder höher ausführen | Bei der Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn das Betriebssystem nicht unterstützt wird.
**VMware-Hypervisor** | Unterstütztes Betriebssystem | [Überprüfen der Anforderungen](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
**Gastbetriebssystem** | Replikation von Hyper-V in Azure: Site Recovery unterstützt alle Betriebssysteme, die [von Azure unterstützt werden](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> Replikation von VMware und physischen Servern: Lesen Sie die [Voraussetzungen](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Architektur des Gastbetriebssystems** | 64 Bit | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Größe des Betriebssystemdatenträgers** | Bis zu 1.023 GB | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Anzahl von Betriebssystemdatenträgern** | 1 | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Anzahl von Datenträgern für Daten** | Maximal 16 (der Höchstwert ist eine Funktion der Größe des erstellten virtuellen Computers: 16 = XL) | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Größe des VHD-Datenträgers** | Bis zu 1.023 GB | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Netzwerkadapter** | Es werden mehrere Adapter unterstützt. |
**Statische IP-Adresse** | Unterstützt | Wenn für den primären virtuellen Computer eine statische IP-Adresse verwendet wird, können Sie die statische IP-Adresse für den virtuellen Computer angeben, der in Azure erstellt wird.<br/><br/> Statische IP-Adressen werden für eine **Linux-VM unter Hyper-V** nicht unterstützt.
**iSCSI-Datenträger** | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Freigegebene VHD** | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**FC-Datenträger** | Nicht unterstützt | Für die Überprüfung der Voraussetzungen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
**Festplattenformat** | VHD  <br/><br/> VHDX | Obwohl VHDX in Azure derzeit nicht unterstützt wird, führt Site Recovery automatisch die Konvertierung von VHDX in VHD durch, wenn Sie das Failover auf Azure anstoßen. Wenn Sie das Failover in den lokalen Speicher durchführen, wird für die virtuellen Computer weiterhin das VHDX-Format verwendet.
**BitLocker** | Nicht unterstützt | BitLocker muss deaktiviert werden, bevor Sie einen virtuellen Computer schützen.
**VM-Name** | Zwischen 1 und 63 Zeichen. Ist auf Buchstaben, Zahlen und Bindestriche beschränkt. Muss mit einem Buchstaben oder einer Zahl beginnen und enden. | Aktualisieren Sie den Wert in den Eigenschaften für virtuelle Computer in Site Recovery.
**VM-Typ** | Generation 1<br/><br/> Generation 2 – Windows | Es werden zwei virtuelle Computer von Generation 2 mit einem Betriebssystemdatenträger des Typs „Basic“ unterstützt, der einen oder zwei Datenträger enthält, die als VHDX formatiert sind und weniger als 300 GB brauchen.<br/><br/>verfügbar. Linux Generation 2-VMs werden nicht unterstützt. [Weitere Informationen](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/) |

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
* [Replizieren von Hyper-V-VMs mit einem einzelnen VMM-Server](site-recovery-single-vmm.md)



<!--HONumber=Dec16_HO1-->


