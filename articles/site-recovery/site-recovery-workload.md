---
title: "Welche Workloads können mit Azure Site Recovery geschützt werden?"
description: "Azure Site Recovery schützt Ihre Workloads und Anwendungen durch Koordinieren von Replikation, Failover und Wiederherstellung lokaler virtueller Computer und physischer Server in Azure oder an einem sekundären lokalen Standort."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: 4953948f-26c0-4699-8fe7-59d3bfc1d3da
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/08/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 6037c1ea1655aa027a0933b1eea5f864103cfd5b
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# Welche Workloads können mit Azure Site Recovery geschützt werden?
<a id="what-workloads-can-you-protect-with-azure-site-recovery" class="xliff"></a>
In diesem Artikel werden Workloads und Anwendungen beschrieben, die Sie mit dem Azure Site Recovery-Dienst replizieren können.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

## Übersicht
<a id="overview" class="xliff"></a>
Unternehmen benötigen eine Strategie für die Bereiche Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR), damit Workloads und Daten bei geplanten und ungeplanten Ausfallzeiten gesichert bleiben und die regulären Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können.

Site Recovery ist ein Azure-Dienst als Beitrag zu Ihrer BCDR-Strategie. Mithilfe von Site Recovery können Sie anwendungsorientierte Replikation in der Cloud oder an einem sekundären Standort bereitstellen. Unabhängig davon, ob Ihre Apps Windows- oder Linux-basiert sind, auf physischen Servern ausgeführt werden oder virtuelle VMware- oder Hyper-V-Computer sind, gilt Folgendes: Sie können Site Recovery verwenden, um die Replikation zu orchestrieren, Tests für die Notfallwiederherstellung durchzuführen und Failover und Failbacks auszuführen.

Site Recovery ist in Microsoft-Anwendungen wie SharePoint, Exchange, Dynamics, SQL Server und Active Directory integriert. Microsoft arbeitet außerdem eng mit führenden Herstellern wie Oracle, SAP, IBM und Red Hat zusammen. Sie können Replikationslösungen für jede App speziell anpassen.

## Gründe für die Verwendung von Site Recovery für die Anwendungsreplikation
<a id="why-use-site-recovery-for-application-replication" class="xliff"></a>
Site Recovery trägt wie folgt zum Schutz und zur Wiederherstellung auf Anwendungsebene bei:

* App-unabhängige Bereitstellung von Replikation für alle Workloads, die auf einem unterstützten Computer ausgeführt werden
* Nahezu synchrone Replikation mit geringen RPOs von bis zu 30 Sekunden, um die Anforderungen der meisten kritischen Apps von Unternehmen zu erfüllen
* Anwendungskonsistente Momentaufnahmen für Anwendungen mit einer oder mehreren Ebenen
* Integration mit SQL Server AlwaysOn und Partnerschaft mit anderen Replikationstechnologien auf Anwendungsebene, einschließlich Active Directory-Replikation, SQL AlwaysOn, Exchange-Datenbankverfügbarkeitsgruppen und Oracle Data Guard.
* Flexible Wiederherstellungspläne, die die Wiederherstellung des gesamten Anwendungsstapels mit einem einzigen Mausklick und das Einfügen von externen Skripts und manuellen Aktionen in den Plan ermöglichen.
* Erweiterte Netzwerkverwaltung in Site Recovery und Azure, um die App-Netzwerkanforderungen zu vereinfachen, z. B. das Reservieren von IP-Adressen, Konfigurieren des Lastenausgleichs und Integrieren in Azure Traffic Manager, um Netzwerk-Switchovers mit niedrigem RTO zu erzielen.
* Eine umfassende Automatisierungsbibliothek bietet produktionsbereite, anwendungsspezifische Skripts, die heruntergeladen und in Wiederherstellungspläne integriert werden können.

## Übersicht über Workloads
<a id="workload-summary" class="xliff"></a>
Mit Site Recovery können alle Apps repliziert werden, die auf einem unterstützten Computer ausgeführt werden. Außerdem haben wir mit Produktteams zusammengearbeitet, um zusätzliche App-spezifische Tests durchzuführen.

