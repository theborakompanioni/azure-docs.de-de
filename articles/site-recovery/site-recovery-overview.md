---
title: Was ist Site Recovery? | Microsoft Docs
description: Enthält eine Übersicht über den Azure Site Recovery-Dienst und eine Zusammenfassung der Bereitstellungsszenarien.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: cfreeman
editor: ''

ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/13/2016
ms.author: raynew

---
# <a name="what-is-site-recovery?"></a>Was ist Site Recovery?
Willkommen bei Azure Site Recovery! Dieser Artikel enthält eine kurze Übersicht über den Site Recovery-Dienst und die Vorteile, die sich für Ihr Unternehmen damit ergeben.

Ihre Organisation benötigt eine Strategie für die Bereiche Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR), mit der bestimmt wird, wie Apps, Workloads und Daten bei geplanten und ungeplanten Ausfallzeiten geschützt und verfügbar bleiben und so schnell wie möglich die normalen Arbeitsbedingungen wiederhergestellt werden können. Site Recovery ist ein Azure-Dienst als Beitrag zu dieser Strategie.

Site Recovery orchestriert die Replikation von Workloads für die Ausführung von lokalen physischen Servern und virtuellen Computern. Sie können Server und VMs aus einem primären Datencenter in die Cloud (Azure) oder in einem sekundären Datencenter replizieren. Wenn es am Hauptstandort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Erreichbarkeit und Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch.

## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery im Azure-Portal
Azure verfügt über zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen. Dies sind das Azure Resource Manager-Modell und das klassische Dienstverwaltungsmodell. Außerdem verfügt Azure über zwei Portale: das [klassische Azure-Portal](https://manage.windowsazure.com/) mit Unterstützung des klassischen Bereitstellungsmodells und das [Azure-Portal](https://portal.azure.com) mit Unterstützung für das klassische und das Resource Manager-Bereitstellungsmodell.

* Site Recovery ist sowohl im klassischen Portal als auch im Azure-Portal verfügbar.
* Im klassischen Azure-Portal können Sie Site Recovery mit dem klassischen Dienstverwaltungsmodell unterstützen.
* Im Azure-Portal können Sie das klassische Modell oder Resource Manager-Bereitstellungen unterstützen. 

Die Informationen in diesem Artikel gelten für Bereitstellungen mit dem klassischen Portal und dem Azure-Portal. Die Unterschiede sind jeweils gekennzeichnet.

## <a name="why-deploy-site-recovery?"></a>Gründe für die Bereitstellung von Site Recovery
Site Recovery hat für Ihr Unternehmen folgende Vorteile:

* **Vereinfachung von BCDR:** Sie können die Replikation, das Failover und die Wiederherstellung von mehreren Workloads an einem einzelnen Standort im Azure-Portal durchführen. Site Recovery orchestriert die Replikation und das Failover, aber das Tool fängt keine Anwendungsdaten ab und besitzt auch keinerlei Informationen hierzu.
* **Bereitstellung einer flexiblen Replikation:** Mit Site Recovery können Sie Workloads replizieren, die auf unterstützten virtuellen Hyper-V-Computern, virtuellen VMware-Computern und physischen Windows bzw. Linux-Servern ausgeführt werden.
* **Einfache Replikationstests:** Site Recovery verfügt über Testfailover, mit denen Übungen zur Notfallwiederherstellung unterstützt werden, ohne dass sich dies auf Produktionsumgebungen auswirkt.
* **Failover und Wiederherstellung:** Sie können geplante Failover ohne Datenverlust für erwartete Ausfälle oder ungeplante Failover mit minimalem Datenverlust (je nach Replikationsfrequenz) für unerwartete Notfälle ausführen. Nach dem Failover können Sie ein Failback zu Ihren primären Standorten durchführen. Site Recovery verfügt über Wiederherstellungspläne, die Skripts und Azure Automation-Arbeitsmappen enthalten können, sodass Sie das Failover und die Wiederherstellung von Anwendungen mit mehreren Ebenen anpassen können.
* **Beseitigung eines sekundären Datencenters:** Sie können Workloads in Azure replizieren, anstatt an einem sekundären Standort. So entfallen die Kosten und die Komplexität, die mit der Verwaltung eines sekundären Datencenters verbunden sind. Replizierte Daten werden in Azure Storage gespeichert, was für Ausfallsicherheit sorgt. VMs werden mit den replizierten Daten erstellt, wenn das Failover durchgeführt wird.
* **Integration mit vorhandenen BCDR-Technologien:** Site Recovery kann mit anderen BCDR-Features integriert werden. Beispielsweise können Sie Site Recovery verwenden, um das SQL Server-Back-End von geschäftlichen Workloads zu schützen, einschließlich der nativen Unterstützung für SQL Server Always On zum Verwalten des Failovers von Verfügbarkeitsgruppen.

## <a name="what-can-i-replicate?"></a>Was kann ich replizieren?
Hier ist eine Übersicht dazu angegeben, was Sie mit Site Recovery replizieren können.

![Lokal zu lokal](./media/site-recovery-overview/asr-overview-graphic.png)

| **REPLICATE** | **REPLIKATIONSZIEL** |
| --- | --- |
| Workloads auf lokalen VMware-VMs |[Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Sekundärer Standort](site-recovery-vmware-to-vmware.md) |
| Workloads auf lokalen Hyper-V-VMs, die in VMM-Clouds verwaltet werden |[Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Sekundärer Standort](site-recovery-vmm-to-vmm.md) |
| Workloads auf lokalen Hyper-V-VMs, die in VMM-Clouds verwaltet werden, mit SAN-Speicher |[Sekundärer Standort](site-recovery-vmm-san.md) |
| Workloads auf lokalen Hyper-V-VMs ohne VMM |[Azure](site-recovery-hyper-v-site-to-azure.md) |
| Workloads auf lokalen physischen Windows-/Linux-Servern |[Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Sekundärer Standort](site-recovery-vmware-to-vmware.md) |

## <a name="what-workloads-can-i-protect?"></a>Welche Workloads können geschützt werden?
Site Recovery ermöglicht anwendungsorientierte BCDR-Vorgänge, damit Workloads und Apps auch bei Ausfällen auf konsistente Weise ausgeführt werden können. Site Recovery bietet Folgendes:

* **Anwendungskonsistente Momentaufnahmen:** Computer werden mit anwendungskonsistenten Momentaufnahmen für Single- oder N-Tier-Apps repliziert. Zusätzlich zur Erfassung von Datenträgerdaten werden mit anwendungskonsistenten Momentaufnahmen alle Daten im Arbeitsspeicher und alle Transaktionen des Prozesses erfasst.
* **Nahezu synchrone Replikation:** Site Recovery ermöglicht eine Replikationsfrequenz von bis zu lediglich 30 Sekunden für Hyper-V und fortlaufende Replikation für VMware.
* **Flexible Wiederherstellungspläne:** Sie können Wiederherstellungspläne mit externen Skripts und manuellen Aktionen erstellen und anpassen. Dank der Integration mit Azure Automation-Runbooks können Sie einen gesamten Anwendungsstapel per Klick wiederherstellen.
* **Integration in SQL Server AlwaysOn:**Sie können das Failover von Verfügbarkeitsgruppen in Site Recovery-Wiederherstellungsplänen verwalten.
* **Automation-Bibliothek:**Eine umfassende Azure Automation-Bibliothek bietet produktionsbereite, anwendungsspezifische Skripts, die heruntergeladen und in Site Recovery integriert werden können.
* **Einfache Netzwerkverwaltung:** Die erweiterte Netzwerkverwaltung in Site Recovery und Azure vereinfacht Anforderungen für Anwendungsnetzwerke, z.B. das Reservieren von IP-Adressen, Konfigurieren von Lastenausgleichsmodulen und Integrieren von Azure Traffic Manager für effiziente Netzwerkwechsel.

## <a name="next-steps"></a>Nächste Schritte
* Unter [Welche Workloads kann Site Recovery schützen?](site-recovery-workload.md)
* Weitere Informationen zur Site Recovery-Architektur finden Sie unter [Wie funktioniert Site Recovery?](site-recovery-components.md)

<!--HONumber=Oct16_HO2-->


