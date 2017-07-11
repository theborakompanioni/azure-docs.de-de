---
title: "Operations Management Suite (OMS) – Übersicht | Microsoft Docs"
description: "Microsoft Operations Management Suite (OMS) ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Ihnen das Verwalten und Schützen Ihrer Infrastruktur lokal und in der Cloud erleichtert.  Dieser Artikel informiert über die Vorteile von OMS und enthält Informationen zu den verschiedenen Diensten und Angeboten von OMS sowie Links zu Seiten mit ausführlicheren Informationen."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: bwren
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 452dd602387db6db04ca87f6834c9e8606185484
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
<a id="what-is-operations-management-suite-oms" class="xliff"></a>

# Was ist die Operations Management Suite (OMS)?
Dieser Artikel bietet eine Einführung in Operations Management Suite (OMS) und enthält eine kurze Übersicht über den geschäftlichen Nutzen, die enthaltenen Dienste und Lösungen sowie über die Angebote, in denen verschiedene Dienste und Lösungen zusammengefasst sind.  Außerdem stehen Links zur ausführlichen Dokumentation für die Bereitstellung und Verwendung der einzelnen Dienste und Lösungen zur Verfügung.

<a id="from-on-premises-to-the-cloud" class="xliff"></a>

## Von der lokalen zur cloudbasierten Lösung
Microsoft bietet schon lange Produkte für die Verwaltung von Unternehmensumgebungen an.  2007 wurden mehrere Produkte zur System Center-Suite mit Verwaltungsprodukten zusammengefasst.  Hierzu zählten Configuration Manager (mit Features wie Softwareverteilung und Inventarisierung), Operations Manager (zur proaktiven Überwachung von Systemen und Anwendungen), Orchestrator (mit Runbooks zur Automatisierung manueller Prozesse) und Data Protection Manager (zur Sicherung und Wiederherstellung wichtiger Daten).

Mit der zunehmenden Verlagerung von Computeressourcen in die Cloud wurden System Center-Produkte mit zusätzlichen Cloudfeatures ausgestattet – etwa mit der Verwaltung von Ressourcen in Azure durch Operations Manager und Orchestrator.  Sie waren allerdings immer noch grundsätzlich als lokale Lösungen konzipiert und mit hohen Investitionen in die Bereitstellung und Verwaltung einer lokalen Verwaltungsumgebung verbunden.  Zur optimalen Nutzung der Cloud sowie zur Unterstützung zukünftiger Anwendungen bedurfte es eines neuen Verwaltungsansatzes.

<a id="introducing-operations-management-suite" class="xliff"></a>

## Einführung von Operations Management Suite
Bei Operations Management Suite (OMS) handelt es sich um eine Sammlung von Verwaltungsdiensten, die von Grund auf für die Cloud konzipiert wurden.  Anstatt lokale Ressourcen bereitzustellen und zu verwalten, werden OMS-Komponenten vollständig in Azure gehostet.  Der Konfigurationsaufwand ist minimal, und die Lösung ist innerhalb weniger Minuten einsatzbereit.  

- **Minimale Kosten und Komplexität der Bereitstellung:**  Da alle Komponenten und Daten für OMS in Azure gespeichert werden, sind Sie in kürzester Zeit einsatzbereit und können die Komplexität lokaler Komponenten und die damit verbundenen Investitionen vermeiden.
- **Skalierung auf der Cloudebene:**  Sie müssen sich keine Gedanken darüber machen, ob Sie womöglich für nicht benötigte Computeressourcen bezahlen oder ob Ihnen womöglich der Speicherplatz ausgeht. Dank der Cloud bezahlen Sie nur für die tatsächliche Nutzung, und die Cloud lässt sich auf jede beliebige Lastanforderung skalieren.  Sie können zunächst mit der Verwaltung einiger weniger Ressourcen beginnen und dann zentral auf Ihre gesamte Umgebung hochskalieren.
- **Neueste Features:**  Die Features der OMS-Dienste werden kontinuierlich erweitert und aktualisiert.  Ihnen stehen stets die neuesten Features zur Verfügung, ohne dass Sie dafür Updates bereitstellen müssen.
- **Integrierte Dienste:**  Die OMS-Dienste bieten für sich genommen bereits erhebliche Vorteile, können aber auch miteinander kombiniert werden, um komplexe Verwaltungsszenarien zu bewältigen.  So kann beispielsweise ein Runbook in Azure Automation einen Failovervorgang mit Azure Site Recovery steuern und anschließend Informationen in Log Analytics protokollieren, um eine Warnung zu generieren.
- **Globale Informationen:**  Verwaltungslösungen in OMS stehen kontinuierlich die neuesten Informationen zur Verfügung.  So kann beispielsweise die Sicherheits- und Überwachungslösung eine Bedrohungsanalyse auf der Grundlage der neuesten Bedrohungen durchführen, die auf der ganzen Welt erkannt werden.
- **Ortsunabhängiger Zugriff:**  Über einen Browser können Sie von überall aus auf Ihre Verwaltungsumgebung zugreifen.  Installieren Sie die OMS-App auf Ihrem Smartphone, um komfortabel auf Ihre Überwachungsdaten zugreifen zu können.

