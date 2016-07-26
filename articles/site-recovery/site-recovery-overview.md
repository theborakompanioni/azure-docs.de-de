<properties
	pageTitle="Was ist Site Recovery? | Microsoft Azure" 
	description="Enthält eine Übersicht über den Azure Site Recovery-Dienst, und es wird beschrieben, wie der Dienst bereitgestellt werden kann." 
	services="site-recovery" 
	documentationCenter="" 
	authors="rayne-wiselman" 
	manager="jwhit" 
	editor=""/>

<tags 
	ms.service="site-recovery" 
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="na"
	ms.workload="storage-backup-recovery" 
	ms.date="02/22/2016" 
	ms.author="raynew"/>

#  Was ist Site Recovery?

Willkommen bei Azure Site Recovery! Beginnen Sie mit diesem Artikel, um sich schnell einen Überblick über den Site Recovery-Dienst und seinen möglichen Beitrag zu Ihrer Strategie für Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR) zu verschaffen.

## Übersicht

Organisationen benötigen eine Strategie für die Geschäftskontinuität und Notfallwiederherstellung (BCDR), mit der bestimmt wird, wie Apps, Workloads und Daten bei geplanten und ungeplanten Ausfällen verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können. Bei Ihrer BCDR-Strategie sollten Sie darauf achten, dass Unternehmensdaten geschützt sind und wiederhergestellt werden können. Stellen Sie außerdem sicher, dass Workloads auch nach dem Eintreten eines Notfalls ständig verfügbar sind.

Site Recovery ist ein Azure-Dienst, der einen Beitrag zu Ihrer BCDR-Strategie leistet, indem die Replikation von lokalen physischen Servern und virtuellen Maschinen in die Cloud (Azure) oder in ein sekundäres Rechenzentrum orchestriert wird. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Weitere Informationen finden Sie unter [Was ist Site Recovery?](site-recovery-overview.md).

## Site Recovery im Azure-Portal

