---
title: "Bewährte Methoden für Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel werden bewährte Methoden für die Bereitstellung von Azure Site Recovery beschrieben."
services: site-recovery
documentationCenter: 
author: rayne-wiselman"
manager: cfreeman
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-support-matrix-to-azure
ms.translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: a53e2a09fb2aabe96dd5347ea7509815d92bfdb8
ms.contentlocale: de-de
ms.lasthandoff: 03/15/2017

---

# <a name="get-ready-to-deploy-azure-site-recovery"></a>Vorbereitung auf die Bereitstellung von Azure Site Recovery

In diesem Artikel wird beschrieben, wie Sie die Bereitstellung von Azure Site Recovery vorbereiten.

## <a name="protecting-hyper-v-virtual-machines"></a>Schutz für virtuelle Hyper-V-Maschinen

Für den Schutz von virtuellen Hyper-V-Maschinen stehen mehrere Bereitstellungsoptionen zur Verfügung. Sie können lokale Hyper-VMs in Azure oder einem sekundären Datencenter replizieren. Bei jeder Bereitstellung sind verschiedene Anforderungen zu beachten.

**Anforderung** | **Replizieren in Azure (mit VMM)** | **Replizieren von Hyper-V-VMs in Azure (ohne VMM)** | **Replizieren von Hyper-V-VMs an einem sekundären Standort (mit VMM)** | **Details**
---|---|---|---|---
**VMM** | VMM unter System Center 2012 R2 <br/><br/>Mindestens eine VMM-Cloud, die mindestens eine VMM-Hostgruppe enthält | NA | VMM-Server am primären und am sekundären Standort mit mindestens System Center 2012 SP1 und aktuellen Updates <br/><br/> Mindestens eine Cloud auf jedem VMM-Server Für die Clouds sollte das Hyper-V-Kapazitätsprofil eingestellt sein.<br/><br/> Die Quellcloud muss mindestens eine VMM-Hostgruppe enthalten. | Optional. System Center VMM muss zum Replizieren virtueller Hyper-V-Maschinen in Azure nicht bereitgestellt werden. Falls Sie die Lösung dennoch bereitstellen, müssen Sie ihre richtige Einrichtung sicherstellen. Dazu gehören u. a. die Ausführung der richtigen VMM-Version und die Einrichtung von Clouds.
**Hyper-V** | Mindestens ein Hyper-V-Hostserver am lokalen Standort mit Windows Server 2012 R2 oder höher | Mindestens ein Hyper-V-Server am Quell- und Zielstandort mit mindestens Windows Server 2012 mit den neuesten Updates und Internetverbindung.<br/><br/> Die Hyper-V-Server müssen sich in einer Hostgruppe in einer VMM-Cloud befinden. | Mindestens ein Hyper-V-Server am Quell- und Zielstandort mit mindestens Windows Server 2012 mit den neuesten Updates und Internetverbindung.<br/><br/> Die Hyper-V-Server müssen sich in einer Hostgruppe in einer VMM-Cloud befinden. |
**Virtuelle Computer** | Mindestens eine VM auf dem Hyper-V-Quellhostserver | Mindestens ein VM auf dem Hyper-V-Hostserver in der VMM-Quellcloud | Mindestens ein VM auf dem Hyper-V-Hostserver in der VMM-Quellcloud |  VMs, für die eine Replikation in Azure ausgeführt wird, müssen die Anforderungen für virtuelle Azure-Computer erfüllen.
**Azure-Konto** | Sie benötigen ein Azure-Konto und ein Abonnement für den Site Recovery-Dienst. | Sie benötigen ein Azure-Konto und ein Abonnement für den Site Recovery-Dienst. | NA | Beginnen Sie mit einer kostenlosen Testversion, falls Sie kein Konto haben.
**Azure-Speicher** | Sie benötigen ein Abonnement für ein Azure-Speicherkonto, für das die Georeplikation aktiviert wurde. | Sie benötigen ein Abonnement für ein Azure-Speicherkonto, für das die Georeplikation aktiviert wurde. | NA | Das Konto muss sich in der gleichen Region befinden wie der Azure Site Recovery-Tresor und dem gleichen Abonnement zugeordnet sein.
**Netzwerk** | Richten Sie eine Netzwerkzuordnung ein, um sicherzustellen, dass alle Maschinen, die im gleichen Azure-Netzwerk ein Failover ausführen, eine Verbindung miteinander herstellen, und zwar unabhängig vom Wiederherstellungsplan, zu dem sie gehören. Wenn im Azure-Zielnetzwerk ein Netzwerkgateway konfiguriert wurde, können die virtuellen Maschinen auch eine Verbindung mit anderen lokalen virtuellen Maschinen herstellen. Wenn Sie keine Netzwerkzuordnung einrichten, können nur Computer Verbindungen miteinander herstellen, für die das Failover in demselben Wiederherstellungsplan festgelegt ist. | NA |  <br/><br/>Richten Sie eine Netzwerkzuordnung ein, um sicherzustellen, dass virtuelle Maschinen nach einem Failover eine Verbindung mit den entsprechenden Netzwerken herstellen können und replizierte virtuelle Maschinen optimal auf Hyper-V-Hostservern platziert werden. Wenn Sie die Netzwerkzuordnung nicht konfigurieren, werden für replizierte Maschinen nach einem Failover keine Verbindungen mit einem VM-Netzwerk hergestellt. |  Zum Einrichten einer Netzwerkzuordnung mit VMM müssen logische Netzwerke und VM-Netzwerke in VMM ordnungsgemäß konfiguriert werden.
**Anbieter und Agents** | Während der Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf VMM-Servern. Auf Hyper-V-Servern in VMM-Clouds installieren Sie den Azure Recovery Services-Agent. | Während der Bereitstellung installieren Sie den Azure Site Recovery-Anbieter und den Azure Recovery Services-Agent auf dem Hyper-V-Hostserver oder -Cluster.| Während der Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf VMM-Servern. Auf Hyper-V-Servern in VMM-Clouds installieren Sie den Azure Recovery Services-Agent. | Anbieter und Agents stellen über das Internet eine verschlüsselte HTTPS-Verbindung mit Site Recovery her. Sie müssen für die Anbieterverbindung keine Firewallausnahmen hinzufügen und keinen speziellen Proxy erstellen.
**Internetkonnektivität** | Nur die VMM-Server benötigen eine Internetverbindung. | Nur die Hyper-V-Hostserver benötigen eine Internetverbindung. | Nur VMM-Server benötigen eine Internetverbindung. | Auf virtuellen Computern muss nichts installiert werden, und sie müssen nicht direkt mit dem Internet verbunden werden.