<a id="is-it-just-for-the-cloud" class="xliff"></a>

### Ist die Lösung auf die Cloud beschränkt?
Die Tatsache, dass OMS-Dienste in der Cloud ausgeführt werden, bedeutet nicht, dass sie nicht auch zur effektiven Verwaltung Ihrer lokalen Umgebung genutzt werden können.  Platzieren Sie einfach einen Agent auf einem beliebigen Windows- oder Linux-Computer in Ihrem Rechenzentrum. Dieser sendet dann Daten an Log Analytics, wo sie zusammen mit all den anderen Daten aus cloudbasierten oder lokalen Diensten analysiert werden.  Verwenden Sie Azure Backup und Azure Site Recovery, um die Cloud für die Sicherung und hohe Verfügbarkeit lokaler Ressourcen zu nutzen.  
Runbooks in der Cloud können üblicherweise nicht auf lokale Ressourcen zugreifen. Sie können jedoch ebenfalls einen Agent auf Computern installieren, um Runbooks in Ihrem Rechenzentrum zu hosten.  Geben Sie beim Starten eines Runbooks einfach an, ob es in der Cloud oder für einen lokalen Worker ausgeführt werden soll.

<a id="hybrid-management-with-system-center" class="xliff"></a>

## Hybridverwaltung mit System Center
Wenn Sie bereits über eine Installation von System Center verfügen, können Sie diese Komponenten in OMS-Dienste integrieren, um eine Hybridlösung für Ihre lokale und Ihre cloudbasierte Umgebung bereitzustellen und von den jeweiligen Vorzügen der Produkte zu profitieren.  Stellen Sie eine Verbindung zwischen Ihrer vorhandenen Operations Manager-Verwaltungsgruppe und Log Analytics her, um verwaltete Agents in der Cloud zu analysieren.  Sichern Sie Ihre Daten mithilfe Ihres bereits vorhandenen Sicherungsprozesses mit Data Protection Manager in die Cloud.  


<a id="oms-services" class="xliff"></a>

## OMS-Dienste
Die Kernfunktionen von OMS werden durch eine Reihe von Diensten bereitgestellt, die in Azure ausgeführt werden.  Jeder Dienst bietet eine bestimmte Verwaltungsfunktion, und Sie können Dienste miteinander kombinieren, um verschiedene Verwaltungsszenarien zu bewältigen.

|| Dienst | Beschreibung |
|:--|:--|:--|
| ![Log Analytics](media/operations-management-suite-overview/icon-log-analytics.png) | Log Analytics | Dient zur Überwachung und Analyse der Verfügbarkeit und Leistung verschiedener Ressourcen (einschließlich physischer und virtueller Computer). |
| ![Azure-Automatisierung](media/operations-management-suite-overview/icon-automation.png) | Automation | Dient zur Automatisierung manueller Prozesse sowie zur Erzwingung von Konfigurationen für physische und virtuelle Computer. |
| ![Azure Backup](media/operations-management-suite-overview/icon-backup.png) | Sicherung | Dient zur Sicherung und Wiederherstellung wichtiger Daten. |
| ![Azure Site Recovery](media/operations-management-suite-overview/icon-site-recovery.png) | Site Recovery | Dient zur Bereitstellung von hoher Verfügbarkeit für kritische Anwendungen. |

<a id="log-analytics" class="xliff"></a>

### Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) sammelt Daten von verwalteten Ressourcen in einem zentralen Repository und stellt dadurch Überwachungsdienste für OMS bereit.  Bei diesen Daten kann es sich um Ereignisse, Leistungsdaten oder benutzerdefinierte Daten handeln, die über die API bereitgestellt wurden. Die gesammelten Daten können für Warnungen und Analysen genutzt und exportiert werden.  Mithilfe dieser Methode können Sie Daten aus einer Vielzahl von Quellen zusammenfassen, um Daten aus Ihren Azure-Diensten mit Ihrer vorhandenen lokalen Umgebung zu kombinieren.  Die Datensammlung wird außerdem klar von der Aktion getrennt, die für diese Daten ausgeführt wird, sodass alle Aktionen für alle Arten von Daten verfügbar sind.  

![Übersicht über Log Analytics](media/operations-management-suite-overview/overview-log-analytics.png)

<a id="collecting-data" class="xliff"></a>

#### Sammeln von Daten
Daten können auf unterschiedliche Weise dem Repository für die Analyse durch Log Analytics hinzugefügt werden.

- **Physische und virtuelle Computer (Windows oder Linux):**  Installieren Sie Microsoft Monitoring Agent auf den physischen oder virtuellen Computern unter [Windows](../log-analytics/log-analytics-windows-agents.md) und [Linux](../log-analytics/log-analytics-linux-agents.md), deren Daten Sie sammeln möchten.  Der Agent lädt von Log Analytics automatisch Konfigurationsinformationen herunter, die die zu sammelnden Ereignisse und Leistungsdaten definieren.  Der Agent kann über das Azure-Portal problemlos auf virtuellen Azure-Computern installiert werden.  Wenn Sie bereits über eine vorhandene Operations Manager-Umgebung verfügen, können Sie die Verwaltungsgruppe mit Log Analytics verknüpfen und automatisch mit dem Sammeln von Daten aller vorhandenen Agents beginnen.
- **Azure-Dienste:**  Log Analytics erfasst Telemetriedaten aus [Azure-Diagnose und Azure-Überwachung](../log-analytics/log-analytics-azure-storage.md) im Repository, um die Überwachung von Azure-Ressourcen zu ermöglichen.
- **Datensammler-API:**  Log Analytics verfügt über eine [REST-API zum Auffüllen von Daten von beliebigen Clients](../log-analytics/log-analytics-data-collector-api.md).  Dadurch können Sie Daten aus Drittanbieteranwendungen sammeln oder benutzerdefinierte Verwaltungsszenarien implementieren.  Daten werden häufig mithilfe eines Runbooks in Azure Automation gesammelt und anschließend unter Verwendung der Datensammler-API in das Repository geschrieben.

<a id="reporting-and-analyzing-data" class="xliff"></a>

#### Melden und Analysieren von Daten
Log Analytics verfügt über eine leistungsstarke Abfragesprache, um im Repository gespeicherte Daten zu extrahieren.  Da die Daten aus allen Quellen als Datensätze gespeichert werden, können Sie in einer einzelnen Abfrage Daten aus mehreren Quellen analysieren.
  
Neben Ad-hoc-Analysen bietet Log Analytics mehrere Möglichkeiten zum Melden und Analysieren von Daten aus einer Abfrage.

- **Sichten und Dashboards:**  [Sichten](../log-analytics/log-analytics-view-designer.md) und [Dashboards](../log-analytics/log-analytics-dashboards.md) dienen zum Visualisieren der Ergebnisse einer Abfrage im Portal.  Verwaltungslösungen enthalten in der Regel Sichten zum Analysieren der Daten aus der Lösung.  Sie können auch eigene benutzerdefinierte Sichten erstellen, um Daten zu analysieren und direkt in Ihrem benutzerdefinierten Portal verfügbar zu machen.
- **Export:**  Die Ergebnisse jeder beliebigen Abfrage können exportiert und außerhalb von Log Analytics analysiert werden.  Sie können sogar regelmäßige Exporte nach [Power BI](../log-analytics/log-analytics-powerbi.md) planen und dessen aussagekräftige Visualisierungs- und Analysefunktionen nutzen.
- **Protokollsuch-API:**  Log Analytics verfügt über eine [REST-API zum Sammeln von Daten von beliebigen Clients](../log-analytics/log-analytics-log-search-api.md).  Dadurch können Sie programmgesteuert mit Daten arbeiten, die im Repository gesammelt wurden, oder über ein anderes Überwachungstool darauf zugreifen.

<a id="alerting" class="xliff"></a>