| **Workload** | **Replizieren von Hyper-V-VMs an einem sekundären Standort** | **Replizieren von Hyper-V-VMs in Azure** | **Replizieren von VMware-VMs an einem sekundären Standort** | **Replizieren von VMware-VMs in Azure** |
| --- | --- | --- | --- | --- |
| Active Directory, DNS |J |J |J |J |
| Web-Apps (IIS, SQL) |J |J |J |J |
| System Center Operations Manager |J |J |J |J |
| SharePoint |J |J |J |J |
| SAP<br/><br/>Replizieren eines SAP-Standorts zu Azure (kein Cluster) |J (getestet von Microsoft) |J (getestet von Microsoft) |J (getestet von Microsoft) |J (getestet von Microsoft) |
| Exchange (kein DAG) |J |J |J |J |
| Remotedesktop/VDI |J |J |J |N/V |
| Linux (Betriebssystem und Apps) |J (getestet von Microsoft) |J (getestet von Microsoft) |J (getestet von Microsoft) |J (getestet von Microsoft) |
| Dynamics AX |J |J |J |J |
| Dynamics CRM |J |In Kürze verfügbar |J |In Kürze verfügbar |
| Oracle |J (getestet von Microsoft) |J (getestet von Microsoft) |J (getestet von Microsoft) |J (getestet von Microsoft) |
| Windows-Dateiserver |J |J |J |J |
| Citrix XenApp und XenDesktop |N/V |J |N/V |J |

## Replizieren von Active Directory und DNS
<a id="replicate-active-directory-and-dns" class="xliff"></a>
Eine Infrastruktur mit Active Directory und DNS ist für die meisten Unternehmens-Apps wichtig. Während der Notfallwiederherstellung müssen Sie diese Infrastrukturkomponenten schützen und wiederherstellen, bevor Sie Ihre Workloads und Apps wiederherstellen.

Sie können Site Recovery verwenden, um für Active Directory und DNS einen vollständigen automatisierten Notfallwiederherstellungsplan zu erstellen. Wenn Sie beispielsweise für SharePoint und SAP ein Failover von einem primären auf einen sekundären Standort durchführen möchten, können Sie einen Wiederherstellungsplan einrichten, bei dem das Failover zuerst für Active Directory erfolgt. Anschließend erstellen Sie einen weiteren App-spezifischen Wiederherstellungsplan, um das Failover für die anderen Apps durchzuführen, die auf Active Directory basieren.

[Erfahren Sie mehr](site-recovery-active-directory.md) zum Schutz von Active Directory und DNS.

## Schützen von SQL Server
<a id="protect-sql-server" class="xliff"></a>
SQL Server stellt die Grundlage für Datendienste vieler Unternehmens-Apps in einem lokalen Rechenzentrum dar.  Site Recovery kann zusammen mit SQL Server-Technologie für hohe Verfügbarkeit und Notfallwiederherstellung verwendet werden, um Unternehmens-Apps mit mehreren Ebenen zu schützen, für die SQL Server verwendet wird. Site Recovery bietet Folgendes:

* Eine einfache und kostengünstige Notfallwiederherstellungslösung für SQL Server. Replikation mehrerer Versionen und Editionen von eigenständigen SQL Server-Servern- und -Clustern zu Azure oder einem sekundären Standort.  
* Integration in SQL AlwaysOn-Verfügbarkeitsgruppen zum Verwalten von Failover- und Failbackprozessen mit Azure Site Recovery-Wiederherstellungsplänen.
* Umfassende Wiederherstellungspläne für alle Ebenen einer Anwendung, einschließlich SQL Server-Datenbanken.
* Skalierung von SQL Server bei Lastspitzen mit Site Recovery, indem in Azure größere virtuelle IaaS-Computer verwendet werden.
* Einfaches Testen der SQL Server-Notfallwiederherstellung. Sie können Testfailover ausführen, um Daten zu analysieren und die Compliance zu prüfen, ohne dass sich dies auf die Produktionsumgebung auswirkt.

[Erfahren Sie mehr](site-recovery-sql.md) über das Schützen von SQL Server.

## Schützen von SharePoint
<a id="protect-sharepoint" class="xliff"></a>
Mit Azure Site Recovery können SharePoint-Bereitstellungen wie folgt geschützt werden:

* Wegfall der Notwendigkeit und zugehörigen Infrastrukturkosten einer Standbyfarm für die Notfallwiederherstellung. Verwenden Sie Site Recovery, um eine gesamte Farm (Web-, App- und Datenbankebenen) zu Azure oder einem sekundären Standort zu replizieren.
* Vereinfachen der Bereitstellung und Verwaltung von Anwendungen. Updates, die am primären Standort bereitgestellt werden, werden automatisch repliziert und sind nach dem Failover und der Wiederherstellung einer Farm daher an einem sekundären Standort verfügbar. Außerdem werden die Verwaltungskomplexität und Kosten verringert, die entstehen, wenn eine Standbyfarm auf dem neuesten Stand gehalten werden muss.
* Vereinfachen von Entwicklung und Tests von SharePoint-Anwendungen, indem für Test- und Debugzwecke eine der Produktionsversion ähnliche Replikatumgebung bedarfsgesteuert erstellt wird.
* Vereinfachen des Übergangs in die Cloud, indem Site Recovery zum Migrieren von SharePoint-Bereitstellungen in Azure verwendet wird.

