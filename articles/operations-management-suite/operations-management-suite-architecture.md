---
title: Operations Management Suite-Architektur (OMS) | Microsoft Docs
description: "Microsoft Operations Management Suite (OMS) ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Ihnen das Verwalten und Schützen Ihrer Infrastruktur lokal und in der Cloud erleichtert.  Dieser Artikel enthält Informationen zu den verschiedenen Diensten von OMS und Links zu Seiten mit ausführlicheren Informationen."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 40e41686-7e35-4d85-bbe8-edbcb295a534
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/27/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: df1a5b2c200b7fa69961247ac036ee35fd14f54d


---
# <a name="oms-architecture"></a>OMS-Architektur
[Operations Management Suite (OMS)](https://azure.microsoft.com/documentation/services/operations-management-suite/) ist eine Sammlung von cloudbasierten Diensten für die Verwaltung von lokalen Umgebungen und Cloudumgebungen.  In diesem Artikel werden die verschiedenen lokalen und cloudbasierten Komponenten von OMS und ihre allgemeine Cloud Computing-Architektur beschrieben.  Weitere Informationen finden Sie in der Dokumentation der jeweiligen Dienste.

## <a name="log-analytics"></a>Log Analytics
Alle von [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) gesammelten Daten werden im OMS-Repository gespeichert, das in Azure gehostet wird.  Verbundene Quellen generieren Daten, die im OMS-Repository gesammelt werden.  Derzeit werden drei Arten von verbundenen Quellen unterstützt.

* Ein installierter Agent auf einem [Windows](../log-analytics/log-analytics-windows-agents.md)- oder [Linux](../log-analytics/log-analytics-linux-agents.md)-Computer, der direkt mit OMS verbunden ist.
* Eine Verwaltungsgruppe von System Center Operations Manager (SCOM), die [mit Log Analytics verbunden ist](../log-analytics/log-analytics-om-agents.md) .  SCOM-Agents kommunizieren weiterhin mit Verwaltungsservern, die Ereignisse und Leistungsdaten an Log Analytics weiterleiten.
* Ein [Azure-Speicherkonto](../log-analytics/log-analytics-azure-storage.md), das [Azure-Diagnosedaten](../cloud-services/cloud-services-dotnet-diagnostics.md) von einer Workerrolle, einer Webrolle oder einem virtuellen Computer in Azure sammelt.

Datenquellen definieren die Daten, die Log Analytics aus verbundenen Quellen sammelt, einschließlich Ereignisprotokollen und Leistungsindikatoren.  Lösungen fügen OMS Funktionen hinzu und können problemlos über den [OMS-Lösungskatalog](../log-analytics/log-analytics-add-solutions.md) Ihrem Arbeitsbereich hinzugefügt werden.  Einige Lösungen erfordern eine direkte Verbindung mit Log Analytics über SCOM-Agents. Andere Lösungen erfordern möglicherweise die Installation eines zusätzlichen Agents.

Log Analytics verfügt über ein webbasiertes Portal, über das Sie OMS-Ressourcen verwalten, OMS-Lösungen hinzufügen und konfigurieren sowie Daten im OMS-Repository anzeigen und analysieren können.

![Log Analytics: allgemeine Architektur](media/operations-management-suite-architecture/log-analytics.png)

## <a name="azure-automation"></a>Azure-Automatisierung
[Azure Automation-Runbooks](http://azure.microsoft.com/documentation/services/automation) werden in der Azure-Cloud ausgeführt und haben Zugriff auf Ressourcen in Azure, in anderen Clouddiensten sowie auf Ressourcen, die über das öffentliche Internet zugänglich sind.  Sie können mit [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md) in Ihrem lokalen Rechenzentrum auch lokale Computer festlegen, damit Runbooks auf lokale Ressourcen zugreifen können.

[DSC-Konfigurationen](../automation/automation-dsc-overview.md) können direkt auf virtuelle Azure-Computer angewendet werden.  Andere physische und virtuelle Computer können Konfigurationen vom DSC-Pullserver von Azure Automation anfordern.

Azure Automation bietet eine OMS-Lösung, die Statistiken und Links zum Starten des Azure-Portals für beliebige Vorgänge anzeigt.

![Azure Automation: allgemeine Architektur](media/operations-management-suite-architecture/automation.png)

## <a name="azure-backup"></a>Azure Backup
Geschützte Daten in [Azure Backup](http://azure.microsoft.com/documentation/services/backup) werden in einem Sicherungstresor gespeichert, der sich in einer bestimmten geografischen Region befindet.  Die Daten werden innerhalb derselben Region repliziert und können je nach Art des Tresors zur erweiterten Redundanz auch in eine andere Region repliziert werden.

Azure Backup verfügt über drei grundlegende Szenarios.

* Windows-Computer mit Azure Backup-Agent.  Auf diese Weise können Sie Dateien und Ordner von einem Windows-Server oder -Client direkt in Ihrem Azure Backup-Tresor sichern.  
* System Center Data Protection Manager (DPM) oder Microsoft Azure Backup Auf diese Weise können Sie DPM oder Microsoft Azure Backup Server zusätzlich zu Anwendungsworkloads wie SQL und SharePoint zum Sichern von Dateien und Ordnern im lokalen Speicher nutzen und diese dann in den Azure Backup-Tresor replizieren.
* Erweiterungen für virtuelle Azure-Computer.  Auf diese Weise können Sie virtuelle Azure-Computer in Ihrem Azure Backup-Tresor sichern.

Azure Backup bietet eine OMS-Lösung, die Statistiken und Links zum Starten des Azure-Portals für beliebige Vorgänge anzeigt.

![Azure Backup: allgemeine Architektur](media/operations-management-suite-architecture/backup.png)

## <a name="azure-site-recovery"></a>Azure Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) koordiniert Replikation, Failover und Failback von virtuellen Computern und physischen Servern. Replikationsdaten werden zwischen Hyper-V-Hosts, VMware-Hypervisoren und physischen Servern in primären und sekundären Rechenzentren oder zwischen dem Rechenzentrum und dem Azure-Speicher ausgetauscht.  Site Recovery speichert Metadaten in Tresoren, die sich in einer bestimmten geografischen Azure-Region befinden. Es werden keine replizierten Daten vom Site Recovery-Dienst gespeichert.

Azure Site Recovery verfügt über drei grundlegende Replikationsszenarios.

**Replikation von virtuellen Hyper-V-Computern**

* Wenn virtuelle Hyper-V-Computer in VMM-Clouds verwaltet werden, können Sie in ein sekundäres Rechenzentrum oder in den Azure-Speicher replizieren.  Die Replikation in Azure erfolgt über eine sichere Internetverbindung.  Die Replikation in ein sekundäres Rechenzentrum erfolgt über das LAN.
* Wenn virtuelle Hyper-V-Computer nicht von VMM verwaltet werden, können Sie nur in den Azure-Speicher replizieren.  Die Replikation in Azure erfolgt über eine sichere Internetverbindung.

**Replikation von virtuellen VMware-Computern**

* Sie können virtuelle VMware-Computer in ein sekundäres Rechenzentrum, in dem VMware ausgeführt wird, oder in den Azure-Speicher replizieren.  Die Replikation in Azure kann über ein Standort-zu-Standort-VPN oder Azure ExpressRoute bzw. über eine sichere Internetverbindung erfolgen. Die Replikation in ein sekundäres Rechenzentrum erfolgt über den InMage Scout-Datenkanal.

**Replikation von physischen Windows- und Linux-Servern** 

* Sie können physische Server in ein sekundäres Rechenzentrum oder in den Azure-Speicher replizieren. Die Replikation in Azure kann über ein Standort-zu-Standort-VPN oder Azure ExpressRoute bzw. über eine sichere Internetverbindung erfolgen. Die Replikation in ein sekundäres Rechenzentrum erfolgt über den InMage Scout-Datenkanal.  Azure Site Recovery bietet eine OMS-Lösung, die einige Statistiken anzeigt, Sie müssen jedoch das Azure-Portal für sämtliche Vorgänge verwenden.

![Azure Site Recovery: allgemeine Architektur](media/operations-management-suite-architecture/site-recovery.png)

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics)
* Weitere Informationen zu [Azure Automation](https://azure.microsoft.com/documentation/services/automation)
* Weitere Informationen zu [Azure Backup](http://azure.microsoft.com/documentation/services/backup)
* Weitere Informationen zu [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery)




<!--HONumber=Dec16_HO2-->