#### Warnungen
Log Analytics kann [proaktive Warnungen](../log-analytics/log-analytics-alerts.md) ausgeben oder Korrekturmaßnahmen ergreifen, wenn ein Problem erkannt wird.  Dieses Feature basiert genau wie die anderen Analysefunktionen von Log Analytics auf einer Protokollsuche.  Die Suche wird in regelmäßigen Abständen ausgeführt, und eine Warnung wird erstellt, wenn die Ergebnisse bestimmten Kriterien entsprechen.

![Log Analytics-Warnungen](media/operations-management-suite-overview/overview-alerts.png)

Neben der Erstellung eines Warnungsdatensatzes im Log Analytics-Repository können Warnungen folgende Maßnahmen ergreifen:

- **E-Mail:**  Sie können proaktiv per E-Mail auf ein erkanntes Problem aufmerksam gemacht werden.
- **Runbook:**  Eine Warnung in Log Analytics kann ein Runbook in Azure Automation starten.  Damit wird in der Regel versucht, das erkannte Problem zu beheben.  Bei einem Problem in Azure oder in einer anderen Cloud kann das Runbook in der Cloud gestartet werden. Liegt ein Problem auf einem physischen oder virtuellen Computer vor, kann es für einen lokalen Agent gestartet werden.
- **Webhook:**  Eine Warnung kann einen Webhook starten und ihm Daten aus den Ergebnissen der Protokollsuche übergeben.  Dies ermöglicht die Integration in externe Dienste (etwa in ein alternatives Warnungssystem). Alternativ kann versucht werden, Korrekturmaßnahmen für eine externe Website zu ergreifen.

<a id="azure-automation" class="xliff"></a>

