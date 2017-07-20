---
title: Was kann Azure Backup Server sichern? | Microsoft-Dokumentation
description: "Dieser Artikel bietet eine Unterstützungsmatrix aller Workloads, Datentypen und Installationen, die von Azure Backup Server v2 geschützt werden."
services: backup
documentation center: 
author: markgalioto
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
keywords: 
ms.date: 05/15/2017
ms.topic: article
ms.author: markgal,masaran
manager: carmonm
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: d538ec6cbf08a39671e3c5bcb4c9ecaa2f0b1ed7
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---

# <a name="azure-backup-server-protection-matrix"></a>Azure Backup Server-Schutzmatrix

In diesem Artikel sind verschiedene Server und Workloads aufgeführt, die Sie mit Azure Backup Server schützen können. In der folgenden Matrix sind die Elemente aufgelistet, die mit Azure Backup Server v1 und v2 geschützt werden können.

## <a name="protection-support-matrix"></a>Schutzunterstützungsmatrix

|Workload|Version|Azure Backup Server</br> installation|Azure Backup</br> Server v2|Azure Backup</br> Server v1 |Schutz und Wiederherstellung|
|------------|-----------|--------------------|--------------------------------------------|--------------------------------|---------------------------|
|System Center VMM|VMM 2016,<br/>VMM 2012, SP1, R2|Physischer Server<br /><br />Virtueller Hyper-V-Computer|J|J|Alle Bereitstellungsszenarien: Datenbank|
|Clientcomputer (64 Bit und 32 Bit)|Windows 10|Physischer Server<br /><br />Virtueller Hyper-V-Computer<br /><br />Virtueller VMware-Computer|J|J|Dateien<br /><br />Geschützte Volumes müssen mit NTFS formatiert sein. FAT und FAT32 werden nicht unterstützt.<br /><br />Volumes müssen mindestens 1 GB groß sein. DPM verwendet den Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS) für die Daten der Momentaufnahme, und die Momentaufnahme funktioniert nur, wenn das Volume mindestens 1 GB groß ist.|
|Clientcomputer (64 Bit und 32 Bit)|Windows 8.1|Physischer Server<br /><br />Virtueller Hyper-V-Computer|J|J|Dateien<br /><br />Geschützte Volumes müssen mit NTFS formatiert sein. FAT und FAT32 werden nicht unterstützt.<br /><br />Volumes müssen mindestens 1 GB groß sein. DPM verwendet den Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS) für die Daten der Momentaufnahme, und die Momentaufnahme funktioniert nur, wenn das Volume mindestens 1 GB groß ist.|
|Clientcomputer (64 Bit und 32 Bit)|Windows 8.1|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J|Dateien<br /><br />Geschützte Volumes müssen mit NTFS formatiert und mindestens 1 GB groß sein.|
|Clientcomputer (64 Bit und 32 Bit)|Windows 8|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Dateien<br /><br />Geschützte Volumes müssen mit NTFS formatiert und mindestens 1 GB groß sein.|
|Clientcomputer (64 Bit und 32 Bit)|Windows 8|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J|Dateien<br /><br />Geschützte Volumes müssen mit NTFS formatiert und mindestens 1 GB groß sein.|
|Clientcomputer (64 Bit und 32 Bit)|Windows 7|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Dateien<br /><br />Geschützte Volumes müssen mit NTFS formatiert und mindestens 1 GB groß sein.|
|Clientcomputer (64 Bit und 32 Bit)|Windows 7|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J |Dateien<br /><br />Geschützte Volumes müssen mit NTFS formatiert und mindestens 1 GB groß sein.|
|Clientcomputer (64 Bit und 32 Bit)|Windows Vista mit SP2|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Dateien<br /><br />Geschützte Volumes müssen mit NTFS formatiert und mindestens 1 GB groß sein.|
|Clientcomputer (64 Bit und 32 Bit)|Windows Vista mit SP1|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Dateien<br /><br />Geschützte Volumes müssen mit NTFS formatiert und mindestens 1 GB groß sein.|
|Clientcomputer (64 Bit und 32 Bit)|Windows Vista|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Dateien<br /><br />Geschützte Volumes müssen mit NTFS formatiert und mindestens 1 GB groß sein.|
|Clientcomputer (64 Bit und 32 Bit)|Windows Vista|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Volume, Freigabe, Ordner, Datei, Systemstatus/Bare-Metal, dedupliziertes Volume|
|Server (32 Bit und 64 Bit)|Windows Server 2016|Virtueller Azure-Computer (wenn die Workload als virtueller Azure-Computer ausgeführt wird)<br /><br />Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)<br /><br />Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J<br /><br />Nicht Nano Server|N|Volume, Freigabe, Ordner, Datei, Systemstatus/Bare-Metal, dedupliziertes Volume|
|Server (32 Bit und 64 Bit)|Windows Server 2012 R2 – Datacenter und Standard|Virtueller Azure-Computer (wenn die Workload als virtueller Azure-Computer ausgeführt wird)|J|J |Volume, Freigabe, Ordner, Datei<br /><br />DPM muss mindestens unter Windows Server 2012 R2 ausgeführt werden, um deduplizierte Windows Server 2012-Volumes zu schützen.|
|Server (32 Bit und 64 Bit)|Windows Server 2012 R2 – Datacenter und Standard|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J|Volume, Freigabe, Ordner, Datei, Systemstatus/Bare-Metal<br /><br />DPM muss unter Windows Server 2012 oder 2012 R2 ausgeführt werden, um deduplizierte Windows Server 2012-Volumes zu schützen.|
|Server (32 Bit und 64 Bit)|Windows Server 2012/2012 mit SP1 – Datacenter und Standard|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Volume, Freigabe, Ordner, Datei, Systemstatus/Bare-Metal<br /><br />DPM muss mindestens unter Windows Server 2012 R2 ausgeführt werden, um deduplizierte Windows Server 2012-Volumes zu schützen.|
|Server (32 Bit und 64 Bit)|Windows Server 2012/2012 mit SP1 – Datacenter und Standard|Virtueller Azure-Computer (wenn die Workload als virtueller Azure-Computer ausgeführt wird)|J|J|Volume, Freigabe, Ordner, Datei<br /><br />DPM muss mindestens unter Windows Server 2012 R2 ausgeführt werden, um deduplizierte Windows Server 2012-Volumes zu schützen.|
|Server (32 Bit und 64 Bit)|Windows Server 2012/2012 mit SP1 – Datacenter und Standard|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J|Volume, Freigabe, Ordner, Datei, Systemstatus/Bare-Metal<br /><br />DPM muss mindestens unter Windows Server 2012 R2 ausgeführt werden, um deduplizierte Windows Server 2012-Volumes zu schützen.|
|Server (32 Bit und 64 Bit)|Windows Server 2008 R2 SP1 – Standard und Enterprise|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J<br /><br />Sie müssen SP1 ausführen und [Windows Management Frame 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855) installieren.|J|Volume, Freigabe, Ordner, Datei, Systemstatus/Bare-Metal|
|Server (32 Bit und 64 Bit)|Windows Server 2008 R2 SP1 – Standard und Enterprise|Virtueller Azure-Computer (wenn die Workload als virtueller Azure-Computer ausgeführt wird)|J<br /><br />Sie müssen SP1 ausführen und [Windows Management Frame 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855) installieren.|J |Volume, Freigabe, Ordner, Datei|
|Server (32 Bit und 64 Bit)|Windows Server 2008 R2 SP1 – Standard und Enterprise|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J<br /><br />Sie müssen SP1 ausführen und [Windows Management Frame 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855) installieren.|J |Volume, Freigabe, Ordner, Datei, Systemstatus/Bare-Metal|
|Server (32 Bit und 64 Bit)|Windows Server 2008 R2|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Volume, Freigabe, Ordner, Datei, Systemstatus/Bare-Metal|
|Server (32 Bit und 64 Bit)|Windows Server 2008 R2|Virtueller Azure-Computer (wenn die Workload als virtueller Azure-Computer ausgeführt wird)|N|J|Volume, Freigabe, Ordner, Datei|
|Server (32 Bit und 64 Bit)|Windows Server 2008 R2|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|N|J|Volume, Freigabe, Ordner, Datei, Systemstatus/Bare-Metal|
|Server (32 Bit und 64 Bit)|Windows Server 2008|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|N|J|Volume, Freigabe, Ordner, Datei, Systemstatus/Bare-Metal|
|Server (32 Bit und 64 Bit)|Windows Server 2008|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J |Volume, Freigabe, Ordner, Datei, Systemstatus/Bare-Metal|
|Server (32 Bit und 64 Bit)|Windows Storage Server 2008|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Volume, Freigabe, Ordner, Datei, Systemstatus/Bare-Metal|
|SQL Server|SQL Server 2016|Physischer Server <br /><br /> Lokaler virtueller Hyper-V-Computer <br /> <br /> Virtueller Azure-Computer <br /><br /> Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J (UR2 oder höher)|N|Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2014|Virtueller Azure-Computer (wenn die Workload als virtueller Azure-Computer ausgeführt wird)|J|J |Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2014|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J|Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2012 mit SP2|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J |Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2012 mit SP2|Virtueller Azure-Computer (wenn die Workload als virtueller Azure-Computer ausgeführt wird)|J|J|Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2012 mit SP2|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J|Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2012, SQL Server 2012 mit SP1|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2012, SQL Server 2012 mit SP1|Virtueller Azure-Computer (wenn die Workload als virtueller Azure-Computer ausgeführt wird)|J|J|Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2012, SQL Server 2012 mit SP1|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J|Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2008 R2|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2008 R2|Virtueller Azure-Computer (wenn die Workload als virtueller Azure-Computer ausgeführt wird)|J|J|Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2008 R2|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J |Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2008|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2008|Virtueller Azure-Computer (wenn die Workload als virtueller Azure-Computer ausgeführt wird)|J|J|Alle Bereitstellungsszenarien: Datenbank|
|SQL Server|SQL Server 2008|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J|Alle Bereitstellungsszenarien: Datenbank|
|Exchange|Exchange 2016|Physischer Server<br/><br/> Lokaler virtueller Hyper-V-Computer|J|J|Schützen (alle Bereitstellungsszenarien): eigenständiger Exchange-Server, Datenbank unter einer Database Availability Group (DAG)<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Postfach, Postfachdatenbanken unter einer DAG|
|Exchange|Exchange 2016|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J|Schützen (alle Bereitstellungsszenarien): eigenständiger Exchange-Server, Datenbank unter einer Database Availability Group (DAG)<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Postfach, Postfachdatenbanken unter einer DAG|
|Exchange|Exchange 2013|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Schützen (alle Bereitstellungsszenarien): eigenständiger Exchange-Server, Datenbank unter einer Database Availability Group (DAG)<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Postfach, Postfachdatenbanken unter einer DAG|
|Exchange|Exchange 2013|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J |Schützen (alle Bereitstellungsszenarien): eigenständiger Exchange-Server, Datenbank unter einer Database Availability Group (DAG)<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Postfach, Postfachdatenbanken unter einer DAG|
|Exchange|Exchange 2010|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Schützen (alle Bereitstellungsszenarien): eigenständiger Exchange-Server, Datenbank unter einer Database Availability Group (DAG)<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Postfach, Postfachdatenbanken unter einer DAG|
|Exchange|Exchange 2010|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J |Schützen (alle Bereitstellungsszenarien): eigenständiger Exchange-Server, Datenbank unter einer Database Availability Group (DAG)<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Postfach, Postfachdatenbanken unter einer DAG|
|Exchange|Exchange 2007|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Schützen (alle Bereitstellungsszenarien): Speichergruppe<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Speichergruppe, Datenbank, Postfach|
|Exchange|Exchange 2007|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J |Schützen (alle Bereitstellungsszenarien): Speichergruppe<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Speichergruppe, Datenbank, Postfach|
|SharePoint|SharePoint 2016|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer<br /><br />Virtueller Azure-Computer (wenn die Workload als virtueller Azure-Computer ausgeführt wird)<br /><br />Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J (UR2 oder höher)|N|Schützen (alle Bereitstellungsszenarien): Farm, Inhalt von Front-End-Webservern<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Farm, Datenbank, Webanwendung, Datei oder Listenelement, SharePoint-Suche, Front-End-Webserver<br /><br />Beachten Sie, dass der Schutz von SharePoint-Farmen, die für die Inhaltsdatenbanken das Always On-Feature von SQL Server 2012 verwenden, nicht unterstützt wird.|
|SharePoint|SharePoint 2013|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Schützen (alle Bereitstellungsszenarien): Farm, Inhalt von Front-End-Webservern<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Farm, Datenbank, Webanwendung, Datei oder Listenelement, SharePoint-Suche, Front-End-Webserver<br /><br />Beachten Sie, dass der Schutz von SharePoint-Farmen, die für die Inhaltsdatenbanken das Always On-Feature von SQL Server 2012 verwenden, nicht unterstützt wird.|
|SharePoint|SharePoint 2013|Virtueller Azure-Computer (wenn die Workload als virtueller Azure-Computer ausgeführt wird) – DPM 2012 R2 Update Rollup 3 und höher|J|J|Schützen (alle Bereitstellungsszenarien): Farm, SharePoint-Suche, Inhalt von Front-End-Webservern<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Farm, Datenbank, Webanwendung, Datei oder Listenelement, SharePoint-Suche, Front-End-Webserver<br /><br />Beachten Sie, dass der Schutz von SharePoint-Farmen, die für die Inhaltsdatenbanken das Always On-Feature von SQL Server 2012 verwenden, nicht unterstützt wird.|
|SharePoint|SharePoint 2013|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J |Schützen (alle Bereitstellungsszenarien): Farm, SharePoint-Suche, Inhalt von Front-End-Webservern<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Farm, Datenbank, Webanwendung, Datei oder Listenelement, SharePoint-Suche, Front-End-Webserver<br /><br />Beachten Sie, dass der Schutz von SharePoint-Farmen, die für die Inhaltsdatenbanken das Always On-Feature von SQL Server 2012 verwenden, nicht unterstützt wird.|
|SharePoint|SharePoint 2010|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Schützen (alle Bereitstellungsszenarien): Farm, SharePoint-Suche, Inhalt von Front-End-Webservern<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Farm, Datenbank, Webanwendung, Datei oder Listenelement, SharePoint-Suche, Front-End-Webserver|
|SharePoint|SharePoint 2010|Virtueller Azure-Computer (wenn die Workload als virtueller Azure-Computer ausgeführt wird)|J|J |Schützen (alle Bereitstellungsszenarien): Farm, SharePoint-Suche, Inhalt von Front-End-Webservern<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Farm, Datenbank, Webanwendung, Datei oder Listenelement, SharePoint-Suche, Front-End-Webserver|
|SharePoint|SharePoint 2010|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J|Schützen (alle Bereitstellungsszenarien): Farm, SharePoint-Suche, Inhalt von Front-End-Webservern<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Farm, Datenbank, Webanwendung, Datei oder Listenelement, SharePoint-Suche, Front-End-Webserver|
|SharePoint|SharePoint 2007|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Schützen (alle Bereitstellungsszenarien): Farm, SharePoint-Suche, Inhalt von Front-End-Webservern<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Farm, Datenbank, Webanwendung, Datei oder Listenelement, SharePoint-Suche, Front-End-Webserver|
|SharePoint|SharePoint 2007|Windows-VM in VMware (schützt Workloads auf virtuellen Windows-Computern in VMware)|J|J|Schützen (alle Bereitstellungsszenarien): Farm, SharePoint-Suche, Inhalt von Front-End-Webservern<br /><br />Wiederherstellen (alle Bereitstellungsszenarien): Farm, Datenbank, Webanwendung, Datei oder Listenelement, SharePoint-Suche, Front-End-Webserver|
|Hyper-V-Host – DPM-Schutz-Agent auf Hyper-V-Hostserver, Cluster oder VM|Windows Server 2016|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|N|Schutz: Hyper-V-Computer, freigegebene Clustervolumes (CSVs)<br /><br />Wiederherstellung: virtueller Computer, Wiederherstellung auf Elementebene von Dateien und Ordnern, Volumes, virtuelle Festplatten|
|Hyper-V-Host – DPM-Schutz-Agent auf Hyper-V-Hostserver, Cluster oder VM|Windows Server 2012 R2 – Datacenter und Standard|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Schutz: Hyper-V-Computer, freigegebene Clustervolumes (CSVs)<br /><br />Wiederherstellung: virtueller Computer, Wiederherstellung auf Elementebene von Dateien und Ordnern, Volumes, virtuelle Festplatten|
|Hyper-V-Host – DPM-Schutz-Agent auf Hyper-V-Hostserver, Cluster oder VM|Windows Server 2012 – Datacenter und Standard|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Schutz: Hyper-V-Computer, freigegebene Clustervolumes (CSVs)<br /><br />Wiederherstellung: virtueller Computer, Wiederherstellung auf Elementebene von Dateien und Ordnern, Volumes, virtuelle Festplatten|
|Hyper-V-Host – DPM-Schutz-Agent auf Hyper-V-Hostserver, Cluster oder VM|Windows Server 2008 R2 SP1 – Enterprise und Standard|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|J|J|Schutz: Hyper-V-Computer, freigegebene Clustervolumes (CSVs)<br /><br />Wiederherstellung: virtueller Computer, Wiederherstellung auf Elementebene von Dateien und Ordnern, Volumes, virtuelle Festplatten|
|Hyper-V-Host – DPM-Schutz-Agent auf Hyper-V-Hostserver, Cluster oder VM|Windows Server 2008|Physischer Server<br /><br />Lokaler virtueller Hyper-V-Computer|N|N|Schutz: Hyper-V-Computer, freigegebene Clustervolumes (CSVs)<br /><br />Wiederherstellung: virtueller Computer, Wiederherstellung auf Elementebene von Dateien und Ordnern, Volumes, virtuelle Festplatten|
|Linux|Linux, ausgeführt als Hyper-V-Gast|Lokaler virtueller Hyper-V-Computer|J|J|Hyper-V muss unter Windows Server 2012 R2 oder Windows Server 2016 ausgeführt werden. Schutz: gesamter virtueller Computer<br /><br />Wiederherstellung: gesamter virtueller Computer|

