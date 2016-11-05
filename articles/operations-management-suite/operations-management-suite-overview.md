---
title: Operations Management Suite (OMS) – Übersicht | Microsoft Docs
description: Microsoft Operations Management Suite (OMS) ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Ihnen das Verwalten und Schützen Ihrer Infrastruktur lokal und in der Cloud erleichtert.  Dieser Artikel enthält Informationen zu den verschiedenen Diensten von OMS und Links zu Seiten mit ausführlicheren Informationen.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn

ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/04/2016
ms.author: bwren

---
# <a name="operations-management-suite-(oms)-overview"></a>Operations Management Suite (OMS) – Übersicht
Microsoft Operations Management Suite (OMS) ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Ihnen das Verwalten und Schützen Ihrer Infrastruktur lokal und in der Cloud erleichtert.  Da OMS als cloudbasierter Dienst implementiert wird, ist die Lösung mit minimalen Investitionen in Infrastrukturdienste schnell betriebsbereit.  Neue Features werden automatisch bereitgestellt, sodass Sie Kosten für die laufende Wartung und Upgrades sparen.

Zusätzlich zur Bereitstellung wertvoller Dienste kann OMS in System Center-Komponenten wie System Center Operations Manager integriert werden, um Ihre bestehenden Investitionen in die Cloud zu erweitern.  Durch die Kombination von System Center und OMS können Sie vollständig vom Hybrid-Management profitieren.

Die folgenden Abschnitte enthalten eine allgemeine Beschreibung der verschiedenen Bereiche von OMS und der Dienste, mit denen diese implementiert werden.  Unter „OMS-Architektur“ finden Sie eine Übersicht über die verschiedenen OMS-Komponenten. In der ausführlichen Dokumentation zu den einzelnen Komponenten können Sie Ihre Kenntnisse vertiefen.

## <a name="![insight-and-analytics](media/operations-management-suite-overview/icon-insight-analytics.png)-insight-and-analytics"></a>![Insight & Analytics](media/operations-management-suite-overview/icon-insight-analytics.png) Insight & Analytics
Mit [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) können Sie die von Betriebssystemen und Anwendungen generierten Protokoll- und Leistungsdaten erfassen, korrelieren, durchsuchen und bearbeiten. Mithilfe integrierter Suchfunktionen und benutzerdefinierter Dashboards erhalten Sie Einblicke in Betriebsabläufe in Echtzeit, sodass Sie Millionen von Datensätzen für all Ihre Workloads und Server unabhängig vom physischen Standort analysieren können.

Sie können Log Analytics problemlos Lösungen hinzufügen, die die zu erfassenden Daten und die entsprechende Logik für deren Analyse definieren.  Lösungen können zusätzliche Funktionen umfassen, die mit minimaler bzw. ohne Konfiguration automatisch für Agents bereitgestellt werden.  Neben der Verwendung der von einzelnen Lösungen bereitgestellten Analysetools können Sie benutzerdefinierte Suchvorgänge für das gesamte Dataset durchführen, um Daten zwischen Systemen und Anwendungen zu korrelieren.  

## <a name="![automation-&-control](media/operations-management-suite-overview/icon-automation-control.png)-automation-&-control"></a>![Automation & Control](media/operations-management-suite-overview/icon-automation-control.png) Automation & Control
Azure Automation automatisiert Verwaltungsprozesse mithilfe von [Runbooks](../automation/automation-runbook-types.md), die auf PowerShell basieren und in der Azure-Cloud ausgeführt werden.  Runbooks können auf alle Produkte oder Dienste zugreifen, die mit PowerShell verwaltet werden können, darunter Ressourcen in anderen Clouds wie Amazon Web Services (AWS).  Runbooks können auch auf einem Server in Ihrem lokalen Rechenzentrum ausgeführt werden, um lokale Ressourcen zu verwalten.

Azure Automation stellt die Konfigurationsverwaltung mit [PowerShell DSC](../automation/automation-dsc-overview.md) bereit.  Sie können DSC-Ressourcen erstellen und verwalten, die in Azure gehostet werden, und sie auf Cloud- und lokale Systeme anwenden, um ihre Konfiguration zu definieren und automatisch zu erzwingen.

## <a name="![protection-and-recovery](media/operations-management-suite-overview/icon-protection-recovery.png)-protection-and-disaster-recovery"></a>![Schutz und Wiederherstellung](media/operations-management-suite-overview/icon-protection-recovery.png) Schutz und Notfallwiederherstellung
[Azure Backup](http://azure.microsoft.com/documentation/services/backup) schützt Ihre Anwendungsdaten und bewahrt sie für Jahre ohne Kapitaleinsatz und mit minimalen Betriebskosten auf.  Mit diesem Dienst können neben Anwendungsworkloads wie SQL Server und SharePoint Daten von physischen und virtuellen Windows-Servern gesichert werden.  Zudem kann er von System Center Data Protection Manager (DPM) verwendet werden, um geschützte Daten zur Redundanz und langfristigen Speicherung in Azure zu replizieren.

[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung, indem Replikation, Failover und Wiederherstellung von lokalen virtuellen Hyper-V-Computern, virtuellen VMware-Computern und physischen Windows- oder Linux-Servern orchestriert werden. Sie können Computer in einem sekundären Rechenzentrum replizieren oder das Rechenzentrum erweitern, indem Sie sie in Azure replizieren. Site Recovery bietet zudem einen einfachen Failover sowie die einfache Wiederherstellung für Workloads. Der Dienst kann in Mechanismen zur Notfallwiederherstellung (z.B. SQL Server AlwaysOn) integriert werden und bietet Wiederherstellungspläne für den einfachen Failover von Workloads, die auf mehreren Computern mehrstufig verteilt sind.

## <a name="![oms-security-and-compliance](media/operations-management-suite-overview/icon-security-compliance.png)-security-and-compliance"></a>![OMS: Sicherheit und Compliance](media/operations-management-suite-overview/icon-security-compliance.png)  Sicherheit und Compliance
Durch Sicherheit und Compliance können Sie Sicherheitsrisiken für Ihre Infrastruktur identifizieren, bewerten und mindern.  Diese Features von OMS werden über mehrere Lösungen in Log Analytics implementiert, die Protokolldaten und die Konfiguration von Agentsystemen analysieren, damit Sie die kontinuierliche Sicherheit Ihrer Umgebung gewährleisten können.

* Mit der [Sicherheits- und Überwachungslösung](oms-security-getting-started.md) werden Sicherheitsereignisse auf verwalteten Systemen gesammelt und analysiert, um verdächtige Aktivitäten zu ermitteln.
* Mit der [Lösung für Antischadsoftware](../log-analytics/log-analytics-malware.md) wird der Status des Antischadsoftware-Schutzes auf verwalteten Systemen gemeldet.  
* Mit der Lösung für Systemupdates wird eine Analyse der Sicherheitsupdates und anderen Updates auf Ihren verwalteten Systemen durchgeführt, sodass Sie problemlos Systeme ermitteln können, für die Patches erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics)
* Weitere Informationen zu [Azure Automation](../automation/automation-intro.md)
* Weitere Informationen zu [Azure Backup](http://azure.microsoft.com/documentation/services/backup)
* Weitere Informationen zu [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery)

<!--HONumber=Oct16_HO2-->