### Azure Automation
[Azure Automation](http://azure.microsoft.com/documentation/services/automation) bietet Prozessautomatisierung und Konfigurationsverwaltung für OMS.  Es automatisiert manuelle Prozesse und trägt zur Erzwingung von Konfigurationen für physische und virtuelle Computer bei.  

<a id="process-automation" class="xliff"></a>

#### Prozessautomatisierung
Azure Automation automatisiert manuelle Prozesse mithilfe von [Runbooks](../automation/automation-runbook-types.md), die auf einem PowerShell-Skript oder -Workflow basieren.  Darüber hinaus enthält Azure Automation Objekte, die Runbooks unterstützen. Hierzu zählen etwa Variablen, die gemeinsam von mehreren Runbooks verwendet werden können, sowie Anmeldeinformationen und Verbindungen, mit denen Sie verschlüsselte Informationen speichern können, die unter Umständen von einem Runbook zur Authentifizierung benötigt werden.
Runbooks bieten Prozessautomatisierung für die anderen Dienste in der Suite.  Da auf jeden der anderen Dienste über PowerShell oder über eine REST-API zugegriffen werden kann, können Sie Runbooks erstellen, um Funktionen wie das Sammeln von Verwaltungsdaten in Log Analytics oder das Initiieren einer Sicherung mit Azure Backup auszuführen.

<a id="accessing-resources" class="xliff"></a>

##### Zugreifen auf Ressourcen
Da Runbooks auf PowerShell basieren, können sie jede Ressource verwalten, auf die über PowerShell-Cmdlets zugegriffen werden kann.  Wenn Sie ein Modul in Ihr Automation-Konto [laden](../automation/automation-integration-modules.md), wird es für alle Runbooks unter diesem Konto verfügbar. 
 
In der Cloud ausgeführte Runbooks haben Zugriff auf jede Ressource, auf die über die Cloud zugegriffen werden kann.  Dabei kann es sich um Ressourcen in Ihrem Azure-Abonnement oder in einer anderen Cloud (etwa Amazon Web Services (AWS)) oder um einen Dienst handeln, auf den über eine REST-API zugegriffen werden kann.  Runbooks in der Cloud werden unter keinen Anmeldeinformationen ausgeführt, können aber Automation-Objekte wie Anmeldeinformationen, Verbindungen und Zertifikate nutzen, um sich bei den Ressourcen, auf die sie zugreifen, zu authentifizieren.

Auf Ressourcen in Ihrem Rechenzentrum kann höchstwahrscheinlich nicht über ein in der Cloud ausgeführtes Runbook zugegriffen werden.  Sie können in Ihrem Rechenzentrum allerdings [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md) installieren, um Runbooks auszuführen, die Zugriff auf lokale Ressourcen benötigen.  Wenn Sie ein Runbook starten, geben Sie an, ob es in der Cloud oder für einen bestimmten Worker ausgeführt werden soll.

![Azure Automation-Runbooks](media/operations-management-suite-overview/overview-runbooks.png)

<a id="starting-a-runbook" class="xliff"></a>

##### Starten eines Runbooks
Runbooks können [auf verschiedene Arten](../automation/automation-starting-a-runbook.md) gestartet und dadurch in verschiedensten Szenarien eingesetzt werden.  

- **Azure-Portal:**  Azure Automation kann genau wie andere Azure-Dienste über das Azure-Portal verwaltet werden.  Hier können Sie Runbooks nicht nur starten, sondern auch importieren oder eigene Runbooks erstellen.
- **Zeitplan:**  Sie können Runbooks so planen, dass sie in regelmäßigen Abständen gestartet werden.  Dadurch können Sie regelmäßige Verwaltungsprozesse automatisch wiederholen oder Daten in Log Analytics sammeln.
- **PowerShell und API:**  Sie können Runbooks mithilfe eines PowerShell-Cmdlets oder über die Azure Automation-REST-API starten und die erforderlichen Parameterinformationen übergeben.  
- **Webhook:**  Ein Webhook kann für jedes beliebige Runbook erstellt werden, um den Start über externe Anwendungen oder Websites zu ermöglichen.
- **Log Analytics-Warnung:**  Eine Warnung in Log Analytics kann automatisch ein Runbook starten, um das von der Warnung erkannte Problem zu beheben.

<a id="configuration-management" class="xliff"></a>

#### Konfigurationsverwaltung
[PowerShell DSC (Desired State Configuration; Konfiguration des gewünschten Zustands)](../automation/automation-dsc-overview.md) ist eine Verwaltungsplattform in Windows PowerShell, über die Sie die Konfiguration physischer und virtueller Computer bereitstellen und erzwingen können.  Azure Automation verwaltet DSC-Konfigurationen und bietet einen Pullserver in der Cloud, auf den Agents zugreifen und die erforderlichen Konfigurationen abrufen können.

![Azure Automation DSC](media/operations-management-suite-overview/overview-dsc.png)

<a id="azure-backup-and-azure-site-recovery" class="xliff"></a>

### Azure Backup und Azure Site Recovery
Azure Backup und Azure Site Recovery tragen zur Geschäftskontinuität und Notfallwiederherstellung bei.  Beide verfügen über Features, die gewährleisten, dass Anwendungen bei einem Ausfall verfügbar bleiben und zum Normalbetrieb zurückkehren, wenn die Systeme wieder online sind.  Beide Dienste tragen zu den für Ihr Unternehmen definierten RPOs (Recovery Point Objectives) und RTOs (Recovery Time Objectives) bei. RPO definiert den zulässigen Grenzwert für die Nichtverfügbarkeit von Daten während eines Ausfalls. RTO schränkt den zulässigen Zeitraum ein, in dem ein Dienst oder eine App während eines Ausfalls nicht verfügbar ist.

<a id="azure-backup" class="xliff"></a>

#### Azure Backup
[Azure Backup](http://azure.microsoft.com/documentation/services/backup) stellt Dienste für die Sicherung und Wiederherstellung von Daten für OMS bereit.  Es schützt Ihre Anwendungsdaten und bewahrt sie jahrelang ohne Kapitaleinsatz und mit minimalen Betriebskosten auf.  Mit diesem Dienst können neben Anwendungsworkloads wie SQL Server und SharePoint Daten von physischen und virtuellen Windows-Servern gesichert werden.  Zudem kann er von System Center Data Protection Manager (DPM) verwendet werden, um geschützte Daten zur Redundanz und langfristigen Speicherung in Azure zu replizieren.

Geschützte Daten werden in Azure Backup in einem Sicherungstresor gespeichert, der sich in einer bestimmten geografischen Region befindet. Die Daten werden innerhalb der gleichen Region repliziert und können je nach Art des Tresors auch in eine andere Region repliziert werden, um eine höhere Resilienz zu erzielen.

Azure Backup verfügt über drei grundlegende Szenarios.

- **Windows-Computer mit Azure Backup-Agent:** Sichern Sie Dateien und Ordner von einem beliebigen Windows-Server oder -Client direkt in Ihrem Azure Backup-Tresor.<br><br>![Windows-Computer mit Azure Backup-Agent](media/operations-management-suite-overview/overview-backup-01.png)
- **System Center Data Protection Manager (DPM) oder Microsoft Azure Backup Server:** Sichern Sie mithilfe von DPM oder Microsoft Azure Backup Server Dateien und Ordner sowie Anwendungsworkloads wie SQL und SharePoint im lokalen Speicher, und replizieren Sie sie dann im Azure Backup-Tresor. Unterstützt virtuelle Windows- und Linux-Computer unter Hyper-V oder VMware.<br><br>![System Center Data Protection Manager (DPM) oder Microsoft Azure Backup Server](media/operations-management-suite-overview/overview-backup-02.png)
- **Erweiterungen für virtuelle Azure-Computer:** Sichern Sie virtuelle Windows- oder Linux-Computer in Azure in Ihrem Azure Backup-Tresor.<br><br>![Erweiterungen für virtuelle Azure-Computer](media/operations-management-suite-overview/overview-backup-03.png)



<a id="azure-site-recovery" class="xliff"></a>

#### Azure Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) bietet Geschäftskontinuität durch Orchestrierung der Replikation von lokalen virtuellen und physischen Computern in Azure oder an einem sekundären Standort. Falls Ihr primärer Standort nicht verfügbar ist, wird ein Failover auf den sekundären Standort durchgeführt, damit die Benutzer weiterarbeiten können. Sind die Systeme wieder verfügbar, erfolgt ein Failback. 

Azure Site Recovery bietet hohe Verfügbarkeit für Server und Anwendungen.  Es orchestriert Replikation, Failover und Wiederherstellung von lokalen virtuellen Hyper-V-Computern, virtuellen VMware-Computern und physischen Windows- oder Linux-Servern und trägt so zu Ihrer Strategie für Geschäftskontinuität und Notfallwiederherstellung bei. Sie können Computer in einem sekundären Rechenzentrum replizieren oder das Rechenzentrum erweitern, indem Sie sie in Azure replizieren. Site Recovery bietet zudem einen einfachen Failover sowie die einfache Wiederherstellung für Workloads. Der Dienst kann in Mechanismen zur Notfallwiederherstellung (z.B. SQL Server AlwaysOn) integriert werden und bietet Wiederherstellungspläne für den einfachen Failover von Workloads, die auf mehreren Computern mehrstufig verteilt sind.

Azure Site Recovery verfügt über drei grundlegende Replikationsszenarios.

- **Replikation virtueller Hyper-V-Computer:**  Wenn virtuelle Hyper-V-Computer in VMM-Clouds verwaltet werden, können Sie in ein sekundäres Rechenzentrum oder in den Azure-Speicher replizieren. Die Replikation in Azure erfolgt über eine sichere Internetverbindung. Die Replikation in ein sekundäres Rechenzentrum erfolgt über das LAN.  Wenn virtuelle Hyper-V-Computer nicht von VMM verwaltet werden, können Sie nur in den Azure-Speicher replizieren. Die Replikation in Azure erfolgt über eine sichere Internetverbindung.<br><br>![Replikation von virtuellen Hyper-V-Computern](media/operations-management-suite-overview/overview-siterecovery-hyperv.png)
- **Replikation virtueller VMware-Computer:**  Sie können virtuelle VMware-Computer in ein sekundäres Rechenzentrum, in dem VMware ausgeführt wird, oder in den Azure-Speicher replizieren. Die Replikation in Azure kann über ein Standort-zu-Standort-VPN oder Azure ExpressRoute bzw. über eine sichere Internetverbindung erfolgen. Die Replikation in ein sekundäres Rechenzentrum erfolgt über den InMage Scout-Datenkanal.<br><br>![Replikation virtueller VMware-Computer](media/operations-management-suite-overview/overview-siterecovery-vmware.png)
- **Replikation physischer Windows- und Linux-Server:**  Sie können physische Server in ein sekundäres Rechenzentrum oder in den Azure-Speicher replizieren. Die Replikation in Azure kann über ein Standort-zu-Standort-VPN oder Azure ExpressRoute bzw. über eine sichere Internetverbindung erfolgen. Die Replikation in ein sekundäres Rechenzentrum erfolgt über den InMage Scout-Datenkanal. Azure Site Recovery bietet eine OMS-Lösung, die einige Statistiken anzeigt, Sie müssen jedoch das Azure-Portal für sämtliche Vorgänge verwenden.<br><br>![Replikation von physischen Windows- und Linux-Servern](media/operations-management-suite-overview/overview-siterecovery-physical.png)


Site Recovery speichert Metadaten in Tresoren, die sich in einer bestimmten geografischen Azure-Region befinden. Vom Site Recovery-Dienst werden keine replizierten Daten gespeichert.

<a id="management-solutions" class="xliff"></a>

## Verwaltungslösungen
[Verwaltungslösungen](operations-management-suite-solutions.md) sind vorgefertigte Logiksätze zur Implementierung eines bestimmten Verwaltungsszenarios unter Verwendung mindestens eines OMS-Diensts.  Von Microsoft und von Partnern werden verschiedene Lösungen angeboten, die Sie problemlos Ihrem Azure-Abonnement hinzufügen und dadurch den Nutzen Ihrer OMS-Investition erhöhen können.  Partner können eigene Lösungen zur Unterstützung ihrer Anwendungen und Dienste erstellen und sie Benutzern über den Azure Marketplace oder über Schnellstartvorlagen zur Verfügung stellen.

Die [Lösung für die Updateverwaltung](oms-solution-update-management.md) ist ein gutes Beispiel für eine Lösung, die mehrere Dienste verwendet, um zusätzliche Funktionen bereitzustellen.  Diese Lösung verwendet den Log Analytics-Agent für Windows und Linux, um Informationen zu erforderlichen Updates für die einzelnen Agents zu sammeln.  Die Daten werden in das Log Analytics-Repository geschrieben, wo Sie sie über ein enthaltenes Dashboard analysieren können.  Wenn Sie eine Bereitstellung erstellen, werden erforderliche Updates mithilfe von Runbooks in Azure Automation installiert.  Der gesamte Prozess wird über das Portal verwaltet, und Sie müssen sich keine Gedanken über die zugrunde liegenden Details machen.

![Lösung](media/operations-management-suite-overview/overview-solution.png)

Die meisten Lösungen erfüllen mindestens eine der folgenden Funktionen:

- Sammeln zusätzlicher Informationen:  Log Analytics sammelt verschiedenste Daten von Clients und Diensten (beispielsweise Ereignisse und Leistungsdaten).  Eine Verwaltungslösung kann zusätzliche Informationen sammeln, die nicht in anderen Datenquellen verfügbar sind – häufig unter Verwendung von Azure Automation-Runbooks.
- Bereitstellen zusätzlicher Analysefunktionen für gesammelte Informationen:  Verwaltungslösungen verfügen über Dashboards und Sichten zur Analyse und Visualisierung von Daten.  Diese sind mit vordefinierten Protokollsuchen verknüpft, die es Ihnen ermöglichen, die Daten im Detail zu untersuchen.  Unter Umständen werden auch Daten analysiert, die bereits im Repository gesammelt wurden (beispielsweise, indem in Sicherheitsereignissen nach Mustern gesucht wird, die auf eine Bedrohung hindeuten).
- Erweitern des Funktionsumfangs:  Einige Lösungen von Microsoft bauen unter Umständen auf den Funktionen der Kerndienste auf, um zusätzliche Funktionen bereitzustellen.  So bietet beispielsweise die Dienstzuordnung eine eigene Konsole zur Ermittlung und Zuordnung von Server- und Prozessabhängigkeiten in Echtzeit.
Von Microsoft und Partnern werden regelmäßig Lösungen zu OMS hinzugefügt, wodurch sich der Nutzen Ihrer Investition immer weiter erhöht.  Lösungen von Microsoft können über den Lösungskatalog des OMS-Portals gesucht und installiert werden. Alternativ können Sie über den Azure Marketplace im Azure-Portal nach Lösungen von Microsoft und von Partnern suchen und sie installieren.  

![Lösungskatalog](media/operations-management-suite-overview/solution-gallery.png)


<a id="next-steps" class="xliff"></a>

## Nächste Schritte
* Weitere Informationen zu [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics)
* Weitere Informationen zu [Azure Automation](../automation/automation-intro.md)
* Weitere Informationen zu [Azure Backup](http://azure.microsoft.com/documentation/services/backup)
* Weitere Informationen zu [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery)
* [Verfügbare Lösungen](../log-analytics/log-analytics-add-solutions.md) in den verschiedenen OMS-Angeboten 