## <a name="protect-vmware-virtual-machines-or-physical-servers"></a>Schützen virtueller VMware-Maschinen oder physischer Server

Für den Schutz virtueller VMware-Maschinen oder physischer Windows-/Linux-Server stehen mehrere Bereitstellungsoptionen zur Verfügung. Sie können sie in Azure oder einem sekundären Datencenter replizieren. Bei jeder Bereitstellung sind verschiedene Anforderungen zu beachten.

**Anforderung** | **Replizieren von VMware-VMs/physischen Servern in Azure** | * **Replizieren von VMware-VMs/physischen Servern an einem sekundären Standort**  
---|---|---
**Primärer Standort** | **Prozessserver**: ein dedizierter Windows-Server (physisch oder virtuell) | **Prozessserver**: ein dedizierter Windows-Server (physischer oder virtueller VMware-Computer)<br/><br/>  
**Lokaler sekundärer Standort** | NA | **Konfigurationsserver**: ein dedizierter Windows-Server (physisch oder virtuell) <br/><br/> **Masterzielserver**: ein dedizierter Server (physisch oder virtuell) Konfiguration mit Windows zum Schutz von Windows-Maschinen bzw. mit Linux zum Schutz von Linux-Maschinen
**Azure** | **Abonnement**: Sie benötigen ein Abonnement für den Site Recovery-Dienst. <br/><br/> **Speicherkonto**: Sie benötigen ein Speicherkonto, für das die Georeplikation aktiviert ist. Das Konto muss sich in der gleichen Region befinden wie der Site Recovery-Tresor und dem gleichen Abonnement zugeordnet sein. <br/><br/> **Konfigurationsserver**: Sie müssen den Konfigurationsserver als virtuellen Azure-Computer einrichten. <br/><br/> **Masterzielserver**: Sie müssen den Masterzielserver als virtuellen Azure-Computer einrichten. <br/><br/> Konfiguration mit Windows zum Schutz von Windows-Maschinen bzw. mit Linux zum Schutz von Linux-Maschinen<br/><br/> **Virtuelles Azure-Netzwerk**: Sie benötigen ein virtuelles Azure-Netzwerk, in dem der Konfigurationsserver und der Masterzielserver bereitgestellt werden. Es muss in demselben Abonnement und in derselben Region enthalten sein wie der Azure Site Recovery-Tresor. | NA  
**Virtuelle Computer/physische Server** | Mindestens ein virtueller VMware-Computer oder ein physischer Windows-/Linux-Server.<br/><br/>Während der Bereitstellung wird der Mobilitätsdienst auf jedem Computer installiert.| Mindestens ein VMware-VM oder physischer Windows-/Linux-Server<br/><br/> Während der Bereitstellung wird der vereinheitlichte Agent auf jeder Maschine installiert.