[Erfahren Sie mehr](site-recovery-sharepoint.md) über das Schützen von SharePoint.

## Schützen von Dynamics AX
<a id="protect-dynamics-ax" class="xliff"></a>
Azure Site Recovery trägt wie folgt zum Schutz Ihrer Dynamics AX ERP-Lösung bei:

* Orchestrieren der Replikation der gesamten Dynamics AX-Umgebung (Web- und AOS-Ebene, Datenbankebenen, SharePoint) zu Azure oder einem sekundären Standort.
* Vereinfachen der Migration von Dynamics AX-Bereitstellungen zur Cloud (Azure).
* Vereinfachen von Entwicklung und Tests von Dynamics AX-Anwendungen, indem für Test- und Debugzwecke eine der Produktionsversion ähnliche Kopie bedarfsgesteuert erstellt wird.

[Erfahren Sie mehr](site-recovery-dynamicsax.md) über das Schützen von Dynamics AX.

## Schützen von RDS
<a id="protect-rds" class="xliff"></a>
Mit Remotedesktopdiensten (RDS) werden eine Virtual Desktop Infrastructure (VDI) sowie sitzungsbasierte Desktops und Anwendungen ermöglicht, damit Benutzer an jedem Ort arbeiten können. Mit Azure Site Recovery haben Sie folgende Möglichkeiten:

* Replizieren von verwalteten oder nicht verwalteten virtuellen Desktops eines Pools an einem sekundären Standort und von Remoteanwendungen und -sitzungen an einem sekundären Standort oder in Azure.
* Sie können Folgendes replizieren:

| **RDS** | **Replizieren von Hyper-V-VMs an einem sekundären Standort** | **Replizieren von Hyper-V-VMs in Azure** | **Replizieren von VMware-VMs an einem sekundären Standort** | **Replizieren von VMware-VMs in Azure** | **Replizieren von physischen Servern an einem sekundären Standort** | **Replizieren physischer Server in Azure** |
| --- | --- | --- | --- | --- | --- | --- |
| **Virtuelle Desktops eines Pools (nicht verwaltet)** |Ja |Nein |Ja |Nein |Ja |Nein |
| **Virtuelle Desktops eines Pools (verwaltet und ohne UPD)** |Ja |Nein |Ja |Nein |Ja |Nein |
| **Remoteanwendungen und Remotedesktopsitzungen (ohne UPD)** |Ja |Ja |Ja |Ja |Ja |Ja |

[Erfahren Sie mehr](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) über das Schützen von RDS.

## Schützen von Exchange
<a id="protect-exchange" class="xliff"></a>
Mit Site Recovery können Sie Exchange wie schützen:

* Für kleine Exchange-Bereitstellungen, z. B. einen einzelnen oder eigenständigen Server, können mit Site Recovery die Replikation und das Failover in Azure oder an einem sekundären Standort durchgeführt werden.
* Für größere Bereitstellungen kann Site Recovery in Exchange-Datenbankverfügbarkeitsgruppen (DAGs) integriert werden.
* Exchange-Datenbankverfügbarkeitsgruppen sind die empfohlene Lösung für die Exchange-Notfallwiederherstellung in einem Unternehmen.  Site Recovery-Wiederherstellungspläne können Datenbankverfügbarkeitsgruppen zum Orchestrieren von DAG-Failovern zwischen Standorten enthalten.

[Erfahren Sie mehr](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) über das Schützen von Exchange.

## Schützen von SAP
<a id="protect-sap" class="xliff"></a>
Verwenden Sie Site Recovery wie folgt zum Schützen Ihrer SAP-Bereitstellung:

* Sie können für den Schutz der gesamten SAP-Bereitstellung sorgen, indem Sie unterschiedliche Bereitstellungsebenen in Azure oder an einem sekundären Standort replizieren.
* Vereinfachen der Cloudmigration mithilfe von Site Recovery zum Migrieren Ihrer SAP-Bereitstellung in Azure.
* Vereinfachen Sie Entwicklung und Tests von SAP-Anwendungen, indem Sie für Test- und Debugzwecke eine der Produktionsversion ähnliche Kopie bedarfsgesteuert erstellen.

