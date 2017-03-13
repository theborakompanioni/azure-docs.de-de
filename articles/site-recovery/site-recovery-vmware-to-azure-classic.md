---
title: Replizieren virtueller VMware-Computer und physischer Server im klassischen Azure-Portal | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Azure Site Recovery bereitgestellt wird, um die Replikation, das Failover und die Wiederherstellung von lokalen virtuellen VMware-Computern und physischen Windows-/Linux-Servern in Azure aufeinander abzustimmen.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: a9022c1f-43c1-4d38-841f-52540025fb46
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 37291b16a966c17b684a4622f15d0393a5a5ce6d
ms.openlocfilehash: 0984ccc8163c94f56517e65919f8ea089a3e8b5f
ms.lasthandoff: 02/27/2017


---
# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery"></a>Replizieren von virtuellen VMware-Computern und physischen Servern in Azure mithilfe von Azure Site Recovery
> [!div class="op_single_selector"]
> * [Azure-Portal](site-recovery-vmware-to-azure.md)
> * [Klassisches Portal](site-recovery-vmware-to-azure-classic.md)
> * [Klassisches Portal (Vorgängerversion)](site-recovery-vmware-to-azure-classic-legacy.md)
>
>

Der Dienst Azure Site Recovery unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung, indem Replikation, Failover und Wiederherstellung virtueller Computer und physischer Server aufeinander abgestimmt werden. Computer können in Azure oder in einem sekundären lokalen Rechenzentrum repliziert werden. Eine kurze Übersicht über das Gesamtthema finden Sie unter [Was ist Azure Site Recovery?](site-recovery-overview.md).

## <a name="overview"></a>Übersicht
Dieser Artikel beschreibt die Vorgehensweise zum

* **Replizieren virtueller VMware-Computer in Azure**: Sie stellen Site Recovery bereit, um die Replikation, das Failover und die Wiederherstellung von lokalen virtuellen VMware-Computern in Azure Storage zu koordinieren.
* **Replizieren physischer Server in Azure**: Sie stellen Azure Site Recovery bereit, um die Replikation, das Failover und die Wiederherstellung von lokalen physischen Windows- und Linux-Servern in Azure zu koordinieren.

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie eine Replikation zu Azure ausführen. Wenn Sie virtuelle VMware-Computer oder physische Windows- und Linux-Server in einem sekundären Rechenzentrum replizieren möchten, helfen Ihnen die Informationen unter [Replizieren lokaler virtueller VMware-Computer oder physischer Server an einen sekundären Standort im klassischen Azure-Portal](site-recovery-vmware-to-vmware.md) weiter.
>
>

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="enhanced-deployment"></a>Erweiterte Bereitstellung
Dieser Artikel enthält Anweisungen für eine erweiterte Bereitstellung im klassischen Azure-Portal. Wir empfehlen Ihnen, diese Version für alle neuen Bereitstellungen zu verwenden. Wenn Ihre Bereitstellung mit der älteren Version durchgeführt wurde, empfehlen wir eine Migration zur neuen Version. Weitere Informationen zur Migration finden Sie unter [Replizieren von virtuellen VMware-Computern und physischen Servern nach Azure mithilfe von Azure Site Recovery und dem klassischen Portal (Legacy)](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment).

Bei der erweiterten Bereitstellung handelt es sich um ein umfangreiches Update. Es folgt eine Zusammenfassung der Verbesserungen, die wir vorgenommen haben:

* **Keine Infrastruktur-VMs in Azure**: Daten werden direkt auf ein Azure-Speicherkonto repliziert. Außerdem ist keine Einrichtung von Infrastruktur-VMs (z.B. Konfigurationsserver oder Masterzielserver) für die Replikation und das Failover erforderlich, wie es bei der Legacybereitstellung der Fall war.  
* **Einheitliche Installation**: Ein einziger Installationsvorgang ermöglicht eine einfache Einrichtung mit Skalierbarkeit für lokale Komponenten.
* **Sichere Bereitstellung**: Der gesamte Datenverkehr wird verschlüsselt, und die Replikationsverwaltungskommunikation wird über HTTPS 443 übermittelt.
* **Wiederherstellungspunkte**: Unterstützung für absturz- und anwendungskonsistente Wiederherstellungspunkte in Windows- und Linux-Umgebungen sowie Unterstützung für Konfigurationen mit Einzel-VM- und Multi-VM-Konsistenz.
* **Testfailover**: Unterstützung für ein unterbrechungsfreies Testfailover auf Azure ohne Beeinträchtigung der Produktion oder Unterbrechung der Replikation.
* **Ungeplantes Failover**: Unterstützung für ein ungeplantes Failover nach Azure mit einer erweiterten Option zum automatischen Herunterfahren von virtuellen Computern vor dem Failover.
* **Failback**: Das integrierte Failback, das nur die Deltaänderungen zurück auf den lokalen Standort repliziert.
* **vSphere 6.0**: Eingeschränkte Unterstützung für VMware vSphere 6.0-Bereitstellungen.

## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Wie trägt Site Recovery zum Schutz von virtuellen Computern und physischen Servern bei?
* VMware-Administratoren können externe Schutzmechanismen konfigurieren, um Azure vor geschäftlichen Workloads und Anwendungen zu schützen, die auf virtuellen VMware-Computern ausgeführt werden. Servermanager können lokale physische Windows- und Linux-Server in Azure replizieren.
* Über die Azure Site Recovery-Konsole können Sie die Vorgänge für Replikation, Failover und Wiederherstellung einfach und zentral einrichten und verwalten.
* Wenn Sie virtuelle VMware-Computer replizieren, die von einem vCenter-Server verwaltet werden, kann Site Recovery diese virtuellen Computer automatisch ermitteln. Wenn sich Computer auf einem ESXi-Host befinden, ermittelt Site Recovery die virtuellen Computer auf dem Host.
* Wenn Sie einfache Failovervorgänge von der lokalen Infrastruktur zu Azure durchführen, können Sie ein Failback (Wiederherstellung) von Azure auf VMware-VM-Server am lokalen Standort durchführen.
* Sie können Wiederherstellungspläne konfigurieren, mit denen Anwendungsworkloads von verschiedenen Computern in Gruppen zusammengefasst werden. Wenn Sie für diese Pläne ein Failover durchführen, sorgt Site Recovery für die Konsistenz zwischen den verschiedenen virtuellen Computern (Multi-VM-Konsistenz). Dadurch können die Computer, die die gleichen Workloads ausführen, gemeinsam an einem konsistenten Datenpunkt wiederhergestellt werden.

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
### <a name="windows-64-bit-only"></a>Windows (nur&64; Bit)
* Windows Server 2008 R2 SP1+
* Windows Server 2012
* Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (nur&64; Bit)
* Red Hat Enterprise Linux 6.7, 7.1 und 7.2
* CentOS 6.5, 6.6, 6.7, 7.0, 7.1 und 7.2
* Oracle Enterprise Linux 6.4 und 6.5, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird
* SUSE Linux Enterprise Server 11 SP3

## <a name="scenario-architecture"></a>Szenarioarchitektur
Komponenten des Szenarios:

* **Ein lokaler Verwaltungsserver**: Der Verwaltungsserver führt folgende Site Recovery-Komponenten aus:
  * **Konfigurationsserver**: Koordiniert die Kommunikation und verwaltet die Datenreplikations- und Wiederherstellungsvorgänge.
  * **Prozessserver**: Fungiert als Replikationsgateway. Er empfängt Daten von geschützten Quellcomputern, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet die Replikationsdaten an den Azure-Speicher. Außerdem wickelt er die Pushinstallation des Mobilitätsdiensts auf geschützten Computern ab und führt die automatische Ermittlung von virtuellen VMware-Computern durch.
  * **Masterzielserver**: Verarbeitet die Replikationsdaten während des Failbacks von Azure.
    Sie können auch einen Verwaltungsserver bereitstellen, der ausschließlich als Prozessserver fungiert, um Ihre Bereitstellung zu skalieren.
* **Der Mobilitätsdienst**: Diese Komponente wird auf jedem Computer (VMware-VM oder physischer Server) bereitgestellt, den Sie nach Azure replizieren möchten. Er erfasst die Datenschreibvorgänge auf dem Computer und sendet sie an den Prozessserver.
* **Azure**: Sie müssen keine virtuellen Azure-Computer für die Replikation und das Failover erstellen. Der Site Recovery-Dienst übernimmt die Datenverwaltung, und die Daten werden direkt im Azure-Speicher repliziert. Die replizierten virtuellen Azure-Computer werden automatisch hochgefahren, aber nur, wenn ein Failover zu Azure stattfindet. Aber wenn Sie ein Failback von Azure zum lokalen Standort durchführen möchten, müssen Sie einen virtuellen Azure-Computer einrichten, der als Prozessserver fungiert.

In der folgenden Grafik (von Henry Robalino) ist dargestellt, wie diese Komponenten interagieren:

![Komponentenarchitektur](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

## <a name="capacity-planning"></a>Kapazitätsplanung
Bei der Kapazitätsplanung sollten Sie folgende Aspekte berücksichtigen:

* **Quellumgebung**: Die Kapazitätsplanung oder Anforderungen an die VMware-Infrastruktur und die Quellcomputer.
* **Verwaltungsserver**: Die Planung für die lokalen Verwaltungsserver, die die Site Recovery-Komponenten ausführen.
* **Netzwerkbandbreite von der Quelle zum Ziel**: Die Planung für die Netzwerkbandbreite, die für die Replikation zwischen der Quelle und Azure benötigt wird.

### <a name="source-environment-considerations"></a>Überlegungen zur Quellumgebung
* **Maximale tägliche Änderungsrate**: Für einen geschützten Computer kann nur ein Prozessserver verwendet werden. Ein einzelner Prozessserver kann pro Tag bis zu 2 TB an Datenänderungen verarbeiten. Daher beträgt die maximale Datenänderungsrate pro Tag, die für einen geschützten Computer unterstützt wird, 2 TB.
* **Maximaler Durchsatz**: Ein replizierter Computer kann zu einem einzelnen Speicherkonto in Azure gehören. Ein Standardspeicherkonto kann maximal 20.000 Anforderungen pro Sekunde verarbeiten, und es wird empfohlen, dass Sie die Anzahl von IOPS über einen Quellcomputer hinweg bei 20.000 halten. Wenn Ihr Quellcomputer beispielsweise fünf Datenträger hat und jeder Datenträger 120 IOPS (mit einer Größe von 8 KB) für die Quelle generiert, wird das Azure-Limit von 500 IOPS pro Datenträger eingehalten. Erforderliche Anzahl von Speicherkonten = Quell-IOPS insgesamt/20.000.

### <a name="management-server-considerations"></a>Überlegungen zu Verwaltungsservern
Auf dem Verwaltungsserver werden die Site Recovery-Komponenten ausgeführt, die die Datenoptimierung, die Replikation und die Verwaltung abwickeln. Der Verwaltungsserver muss in der Lage sein, die tägliche Änderungsrate für alle auf geschützten Computern ausgeführten Workloads zu verarbeiten, und er muss über genügend Bandbreite zum ständigen Replizieren der Daten im Azure-Speicher verfügen. Dies bedeutet Folgendes:

* Der Prozessserver empfängt Replikationsdaten von geschützten Computern und optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung, bevor er sie an Azure sendet. Der Verwaltungsserver muss über genügend Ressourcen zum Ausführen dieser Aufgaben verfügen.
* Der Prozessserver verwendet einen datenträgerbasierten Cache. Wir empfehlen die Verwendung eines separaten Cachedatenträgers mit einer Größe von mindestens 600 GB, um Datenänderungen bei einem Netzwerkengpass oder -ausfall zwischenspeichern und verarbeiten zu können. Bei der Bereitstellung können Sie den Cache auf jedem Datenträger konfigurieren, auf dem mindestens 5 GB Speicher verfügbar sind, aber 600 GB werden als Minimum empfohlen.
* Als bewährte Methode wird empfohlen, den Verwaltungsserver im demselben Netzwerk und demselben LAN-Segment anzuordnen, in dem sich auch die zu schützenden Computer befinden. Er kann in einem anderen Netzwerk platziert werden, aber die zu schützenden Computer sollten über L3-Netzwerksichtbarkeit für den Verwaltungsserver verfügen.

Die empfohlenen Größen für den Verwaltungsserver werden in der folgenden Tabelle zusammengefasst:

| **CPU des Verwaltungsservers** | **Arbeitsspeicher** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 Sockets * 4 Kerne mit 2,5 GHz) |16 GB |300 GB |500 GB oder weniger |Stellen Sie einen Verwaltungsserver mit diesen Einstellungen bereit, um weniger als 100 Computer zu replizieren. |
| 12 vCPUs (2 Sockets · 6 Kerne mit 2,5 GHz) |18 GB |600 GB |500 GB bis 1 TB |Stellen Sie einen Verwaltungsserver mit diesen Einstellungen bereit, um 100 bis 150 Computer zu replizieren. |
| 16 vCPUs (2 Sockets · 8 Kerne mit 2,5 GHz) |32 GB |1 TB |1 TB bis 2 TB |Stellen Sie einen Verwaltungsserver mit diesen Einstellungen bereit, um 150 bis 200 Computer zu replizieren. |
| Bereitstellen eines weiteren Prozessservers | | |> 2 TB |Stellen Sie zusätzliche Prozessserver bereit, wenn Sie mehr als 200 Computer replizieren oder wenn die tägliche Änderungsrate 2 TB überschreitet. |

Hierbei gilt:

* Jeder Quellcomputer ist mit 3 Datenträgern von jeweils 100 GB konfiguriert.
* Bei den Benchmarkingmessungen für den Cachedatenträger wurde ein Speicher aus acht SAS-Laufwerken mit 10.000 U/min und RAID 10 verwendet.

### <a name="network-bandwidth-from-source-to-target"></a>Netzwerkbandbreite von der Quelle zum Ziel
Berechnen Sie unbedingt die erforderliche Bandbreite für die Erstreplikation und die Deltareplikation mithilfe des [Capacity Planner-Tools](site-recovery-capacity-planner.md).

#### <a name="throttling-bandwidth-used-for-replication"></a>Einschränken der Bandbreite für die Replikation
Der in Azure replizierte VMware-Datenverkehr wird durch einen bestimmten Prozessserver geleitet. Sie können die für die Site Recovery-Replikation verfügbare Bandbreite auf diesem Server wie folgt einschränken:

1. Öffnen Sie das Microsoft Azure Backup-MMC-Snap-In auf dem Hauptverwaltungsserver oder auf einem Verwaltungsserver, auf dem zusätzlich bereitgestellte Prozessserver ausgeführt werden. Standardmäßig wird auf dem Desktop eine Verknüpfung für Microsoft Azure Backup erstellt. Ansonsten finden Sie die Anwendung unter „C:\Programme\Microsoft Azure Recovery Services Agent\bin\wabadmin“.
2. Klicken Sie im Snap-In auf **Eigenschaften ändern**.

    ![Drosseln der Bandbreite – Änderungseigenschaften](./media/site-recovery-vmware-to-azure-classic/throttle1.png)
3. Definieren Sie auf der Registerkarte **Bandbreiteneinschränkung** die Bandbreite, die für die Site Recovery-Replikation genutzt werden kann, sowie den anzuwendenden Zeitplan.

    ![Drosseln der Bandbreite – Replikation](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

Sie können die Drosselung auch mit PowerShell festlegen. Hier sehen Sie ein Beispiel:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>Maximales Ausnutzen der Bandbreite
Um die Bandbreite zu erhöhen, die von Azure Site Recovery für die Replikation genutzt wird, müssen Sie einen Registrierungsschlüssel ändern.

Der folgende Schlüssel steuert die Anzahl der Threads pro repliziertem Datenträger, die bei der Replikation genutzt werden:

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 In einem absichtlich mit großen Reserven ausgestatteten Netzwerk müssen die Standardwerte dieses Registrierungsschlüssels geändert werden. Wir unterstützen ein Maximum von 32.  

Weitere Informationen zur ausführlichen Kapazitätsplanung finden Sie unter [Site Recovery Capacity Planner](site-recovery-capacity-planner.md).

### <a name="additional-process-servers"></a>Zusätzliche Prozessserver
Wenn Sie mehr als 200 Computer schützen müssen oder die tägliche Änderungsrate größer als 2 TB ist, können Sie zusätzliche Server zum Verarbeiten der Last hinzufügen. Zum horizontalen Hochskalieren haben Sie folgende Möglichkeiten:

* Erhöhen Sie die Anzahl von Verwaltungsservern. Beispielsweise können Sie mit zwei Verwaltungsservern bis zu 400 Computer schützen.
* Fügen Sie zusätzliche Prozessserver hinzu, und verwenden Sie diese Prozessserver anstelle des Verwaltungsservers (oder als Ergänzung zu diesem) für die Abwicklung des Datenverkehrs.

Die folgende Tabelle beschreibt dieses Szenario:

* Sie richten den ursprünglichen Verwaltungsserver so ein, dass er nur als Konfigurationsserver verwendet wird.
* Sie richten einen zusätzlichen Prozessserver ein.
* Sie konfigurieren die geschützten virtuellen Computer so, dass sie den zusätzlichen Prozessserver verwenden.
* Jeder geschützte Quellcomputer ist mit drei Datenträgern von jeweils 100 GB konfiguriert.

| **Ursprünglicher Verwaltungsserver**<br/><br/>(Konfigurationsserver) | **Zusätzlicher Prozessserver** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 Sockets · 4 Kerne mit 2,5 GHz), 16 GB RAM |4 vCPUs (2 Sockets · 2 Kerne mit 2,5 GHz), 8 GB RAM |300 GB |250 GB oder weniger |Sie können maximal 85 Computer replizieren. |
| 8 vCPUs (2 Sockets · 4 Kerne mit 2,5 GHz), 16 GB RAM |8 vCPUs (2 Sockets · 4 Kerne mit 2,5 GHz), 12 GB RAM |600 GB |250 GB bis 1 TB |Sie können 85 bis 150 Computer replizieren. |
| 12 vCPUs (2 Sockets · 6 Kerne mit 2,5 GHz), 18 GB RAM |12 vCPUs (2 Sockets · 6 Kerne mit 2,5 GHz), 24 GB RAM |1 TB |1 TB bis 2 TB |Sie können 150 bis 225 Computer replizieren. |

Wie Sie Ihre Server skalieren, hängt davon ab, ob Sie das zentrale Hochskalieren oder das horizontale Hochskalieren als Modell bevorzugen. Beim zentralen Hochskalieren stellen Sie wenige besonders leistungsstarke Verwaltungs- und Prozessserver bereit. Beim horizontalen Hochskalieren stellen Sie mehr Server mit geringeren Ressourcen bereit. Wenn Sie zum Beispiel 220 Computer schützen müssen, können Sie sich für eine der folgenden Optionen entscheiden:

* Konfigurieren Sie den ursprünglichen Verwaltungsserver mit 12 vCPUs und 18 GB RAM. Konfigurieren Sie einen zusätzlichen Prozessserver mit 12 vCPUs und 24 GB RAM. Konfigurieren Sie die geschützten Computer so, dass sie nur den zusätzlichen Prozessserver verwenden.
* Konfigurieren Sie zwei Verwaltungsserver (2 x 8 vCPUs, 16 GB RAM) und zwei zusätzliche Prozessserver (1 x 8 vCPUs und 1 x 4 vCPUs für 135 + 85 (220) Computer). Konfigurieren Sie die geschützten Computer so, dass sie nur den zusätzlichen Prozessserver verwenden.

