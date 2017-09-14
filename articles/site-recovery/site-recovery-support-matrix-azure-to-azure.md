---
title: Azure Site Recovery-Supportmatrix zum Replizieren aus Azure in Azure | Microsoft-Dokumentation
description: "Übersicht über die unterstützten Betriebssysteme und Konfigurationen für die Azure Site Recovery-Replikation von virtuellen Azure-Computern (VMs) aus einer Region in eine andere für die Notfallwiederherstellung."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/31/2017
ms.author: sujayt
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: 144078bbee8e9633fac12231daa07da6c295f46e
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---
# <a name="azure-site-recovery-support-matrix-for-replicating-from-azure-to-azure"></a>Azure Site Recovery-Supportmatrix zum Replizieren aus Azure in Azure


>[!NOTE]
>
> Die Site Recovery-Replikation für virtuelle Azure-Computer ist derzeit als Vorschauversion verfügbar.

In diesem Artikel werden die unterstützten Konfigurationen und Komponenten für Azure Site Recovery bei der Replikation und Wiederherstellung von virtuellen Azure-Computern zwischen verschiedenen Regionen beschrieben.

## <a name="user-interface-options"></a>Optionen der Benutzeroberfläche

**Benutzeroberfläche** |  **Unterstützt/Nicht unterstützt**
--- | ---
**Azure-Portal** | Unterstützt
**Klassisches Portal** | Nicht unterstützt
**PowerShell** | Derzeit nicht unterstützt
**REST-API** | Derzeit nicht unterstützt
**BEFEHLSZEILENSCHNITTSTELLE (CLI)** | Derzeit nicht unterstützt


## <a name="resource-move-support"></a>Unterstützung für das Verschieben von Ressourcen

**Typ des Verschiebevorgangs für Ressourcen** | **Unterstützt/Nicht unterstützt** | **Hinweise:**  
--- | --- | ---
**Tresor über Ressourcengruppen hinweg verschieben** | Nicht unterstützt |Sie können den Recovery Services-Tresor nicht über Ressourcengruppen hinweg verschieben.
**Verschieben von Compute, Speicher und Netzwerk über Ressourcengruppen hinweg** | Nicht unterstützt |Wenn Sie nach dem Aktivieren der Replikation einen virtuellen Computer (oder zugehörige Komponenten wie Speicher und Netzwerk) verschieben, müssen Sie die Replikation deaktivieren und dann wieder für den virtuellen Computer aktivieren.



## <a name="support-for-deployment-models"></a>Unterstützung für Bereitstellungsmodelle

**Bereitstellungsmodell** | **Unterstützt/Nicht unterstützt** | **Hinweise:**  
--- | --- | ---
**Klassisch** | Unterstützt | Sie können einen replizierten klassischen virtuellen Computer nur als klassischen virtuellen Computer wiederherstellen. Eine Wiederherstellung als virtueller Ressourcen-Manager-Computer ist nicht möglich. Wenn Sie einen klassischen virtuellen Computer ohne ein virtuelles Netzwerk und direkt in einer Azure-Region bereitstellen, wird er nicht unterstützt.
**Ressourcen-Manager** | Unterstützt |

>[!NOTE]
>
> 1. Das Replizieren virtueller Azure-Computer zwischen Abonnements für Notfallwiederherstellungsszenarien wird nicht unterstützt.
> 2. Die abonnementübergreifende Migration virtueller Azure-Computer wird nicht unterstützt.
> 3. Die Migration virtueller Azure-Computer in derselben Region wird nicht unterstützt.
> 4. Die Migration virtueller Azure-Computer vom klassischen Bereitstellungsmodell zum Resource Manager-Bereitstellungsmodell wird nicht unterstützt.

## <a name="support-for-replicated-machine-os-versions"></a>Unterstützung für replizierte Computer-Betriebssystemversionen

Die unten aufgeführte Unterstützung gilt für alle Workloads unter dem genannten Betriebssystem.

#### <a name="windows"></a>Windows