[Erfahren Sie mehr](http://aka.ms/asr-sap) über das Schützen von SAP.

## Schützen von IIS
<a id="protect-iis" class="xliff"></a>
Verwenden Sie Site Recovery wie folgt zum Schützen Ihrer IIS-Bereitstellung:

Azure Site Recovery stellt die Notfallwiederherstellung bereit, indem die kritischen Komponenten in Ihrer Umgebung an einem „kalten“ Remotestandort oder in einer öffentlichen Cloud wie Microsoft Azure repliziert werden. Da der virtuelle Computer mit dem Webserver und der Datenbank am Wiederherstellungsstandort repliziert werden, ist es nicht erforderlich, Konfigurationsdateien oder Zertifikate separat zu sichern. Die Anwendungszuordnungen und Bindungen sind von Umgebungsvariablen abhängig, die nach dem Failover geändert werden. Sie können mithilfe von Skripts aktualisiert werden, die in die Pläne für die Notfallwiederherstellung integriert sind. Virtuelle Computer werden am Wiederherstellungsstandort nur bei einem Failover genutzt. Außerdem werden Sie von Azure Site Recovery beim Orchestrieren des End-to-End-Failovers unterstützt, indem die folgenden Funktionen bereitgestellt werden:

-   Sequenzieren des Herunterfahrens und Startens von virtuellen Computern auf verschiedenen Ebenen
-   Hinzufügen von Skripts zum Zulassen des Updates von Anwendungsabhängigkeiten und Bindungen auf den virtuellen Computern nach dem Starten (Die Skripts können auch verwendet werden, um den DNS-Server so zu aktualisieren, dass er auf den Wiederherstellungsstandort zeigt.)
-   Zuordnen von IP-Adressen zu virtuellen Computern vor dem Failover, indem primäre Netzwerke und Wiederherstellungsnetzwerke zugeordnet werden und so Skripts verwendet werden, die nach dem Failover nicht aktualisiert werden müssen
-   Ermöglichen eines Failovers mit einem Klick für mehrere Webanwendungen auf den Webservern, um im Notfall Verwirrung zu vermeiden
-   Ermöglichen von Tests des Wiederherstellungsplans in einer isolierten Umgebung für Notfallwiederherstellungsübungen

[Hier finden Sie weitere Informationen](https://aka.ms/asr-iis) zum Schützen einer IIS-Webfarm.

## Schützen von Citrix XenApp und XenDesktop
<a id="protect-citrix-xenapp-and-xendesktop" class="xliff"></a>
Gehen Sie wie folgt vor, um Ihre Bereitstellungen von Citrix XenApp und XenDesktop mithilfe von Site Recovery zu schützen:

* Aktivieren Sie den Schutz der Citrix XenApp- und XenDesktop-Bereitstellung, indem Sie verschiedene Bereitstellungsebenen in Azure replizieren. Zu diesen zählen unter anderem AD-DNS-Server, SQL-Datenbankserver, Citrix Delivery Controller, StoreFront-Server, XenApp Master (VDA) und Citrix XenApp-Lizenzserver.
* Vereinfachen Sie die Cloudmigration, indem Sie Ihre Citrix XenApp- und XenDesktop-Bereitstellung mithilfe von Site Recovery zu Azure migrieren.
* Vereinfachen Sie das Testen von Citrix XenApp/XenDesktop, indem Sie bei Bedarf eine der Produktionsversion ähnliche Kopie zu Test- und Debugzwecken erstellen.
* Diese Lösung ist nur für virtuelle Desktopcomputer unter dem Windows Server-Betriebssystem relevant und nicht für virtuelle Clientdesktopcomputer, da diese noch nicht für die Lizenzierung in Azure unterstützt werden. 
Weitere Informationen zur Lizenzierung für Client-/Serverdesktopcomputer in Azure finden Sie [hier](https://azure.microsoft.com/pricing/licensing-faq/).

Weitere Informationen zum Schutz von Citrix XenApp- und XenDesktop-Bereitstellungen finden Sie [hier](site-recovery-citrix-xenapp-and-xendesktop.md). Alternativ können Sie das [Whitepaper von Citrix](https://aka.ms/citrix-xenapp-xendesktop-with-asr) lesen. Es enthält die gleichen Informationen. 

## Nächste Schritte
<a id="next-steps" class="xliff"></a>
[Überprüfen der Voraussetzungen](site-recovery-prereq.md) 