## <a name="azure-virtual-machine-requirements"></a>Anforderungen für virtuelle Azure-Maschinen

Sie können Site Recovery zum Replizieren virtueller Maschinen und physischer Server bereitstellen, auf denen ein von Azure unterstütztes Betriebssystem ausgeführt wird. Dies gilt für die meisten Versionen von Windows und Linux. Sie müssen sicherstellen, dass lokale virtuelle Maschinen, die Sie schützen möchten, den Azure-Anforderungen entsprechen.


## <a name="optimizing-your-deployment"></a>Optimieren der Bereitstellung

Mit den folgenden Tipps können Sie Ihre Bereitstellung optimieren und skalieren.

- **Volumegröße des Betriebssystems**: Wenn Sie einen virtuellen Computer zu Azure replizieren, muss das Volume des Betriebssystems kleiner als 1 TB sein. Falls Sie mehr als ein Volume nutzen, können Sie diese manuell auf einen anderen Datenträger verschieben, bevor Sie mit der Bereitstellung beginnen.
- **Größe des Datenträgers für Daten**: Wenn Sie zu Azure replizieren, können Sie auf einem virtuellen Computer bis zu 32 Datenträger für Daten nutzen, die jeweils maximal 1 TB groß sind. Für einen virtuellen Computer mit ~32 TB können Sie die Replikation und das Failover effektiv durchführen.
- **Einschränkungen des Wiederherstellungsplans**: Für Site Recovery ist eine Skalierung auf Tausende von virtuellen Computern möglich. Wiederherstellungspläne wurden als Modell für Anwendungen entwickelt, für die das Failover zusammen erfolgen soll. Die Anzahl von Computern in einem Wiederherstellungsplan wird daher auf 50 beschränkt.
- **Einschränkungen für Azure-Dienste**: Jedes Azure-Abonnement weist eine Reihe von standardmäßigen Einschränkungen in Bezug auf Kerne, Clouddienste usw. auf. Wir empfehlen Ihnen, ein Testfailover auszuführen, um die Verfügbarkeit von Ressourcen in Ihrem Abonnement zu überprüfen. Sie können diese Beschränkungen über den Azure-Support ändern.
- **Kapazitätsplanung**: Planen Sie die Skalierung und Leistung.
- **Replikationsbandbreite**: Beachten Sie Folgendes, falls die Replikationsbandbreite knapp ist:
    - **ExpressRoute**: Für Site Recovery können Azure ExpressRoute und WAN-Optimierer wie Riverbed verwendet werden.
    - **Replikationsdatenverkehr**: Site Recovery führt eine intelligente anfängliche Replikation durch, bei der nur Datenblöcke verwendet werden, und nicht die gesamte VHD. Während der laufenden Replikation werden nur Änderungen repliziert.
    - **Netzwerkdatenverkehr**: Sie können Netzwerkdatenverkehr steuern, der für die Replikation verwendet wird, indem Sie Windows-Dienstqualität mit einer Richtlinie nutzen, die auf der IP-Zieladresse und dem Port basiert.  Falls Sie die Replikation zu Azure Site Recovery mit dem Azure Backup-Agent durchführen, können Sie die Drosselung für den Agent konfigurieren.
- **RTO**: Wenn Sie die Recovery Time Objective (RTO) messen möchten, die mit Site Recovery zu erwarten ist, sollten Sie ein Testfailover ausführen und die Site Recovery-Aufträge anzeigen, um zu analysieren, wie lange die Durchführung der Vorgänge dauert. Falls Sie das Failover zu Azure nutzen, erzielen Sie den besten RTO-Wert, wenn Sie alle manuellen Aktionen automatisieren. Integrieren Sie hierfür die Azure-Automatisierung und Wiederherstellungspläne.
- **RPO**: Site Recovery unterstützt beim Replizieren zu Azure eine nahezu synchrone Recovery Point Objective (RPO). Dies setzt ausreichende Bandbreite zwischen Ihrem Rechenzentrum und Azure voraus.

