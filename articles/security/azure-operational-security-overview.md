---
title: "Azure Operational Security – Übersicht | Microsoft-Dokumentation"
description: "Dieser Artikel enthält eine Übersicht über Azure Operational Security."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: tomsh
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: d51ba4a40ff39f84ccad28c953fe57c845542b38
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---

# <a name="azure-operational-security-overview"></a>Azure Operational Security – Übersicht
Azure Operational Security bezieht sich auf die Dienste, Steuerelemente und Features, die für Benutzer zum Schützen ihrer Daten, Anwendungen und anderen Ressourcen in Microsoft Azure zur Verfügung stehen. [Azure Operational Security](https://docs.microsoft.com/azure/security/azure-operational-security) ist ein Framework, mit dem die über verschiedene einzigartige Microsoft-Funktionen erworbenen Kenntnisse einbezogen werden, z.B. Microsoft Security Development Lifecycle (SDL), Microsoft Security Response Center-Programm und umfassende Informationen zur Bedrohungslage hinsichtlich der Sicherheit im Internet.

Azure Operational Security bezieht sich auf die Dienste, Steuerelemente und Features, die für Benutzer zum Schützen ihrer Daten, Anwendungen und anderen Ressourcen in Microsoft Azure zur Verfügung stehen.

In diesem Artikel zu Azure Operational Security werden die folgenden Bereiche behandelt:

- Azure Operations Management Suite
-   Azure Security Center
-   Azure Monitor
-   Azure Network Watcher
-   Azure-Speicheranalyse
-   Azure Active Directory

## <a name="azure-operations-management-suite"></a>Azure Operations Management Suite
Die IT-Abteilung (IT Operations) ist für die Verwaltung der Datencenterinfrastruktur, Anwendungen und Daten verantwortlich, z.B. die Stabilität und Sicherheit dieser Systeme. Für die Gewinnung von Erkenntnissen zur Sicherheit in immer komplexeren IT-Umgebungen müssen Organisationen aber häufig Daten aus mehreren Sicherheits- und Verwaltungssystemen zusammentragen.

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Ihnen das Verwalten und Schützen Ihrer Infrastruktur lokal und in der Cloud erleichtert.

OMS ist eine cloudbasierte IT-Verwaltungslösung mit vielen Angeboten, z.B. IT-Automatisierung, Security & Compliance, Log Analytics und Sicherung und Wiederherstellung. Somit ist OMS ein perfektes Hilfsmittel zum Verwalten und Schützen Ihrer IT-Infrastruktur – lokal und in der Cloud.

Die Kernfunktionen von OMS werden durch eine Reihe von Diensten bereitgestellt, die in Azure ausgeführt werden. Jeder Dienst bietet eine bestimmte Verwaltungsfunktion, und Sie können Dienste miteinander kombinieren, um verschiedene Verwaltungsszenarien zu bewältigen. Dies ist für Folgendes möglich:

-   Log Analytics
-   Automation
-   Sicherung
-   Site Recovery

### <a name="log-analytics"></a>Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) sammelt Daten von verwalteten Ressourcen in einem zentralen Repository und stellt dadurch Überwachungsdienste für OMS bereit. Bei diesen Daten kann es sich um Ereignisse, Leistungsdaten oder benutzerdefinierte Daten handeln, die über die API bereitgestellt wurden. Die gesammelten Daten können für Warnungen und Analysen genutzt und exportiert werden. Mithilfe dieser Methode können Sie Daten aus einer Vielzahl von Quellen zusammenfassen, um Daten aus Ihren Azure-Diensten mit Ihrer vorhandenen lokalen Umgebung zu kombinieren. Die Datensammlung wird außerdem klar von der Aktion getrennt, die für diese Daten ausgeführt wird, sodass alle Aktionen für alle Arten von Daten verfügbar sind.

### <a name="automation"></a>Automation
Microsoft [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) ermöglicht es Benutzern, die manuellen, längeren, fehleranfälligen und häufig wiederholten Aufgaben zu automatisieren, die in einer Cloud- und Unternehmensumgebung normalerweise ausgeführt werden. Dies spart Zeit, erhöht die Zuverlässigkeit normaler Verwaltungsaufgaben und plant diese sogar so ein, dass sie in regelmäßigen Abständen automatisch ausgeführt werden. Sie können Prozesse mit Runbooks automatisieren oder die Konfigurationsverwaltung über die Konfiguration für den gewünschten Zustand automatisieren.

### <a name="backup"></a>Sicherung
[Azure Backup](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) ist der Azure-basierte Dienst, den Sie zum Sichern (bzw. Schützen) und Wiederherstellen Ihrer Daten in der Microsoft Cloud verwenden können. Azure Backup ersetzt Ihre vorhandene lokale bzw. standortexterne Lösung durch eine zuverlässige, sichere und wirtschaftliche Cloudlösung. Azure Backup verfügt über mehrere Komponenten, die Sie herunterladen und auf dem jeweiligen Computer, Server oder in der Cloud bereitstellen. Die Komponente (der Agent), die Sie bereitstellen, richtet sich danach, was geschützt werden soll. Alle Azure Backup-Komponenten (unabhängig davon, ob Daten lokal oder in der Cloud geschützt werden sollen) können genutzt werden, um Daten in einem Recovery Services-Tresor in Azure zu sichern. Weitere Informationen finden Sie in der [Tabelle mit den Azure Backup-Komponenten](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use).

### <a name="site-recovery"></a>Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) bietet Geschäftskontinuität durch Orchestrierung der Replikation von lokalen virtuellen und physischen Computern in Azure oder an einem sekundären Standort. Falls Ihr primärer Standort nicht verfügbar ist, wird ein Failover auf den sekundären Standort durchgeführt, damit die Benutzer weiterarbeiten können. Sind die Systeme wieder verfügbar, erfolgt ein Failback. Verwenden Sie Microsoft-Sicherheitsdaten und -analysen, um eine intelligentere und effektivere Bedrohungserkennung vorzunehmen.

## <a name="azure-active-directory"></a>Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-enable-sso-scenario) ist die umfassende IDaaS-Lösung (Identity-as-a-Service) von Microsoft, die folgende Vorteile bietet:

-   Sie ermöglicht IAM als Clouddienst.
-   Sie stellt eine zentrale Zugriffsverwaltung, einmaliges Anmelden (SSO) und Berichterstellung bereit.
-   Sie unterstützt die integrierte Zugriffsverwaltung für [Tausende von Anwendungen](https://azure.microsoft.com/marketplace/active-directory/) im Anwendungskatalog, einschließlich Salesforce, Google Apps, Box, Concur und viele mehr.

Azure AD bietet auch einen vollständigen Satz von [Identitätsverwaltungsfunktionen](https://docs.microsoft.com/azure/security/security-identity-management-overview#security-monitoring-alerts-and-machine-learning-based-reports), z.B.: [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), [Geräteregistrierung]( https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-overview), [Self-Service-Kennwortverwaltung](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/), [Self-Service-Gruppenverwaltung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password), [privilegierte Kontenverwaltung](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure), [rollenbasierte Zugriffssteuerung](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is), [Überwachung der Anwendungsnutzung](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health), [umfassende Überwachungsfunktionen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) sowie [Sicherheitsüberwachung und -warnungen](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts).

Mit Azure Active Directory gelten für alle Anwendungen, die Sie für Ihre Partner und Kunden (Geschäftskunden oder Heimanwender) veröffentlichen, dieselben Identitäts- und Zugriffsverwaltungsfunktionen. Dadurch können Sie die Betriebskosten erheblich reduzieren.

## <a name="azure-security-center"></a>Azure Security Center
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-get-started) unterstützt Sie beim Verhindern, Erkennen und Beheben von Bedrohungen durch größere Transparenz und bessere Kontrolle über die Sicherheit Ihrer Azure-Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Sektrum an Sicherheitslösungen verwendet werden.

Mit [Security Center](https://docs.microsoft.com/azure/security-center/security-center-linux-virtual-machine) können Sie die Daten von virtuellen Computern in Azure schützen, indem Sie Einblicke in die Sicherheitseinstellungen des jeweiligen virtuellen Computers erhalten und die Computer auf Bedrohungen überwachen. Security Center kann auf Ihren virtuellen Computern Folgendes überwachen:

-   Sicherheitseinstellungen des Betriebssystems mit den empfohlenen Konfigurationsregeln
-   Systemsicherheit und fehlende kritische Updates
-   Empfehlungen zum Endpunktschutz
-   Überprüfung der Datenträgerverschlüsselung
-   Netzwerkbasierte Angriffe

Azure Security Center verwendet die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure). Diese stellt [integrierte Rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) bereit, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können.

Security Center bewertet die Konfiguration Ihrer Ressourcen, um die Sicherheitsprobleme und Sicherheitsrisiken zu identifizieren. In Security Center werden Ihnen nur dann Informationen zu einer Ressource angezeigt, wenn Ihnen für das Abonnement oder die Ressourcengruppe, der eine Ressource angehört, die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ zugewiesen ist.

>[!Note]
>Informationen zu Rollen und zulässigen Aktionen in Security Center finden Sie unter [Permissions in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-permissions) (Berechtigungen in Azure Security Center).

Security Center verwendet den Microsoft Monitoring Agent. Dies ist derselbe Agent, den auch die Operations Management Suite und der Log Analytics-Dienst verwenden. Die von diesem Agent gesammelten Daten werden entweder in einem vorhandenen Log Analytics-[Arbeitsbereich](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access), der mit Ihrem Azure-Abonnement verknüpft ist, oder unter Berücksichtigung des geografischen Standorts des virtuellen Computers in neuen Arbeitsbereichen gespeichert.

## <a name="azure-monitor"></a>Azure Monitor
Leistungsprobleme in Ihrer Cloud-App können Ihr Unternehmen beeinträchtigen. Durch mehrere miteinander verbundene Komponenten und häufige Versionswechsel können Leistungseinbußen jederzeit vorkommen. Bei der Entwicklung einer App werden von Ihren Benutzern in der Regel Probleme entdeckt, die Sie beim Testen nicht gefunden haben. Sie sollten sofort über diese Probleme Bescheid wissen und über Tools zu deren Diagnose und Beseitigung verfügen.

[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) ist ein grundlegendes Tool zum Überwachen von Diensten, die in Azure ausgeführt werden. Dieses Tool bietet Ihnen Daten auf Infrastrukturebene über den Durchsatz eines Diensts und über dessen Umgebung. Wenn Sie Ihre Apps vollständig in Azure verwalten und entscheiden, ob Sie Ihre Ressourcen zentral hoch- oder herunterskalieren, bietet Azure Monitor Ihnen einen Ausgangspunkt.

Darüber hinaus können Sie auf der Grundlage von Überwachungsdaten umfassende Erkenntnisse über Ihre Anwendung gewinnen. Mithilfe dieser Kenntnisse können Sie die Leistung oder Wartungsfreundlichkeit der Anwendung verbessern oder Aktionen automatisieren, die andernfalls manuell ausgeführt werden müssten. Sie hat folgenden Inhalt:

-   Azure-Aktivitätsprotokoll
-   Azure-Diagnoseprotokolle
-   Metriken
-   Azure-Diagnose

### <a name="azure-activity-log"></a>Azure-Aktivitätsprotokoll
Das Aktivitätsprotokoll bietet Einblicke in Vorgänge, die für Ressourcen Ihres Abonnements durchgeführt wurden. Das [Aktivitätsprotokoll](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) wurde bisher als „Überwachungsprotokoll“ oder „Vorgangsprotokoll“ bezeichnet, da es Ereignisse der Steuerungsebene für Ihre Abonnements enthält.

### <a name="azure-diagnostic-logs"></a>Azure-Diagnoseprotokolle
[Azure-Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) werden von einer Ressource ausgegeben und stellen umfangreiche und in kurzen Abständen erfasste Betriebsdaten der Ressource bereit. Der Inhalt dieser Protokolle variiert je nach Ressourcentyp.

Windows-Systemereignisprotokolle sind z.B. eine Kategorie des Diagnoseprotokolls für virtuelle Computer und Blob-, Tabellen- und Warteschlangenprotokolle sind Kategorien der Diagnoseprotokolle für Speicherkonten.

Diagnoseprotokolle unterscheiden sich vom [Aktivitätsprotokoll (früher als Überwachungsprotokoll, oder Betriebsprotokoll bezeichnet)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Das Aktivitätsprotokoll bietet Einblicke in Vorgänge, die für Ressourcen Ihres Abonnements durchgeführt wurden. Diagnoseprotokolle bieten Einblick in Vorgänge, die Ihre Ressource selbst ausgeführt hat.

### <a name="metrics"></a>Metriken
Mit Azure Monitor können Sie Telemetriedaten verwenden, um sich einen Überblick über Leistung und Integrität Ihrer Workloads in Azure zu verschaffen. Die wichtigsten Typen von Telemetriedaten sind Metriken (auch Leistungsindikatoren genannt), die von den meisten Azure-Ressourcen ausgegeben werden. Azure Monitor bietet Ihnen verschiedene Möglichkeiten, diese [Metriken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) für die Überwachung und Problembehandlung zu konfigurieren und zu nutzen.

### <a name="azure-diagnostics"></a>Azure-Diagnose
Sie ist eine Funktion in Azure, mit der Diagnosedaten für eine bereitgestellte Anwendung erfasst werden können. Sie können die Diagnoseerweiterung von einer Reihe verschiedener Quellen aus verwenden. Derzeit werden die [Web- und Workerrollen des Azure-Clouddiensts](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure Virtual Machines](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) unter Microsoft Windows und [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) unterstützt.


## <a name="network-watcher"></a>Network Watcher
Kunden erstellen ein End-to-End-Netzwerk in Azure und orchestrieren und kombinieren dabei verschiedene Netzwerkressourcen wie z.B. VNET, ExpressRoute, Application Gateway, Load Balancer u.v.m. Überwachung steht für jede der Netzwerkressourcen zur Verfügung.

Das End-to-End-Netzwerk kann über komplexe Konfigurationen und Interaktionen zwischen den Ressourcen verfügen. Dies führt zu komplexen Szenarien, die eine szenariobasierte Überwachung mit Network Watcher erfordern.

Mit dem [Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) wird die Überwachung und Diagnose Ihres Azure-Netzwerks vereinfacht. Mit den Diagnose- und Visualisierungstools von Network Watcher können Sie Remotepaketerfassungen auf einer Azure-VM durchführen, anhand von Flowprotokollen Erkenntnisse zu Ihrem Netzwerkdatenverkehr gewinnen und VPN Gateway und Verbindungen diagnostizieren.

Network Watcher verfügt derzeit über die folgenden Funktionen:

- [Topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview): Bietet eine Ansicht auf Netzwerkebene mit verschiedenen Verbindungen und Beziehungen zwischen Netzwerkressourcen innerhalb einer Ressourcengruppe.
-   [Variable Paketerfassung](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview): Erfasst am virtuellen Computer ein- und ausgehende Paketdaten. Erweiterte Filteroptionen und präzise Steuerelemente erlauben z.B. das Festlegen von Zeitraum und Größeneinschränkungen und bieten damit viel Flexibilität. Die Paketdaten können in einem Blobspeicher oder auf dem lokalen Datenträger im CAP-Format gespeichert werden.
-   [IP-Datenflussüberprüfung](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview): Überprüft basierend auf 5-Tupel-Paketparametern (Ziel-IP, Quell-IP, Zielport, Quellport und Protokoll) anhand der Datenflussinformationen, ob ein Paket zugelassen oder verweigert wird. Wenn das Paket durch eine Sicherheitsgruppe verweigert wird, werden die Namen der Regel und der Gruppe, die das Paket verweigert haben, zurückgegeben.
-   [Nächster Hop](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview): Ermittelt den nächsten Hop für Pakete, die im Azure-Netzwerkfabric weitergeleitet werden, sodass Sie eine Diagnose auf falsch konfigurierte benutzerdefinierte Routen durchführen können.
-   [Sicherheitsgruppenansicht](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview): Ruft die geltenden und angewendeten Sicherheitsregeln ab, die auf einen virtuellen Computer angewendet werden.
-   [NSG-Datenflussprotokollierung](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview): Mithilfe von Datenflussprotokollen für Netzwerksicherheitsgruppen können Sie Protokolle zum Datenverkehr erfassen, der von den Sicherheitsregeln in der Gruppe zugelassen oder verweigert wird. Der Datenfluss wird durch 5-Tupel-Informationen definiert: Quell-IP-Adresse, Ziel-IP-Adresse, Quellport, Zielport und Protokoll.
-   [Problembehandlung für Virtual Network-Gateways und -Verbindungen](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest): Bietet die Möglichkeit zum Beheben von Problemen, die bei Virtual Network-Gateways und -Verbindungen auftreten können.
-   [Grenzwerte für Netzwerkabonnements](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Ermöglicht die Anzeige der Verwendung von Netzwerkressourcen mit bestimmten Grenzwerten.
-   [Konfigurieren von Diagnoseprotokollen](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Stellt einen zentralen Bereich für das Aktivieren oder Deaktivieren von Diagnoseprotokollen für Netzwerkressourcen in einer Ressourcengruppe bereit.

Weitere Informationen zum Konfigurieren von Network Watcher finden Sie unter [Erstellen einer Azure Network Watcher-Instanz](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="developer-operations-devops"></a>Developer-Vorgänge (DevOps)
Vor der Einführung der DevOps-Anwendungsentwicklung waren die Teams für das Zusammenstellen der Geschäftsanforderungen für ein Softwareprogramm und das Schreiben von Code zuständig. Anschließend wurde das Programm von einem separaten QA-Team in einer isolierten Entwicklungsumgebung darauf getestet, ob die Anforderungen erfüllt waren, und der Code für die Bereitstellung durch das Operations-Personal freigegeben. Die Bereitstellungsteams waren noch weiter in einzelne Gruppen unterteilt, z.B. für Netzwerk und Datenbank. Jedes Mal, wenn ein unabhängiges Team ein Softwareprogramm vorgesetzt bekommt, kommt es zu Engpässen.

Dank [DevOps](https://www.visualstudio.com/learn/what-is-devops/) können Teams schneller und kostengünstiger Lösungen bereitstellen, die eine höhere Sicherheit und Qualität aufweisen. Kunden erwarten beim Konsumieren von Software und Diensten eine dynamische und zuverlässige Benutzeroberfläche.  Die Teams müssen Softwareupdates schnell durchlaufen, die Auswirkung der Updates messen und in kurzer Zeit mit neuen Entwicklungsiterationen reagieren, um Probleme zu beheben oder den Nutzen zu erhöhen.  Cloudplattformen wie Microsoft Azure haben dazu geführt, dass herkömmliche Engpässe entfernt wurden und die Infrastruktur allgemein verfügbar ist. Die Software ist in jedem Unternehmen ein bedeutendes Unterscheidungsmerkmal und ein wichtiger Faktor in Bezug auf das Geschäftsergebnis. Organisationen, Entwickler und IT-Experten können bzw. sollten die DevOps-Bewegung nicht mehr ignorieren.

Erfahrene DevOps-Praktiker nutzen beispielsweise die folgenden Vorgehensweisen. Bei diesen Vorgehensweisen legen [Personen](https://www.visualstudio.com/learn/what-is-devops-culture/) Strategien anhand des Geschäftsszenarios fest.  Tools können zur Automatisierung der unterschiedlichen Vorgehensweisen beitragen:

-   Verfahren für [Planung und Projektmanagement auf „agile“ Weise](https://www.visualstudio.com/learn/what-is-agile/) werden genutzt, um Arbeit in Form von „Sprints“ zu planen und zu isolieren, die Teamkapazität zu verwalten und Teams bei der schnellen Anpassung an sich ändernde Geschäftsanforderungen zu unterstützen.
-   Mit der [Versionskontrolle (normalerweise per Git)](https://www.visualstudio.com/learn/what-is-git/) können Teams, die sich an beliebigen Orten weltweit befinden, Quellen gemeinsam verwenden und Tools für die Softwareentwicklung integrieren, um die Releasepipeline zu automatisieren.
-   [Continuous Integration](https://www.visualstudio.com/learn/what-is-continuous-integration/) unterstützt das laufende Zusammenführen und Testen von Code, sodass Fehler zu einem frühen Zeitpunkt erkannt werden können.  Weitere Vorteile sind, dass weniger Zeit für die Behebung von Zusammenführungsproblemen aufgewendet werden muss und Entwicklungsteams schnell Feedback erhalten.
-   [Continuous Delivery](https://www.visualstudio.com/learn/what-is-continuous-delivery/) (fortlaufende Bereitstellung) von Softwarelösungen für Produktions- und Testumgebungen ermöglicht Organisationen die schnelle Behebung von Fehlern und Reaktion auf sich ständig ändernde Geschäftsanforderungen.
-   Die [Überwachung](https://www.visualstudio.com/learn/what-is-monitoring/) von ausgeführten Anwendungen, einschließlich Produktionsumgebungen für Anwendungsintegrität sowie Kundenverwendung, unterstützt Unternehmen dabei, eine Hypothese aufzustellen und Strategien schnell zu überprüfen oder zu widerlegen.  In verschiedenen Protokollierungsformaten werden umfassende Daten erfasst und gespeichert.
-   [Infrastruktur als Code (IaC)](https://www.visualstudio.com/learn/what-is-infrastructure-as-code/) ist eine Methode, die die Automatisierung und Überprüfung der Erstellung und Beendigung von Netzwerken und virtuellen Computern ermöglicht, um die Bereitstellung von sicheren, stabilen Anwendungsplattformen zu unterstützen.
-   Die [Microservices](https://www.visualstudio.com/learn/what-are-microservices/)-Architektur wird genutzt, um Geschäftsanwendungsfälle in Form von kleinen wiederverwendbaren Diensten zu isolieren.  Diese Architektur ermöglicht Skalierbarkeit und Effizienz.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Sicherheits- und Überwachungslösung von OMS finden Sie in den folgenden Artikeln:

- [Operations Management Suite | Security & Compliance](https://www.microsoft.com/cloud-platform/security-and-compliance)
- [Überwachen von und Reagieren auf Sicherheitswarnungen in der Sicherheits- und Überwachungslösung von Operations Management Suite](https://docs.microsoft.com/en-us/azure/operations-management-suite/oms-security-responding-alerts)
- [Überwachen von Ressourcen in der Sicherheits- und Überwachungslösung von Operations Management Suite](https://docs.microsoft.com/en-us/azure/operations-management-suite/oms-security-monitoring-resources)