## <a name="cluster-support"></a>Unterstützung für Cluster
Azure Backup Server kann Daten in den folgenden gruppierten Anwendungen schützen:

-   Dateiserver

-   SQL Server

-   Hyper-V – Wenn Sie einen Hyper-V-Cluster mithilfe von horizontal skaliertem DPM-Schutz schützen, können Sie keinen sekundären Schutz für die geschützten Hyper-V-Workloads hinzufügen.

    Wenn Sie Hyper-V unter Windows Server 2008 R2 ausführen, installieren Sie unbedingt das in KB [975354](https://support.microsoft.com/en-us/kb/975354) beschriebene Update.
    Wenn Sie Hyper-V unter Windows Server 2008 R2 in einer Clusterkonfiguration ausführen, installieren Sie unbedingt SP2 und KB [971394](https://support.microsoft.com/en-us/kb/971394).

-   Exchange Server – Azure Backup Server kann nicht freigegebene Datenträgercluster für unterstützte Exchange Server-Versionen schützen (fortlaufende Clusterreplikation). Auch der Schutz von Exchange Server in einer Konfiguration für fortlaufende lokale Replikation ist mit Azure Backup Server möglich.

-   SQL Server – Azure Backup Server unterstützt nicht das Sichern von SQL Server-Datenbanken, die auf freigegebenen Clustervolumes (CSVs) gehostet werden.

Azure Backup Server kann Workloads schützen, die sich in derselben Domäne wie der DPM-Server, in einer untergeordneten oder in einer vertrauenswürdigen Domäne befinden. Wenn Sie Datenquellen in nicht vertrauenswürdigen Domänen oder Arbeitsgruppen schützen möchten, verwenden Sie NTLM, Zertifikatauthentifizierung für einen einzelnen Server oder Zertifikatauthentifizierung nur für einen Cluster.