Führen Sie die Schritte unter [Bereitstellen zusätzlicher Prozessserver](#deploy-additional-process-servers) aus, um einen zusätzlichen Prozessserver einzurichten.

## <a name="before-you-start-deployment"></a>Vor der Bereitstellung
In den folgenden Tabellen werden die Voraussetzungen für die Bereitstellung dieses Szenarios zusammengefasst.

### <a name="azure-prerequisites"></a>Voraussetzungen für Azure
| **Voraussetzung** | **Details** |
| --- | --- |
| **Azure-Konto** |Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung. Weitere Informationen zu den Preisen von Site Recovery finden Sie unter [Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). |
| **Azure-Speicher** |Sie benötigen ein Azure Storage-Konto, um replizierte Daten zu speichern. Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden bei einem Failover hochgefahren. <br/><br/>Sie benötigen ein [standardmäßiges georedundantes Speicherkonto](../storage/storage-redundancy.md#geo-redundant-storage). Das Konto muss sich in der gleichen Region wie der Site Recovery-Dienst befinden und dem gleichen Abonnement zugeordnet sein. Eine Replikation in Storage Premium-Konten wird derzeit nicht unterstützt und sollte nicht verwendet werden.<br/><br/>Das Verschieben von Speicherkonten, die mit dem [Azure-Portal](../storage/storage-create-storage-account.md) erstellt wurden, über Ressourcengruppen hinweg wird nicht unterstützt. Weitere Informationen finden Sie unter [Einführung in Microsoft Azure Storage](../storage/storage-introduction.md).<br/><br/> |
| **Azure-Netzwerk** |Sie benötigen ein virtuelles Azure-Netzwerk, mit dem die virtuellen Azure-Computer eine Verbindung herstellen, wenn ein Failover stattfindet. Das virtuelle Azure-Netzwerk muss sich in der gleichen Region wie der Site Recovery-Tresor befinden.<br/><br/>Nach einem Failover in Azure benötigen Sie zum Ausführen eines Failbacks eine VPN-Verbindung (oder Azure ExpressRoute-Verbindung) zwischen dem Azure-Netzwerk und dem lokalen Standort. |

### <a name="on-premises-prerequisites"></a>Lokale Voraussetzungen
| **Voraussetzung** | **Details** |
| --- | --- |
| **Verwaltungsserver** |Sie benötigen einen lokalen Server mit Windows Server 2012 R2, der auf einem virtuellen Computer oder einem physischen Server ausgeführt wird. Alle lokalen Site Recovery-Komponenten werden auf diesem Verwaltungsserver installiert.<br/><br/> Es wird empfohlen, den Server als hoch verfügbare VMware-VM bereitzustellen. Das Failback von Azure auf den lokalen Standort erfolgt immer zu virtuellen VMware-Computern, unabhängig davon, ob das Failover für virtuelle Computer oder für physische Server durchgeführt wurde. Wenn Sie den Verwaltungsserver nicht als VMware-VM konfigurieren, müssen Sie einen separaten Masterzielserver als VMware-VM einrichten, um Failbackdatenverkehr zu empfangen.<br/><br/>Der Server darf kein Domänencontroller sein.<br/><br/>Der Server muss über eine statische IP-Adresse verfügen.<br/><br/>Der Hostname des Servers darf höchstens 15 Zeichen umfassen.<br/><br/>Das Gebietsschema des Betriebssystems darf nur Englisch sein.<br/><br/>Der Verwaltungsserver muss Zugriff auf das Internet haben.<br/><br/>Sie benötigen wie folgt ausgehenden Zugriff vom Server: temporärer Zugriff auf HTTP (Port 80) während des Setups der Site Recovery-Komponenten (zum Herunterladen von MySQL), dauerhaften ausgehenden Zugriff auf HTTPS (Port 443) für Replikationsverwaltung und dauerhaften ausgehenden Zugriff auf HTTPS (Port 9443) für Replikationsdatenverkehr (dieser Port kann geändert werden).<br/><br/> Stellen Sie sicher, dass die folgenden URLs vom Verwaltungsserver aus zugänglich sind: <br/>- \*.hypervrecoverymanager.windowsazure.com<br/>- \*.accesscontrol.windows.net<br/>- \*.backup.windowsazure.com<br/>- \*.blob.core.windows.net<br/>- \*.store.core.windows.net<br/>- https://www.msftncsi.com/ncsi.txt<br/>- [ https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Wenn Sie auf dem Server über Firewallregeln verfügen, die auf IP-Adressen basieren, sollten Sie sicherstellen, dass die Regeln die Kommunikation mit Azure zulassen. Sie müssen die [IP-Bereiche für Azure-Rechenzentrum](https://www.microsoft.com/download/details.aspx?id=41653) und den HTTPS-Port (443) zulassen. Außerdem müssen Sie die IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ über eine Whitelist zulassen. Die URL [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") dient zum Herunterladen von MySQL. |
| **VMware vCenter-/ESXi-Host** |Sie benötigen mindestens einen VMware vSphere ESX/ESXi-Hypervisor, der Ihre virtuellen VMware-Computer verwaltet und auf dem ESX/ESXi in der Version 6.0, 5.5 oder 5.1 mit den neuesten Updates ausgeführt wird.<br/><br/> Es wird empfohlen, einen VMware vCenter-Server zum Verwalten Ihrer ESXi-Hosts bereitzustellen. Auf diesem Server muss vCenter 6.0 oder 5.5 mit den neuesten Updates ausgeführt werden.<br/><br/>Beachten Sie, dass Site Recovery neue Features von vCenter und vSphere 6.0 nicht unterstützt. Hierzu zählen etwa Cross vCenter vMotion, virtuelle Volumes und Storage DRS. Site Recovery unterstützt nur Features, die auch in Version 5.5 verfügbar waren. |
| **Geschützte Computer** |**Azure**<br/><br/>Die zu schützenden Computer müssen die [Voraussetzungen für Azure](site-recovery-prereq.md) zum Erstellen von Azure-VMs erfüllen.<br><br/>Wenn Sie nach dem Failover eine Verbindung mit den Azure-VMs herstellen möchten, müssen Sie Remotedesktopverbindungen in der lokalen Firewall aktivieren.<br/><br/>Die individuelle Datenträgerkapazität auf geschützten Computern sollte nicht mehr als 1023 GB betragen. Ein virtueller Computer kann bis zu 64 Datenträger haben (also bis zu 64 TB). Wenn Sie über Datenträger verfügen, die größer als 1 TB sind, sollten Sie die Nutzung einer Datenbank-Replikationslösung wie SQL Server Always On oder Oracle Data Guard in Erwägung ziehen.<br/><br/>Für die Komponenteninstallation sind mindestens 2 GB verfügbarer Speicherplatz auf dem Installationslaufwerk erforderlich.<br/><br/>Gastcluster mit freigegebenen Datenträgern werden nicht unterstützt. Wenn in Ihrer Bereitstellung Cluster vorhanden sind, sollten Sie die Nutzung einer Datenbank-Replikationslösung wie SQL Server Always On oder Oracle Data Guard in Erwägung ziehen.<br/><br/>Das Starten über Unified Extensible Firmware Interface (UEFI)/Extensible Firmware Interface (EFI) wird nicht unterstützt.<br/><br/>Computernamen dürfen zwischen 1 und 63 Zeichen enthalten (Buchstaben, Zahlen und Bindestriche). Der Name muss mit einem Buchstaben oder einer Zahl beginnen und mit einem Buchstaben oder einer Zahl enden. Nachdem ein Computer geschützt wurde, können Sie den Azure-Namen ändern.<br/><br/>**Virtuelle VMware-Computer**<br/><br>Sie müssen VMware vSphere PowerCLI 6.0 auf dem Verwaltungsserver (Konfigurationsserver) installieren.<br/><br/>Auf virtuellen VMware-Computern, die Sie schützen möchten, müssen VMware-Tools installiert sein und ausgeführt werden.<br/><br/>Wenn auf dem virtuellen Quellcomputer NIC-Teamvorgänge ausgeführt wurden, wird dies nach dem Failover zu Azure in eine einzelne NIC konvertiert.<br/><br/>Sofern geschützte virtuelle Computer über einen iSCSI-Datenträger verfügen, konvertiert Site Recovery die iSCSI-Datenträger der geschützten virtuellen Computer in eine VHD-Datei, wenn ein Failover des virtuellen Computers in Azure durchgeführt wird. Falls ein iSCSI-Ziel vom virtuellen Azure-Computer erreicht werden kann, stellt er eine Verbindung zum ISCSI-Ziel her. Dadurch sind für ihn effektiv zwei Datenträger sichtbar – der VHD-Datenträger auf dem virtuellen Azure-Computer und der iSCSI-Quelldatenträger. In diesem Fall müssen Sie auf dem virtuellen Azure-Computer die Verbindung mit dem nach dem Failover angezeigten iSCSI-Ziel trennen.<br/><br/>Weitere Informationen zu VMware-Benutzerberechtigungen, die für Site Recovery erforderlich sind, finden Sie unter [VMware-Berechtigungen für den vCenter-Zugriff](#vmware-permissions-for-vcenter-access).<br/><br/> **Windows-Servercomputer (auf VMware-VM oder physischem Server)**<br/><br/>Auf dem Server muss ein unterstütztes 64-Bit-Betriebssystem ausgeführt werden: Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 mit SP1 oder höher.<br/><br/>Das Betriebssystem muss auf dem Laufwerk „C“ installiert sein. Bei dem Betriebssystemdatenträger sollte es sich um einen einfachen Windows-Datenträger handeln. (Das Betriebssystem sollte nicht auf einem dynamischen Windows-Datenträger installiert werden.)<br/><br/>Auf Computern mit Windows Server 2008 R2 muss .NET Framework 3.5.1 installiert sein.<br/><br/>Für die Pushinstallation des Mobilitätsdiensts auf Windows-Servern muss ein Administratorkonto (lokaler Administrator auf dem Windows-Computer) angegeben werden. Wenn das bereitgestellte Konto kein Domänenkonto ist, müssen Sie die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren. Weitere Informationen finden Sie unter [Installieren des Mobilitätsdiensts mit Pushinstallation](#install-the-mobility-service-with-push-installation).<br/><br/>Site Recovery unterstützt virtuelle Computer mit einem RDM-Datenträger. Beim Failback wird der RDM-Datenträger wiederverwendet, sofern die ursprüngliche Quell-VM und der RDM-Datenträger verfügbar sind. Wenn sie nicht verfügbar sind, erstellt Site Recovery beim Failback für jeden Datenträger eine neue VMDK-Datei.<br/><br/>**Linux-Computer**<br/><br/>Sie benötigen ein unterstütztes 64-Bit-Betriebssystem: Red Hat Enterprise Linux 6.7, Centos 6.5, 6.6 oder 6.7, Oracle Enterprise Linux 6.4 oder 6.5 (unter dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird) oder SUSE Linux Enterprise Server 11 SP3.<br/><br/>Dateien vom Typ „/etc/hosts“ auf geschützten Computern müssen Einträge enthalten, die den Namen des lokalen Hosts den IP-Adressen aller Netzwerkkarten zuordnen. <br/><br/>Wenn Sie nach einem Failover über einen Secure Shell-Client (SSH) eine Verbindung mit virtuellen Azure-Computern unter Linux herstellen möchten, sollten Sie sicherstellen, dass der Secure Shell-Dienst auf dem geschützten Computer beim Systemstart automatisch gestartet wird und dass die Firewallregeln eine SSH-Verbindung zulassen.<br/><br/>Der Schutz kann nur für Linux-Computer mit folgendem Speicher aktiviert werden: Dateisystem (EXT3, ETX4, ReiserFS, XFS), Multipath-Softwaregeräte-Mapper (multipath), Volume-Manager (LVM2). Physische Server mit HP CCISS-Controllerspeicher werden nicht unterstützt. Das ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.<br/><br/>Site Recovery unterstützt virtuelle Computer mit einem RDM-Datenträger. Beim Failback für Linux wird der RDM-Datenträger nicht wiederverwendet. Stattdessen erstellt Site Recovery für jeden entsprechenden RDM-Datenträger eine neue VMDK-Datei. |

Nur für eine Linux-VM: Stellen Sie sicher, dass Sie die Einstellung „disk.enableUUID=true“ in den Konfigurationsparametern der VM in VMware festlegen. Wenn diese Zeile nicht vorhanden ist, fügen Sie sie hinzu. Dies ist erforderlich, um eine konsistente UUID der VMDK-Datei bereitzustellen, damit sie ordnungsgemäß bereitgestellt wird. Ohne diese Einstellung führt ein Failback auch dann zu einem vollständigen Download, wenn die VM lokal verfügbar ist. Mit dem Hinzufügen dieser Einstellung wird sichergestellt, dass während des Failbacks nur die Deltaänderungen übertragen werden.

## <a name="step-1-create-a-vault"></a>Schritt 1: Erstellen eines Tresors
1. Melden Sie sich beim [Azure-Portal](https://manage.windowsazure.com/)an.
2. Erweitern Sie **Data Services** > **Recovery Services**, und klicken Sie auf **Site Recovery-Tresor**.
3. Klicken Sie auf **Neu erstellen** > **Schnellerfassung**.
4. Geben Sie unter **Name**einen Anzeigenamen ein, über den der Tresor identifiziert wird.
5. Wählen Sie unter **Region**die geografische Region für den Tresor aus. Informationen zu den unterstützten Regionen finden Sie unter [Site Recovery Preise](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klicken Sie auf **Tresor erstellen**.
    ![Tresor erstellen](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Überprüfen Sie auf der Statusleiste, ob der Tresor erfolgreich erstellt wurde. Der Tresor wird auf der Hauptseite von **Recovery Services** als **aktiv** aufgelistet.

## <a name="step-2-set-up-an-azure-network"></a>Schritt 2: Einrichten eines Azure-Netzwerks
Richten Sie ein Azure-Netzwerk ein, damit die virtuellen Azure-Computer nach dem Failover mit einem Netzwerk verbunden sind und damit das Failback zum lokalen Standort wie erwartet durchgeführt werden kann.

1. Wählen Sie im Azure-Portal die Option **Virtuelles Netzwerk erstellen**, und geben Sie den Netzwerknamen, den IP-Adressbereich und den Subnetznamen an.
2. Wenn Sie ein Failback durchführen möchten, müssen Sie dem Netzwerk eine VPN- oder ExpressRoute-Verbindung hinzufügen. Das VPN oder ExpressRoute können dem Netzwerk auch nach dem Failover noch hinzugefügt werden.

Weitere Informationen zu Azure-Netzwerken finden Sie unter [Virtuelle Netzwerke im Überblick](../virtual-network/virtual-networks-overview.md).

> [!NOTE]
> Für Netzwerke, die für die Site Recovery-Bereitstellung verwendet werden, wird die [Migration von Netzwerken](../azure-resource-manager/resource-group-move-resources.md) zwischen Ressourcengruppen im selben Abonnement oder zwischen verschiedenen Abonnements nicht unterstützt.
>
>

## <a name="step-3-install-the-vmware-components"></a>Schritt 3: Installieren der VMware-Komponenten
Führen Sie diese Schritte auf dem Verwaltungsserver aus, wenn Sie virtuelle VMware-Computer replizieren möchten:

1. [Laden Sie VMware vSphere PowerCLI 6.0 herunter](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) , und installieren Sie es.
2. Starten Sie den Server neu.

## <a name="step-4-download-a-vault-registration-key"></a>Schritt 4: Herunterladen eines Tresorregistrierungsschlüssels
1. Öffnen Sie auf dem Verwaltungsserver die Site Recovery-Konsole in Azure. Klicken Sie auf der Seite **Recovery Services** auf den Tresor, um die Seite **Schnellstart** zu öffnen. Sie können die Seite **Schnellstart** auch jederzeit öffnen, indem Sie auf das Symbol klicken.

    ![Symbol „Schnellstart“](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)
2. Klicken Sie auf der Seite **Schnellstart** auf **Zielressourcen vorbereiten** > **Registrierungsschlüssel herunterladen**. Die Registrierungsdatei wird automatisch generiert. Sie ist nach der Erstellung fünf Tage lang gültig.

## <a name="step-5-install-the-management-server"></a>Schritt 5: Installieren des Verwaltungsservers
> [!TIP]
> Stellen Sie sicher, dass die folgenden URLs vom Verwaltungsserver aus zugänglich sind:
>
> * *.hypervrecoverymanager.windowsazure.com
> * *.accesscontrol.windows.net
> * *.backup.windowsazure.com
> * *.blob.core.windows.net
> * *.store.core.windows.net
> * https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi
> * https://www.msftncsi.com/ncsi.txt
>
>



>[!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Setup-Registration/player]



1. Laden Sie auf der Seite **Schnellstart** die einheitliche Installationsdatei auf den Server herunter.
2. Führen Sie die Installationsdatei aus, um die Einrichtung im **Site Recovery-Assistenten für einheitliches Setup** zu starten.
3. Wählen Sie unter **Vorbereitung** die Option **Install the configuration server and process server** (Konfigurationsserver und Prozessserver installieren) aus.

   ![Voraussetzungen](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. Klicken Sie unter **Third-Party Software License** (Drittanbietersoftwarelizenz) auf **I Accept** (Ich stimme zu), um MySQL herunterzuladen und zu installieren.

    ![Drittanbietersoftware](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)
5. Navigieren Sie unter **Registrierung** zu dem Registrierungsschlüssel, den Sie aus dem Tresor heruntergeladen haben, und wählen Sie ihn aus.

    ![Registrierung](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)
6. Geben Sie unter **Internet Settings** (Interneteinstellungen) an, wie der auf dem Konfigurationsserver ausgeführte Anbieter eine Internetverbindung mit Azure Site Recovery herstellen soll.

   * Wenn die Verbindung über den derzeit auf dem Computer eingerichteten Proxy hergestellt werden soll, wählen Sie **Connect with existing proxy settings** (Mit vorhandenen Proxyeinstellungen verbinden) aus.
   * Wenn der Anbieter eine direkte Verbindung herstellt, wählen Sie **Ohne Proxy direkt verbinden** aus.
   * Falls für den vorhandenen Proxy eine Authentifizierung erforderlich ist oder Sie für die Anbieterverbindung einen benutzerdefinierten Proxy verwenden möchten, wählen Sie **Mit benutzerdefinierten Proxyeinstellungen verbinden** aus.
     * Bei einem benutzerdefinierten Proxy müssen Sie Adresse, Port und Anmeldeinformationen eingeben.
     * Bei Verwendung eines Proxys sollten Sie die folgenden URLs bereits zugelassen haben:
       * *.hypervrecoverymanager.windowsazure.com    
       * *.accesscontrol.windows.net
       * *.backup.windowsazure.com
       * *.blob.core.windows.net
       * *.store.core.windows.net

    ![Firewall](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

1. Bei der **Voraussetzungsüberprüfung** führt das Setup eine Überprüfung durch, um sicherzustellen, dass die Installation ausgeführt werden kann.

    ![Voraussetzungen](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Falls beim **Überprüfen der Synchronisierung der globalen Zeit** eine Warnung angezeigt wird, stellen Sie sicher, dass die Zeit der Systemuhr (Einstellungen für **Datum und Uhrzeit**) mit der Zeitzone übereinstimmt.

     ![Problem mit der Zeitsynchronisierung](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

1. Erstellen Sie unter **MySQL Configuration** (MySQL-Konfiguration) Anmeldeinformationen für die Anmeldung bei der MySQL-Serverinstanz, die installiert wird.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)
2. Wählen Sie unter **Umgebungsdetails** aus, ob Sie VMware-VMs replizieren werden. Wenn dies der Fall ist, überprüft das Setup, ob PowerCLI 6.0 installiert ist.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)
3. Wählen Sie unter **Install Location** (Installationsspeicherort) aus, wo die Binärdateien installiert werden sollen und wo der Cache gespeichert werden soll. Sie können ein Laufwerk auswählen, auf dem mindestens 5 GB an Speicherplatz verfügbar sind, aber wir raten Ihnen zu einem Cachelaufwerk mit mindestens 600 GB verfügbarem Speicherplatz.

   ![Installationsspeicherort](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)
4. Geben Sie unter **Network Selection** (Netzwerkauswahl) den Listener (Netzwerkadapter und SSL-Port) an, über den der Konfigurationsserver Replikationsdaten senden und empfangen wird. Sie können den Standardport (9443) ändern. Zusätzlich zu diesem Port wird Port 443 von einem Webserver verwendet, der Replikationsvorgänge orchestriert. Verwenden Sie für den Empfang von Replikationsdatenverkehr nicht Port 443.

    ![Netzwerkauswahl](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



1. Lesen Sie die unter **Summary** (Zusammenfassung) angezeigten Informationen und klicken Sie auf **Install** (Installieren). Nach Abschluss der Installation wird eine Passphrase generiert. Diese benötigen Sie bei der Aktivierung der Replikation. Kopieren Sie sie deshalb, und bewahren Sie sie an einem sicheren Ort auf.

   ![Zusammenfassung](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)


> [!WARNING]
> Der Proxy für den Microsoft Azure Recovery Services-Agent muss eingerichtet werden.
> Starten Sie nach Abschluss der Installation im Menü „Start“ von Windows die Microsoft Azure Recovery Services Shell. Führen Sie im daraufhin geöffneten Befehlsfenster den folgenden Befehlssatz aus, um die Proxyservereinstellungen einzurichten.
>
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
>


### <a name="run-setup-from-the-command-line"></a>Ausführen des Setupprogramms über die Befehlszeile
Sie können den Assistenten für einheitliches Setup auch wie folgt über die Befehlszeile ausführen:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Hierbei gilt:

* /ServerMode: Obligatorisch. Gibt an, ob bei der Installation der Konfigurationsserver und der Prozessserver oder nur der Prozessserver installiert werden soll. (Letzteres wird zum Installieren zusätzlicher Prozessserver verwendet.) Eingabewerte: CS, PS.
* InstallDrive: Obligatorisch. Gibt den Ordner an, in dem die Komponenten installiert werden.
* /MySQLCredFilePath. Obligatorisch. Gibt den Pfad zu einer Datei an, in der die Anmeldeinformationen für MySQL Server gespeichert sind. Rufen Sie die Vorlage zum Erstellen der Datei ab.
* /VaultCredFilePath. Obligatorisch. Speicherort der Datei mit Anmeldeinformationen für den Tresor.
* /EnvType. Obligatorisch. Typ der Installation. Werte: VMware, NonVMware.
* /PSIP und /CSIP. Obligatorisch. Die IP-Adressen für den Prozessserver (/PSIP) und den Konfigurationsserver (/CSIP).
* /PassphraseFilePath. Obligatorisch. Speicherort der Datei mit der Passphrase.
* /ByPassProxy. Optional. Gibt an, dass der Verwaltungsserver die Verbindung mit Azure ohne Proxy herstellt.
* /ProxySettingsFilePath. Optional. Gibt die Einstellungen für einen benutzerdefinierten Proxy an (entweder für einen Standardproxy auf dem Server, der eine Authentifizierung erfordert, oder für einen benutzerdefinierten Proxy).

## <a name="step-6-set-up-credentials-for-the-vcenter-server"></a>Schritt 6: Einrichten der Anmeldeinformationen für den vCenter-Server
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Discovery/player]
>
>

Der Prozessserver kann automatisch virtuelle VMware-Computer ermitteln, die von einem vCenter-Server verwaltet werden. Für diese automatische Ermittlung benötigt Site Recovery ein Konto und Anmeldeinformationen, mit denen auf den vCenter-Server zugegriffen werden kann. Wenn Sie nur physische Server replizieren, ist dieser Abschnitt nicht relevant.

Gehen Sie wie folgt vor, um das Konto und die Anmeldeinformationen festzulegen:

1. Erstellen Sie auf dem vCenter-Server auf vCenter-Ebene eine Rolle namens **Azure_Site_Recovery** mit den [erforderlichen Berechtigungen](#vmware-permissions-for-vcenter-access).
2. Weisen Sie die Rolle **Azure_Site_Recovery** einem vCenter-Benutzer zu.

   > [!NOTE]
   > Ein vCenter-Benutzerkonto mit der nur zum Lesezugriff berechtigenden schreibgeschützten Rolle kann das Failover ausführen, ohne geschützte Quellcomputer herunterzufahren. Wenn Sie diese Computer herunterfahren möchten, benötigen Sie die Rolle „Azure_Site_Recovery“. Wenn Sie nur virtuelle Computer (VMs) von VMware zu Azure migrieren und kein Failback benötigen, ist die schreibgeschützte Rolle ausreichend.
   >
   >
3. Öffnen Sie **cspsconfigtool**, um das Konto hinzuzufügen. Das Tool steht als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner „[INSTALLATIONSPFAD]\home\svsystems\bin“.
4. Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.

    ![Konto hinzufügen](./media/site-recovery-vmware-to-azure-classic/credentials1.png)
5. Fügen Sie unter **Kontodetails** Anmeldeinformationen hinzu, mit denen auf den vCenter-Server zugegriffen werden kann. Es kann länger als 15 Minuten dauern, bis der Kontoname im Portal angezeigt wird. Klicken Sie zur sofortigen Aktualisierung auf der Registerkarte **Konfigurationsserver** auf **Aktualisieren**.

    ![Details](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>Schritt 7: Hinzufügen von vCenter-Servern und ESXi-Hosts
Wenn Sie virtuelle VMware-Computer replizieren, müssen Sie einen vCenter-Server (oder einen ESXi-Host) hinzufügen.

1. Wählen Sie auf der Registerkarte **Server** > **Konfigurationsserver** die Option **vCenter-Server hinzufügen**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)
2. Fügen Sie folgende Informationen hinzu: die Details für den vCenter-Server oder den ESXi-Host, den Namen des Kontos, das Sie im vorherigen Schritt für den Zugriff auf den vCenter-Server festgelegt haben, und den Prozessserver, der zum Ermitteln der vom vCenter-Server verwalteten virtuellen VMware-Computer verwendet wird. Der vCenter-Server oder der ESXi-Host sollte sich in demselben Netzwerk wie der Server befinden, auf dem der Prozessserver installiert wird.

   > [!NOTE]
   > Wenn Sie den vCenter-Server oder ESXi-Host mit einem Konto hinzufügen, das auf dem vCenter- oder Hostserver keine Administratorrechte besitzt, müssen Sie sicherstellen, dass für die vCenter- oder ESXi-Konten folgende Berechtigungen aktiviert sind: Datacenter, Datastore, Folder, Jost, Network, Resource, Virtual Machine und vSphere Distributed Switch. Für den vCenter-Server muss die Anzeigeberechtigung für den Speicher aktiviert sein.
   >
   >

    ![Hinzufügen des vCenter-Servers](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)
3. Nach Abschluss der Ermittlung wird der vCenter-Server auf der Registerkarte **Konfigurationsserver** aufgeführt.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)

## <a name="step-8-create-a-protection-group"></a>Schritt 8: Erstellen einer Schutzgruppe
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Protection/player]
>
>

Schutzgruppen sind logische Gruppen mit virtuellen Computern oder physischen Servern, für die die gleichen Schutzeinstellungen gelten sollen. Sie weisen einer Schutzgruppe Schutzeinstellungen zu. Sie gelten für alle Computer (virtuell oder physisch), die Sie der Gruppe hinzufügen.

1. Navigieren Sie zu **Geschützte Elemente** > **Schutzgruppe**, und klicken Sie auf das entsprechende Symbol, um eine Schutzgruppe hinzuzufügen.

    ![Schutzgruppe erstellen](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)
2. Geben Sie auf der Seite **Schutzgruppeneinstellungen festlegen** einen Namen für die Gruppe an. Wählen Sie in der Dropdownliste **Von** den Konfigurationsserver aus, auf dem Sie die Gruppe erstellen möchten. Das **Ziel** ist Microsoft Azure.

    ![Schutzgruppeneinstellungen](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)
3. Konfigurieren Sie auf der Seite **Replikationseinstellungen festlegen** die Replikationseinstellungen, die für alle Computer in der Gruppe verwendet werden.

    ![Schutzgruppenreplikation](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

   * **Multi-VM-Konsistenz:**Wenn Sie diese Option aktivieren, werden freigegebene anwendungskonsistente Wiederherstellungspunkte auf den Computern in der Schutzgruppe erstellt. Diese Einstellung ist besonders wichtig, wenn auf allen Computern in der Schutzgruppe die gleiche Workload ausgeführt wird. Alle Computer werden an demselben Datenpunkt wiederhergestellt. Diese Option ist beim Replizieren von virtuellen VMware-Computern genauso verfügbar wie beim Replizieren von physischen Servern (Windows oder Linux).
   * **RPO-Schwellenwert:**Legt die RPO (Recovery Point Objective) fest. Wenn bei der Replikation für den kontinuierlichen Datenschutz der konfigurierte RPO-Schwellenwert überschritten wird, werden Warnungen generiert.
   * **Aufbewahrungszeitraum des Wiederherstellungspunkts:**Gibt die Aufbewahrungsdauer an. Geschützte Computer können innerhalb dieses Zeitfensters an einem beliebigen Punkt wiederhergestellt werden.
   * **Anwendungskonsistente Momentaufnahmehäufigkeit:** Gibt an, wie oft Wiederherstellungspunkte erstellt werden, die anwendungskonsistente Momentaufnahmen enthalten.

Wenn Sie auf das Häkchen klicken, wird eine Schutzgruppe mit dem von Ihnen angegebenen Namen erstellt. Außerdem wird eine zweite Schutzgruppe mit dem Namen „*Schutzgruppenname*-Failback“ erstellt. Diese Schutzgruppe wird verwendet, wenn Sie nach einem Failover zu Azure das Failback zum lokalen Standort ausführen. Sie können die Erstellung der Schutzgruppen auf der Seite **Geschützte Elemente** überwachen.

## <a name="step-9-install-the-mobility-service"></a>Schritt 9: Installieren des Mobilitätsdiensts
Der erste Schritt beim Aktivieren des Schutzes für virtuelle Computer und physische Server besteht im Installieren des Mobilitätsdiensts. Hierzu stehen zwei Möglichkeiten zur Verfügung:

* Automatische Pushinstallation des Diensts auf jedem Computer über den Prozessserver Wenn Sie einer Schutzgruppe Computer hinzufügen, auf denen bereits eine geeignete Version des Mobilitätsdiensts ausgeführt wird, wird die Pushinstallation nicht ausgeführt. Sie können den Dienst auch automatisch installieren, indem Sie die in Ihrem Unternehmen verwendete Pushmethode nutzen, z.B. WSUS (Windows Server Update Services) oder System Center Configuration Manager. Stellen Sie sicher, dass Sie den Verwaltungsserver eingerichtet haben, bevor Sie diesen Schritt ausführen.
* Installieren Sie den Dienst manuell auf jedem Computer, den Sie schützen möchten. Stellen Sie sicher, dass Sie den Verwaltungsserver eingerichtet haben, bevor Sie diesen Schritt ausführen.

### <a name="install-the-mobility-service-with-push-installation"></a>Installieren des Mobilitätsdiensts mit Pushinstallation
Beim Hinzufügen von Computern zu einer Schutzgruppe wird der Mobilitätsdienst vom Prozessserver automatisch per Pushfunktion auf jedem Computer installiert.

#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Vorbereiten des automatischen Pushs auf Windows-Computern
Gehen Sie wie folgt vor, um die Windows-Computer vorzubereiten, damit der Prozessserver automatisch den Mobilitätsdienst darauf installieren kann:

1. Erstellen Sie ein Konto, das vom Prozessserver zum Zugreifen auf den Computer verwendet werden kann. Das Konto muss über Administratorrechte verfügen (lokal oder für die Domäne). Diese Anmeldeinformationen werden nur für die Pushinstallation des Mobilitätsdiensts verwendet.

   > [!NOTE]
   > Wenn Sie kein Domänenkonto verwenden, müssen Sie die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren. Zu diesem Zweck fügen Sie in der Registrierung unter „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System“ den DWORD-Eintrag „LocalAccountTokenFilterPolicy“ mit dem Wert 1 hinzu. Geben Sie an einer Befehlszeile oder in PowerShell **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** ein, um den Registrierungseintrag über eine Befehlszeilenschnittstelle hinzuzufügen.
   >
   >
2. Wählen Sie für die Windows-Firewall des zu schützenden Computers die Option **Eine App oder ein Feature durch die Windows-Firewall zulassen** aus, und aktivieren Sie **Datei- und Druckerfreigabe** und **Windows-Verwaltungsinstrumentation**. Für Computer, die einer Domäne angehören, können Sie die Firewallrichtlinie mit einem Gruppenrichtlinienobjekt konfigurieren.

   ![Firewalleinstellungen](./media/site-recovery-vmware-to-azure-classic/mobility1.png)
3. Fügen Sie das von Ihnen erstellte Konto hinzu:

   * Öffnen Sie **cspsconfigtool**. Das Tool steht als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner „[INSTALLATIONSPFAD]\home\svsystems\bin“.
   * Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
   * Fügen Sie das von Ihnen erstellte Konto hinzu. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie einen Computer einer Schutzgruppe hinzufügen.

#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Vorbereiten des automatischen Pushs auf Linux-Servern
1. Vergewissern Sie sich, dass der zu schützende Linux-Computer unterstützt wird, wie unter [Lokale Voraussetzungen](#on-premises-prerequisites)beschrieben. Stellen Sie sicher, dass zwischen dem zu schützenden Computer und dem Verwaltungsserver, auf dem der Prozessserver ausgeführt wird, eine funktionierende Netzwerkverbindung besteht.
2. Erstellen Sie ein Konto, das vom Prozessserver zum Zugreifen auf den Computer verwendet werden kann. Das Konto muss einem Root-Benutzer auf dem Linux-Quellserver entsprechen. Diese Anmeldeinformationen werden nur für die Pushinstallation des Mobilitätsdiensts verwendet.

   * Öffnen Sie **cspsconfigtool**. Das Tool steht als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner „[INSTALLATIONSPFAD]\home\svsystems\bin“.
   * Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
   * Fügen Sie das von Ihnen erstellte Konto hinzu. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie einen Computer einer Schutzgruppe hinzufügen.
3. Vergewissern Sie sich, dass die Datei „/etc/hosts“ auf dem Linux-Quellserver Einträge enthält, die den Namen des lokalen Hosts den IP-Adressen zuordnen, die allen Netzwerkkarten zugewiesen sind.
4. Installieren Sie die neuesten openssh-, openssh-server- und openssl-Pakete auf dem Computer, den Sie schützen möchten.
5. Stellen Sie sicher, dass SSH auf Port 22 aktiviert ist und ausgeführt wird.
6. Aktivieren Sie das SFTP-Subsystem und die Kennwortauthentifizierung in der Datei "sshd_config" wie folgt:

   * Melden Sie sich als Root-Benutzer an.
   * Suchen Sie in der Datei „/etc/ssh/sshd_config“ nach der Zeile, die mit „PasswordAuthentication“ beginnt.
   * Heben Sie die Auskommentierung der Zeile auf, und ändern Sie den Wert von **no** in **yes**.
   * Suchen Sie die Zeile, die mit **Subsystem** beginnt, und heben Sie die Auskommentierung der Zeile auf.

     ![Linux: override default of no subsystems](./media/site-recovery-vmware-to-azure-classic/mobility2.png)

### <a name="install-the-mobility-service-manually"></a>Manuelles Installieren des Mobilitätsdiensts
Die Installationsprogramme stehen unter „C:\Programme (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository“ zur Verfügung.

| Quellbetriebssystem | Installationsdatei für den Mobilitätsdienst |
| --- | --- |
| Windows Server (nur&64; Bit) |Microsoft-ASR_UA_9*.0.0_Windows_* release.exe |
| CentOS 6.4, 6.5, 6.6 (nur 64 Bit) |Microsoft-ASR_UA_9*.0.0_RHEL6-64_*release.tar.gz |
| SUSE Linux Enterprise Server 11 SP3 (nur 64 Bit) |Microsoft-ASR_UA_9*.0.0_SLES11-SP3-64_*release.tar.gz |
| Oracle Enterprise Linux 6.4, 6.5 (nur 64 Bit) |Microsoft-ASR_UA_9*.0.0_OL6-64_*release.tar.gz |

#### <a name="install-the-mobility-service-manually-on-a-windows-server"></a>Manuelles Installieren des Mobilitätsdiensts auf einem Windows-Server
1. Laden Sie das entsprechende Installationsprogramm herunter, und führen Sie es aus.
2. Wählen Sie unter **Before you begin** (Vorbereitung) die Option **Mobility service** (Mobilitätsdienst) aus.

    ![Installation des Mobilitätsdiensts](./media/site-recovery-vmware-to-azure-classic/mobility3.png)
3. Geben Sie unter **Konfigurationsserverdetails** die IP-Adresse des Verwaltungsservers und die Passphrase ein, die beim Installieren der Verwaltungsserverkomponenten generiert wurde. Sie können die Passphrase abrufen, indem Sie auf dem Verwaltungsserver **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe –n** ausführen.

    ![Mobilitätsdienst](./media/site-recovery-vmware-to-azure-classic/mobility6.png)
4. Behalten Sie unter **Installationsspeicherort** den Standardspeicherort bei, und klicken Sie auf **Weiter**, um mit der Installation zu beginnen.
5. Überprüfen Sie die Installation unter **Installation Progress** (Installationsstatus), und starten Sie den Computer neu, wenn Sie dazu aufgefordert werden.

Sie können die Installation auch durchführen, indem Sie den folgenden Text in die Befehlszeile eingeben:

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]

Für den obigen Befehl gilt Folgendes:

* /Role: Obligatorisch. Gibt an, ob der Mobilitätsdienst installiert werden soll.
* /InstallLocation: Obligatorisch. Gibt an, wo der Dienst installiert werden soll.
* /PassphraseFilePath: Obligatorisch. Gibt die Passphrase des Konfigurationsservers an.
* /LogFilePath: Obligatorisch. Gibt den Speicherort für die Setupprotokolldatei an.

#### <a name="uninstall-the-mobility-service-manually"></a>Manuelles Deinstallieren des Mobilitätsdiensts
Sie können den Mobilitätsdienst deinstallieren, indem Sie in der Systemsteuerung die Option **Programm deinstallieren oder ändern** oder die Befehlszeile verwenden.

Der Befehl zum Deinstallieren des Mobilitätsdiensts über die Befehlszeile lautet:

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="change-the-ip-address-of-the-management-server"></a>Ändern der IP-Adresse des Verwaltungsservers
Nach dem Ausführen des Assistenten können Sie die IP-Adresse des Verwaltungsservers wie folgt ändern:

1. Öffnen Sie die Datei „hostconfig.exe“ (sie befindet sich auf dem Desktop).
2. Ändern Sie auf der Registerkarte **Global** die IP-Adresse des Verwaltungsservers.

   > [!NOTE]
   > Ändern Sie nur die IP-Adresse des Verwaltungsservers. Die Portnummer für die Kommunikation des Verwaltungsservers muss 443 lauten, und **HTTPS verwenden** muss aktiviert bleiben. Ändern Sie die Passphrase nicht.
   >
   >

    ![IP-Adresse des Verwaltungsservers](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-the-mobility-service-manually-on-a-linux-server"></a>Manuelles Installieren des Mobilitätsdiensts auf einem Linux-Server
1. Kopieren Sie das entsprechende TAR-Archiv auf den Linux-Computer, den Sie schützen möchten. In der Tabelle unter [Manuelles Installieren des Mobilitätsdiensts](#install-the-mobility-service-manually) können Sie ermitteln, welches TAR-Archiv verwendet werden sollte.
2. Öffnen Sie ein Shellprogramm, und extrahieren Sie das gezippte TAR-Archiv in einen lokalen Pfad, indem Sie Folgendes ausführen: `tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Erstellen Sie in dem lokalen Verzeichnis, in das Sie den Inhalt des TAR-Archivs extrahiert haben, die Datei „passphrase.txt“. Hierzu kopieren Sie die Passphrase aus „C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase“ auf dem Verwaltungsserver und speichern sie in „passphrase.txt“, indem Sie in der Shell *`echo <passphrase> >passphrase.txt`* ausführen.
4. Installieren Sie den Mobilitätsdienst, indem Sie den folgenden Befehl eingeben: *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*
5. Geben Sie die interne IP-Adresse des Verwaltungsservers an, und stellen Sie sicher, dass Port 443 ausgewählt ist.

#### <a name="install-the-mobility-service-from-the-command-line"></a>Installieren des Mobilitätsdiensts über die Befehlszeile

Kopieren Sie die Passphrase aus „C:\Programme (x86)\InMage Systems\private\connection“ auf dem Verwaltungsserver, und speichern Sie sie in „passphrase.txt“ auf dem Verwaltungsserver. Führen Sie die folgenden Befehle aus: In unserem Beispiel wird für den Verwaltungsserver die IP-Adresse 104.40.75.37 verwendet, und der HTTPS-Port lautet 443:

So installieren Sie den Dienst auf einem Produktionsserver:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

So installieren Sie den Dienst auf dem Masterzielserver:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>Schritt 10: Aktivieren des Schutzes für einen Computer
Zum Aktivieren des Schutzes fügen Sie einer Schutzgruppe virtuelle Computer und physische Server hinzu. Wenn Sie virtuelle VMware-Computer schützen, sollten Sie vor dem Starten Folgendes berücksichtigen:

* Virtuelle VMware-Computer werden alle 15 Minuten ermittelt, und es kann länger als 15 Minuten dauern, bis sie nach der Ermittlung im Site Recovery-Portal angezeigt werden.
* Es kann auch länger als 15 Minuten dauern, bis Umgebungsänderungen auf dem virtuellen Computer (z. B. die Installation von VMware-Tools) in Site Recovery aktualisiert werden.
* Sie können den Zeitpunkt der letzten Ermittlung für die virtuellen VMware-Computer Sie auf der Registerkarte **Konfigurationsserver** im Feld **Letzter Kontakt um** für den vCenter-Server oder den ESXi-Host überprüfen.
* Wenn Sie nach dem Erstellen einer Schutzgruppe einen vCenter-Server oder ESXi-Host hinzufügen, kann es mehr als 15 Minuten dauern, bis das Azure Site Recovery-Portal aktualisiert wird und virtuelle Computer im Dialogfeld **Computer zu Schutzgruppe hinzufügen** angezeigt werden.
* Wenn Sie gleich fortfahren und einer Schutzgruppe Computer hinzufügen möchten, ohne auf die planmäßige Ermittlung zu warten, können Sie den Konfigurationsserver markieren (klicken Sie nicht darauf) und auf **Aktualisieren** klicken.

Außerdem haben Sie folgende Möglichkeiten:

* Es wird empfohlen, Ihre Schutzgruppen so entwerfen, dass sie Ihre Workloads widerspiegeln. Fügen Sie beispielsweise Computer, auf denen eine bestimmte Anwendung ausgeführt wird, derselben Gruppe hinzu.
* Beim Hinzufügen von Computern zu einer Schutzgruppe installiert der Prozessserver den Mobilitätsdienst automatisch per Pushfunktion auf jedem Computer, sofern er nicht bereits vorhanden ist. Sie müssen aber für den Pushmechanismus einige Vorbereitungen treffen, wie im vorherigen Schritt beschrieben.

### <a name="add-machines-to-a-protection-group"></a>Hinzufügen von Computern zu einer Schutzgruppe

1. Navigieren Sie zu **Geschützte Elemente** > **Schutzgruppe** > **Computer** > **Computer hinzufügen**.
2. Wenn Sie virtuelle VMware-Computer schützen, wählen Sie unter **Virtuelle Computer auswählen** einen vCenter-Server aus, der die virtuellen Computer verwaltet (oder den EXSi-Host, auf dem sie ausgeführt werden), und wählen Sie dann die Computer aus.

    ![Aktivieren des Schutzes für virtuelle Computer](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)
3. Wenn Sie physische Server schützen, öffnen Sie unter **Virtuelle Computer auswählen** den Assistenten **Physische Computer hinzufügen** und geben die IP-Adresse und den Anzeigenamen an. Wählen Sie dann die Betriebssystemfamilie aus.

   ![Aktivieren des Schutzes für physische Server](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)
4. Wählen Sie unter **Zielressourcen angeben** das Speicherkonto aus, das für die Replikation verwendet werden soll, und geben Sie an, ob die Einstellungen für alle Workloads verwendet werden sollen. Storage Premium-Konten werden derzeit nicht unterstützt.

   > [!NOTE]
   > Das Verschieben von Speicherkonten, die mit dem [Azure-Portal](../storage/storage-create-storage-account.md) erstellt wurden, über Ressourcengruppen hinweg wird nicht unterstützt.                           
   > [Die Migration von Speicherkonten](../azure-resource-manager/resource-group-move-resources.md) zwischen Ressourcengruppen im gleichen Abonnement oder zwischen verschiedenen Abonnements wird für Speicherkonten nicht unterstützt, die für die Site Recovery-Bereitstellung verwendet werden.
   >
   >

    ![Konfigurieren von Einstellungen für das Ziel](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)
5. Wählen Sie unter **Konten angeben** das Konto aus, das Sie zur Verwendung für die automatische Installation des Mobilitätsdiensts [eingerichtet](#install-the-mobility-service-with-push-installation) haben.

    ![Angeben von Konten](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)
6. Klicken Sie auf das Häkchen, um das Hinzufügen von Computern zur Schutzgruppe zu beenden und die erste Replikation für jeden Computer zu starten.

   > [!NOTE]
   > Wenn die Pushinstallation vorbereitet wurde und Computer einer Schutzgruppe hinzugefügt werden, wird der Mobilitätsdienst automatisch auf diesen Computern installiert, sofern er dort noch nicht vorhanden ist. Nachdem der Dienst installiert wurde, wird ein Schutzauftrag gestartet, der aber mit einem Fehler abgebrochen wird. Nach dem Fehler müssen Sie jeden Computer, auf dem der Mobilitätsdienst installiert wurde, manuell neu starten. Nach dem Neustart wird der Schutzauftrag erneut gestartet, und die erste Replikation wird ausgeführt.
   >
   >

Sie können den Status auf der Seite **Aufträge** überwachen.

![Überwachen des Status auf der Seite „Aufträge“](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Der Schutzstatus kann auch unter **Geschützte Elemente** > *Schutzgruppenname* > **Virtuelle Computer** überwacht werden. Nachdem die erste Replikation abgeschlossen ist und die Daten synchronisiert sind, ändert sich der Status des Computers in **Geschützt**.

![Überwachen des Status unter „Geschützte Elemente“](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)

## <a name="step-11-set-protected-machine-properties"></a>Schritt 11: Festlegen der Eigenschaften geschützter Computer
1. Sobald ein Computer den Status **Geschützt** aufweist, können Sie die Failovereigenschaften konfigurieren. Wählen Sie in den Schutzgruppendetails den Computer aus, und öffnen Sie die Registerkarte **Konfigurieren**.
2. Site Recovery schlägt automatisch Eigenschaften für einen virtuellen Azure-Computer vor und erkennt die lokalen Netzwerkeinstellungen.

    ![Eigenschaften für virtuelle Computer festlegen](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)
3. Sie können diese Einstellungen ändern:

   * **Name des virtuellen Azure-Computers**: Dies ist der Name, der dem Computer in Azure nach dem Failover zugewiesen wird. Der Name muss den Azure-Anforderungen entsprechen.
   * **Größe des virtuellen Azure-Computers**: Die Anzahl der Netzwerkkarten hängt von der Größe ab, die Sie für den virtuellen Zielcomputer angeben. Weitere Informationen zu Größen und Adaptern finden Sie in den [Größentabellen](../virtual-machines/virtual-machines-linux-sizes.md#size-tables). Beachten Sie Folgendes:

     * Wenn Sie die Größe eines virtuellen Computers ändern und die Einstellungen speichern, wird die Anzahl von Netzwerkkarten beim nächsten Öffnen der Registerkarte **Konfigurieren** geändert. Die Mindestanzahl von Netzwerkkarten auf den virtuellen Zielcomputern entspricht der Mindestanzahl von Netzwerkkarten auf einem virtuellen Quellcomputer. Die maximale Anzahl von Netzwerkkarten wird durch die Größe des virtuellen Computers bestimmt.
       * Wenn die Anzahl von Netzwerkkarten des Quellcomputers maximal der Anzahl von Netzwerkkarten entspricht, die für die Größe des Zielcomputers zulässig ist, hat der Zielcomputer die gleiche Anzahl von Netzwerkkarten wie der Quellcomputer.
       * Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputer die maximal zulässige Anzahl für die Größe des Zielcomputers übersteigt, wird die Anzahl verwendet, die maximal für die Größe des Zielcomputers zulässig ist.
        Wenn beispielsweise ein Quellcomputer zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe vier Netzwerkadapter unterstützt, erhält der Zielcomputer zwei Netzwerkadapter. Falls der Quellcomputer dagegen zwei Netzwerkkarten besitzt und der Zielcomputer aufgrund seiner Größe nur eine Karte unterstützt, erhält der Zielcomputer nur eine Karte.
     * Besitzt der virtuelle Computer mehrere Netzwerkkarten, sollten alle Karten mit demselben Azure-Netzwerk verbunden werden.
   * **Azure-Netzwerk**: Sie müssen ein Azure-Netzwerk angeben, mit dem die virtuellen Azure-Computer nach dem Failover verbunden werden sollen. Wenn Sie kein Netzwerk angeben, werden die virtuellen Azure-Computer nicht mit einem Netzwerk verbunden. Außerdem müssen Sie ein Azure-Netzwerk angeben, wenn Sie ein Failback von Azure zum lokalen Standort ausführen möchten. Für das Failback wird eine VPN-Verbindung zwischen einem Azure-Netzwerk und dem lokalen Netzwerk benötigt.
   * **Azure-IP-Adresse/-Subnetz**: Wählen Sie für jede Netzwerkkarte das Subnetz aus, mit dem der virtuelle Azure-Computer verbunden werden soll. Beachten Sie Folgendes: Wenn die Netzwerkkarte des Quellcomputers für eine statische IP-Adresse konfiguriert wurde, können Sie für den virtuellen Azure-Computer eine statische IP-Adresse angeben. Wenn Sie keine statische IP-Adresse angeben, wird eine beliebige verfügbare IP-Adresse zugeordnet. Wenn die Ziel-IP-Adresse angegeben ist, sie aber bereits von einem anderen virtuellen Computer in Azure verwendet wird, tritt beim Failover ein Fehler auf. Wenn die Netzwerkkarte des Quellcomputers für DHCP konfiguriert ist, wird für Azure als Einstellung DHCP verwendet.      

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>Schritt 12: Erstellen eines Wiederherstellungsplans und Ausführen eines Failovers
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Failover/player]
>
>

Sie können ein Failover für einen einzelnen Computer oder auch für mehrere virtuelle Computer durchführen, auf denen die gleiche Aufgabe oder die gleiche Workload ausgeführt wird. Um für mehrere Computer ein gleichzeitiges Failover durchzuführen, fügen Sie sie einem Wiederherstellungsplan hinzu.

Gehen Sie wie folgt vor, um einen Wiederherstellungsplan zu erstellen:

1. Klicken Sie auf der Seite **Wiederherstellungspläne** auf **Wiederherstellungsplan hinzufügen**, und fügen Sie einen Wiederherstellungsplan hinzu. Geben Sie Details für den Plan an, und wählen Sie **Azure** als Ziel aus.

 ![Wiederherstellungsplan konfigurieren](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)
2. Wählen Sie unter **Virtuellen Computer auswählen** eine Schutzgruppe aus, und wählen Sie dann Computer in der Gruppe aus, die dem Wiederherstellungsplan hinzugefügt werden sollen.

 ![Virtuelle Computer hinzufügen](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

Sie können den Plan anpassen und Gruppen erstellen sowie die Reihenfolge festlegen, in der für die Computer im Wiederherstellungsplan ein Failover durchgeführt wird. Sie können auch Skripts und Aufforderungen für manuelle Aktionen hinzufügen. Skripts können manuell oder mithilfe von [Azure Automation-Runbooks](site-recovery-runbook-automation.md) erstellt werden. Weitere Informationen zur Anpassung von Wiederherstellungsplänen finden Sie unter [Erstellen von Wiederherstellungsplänen](site-recovery-create-recovery-plans.md).

## <a name="run-a-failover"></a>Ausführen eines Failovers
Beachten Sie vor dem Ausführen eines Failovers Folgendes:

* Stellen Sie sicher, dass der Verwaltungsserver ausgeführt wird und verfügbar ist. Wenn nicht, schlägt das Failover fehl.
* Bei Durchführung eines ungeplanten Failovers:

  * Fahren Sie primäre Computer nach Möglichkeit herunter, bevor Sie ein ungeplantes Failover ausführen. Dadurch wird sichergestellt, dass die Quell- und Replikatcomputer nicht gleichzeitig ausgeführt werden. Wenn Sie virtuelle VMware-Computer replizieren, können Sie beim Ausführen eines ungeplanten Failovers festlegen, dass Site Recovery die Quellcomputer herunterfahren soll, sofern dies möglich ist. Dies funktioniert abhängig vom Zustand am primären Standort. Wenn Sie physische Server replizieren, bietet Site Recovery diese Möglichkeit nicht.
  * Bei einem ungeplanten Failover wird die Datenreplikation der primären Computer beendet. Dies bedeutet, dass nach dem Beginn des Failovers keine Datenänderungen mehr übertragen werden.
  * Wenn Sie nach dem Failover eine Verbindung mit dem virtuellen Replikatcomputer in Azure herstellen möchten, aktivieren Sie vor dem Ausführen des Failovers die Remotedesktopverbindung auf dem Quellcomputer. Lassen Sie anschließend die RDP-Verbindung durch die Firewall zu. Außerdem müssen Sie RDP (Remote Desktop Protocol, Remotedesktopprotokoll) nach dem Failover auf dem öffentlichen Endpunkt des virtuellen Azure-Computers zulassen. Befolgen Sie die [bewährten Methoden](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx), um sicherzustellen, dass RDP nach dem Failover funktioniert.

> [!NOTE]
> Um beim Durchführen eines Failovers in Azure die bestmögliche Leistung zu erzielen, sollten Sie sicherstellen, dass Sie den Azure-Agent auf dem geschützten Computer installiert haben. Der Computer kann dann schneller gestartet werden, und Probleme können besser diagnostiziert werden. Der Azure-Agent ist für [Linux](https://github.com/Azure/WALinuxAgent) und [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) verfügbar.
>
>

### <a name="run-a-test-failover"></a>Durchführen eines Test-Failovers
Führen Sie ein Testfailover aus, um Ihre Prozesse für Failover und Wiederherstellung in einem isolierten Netzwerk zu simulieren, das keine Auswirkungen auf Ihre Produktionsumgebung hat. Bei dieser Simulation wird die reguläre Replikation ganz normal fortgesetzt. Das Testfailover wird an der Quelle initiiert, und Sie können es auf verschiedene Weise ausführen:

* **Ohne Angabe eines Azure-Netzwerks**: Wenn Sie das Testfailover ohne Netzwerk ausführen, wird beim Test überprüft, ob die virtuellen Computer starten und in Azure richtig angezeigt werden. Die virtuellen Computer werden nach einem Failover nicht mit einem Azure-Netzwerk verbunden.
* **Mit Angabe eines Azure-Netzwerks**: Bei dieser Art von Failover wird überprüft, ob die gesamte Replikationsumgebung wie erwartet hochgefahren wird und die betreffenden virtuellen Azure-Computer mit dem angegebenen Netzwerk verbunden werden.

So führen Sie ein Testfailover aus:

1. Wählen Sie auf der Seite **Wiederherstellungspläne** den Plan aus, und klicken Sie auf **Testfailover**.

 ![Auswählen des Plans](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)
2. Wählen Sie unter **Testfailover bestätigen** den Eintrag **Kein** aus, wenn Sie für das Testfailover kein Azure-Netzwerk verwenden möchten, oder wählen Sie das Netzwerk aus, mit dem die Test-VMs nach dem Failover verbunden werden sollen. Klicken Sie auf das Häkchen, um das Failover zu starten.

 ![Treffen einer Auswahl](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)
3. Überwachen Sie den Fortschritt des Failovers auf der Registerkarte **Aufträge** .

 ![Fortschritt überwachen](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)
4. Nach Abschluss des Failovers sollte der Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt werden. Wenn Sie eine RDP-Verbindung mit dem virtuellen Azure-Computer herstellen möchten, müssen Sie auf dem VM-Endpunkt Port 3389 öffnen.
5. Wenn das Failover die ****Endphase des Tests erreicht und Sie fertig sind, klicken Sie auf **Test abschließen**, um den Test zu beenden. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover.
6. Klicken Sie auf **Das Testfailover ist abgeschlossen.**, um eine automatische Bereinigung der Testumgebung durchzuführen. Nach Abschluss dieses Vorgangs wird für das Testfailover der Status **Abgeschlossen** angezeigt. Alle Elemente und virtuellen Computer, die im Zuge des Testfailovers automatisch erstellt wurden, werden gelöscht. Sollte ein Testfailover länger als zwei Wochen ausgeführt werden, wird sein Abschluss erzwungen.

### <a name="run-an-unplanned-failover"></a>Ausführen eines ungeplanten Failovers
Ein ungeplantes Failover wird in Azure initiiert und kann auch ausgeführt werden, wenn der primäre Standort nicht verfügbar ist.

1. Wählen Sie auf der Seite **Wiederherstellungspläne** den Plan aus, und klicken Sie auf **Failover** > **Ungeplantes Failover**.

 ![Auswählen des Plans](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)
2. Wenn Sie virtuelle VMware-Computer replizieren, können Sie versuchen, die lokalen VMs herunterzufahren. Dies ist eine „Best-Effort-Aktion“, und das Failover wird unabhängig davon fortgesetzt, ob die Aktion erfolgreich ist. Ist die Aktion nicht erfolgreich, werden auf der Registerkarte **Aufträge** unter **Unplanned Failover Jobs** (Ungeplante Failoveraufträge) entsprechende Fehlerdetails angezeigt.

 ![Option zum Herunterfahren von lokalen VMs](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

 > [!NOTE]
 > Wenn Sie physische Server replizieren, ist diese Option nicht verfügbar. Sie müssen versuchen, sie nach Möglichkeit manuell herunterzufahren.
 >
 >

3. Überprüfen Sie unter **Failover bestätigen** die Failoverrichtung (nach Azure), und wählen Sie den Wiederherstellungspunkt aus, der für das Failover verwendet werden soll. Wenn Sie beim Konfigurieren der Replikationseigenschaften die Option für Multi-VM aktiviert haben, können Sie den letzten anwendungs- oder absturzkonsistenten Wiederherstellungspunkt wiederherstellen. Sie können auch **Benutzerdefinierter Wiederherstellungspunkt** auswählen, um einen früheren Zustand wiederherzustellen. Klicken Sie auf das Häkchen, um das Failover zu starten.

 ![Bestätigen der Failoverrichtung](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)
4. Warten Sie, bis der ungeplante Failoverauftrag abgeschlossen ist. Sie können den Fortschritt des Failovers auf der Registerkarte **Aufträge** überwachen. Der Wiederherstellungsplan wird bei einem ungeplanten Failover auch dann bis zum Abschluss ausgeführt, wenn Fehler auftreten. Außerdem sollte der Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt werden.

### <a name="connect-to-replicated-azure-virtual-machines-after-failover"></a>Herstellen einer Verbindung mit replizierten virtuellen Azure-Computern nach dem Failover
Sie benötigen Folgendes, um nach dem Failover eine Verbindung mit replizierten virtuellen Computern in Azure herzustellen:

- Auf dem primären Computer aktivierte Remotedesktopverbindung
- Zulassung von RDP in der Windows-Firewall des primären Computers
- Hinzufügung von RDP zum öffentlichen Endpunkt für den virtuellen Azure-Computer

Weitere Informationen zur Einrichtung finden Sie unter [Troubleshooting remote desktop connection after failover using ASR](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) (Problembehandlung für Remotedesktopverbindung nach einem Failover mit ASR).

## <a name="deploy-additional-process-servers"></a>Bereitstellen zusätzlicher Prozessserver
Wenn Sie Ihre Bereitstellung über 200 Quellcomputer hinaus horizontal hochskalieren müssen oder die gesamte tägliche Änderungsrate 2 TB überschreitet, benötigen Sie zusätzliche Prozessserver zur Bewältigung des Datenverkehrsaufkommens. Überprüfen Sie vor der Einrichtung eines zusätzlichen Prozessservers zunächst die Anforderungen unter [Zusätzliche Prozessserver](#additional-process-servers), und richten Sie den Prozessserver dann gemäß der unten angegebenen Anleitung ein. Nachdem Sie den Prozessserver eingerichtet haben, können Sie die Quellcomputer so konfigurieren, dass sie diesen verwenden.

### <a name="set-up-an-additional-process-server"></a>Einrichten eines zusätzlichen Prozessservers
Gehen Sie beim Einrichten eines zusätzlichen Prozessservers wie folgt vor:

* Führen Sie den Assistenten für einheitliches Setup aus, und konfigurieren Sie einen Verwaltungsserver ausschließlich als Prozessserver.
* Wenn Sie die Datenreplikation nur mit dem neuen Prozessserver verwalten möchten, müssen Sie Ihre geschützten Computer migrieren.

### <a name="install-the-process-server"></a>Installieren des Prozessservers
1. Laden Sie auf der Seite **Schnellstart** die einheitliche Installationsdatei für die Installation der Komponenten von Site Recovery herunter. Führen Sie das Setup aus.
2. Wählen Sie unter **Before you begin** die Option **Add additional process servers to scale out deployment** (Weitere Prozessserver zum horizontalen Hochskalieren der Bereitstellung hinzufügen) aus.

 ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)
3. Verwenden Sie den Assistenten genau wie beim [Einrichten](#step-5-install-the-management-server) des ersten Verwaltungsservers.
4. Geben Sie unter **Konfigurationsserverdetails** die IP-Adresse des ursprünglichen Verwaltungsservers ein, auf dem Sie den Konfigurationsserver installiert haben, und geben Sie anschließend die Passphrase ein. Führen Sie **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe –n** auf dem ursprünglichen Verwaltungsserver aus, um sie abzurufen, falls Sie nicht im Besitz der Passphrase sind.

 ![Konfigurationsserverdetails](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migrieren der Computer für die Verwendung des neuen Prozessservers
1. Öffnen Sie **Konfigurationsserver** > **Server** > *Name des ursprünglichen Verwaltungsservers* > **Serverdetails**.

 ![Serverdetails](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)
2. Wählen Sie in der Liste **Prozessserver** neben dem Server, den Sie ändern möchten, die Option **Prozessserver ändern**.

 ![Aktualisieren des Prozessservers](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)
3. Wählen Sie die Option **Prozessserver ändern** und dann **Zielprozessserver**, und wählen Sie den neuen Verwaltungsserver aus. Wählen Sie anschließend die virtuellen Computer aus, die vom neuen Prozessserver verarbeitet werden. Klicken Sie auf das Informationssymbol, um Informationen über den Server zu erhalten. Um Sie bei Entscheidungen zur Last zu unterstützen, wird der durchschnittliche Speicherplatz angezeigt, der zum Replizieren jedes ausgewählten virtuellen Computers auf den neuen Prozessserver benötigt wird. Klicken Sie auf das Häkchen, um mit dem Replizieren auf den neuen Prozessserver zu beginnen.

 ![Ändern des Prozessservers](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)

## <a name="vmware-permissions-for-vcenter-access"></a>VMware-Berechtigungen für den vCenter-Zugriff
Der Prozessserver kann virtuelle Computer auf einem vCenter-Server automatisch ermitteln. Zum Ausführen dieser automatischen Ermittlung müssen Sie auf vCenter-Ebene eine Rolle namens „Azure_Site_Recovery“ definieren, die Site Recovery den Zugriff auf den vCenter-Server erlaubt. Wenn Sie VMware-Computer nur zu Azure migrieren müssen und kein Failback von Azure auszuführen brauchen, können Sie eine nur zum Lesezugriff berechtigende schreibgeschützte Rolle definieren – dies ist ausreichend. Richten Sie die Berechtigungen wie unter [Schritt 6: Einrichten der Anmeldeinformationen für den vCenter-Server](#step-6-set-up-credentials-for-the-vcenter-server) beschrieben ein. Die Rollenberechtigungen sind in der folgenden Tabelle zusammengefasst:

| **Rolle** | **Details** | **Berechtigungen** |
| --- | --- | --- |
| Rolle „Azure_Site_Recovery“ |Ermittlung von virtuellen VMware-Computern |Weisen Sie die folgenden Berechtigungen für den vCenter-Server zu:<br/><br/>Datenspeicher: Speicherplatz zuordnen, Datenspeicher durchsuchen, Low-Level-Dateivorgänge, Datei entfernen, Dateien virtueller Computer aktualisieren<br/><br/>Netzwerk: Netzwerk zuweisen<br/><br/>Resource (Ressource): Assign virtual machine to resource pool (Virtuelle Computer an Ressourcenpool zuweisen), Migrate powered off virtual machine (Ausgeschalteten virtuellen Computer migrieren), Migrate powered on virtual machine (Eingeschalteten virtuellen Computer migrieren)<br/><br/>Tasks (Aufgaben): Create task (Aufgabe erstellen), Update task (Aufgabe aktualisieren)<br/><br/>Virtueller Computer > Konfiguration<br/><br/>Virtueller Computer > Interagieren > Frage beantworten, Geräteverbindung, CD-Medien konfigurieren, Diskettenmedien konfigurieren, Ausschalten, Einschalten, VMware-Tools installieren<br/><br/>Virtueller Computer > Inventar > Erstellen, Registrieren, Registrierung aufheben<br/><br/>Virtueller Computer > Bereitstellung > Download virtueller Computer zulassen, Upload von Dateien virtueller Computer zulassen<br/><br/>Virtueller Computer > Momentaufnahmen > Momentaufnahmen entfernen |
| vCenter-Benutzerrolle |Ermittlung von virtuellen VMware-Computern/Failover ohne Herunterfahren der Quell-VM |Weisen Sie die folgenden Berechtigungen für den vCenter-Server zu:<br/><br/>Data Center object (Rechenzentrenobjekt) > Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt) <br/><br/>Der Benutzer wird auf Rechenzentrumsebene zugewiesen und hat daher Zugriff auf alle Objekte im Rechenzentrum. Wenn Sie den Zugriff einschränken möchten, weisen Sie den untergeordneten Objekten (ESX-Hosts, Datenspeicher, virtuelle Computer und Netzwerke) die Rolle **No access** (Kein Zugriff) mit der Berechtigung **Propagate to child object** (An untergeordnetes Objekt weitergeben) zu. |
| vCenter-Benutzerrolle |Failover und Failback |Weisen Sie die folgenden Berechtigungen für den vCenter-Server zu:<br/><br/>Datacenter object Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Azure_Site_Recovery (Rolle=Azure_Site_Recovery)<br/><br/>Der Benutzer wird auf Rechenzentrumsebene zugewiesen und hat daher Zugriff auf alle Objekte im Rechenzentrum.  Wenn Sie den Zugriff einschränken möchten, weisen Sie den untergeordneten Objekten (ESX-Hosts, Datenspeicher, virtuelle Computer und Netzwerke) die Rolle **No access** (Kein Zugriff) mit der Berechtigung **Propagate to child object** (An untergeordnetes Objekt weitergeben) zu. |

## <a name="third-party-software-notices-and-information"></a>Hinweise und Informationen zu Drittanbietersoftware
<!--Do Not Translate or Localize-->

The software and firmware running in the Microsoft product or service is based on or incorporates material from the projects listed below (collectively, “Third Party Code”).  Microsoft is the not original author of the Third Party Code.  The original copyright notice and license, under which Microsoft received such Third Party Code, are set forth below.

The information in Section A is regarding Third Party Code components from the projects listed below. Such licenses and information are provided for informational purposes only.  This Third Party Code is being relicensed to you by Microsoft under Microsoft's software licensing terms for the Microsoft product or service.  

The information in Section B is regarding Third Party Code components that are being made available to you by Microsoft under the original licensing terms.

The complete file may be found on the [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft reserves all rights not expressly granted herein, whether by implication, estoppel or otherwise.

## <a name="next-steps"></a>Nächste Schritte
[Erfahren Sie mehr über das Failback](site-recovery-failback-azure-to-vmware-classic.md), um Computer, die per Failover nach Azure verschoben wurden, wieder in der lokalen Umgebung auszuführen.