- Windows Server 2016 (Server Core und Server mit Desktopdarstellung)*
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 mit mindestens SP1

>[!NOTE]
>
> \*Windows Server 2016, Nano Server wird nicht unterstützt.

#### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 6.7, 6.8, 6.9, 7.0, 7.1, 7.2, 7.3
- CentOS 6.5, 6.6, 6.7, 6.8, 6.9, 7.0, 7.1, 7.2, 7.3
- Ubuntu 14.04 LTS Server[ (unterstützte Kernel-Versionen)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Ubuntu 16.04 LTS Server[ (unterstützte Kernel-Versionen)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Oracle Enterprise Linux 6.4, 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird
- SUSE Linux Enterprise Server 11 SP3
- SUSE Linux Enterprise Server 11 SP4

(Ein Upgrade von replizierenden Computern von SLES 11 SP3 auf SLES 11 SP4 wird nicht unterstützt. Wenn für einen replizierten Computer ein Upgrade von SLES 11 SP3 auf SLES 11 SP4 durchgeführt wurde, müssen Sie die Replikation deaktivieren und den Computer nach dem Upgrade erneut schützen.)

>[!NOTE]
>
> Bei Ubuntu-Servern mit kennwortbasierter Authentifizierung und Anmeldung, auf denen das „cloud-init“-Paket zum Konfigurieren von Cloud-VMs verwendet wird, kann bei einem Failover (abhängig von der „cloud-init“-Konfiguration) die kennwortbasierte Anmeldung deaktiviert sein. Die kennwortbasierte Anmeldung kann auf der VM wieder aktiviert werden, indem das Kennwort im Azure-Portal im Menü „Einstellungen“ (im Abschnitt SUPPORT + PROBLEMBEHANDLUNG) der VM nach erfolgtem Failover zurückgesetzt wird.

### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Unterstützte Ubuntu-Kernelversionen für virtuelle Azure-Computer

**Release** | **Mobility Service-Version** | **Kernelversion** |
--- | --- | --- |
14.04 LTS | 9.9 | 3.13.0-24-generic bis 3.13.0-117-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-75-generic |
14.04 LTS | 9.10 | 3.13.0-24-generic bis 3.13.0-121-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-81-generic |
14.04 LTS | 9.11 | 3.13.0-24-generic bis 3.13.0-125-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-83-generic |
16.04 LTS | 9.10 | 4.4.0-21-generic bis 4.4.0-81-generic,<br/>4.8.0-34-generic bis 4.8.0-56-generic,<br/>4.10.0-14-generic bis 4.10.0-24-generic |
16.04 LTS | 9.11 | 4.4.0-21-generic bis 4.4.0-83-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-27-generic |

## <a name="supported-file-systems-and-guest-storage-configurations-on-azure-virtual-machines-running-linux-os"></a>Unterstützte Dateisysteme und Gastspeicherkonfigurationen auf virtuellen Azure-Computern unter Linux

* Dateisysteme: ext3, ext4, ReiserFS (nur Suse Linux Enterprise Server), XFS
* Volume-Manager: LVM2
* Multipfad-Software: Gerätemapper

## <a name="region-support"></a>Unterstützung für Regionen

Sie können virtuelle Computer zwischen zwei beliebigen Regionen im gleichen geografischen Cluster replizieren.

**Geografischer Cluster** | **Azure-Regionen**
-- | --
Amerika | Kanada, Osten; Kanada, Mitte; USA, Süden-Mitte; USA, Westen-Mitte; USA, Osten; USA, Osten 2; USA, Westen; USA, Westen 2; USA, Mitte; USA, Norden-Mitte
Europa | Vereinigtes Königreich, Westen; Vereinigtes Königreich, Süden; Europa, Norden; Europa, Westen
Asien | Indien, Süden; Indien, Mitte; Asien, Südosten; Asien, Osten; Japan, Osten; Japan, Westen; Korea, Mitte; Korea, Süden
Australien   | „Australien, Osten“, „Australien, Südosten“

>[!NOTE]
>
> Für die Region „Brasilien, Süden“ sind Replikation, Failover und Failback nur zu „USA, Süden-Mitte“, „USA, Westen-Mitte“, „USA, Osten“, „USA, Osten 2“, „USA, Westen“, „USA, Westen 2“ und „USA, Norden-Mitte“ möglich.


## <a name="support-for-compute-configuration"></a>Unterstützung für Computekonfiguration

**Konfiguration** | **Unterstützt/Nicht unterstützt.** | **Hinweise:**
--- | --- | ---
Größe | Jede Größe von virtuellen Azure-Computern mit mindestens 2 CPU-Kernen und 1 GB RAM | Weitere Informationen finden Sie unter [Größen für virtuelle Azure-Computer](../virtual-machines/windows/sizes.md).
Verfügbarkeitsgruppen | Unterstützt | Wenn Sie die Standardoption beim Schritt „Replikation aktivieren“ im Verwaltungsportal verwenden, wird die Verfügbarkeitsgruppe automatisch basierend auf der Konfiguration der Quellregion erstellt. Sie können die Zielverfügbarkeitsgruppe jederzeit unter „Repliziertes Element“ > „Einstellungen“ > „Compute und Netzwerk“ > „Verfügbarkeitsgruppe“ ändern.
Virtuelle Computer mit Hybridnutzungsvorteil (HUB) | Unterstützt | Wenn für den virtuellen Quellcomputer eine HUB-Lizenz aktiviert wurde, verwendet auch der virtuelle Computer für Testfailover oder Failover die HUB-Lizenz.
VM-Skalierungsgruppen | Nicht unterstützt |
Azure-Katalogimages – von Microsoft veröffentlicht | Unterstützt | Unterstützt, solange der virtuelle Computer unter einem von Site Recovery unterstützten Betriebssystem ausgeführt wird.
Azure-Katalogimages – von Drittanbietern veröffentlicht | Unterstützt | Unterstützt, solange der virtuelle Computer unter einem von Site Recovery unterstützten Betriebssystem ausgeführt wird.
Benutzerdefinierte Images – von Drittanbietern veröffentlicht | Unterstützt | Unterstützt, solange der virtuelle Computer unter einem von Site Recovery unterstützten Betriebssystem ausgeführt wird.
Mit Site Recovery migrierte virtuelle Computer | Unterstützt | Wenn ein VMware-/physischer Computer mit Site Recovery in Azure migriert wurde, müssen Sie die ältere Version des Mobility Service deinstallieren und den Computer neu starten, bevor er in einer anderen Azure-Region repliziert werden kann.

## <a name="support-for-storage-configuration"></a>Unterstützung für Speicherkonfiguration

**Konfiguration** | **Unterstützt/Nicht unterstützt.** | **Hinweise:**
--- | --- | ---
Maximale Datenträgergröße für das Betriebssystem | 1023 GB | Weitere Informationen finden Sie unter [Von VMs verwendete Datenträger](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Maximale Größe des Datenträgers für Daten | 1023 GB | Weitere Informationen finden Sie unter [Von VMs verwendete Datenträger](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Anzahl von Datenträgern für Daten | Bis zu 64, sofern von einer bestimmten Größe für virtuelle Azure-Computer unterstützt | Weitere Informationen finden Sie unter [Größen für virtuelle Azure-Computer](../virtual-machines/windows/sizes.md).
Temporärer Datenträger | Immer von der Replikation ausgeschlossen | Temporäre Datenträger sind immer von der Replikation ausgeschlossen. Gemäß den Azure-Richtlinien sollten Sie keine persistenten Daten auf einem temporären Datenträger speichern. Unter [Temporärer Datenträger für virtuelle Azure-Computer](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk) finden Sie weitere Informationen.
Datenänderungsrate auf dem Datenträger | Maximal 6 Mbit/s pro Datenträger | Wenn die durchschnittliche Datenänderungsrate auf dem Datenträger dauerhaft über 6 Mbit/s liegt, kann die Replikation nicht folgen. Wenn nur gelegentlich viele Daten anfallen und die Datenänderungsrate zeitweise über 6 Mbit/s liegt und dann zurückgeht, kann die Replikation folgen. In diesem Fall kann es möglicherweise zu etwas verzögerten Wiederherstellungspunkten kommen.
Datenträger in Standardspeicherkonten | Unterstützt |
Datenträger in Storage Premium-Konten | Unterstützt | Wenn ein virtueller Computer Datenträger in Premium- und Standard-Speicherkonten aufweist, können Sie für jeden Datenträger ein eigenes Zielspeicherkonto auswählen, um sicherzustellen, dass die gleiche Speicherkonfiguration in der Zielregion vorhanden ist.
Verwaltete Standard-Datenträger | Nicht unterstützt |  
Verwaltete Premium-Datenträger | Nicht unterstützt |
Speicherplätze | Unterstützt |         
Verschlüsselung ruhender Daten (SSE) | Unterstützt | Für Cache- und Zielspeicherkonten können Sie ein Speicherkonto mit aktiviertem SSE auswählen.     
Azure Disk Encryption (ADE) | Nicht unterstützt |
Datenträger laufendem Systembetrieb hinzufügen/entfernen | Nicht unterstützt | Wenn Sie Datenträger auf dem virtuellen Computer hinzufügen oder entfernen, müssen Sie die Replikation deaktivieren und dann für den virtuellen Computer wieder aktivieren.
Ausschließen von Datenträgern | Nicht unterstützt|   Temporäre Datenträger sind standardmäßig ausgeschlossen.
LRS | Unterstützt |
GRS | Unterstützt |
RA-GRS | Unterstützt |
ZRS | Nicht unterstützt |  
Kalter und heißer Speicher | Nicht unterstützt | Datenträger für virtuelle Computer werden auf kaltem und heißem Speicher nicht unterstützt.

>[!IMPORTANT]
> Halten Sie sich an die [Speichervorgaben](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) für virtuellen Azure-Quellcomputer, um Leistungsprobleme zu vermeiden. Wenn Sie die Standardeinstellungen übernehmen, erstellt Site Recovery die erforderlichen Speicherkonten auf Basis der Quellkonfiguration. Wenn Sie Einstellungen anpassen, achten Sie darauf, die Speicherempfehlungen für virtuelle-Quellcomputer einzuhalten (../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).

## <a name="support-for-network-configuration"></a>Unterstützung der Netzwerkkonfiguration
**Konfiguration** | **Unterstützt/Nicht unterstützt.** | **Hinweise:**
--- | --- | ---
Netzwerkschnittstelle (NIC) | Bis zur maximalen Anzahl von NICs, die von einer bestimmten Größe von virtuellen Azure-Computern unterstützt werden | Netzwerkkarten werden bei der Erstellung des virtuellen Computers als Teil des Testfailover- oder Failovervorgangs erstellt. Die Anzahl der Netzwerkkarten auf dem virtuellen Failovercomputer ist abhängig von der Anzahl der Netzwerkkarten, die auf dem virtuellen Quellcomputer vorhanden waren, als die Replikation aktiviert wurde. Wenn Sie Netzwerkkarten nach dem Aktivieren der Replikation hinzufügen/entfernen, hat dies keine Auswirkungen auf den virtuellen Failovercomputer.
Internetlastenausgleich | Unterstützt | Sie müssen den vorkonfigurierten Lastenausgleich mit einem Azure-Automatisierungsskript in einem Wiederherstellungsplan zuordnen.
Interner Lastenausgleich | Unterstützt | Sie müssen den vorkonfigurierten Lastenausgleich mit einem Azure-Automatisierungsskript in einem Wiederherstellungsplan zuordnen.
Öffentliche IP-Adresse| Unterstützt | Sie müssen eine bereits vorhandene öffentliche IP-Adresse der Netzwerkkarte zuordnen oder eine IP-Adresse erstellen und der Netzwerkkarte mit einem Azure Automatisierungsskript in einem Wiederherstellungsplan zuordnen.
NSG auf Netzwerkkarte (Ressourcen-Manager)| Unterstützt | Sie müssen die NSG mit einem Azure-Automatisierungsskript in einem Wiederherstellungsplan der Netzwerkkarte zuordnen.  
NSG in Subnetz (Ressourcen-Manager und klassische Bereitstellung)| Unterstützt | Sie müssen die NSG mit einem Azure-Automatisierungsskript in einem Wiederherstellungsplan der Netzwerkkarte zuordnen.
NSG auf virtuellem Computer (klassische Bereitstellung)| Unterstützt | Sie müssen die NSG mit einem Azure-Automatisierungsskript in einem Wiederherstellungsplan der Netzwerkkarte zuordnen.
Reservierte IP (statische IP-Adresse)/Quell-IP beibehalten | Unterstützt | Wenn die NIC auf dem virtuellen Quellcomputer eine statische IP-Konfiguration aufweist und im Zielsubnetz die gleiche IP-Adresse verfügbar ist, wird sie dem virtuellen Failovercomputer zugewiesen. Wenn im Zielsubnetz nicht die gleiche IP-Adresse verfügbar ist, wird eine der verfügbaren IP-Adressen im Subnetz für diesen virtuellen Computer reserviert. Sie können unter „Repliziertes Element“ > „Einstellungen“ > „Compute und Netzwerk“ > „Netzwerkschnittstellen“ eine beliebige feste IP-Adresse angeben. Sie können die Netzwerkkarte auswählen und das gewünschte Subnetz und die IP-Adresse angeben.
Dynamische IP| Unterstützt | Wenn die Netzwerkkarte auf dem virtuellen Quellcomputer über eine dynamische IP-Konfiguration verfügt, ist die Netzwerkkarte auf dem virtuellen Failovercomputer standardmäßig ebenfalls dynamisch. Sie können unter „Repliziertes Element“ > „Einstellungen“ > „Compute und Netzwerk“ > „Netzwerkschnittstellen“ eine beliebige feste IP-Adresse angeben. Sie können die Netzwerkkarte auswählen und das gewünschte Subnetz und die IP-Adresse angeben.
Traffic Manager-Integration | Unterstützt | Sie können Traffic Manager so vorkonfigurieren, dass der Datenverkehr in regelmäßigen Abständen an den Endpunkt in der Quellregion und bei einem Failover an den Endpunkt in der Zielregion weitergeleitet wird.
Mit Azure verwaltetes DNS | Unterstützt |
Benutzerdefinierter DNS  | Unterstützt |    
Nicht authentifizierter Proxy | Unterstützt | Weitere Informationen finden Sie im [Richtliniendokument für Netzwerke](site-recovery-azure-to-azure-networking-guidance.md).    
Authentifizierter Proxy | Nicht unterstützt | Wenn der virtuelle Computer einen authentifizierten Proxy für ausgehende Verbindungen verwendet, kann er nicht mit Azure Site Recovery repliziert werden.    
Standort-zu-Standort-VPN mit lokalem Netzwerk (mit oder ohne ExpressRoute)| Unterstützt | Stellen Sie sicher, dass die UDRs und NSGs so konfiguriert sind, dass der Datenverkehr für die Sitewiederherstellung nicht lokal weitergeleitet wird. Weitere Informationen finden Sie im [Richtliniendokument für Netzwerke](site-recovery-azure-to-azure-networking-guidance.md).  
VNet-zu-VNet-Verbindung | Unterstützt | Weitere Informationen finden Sie im [Richtliniendokument für Netzwerke](site-recovery-azure-to-azure-networking-guidance.md).  


## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Netzwerkrichtlinien zum Replizieren von virtuellen Azure-Computern](site-recovery-azure-to-azure-networking-guidance.md)
- Schützen von Workloads durch die [Replikation virtueller Azure-Computer](site-recovery-azure-to-azure.md)