Azure verfügt über zwei unterschiedliche [Bereitstellungsmodelle](../resource-manager-deployment-model.md) zum Erstellen und Verwenden von Ressourcen: das Azure Resource Manager-Modell und das klassische Dienstverwaltungsmodell. Außerdem verfügt Azure über zwei Portale: das [klassische Azure-Portal](https://manage.windowsazure.com/) mit Unterstützung des klassischen Bereitstellungsmodells und das [Azure-Portal](https://portal.azure.com) mit Unterstützung für beide Bereitstellungsmodelle.

Site Recovery ist sowohl im klassischen Portal als auch im Azure-Portal verfügbar. Im klassischen Azure-Portal können Sie Site Recovery mit dem klassischen Dienstverwaltungsmodell unterstützen. Im Azure-Portal können Sie das klassische Modell oder Resource Manager-Bereitstellungen unterstützen. Weitere Informationen zur Bereitstellung mit dem Azure-Portal finden Sie [hier](site-recovery-overview.md#site-recovery-in-the-azure-portal).

Die Informationen in diesem Artikel gelten für Bereitstellungen mit dem klassischen Portal und dem Azure-Portal. Die Unterschiede sind jeweils gekennzeichnet.


## Gründe für Azure Site Recovery 

Site Recovery hat für Ihr Unternehmen folgende Vorteile:

- **Vereinfachung Ihrer BCDR-Strategie:** Site Recovery vereinfacht die Replikation, das Failover und die Wiederherstellung mehrerer Geschäftsworkloads und -Apps von einem einzelnen Standort. Site Recovery orchestriert die Replikation und das Failover, aber das Tool fängt keine Anwendungsdaten ab und besitzt auch keinerlei Informationen hierzu.
- **Bereitstellung einer flexiblen Replikation:** Mit Site Recovery können Sie Workloads replizieren, die auf virtuellen Hyper-V-Computern, virtuellen VMware-Computern und physischen Windows bzw. Linux-Servern ausgeführt werden.
- **Einfaches Failover und leichte Wiederherstellung:** Site Recovery verfügt über Testfailover, mit denen Übungen zur Notfallwiederherstellung unterstützt werden, ohne dass sich dies auf Produktionsumgebungen auswirkt. Außerdem können Sie geplante Failover ohne Datenverlust für erwartete Ausfälle oder ungeplante Failover mit minimalem Datenverlust (je nach Replikationsfrequenz) für unerwartete Notfälle ausführen. Nach dem Failover können Sie ein Failback zu Ihren primären Standorten durchführen. Site Recovery verfügt über Wiederherstellungspläne, die Skripts und Azure Automation-Arbeitsmappen enthalten können, sodass Sie das Failover und die Wiederherstellung von Anwendungen mit mehreren Ebenen anpassen können.
- **Beseitigung des sekundären Datencenters:** Sie können die Replikation zu einem sekundären lokalen Standort oder zu Azure durchführen. Bei Verwendung von Azure als Ziel für die Notfallwiederherstellung werden die Kosten und die Komplexität für die Verwaltung eines sekundären Standorts beseitigt, und replizierte Daten werden in Azure Storage gespeichert, sodass die damit verbundene Resilienz genutzt werden kann.
- **Integration in vorhandene BCDR-Technologien:** Site Recovery kann zusammen mit anderen BCDR-Features für Anwendungen eingesetzt werden. Beispielsweise können Sie Site Recovery verwenden, um das SQL Server-Back-End von geschäftlichen Workloads zu schützen, einschließlich der nativen Unterstützung für SQL Server AlwaysOn zum Verwalten des Failovers von Verfügbarkeitsgruppen.

## Was kann ich replizieren?

Hier ist eine Übersicht dazu angegeben, was Sie mit Site Recovery replizieren können.

![Lokal zu lokal](./media/site-recovery-overview/asr-overview-graphic.png)

**REPLICATE** | **REPLIKATIONSQUELLE (LOKAL)** | **REPLIKATIONSZIEL** | **ARTIKEL**
---|---|---|---
Virtuelle VMware-Computer | VMware-Server | Azure | [Weitere Informationen](site-recovery-vmware-to-azure-classic.md)
Virtuelle VMware-Computer | VMware-Server | Sekundärer VMware-Standort | [Weitere Informationen](site-recovery-vmware-to-vmware.md) 
Virtuelle Hyper-V-Computer | Hyper-V-Host in VMM-Cloud | Azure | [Weitere Informationen](site-recovery-vmm-to-azure.md) 
Virtuelle Hyper-V-Computer | Hyper-V-Host in VMM-Cloud | Sekundärer VMM-Standort | [Weitere Informationen](site-recovery-vmm-to-vmm.md)
Virtuelle Hyper-V-Computer | Hyper-V-Host in VMM-Cloud mit SAN-Speicher| Sekundärer VMM-Standort mit SAN-Speicher | [Weitere Informationen](site-recovery-vmm-san.md)
Virtuelle Hyper-V-Computer | Hyper-V-Host (kein VMM) | Azure | [Weitere Informationen](site-recovery-hyper-v-site-to-azure.md)
Physische Windows-/Linux-Server | Physischer Server | Azure | [Weitere Informationen](site-recovery-vmware-to-azure-classic.md)
Workloads auf physischen Windows-/Linux-Servern | Physischer Server | Sekundäres Datencenter | [Weitere Informationen](site-recovery-vmware-to-vmware.md) 


## Welche Workloads können geschützt werden?

Site Recovery kann anwendungsorientierte BCDR-Vorgänge unterstützen, damit Workloads und Apps auch bei Ausfällen auf konsistente Weise ausgeführt werden können. Site Recovery bietet Folgendes:

- **Anwendungskonsistente Momentaufnahmen:** Replikation mit anwendungskonsistenten Momentaufnahmen für Single- oder N-Tier-Apps.
- **Nahezu synchrone Replikation:** Replikationsfrequenz von bis zu lediglich 30 Sekunden für Hyper-V und fortlaufende Replikation für VMware.
- **Integration in SQL Server AlwaysOn:** Sie können das Failover von Verfügbarkeitsgruppen in Site Recovery-Wiederherstellungsplänen verwalten.
- **Flexible Wiederherstellungspläne:** Sie können Wiederherstellungspläne mit externen Skripts, manuellen Aktionen und Azure Automation-Runbooks erstellen und anpassen. Diese Pläne ermöglichen es Ihnen, einen gesamten Anwendungsstapel mit nur einem Klick wiederherzustellen.
- **Automation-Bibliothek:** Eine umfassende Azure Automation-Bibliothek bietet produktionsbereite, anwendungsspezifische Skripts, die heruntergeladen und in Site Recovery integriert werden können.
- **Einfache Netzwerkverwaltung:** Die erweiterte Netzwerkverwaltung in Site Recovery und Azure vereinfacht Anforderungen für Anwendungsnetzwerke, z.B. das Reservieren von IP-Adressen, Konfigurieren von Lastenausgleichsmodulen und Integrieren von Azure Traffic Manager für effiziente Netzwerkwechsel.


## Nächste Schritte

- Unter [Welche Workloads kann Site Recovery schützen?](site-recovery-workload.md) finden Sie weitere Informationen.
- Weitere Informationen zur Site Recovery-Architektur finden Sie unter [Wie funktioniert Site Recovery?](site-recovery-components.md).
 

<!---HONumber=AcomDC_0720_2016-->