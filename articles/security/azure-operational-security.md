---
title: Azure Operational Security | Microsoft-Dokumentation
description: Erhalten Sie Informationen zur Microsoft Operations Management Suite (OMS), zu deren Diensten und zur Funktionsweise.
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: TomSh
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: ec9e0fc7d67537a47d5c0d3bb376b60dc6ccffcd
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---

# <a name="azure-operational-security"></a>Azure Operational Security
## <a name="introduction"></a>Einführung

### <a name="overview"></a>Übersicht
Wir wissen, dass Sicherheit in der Cloud an erster Stelle steht und wie wichtig es ist, dass Sie präzise und zeitnahe Informationen zur Azure-Sicherheit finden. Eines der schlagkräftigsten Argumente dafür, Azure für Anwendungen und Dienste zu verwenden, ist die Vielzahl an verfügbaren Sicherheitstools und -funktionen. Diese Tools und Funktionen ermöglichen die Erstellung sicherer Lösungen auf der Grundlage der sicheren Azure-Plattform. Microsoft Azure muss sowohl Vertraulichkeit, Integrität und Verfügbarkeit von Kundendaten als auch eine transparente Verantwortlichkeit bieten.

Damit Kunden die Aufstellung von Sicherheitskontrollen, die in Microsoft Azure implementiert sind, sowohl aus Kundensicht als auch aus Sicht der Microsoft-Vorgänge besser verstehen, ist dieses Whitepaper, „Azure Operational Security“, so konzipiert, dass es einen umfassenden Überblick über die mit Microsoft Azure verfügbare funktionale Sicherheit bereitstellt.

### <a name="azure-platform"></a>Azure Platform
Azure ist eine öffentliche Clouddienstplattform, die eine breite Palette an Betriebssystemen, Programmiersprachen, Frameworks, Tools, Datenbanken und Geräten unterstützt. Es kann Linux-Container mit Docker-Integration ausführen, Apps mit JavaScript, Python, .NET, PHP, Java und Node.js sowie Back-Ends für iOS-, Android- und Windows-Geräte erstellen. Der Azure-Clouddienst unterstützt dieselben Technologien, die bereits von Millionen von Entwicklern und IT-Profis zuverlässig eingesetzt werden.

Wenn Sie IT-Ressourcen darauf erstellen oder zu einem öffentlichen Clouddienstanbieter migrieren, verlassen Sie sich darauf, dass die jeweilige Organisation Ihre Anwendungen und Daten mit den Diensten und Steuerungsmöglichkeiten schützen kann, die sie zum Verwalten der Sicherheit Ihrer cloudbasierten Ressourcen bereitstellt.

Die Infrastruktur von Azure ist von den Hardwareressourcen bis zu den Anwendungen vollständig auf das gleichzeitige Hosten von Millionen von Kunden ausgelegt und stellt für Unternehmen eine vertrauenswürdige Grundlage zur Erfüllung ihrer Sicherheitsanforderungen dar. Darüber hinaus bietet Azure Ihnen ein breites Spektrum an konfigurierbaren Sicherheitsoptionen, die Sie selbst steuern können, um die Sicherheit an die individuellen Anforderungen der Bereitstellungen Ihrer Organisation anzupassen. Dieses Dokument hilft Ihnen dabei zu verstehen, wie Ihnen die Azure-Sicherheitsfunktionen dabei helfen können, diese Anforderungen zu erfüllen.

### <a name="abstract"></a>Zusammenfassung
Azure Operational Security bezieht sich auf die Dienste, Steuerelemente und Features, die für Benutzer zum Schützen ihrer Daten, Anwendungen und anderen Ressourcen in Microsoft Azure zur Verfügung stehen. Azure Operational Security basiert auf einem Framework, das die über verschiedene für Microsoft einzigartige Funktionen erworbenen Kenntnisse einbezieht, einschließlich Microsoft Security Development Lifecycle (SDL), dem Microsoft Security Response Center-Programm und den umfassenden Informationen zur Bedrohungslage hinsichtlich der Sicherheit im Internet.

Dieses Whitepaper beschreibt den Ansatz von Microsoft zu Azure Operational Security innerhalb der Microsoft Azure-Cloudplattform und behandelt die folgenden Dienste:
1.  [Azure Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)

2.  [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

3.  [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

4.  [Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

5.  [Azure-Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

6.  [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)


## <a name="microsoft-operations-management-suite"></a>Microsoft Operations Management Suite

Microsoft Operations Management Suite (OMS) ist die IT-Verwaltungslösung für Hybridcloud. Bei der eigenständigen Verwendung oder zur Erweiterung Ihrer vorhandenen Bereitstellung von System Center bietet Ihnen die OMS die maximale Flexibilität und Kontrolle für die cloudbasierte Verwaltung Ihrer Infrastruktur.

![Microsoft Operations Management Suite](./media/azure-operational-security/azure-operational-security-fig1.png)

Mit der OMS können Sie jede Instanz in einer beliebigen Cloud, einschließlich lokaler Cloud, Azure, AWS, Windows Server, Linux, VMware und Openstack, zu geringeren Kosten als die Lösungen der Wettbewerber verwalten. Die primär für die Cloud konzipierte OMS bietet einen neuen Ansatz für die Verwaltung Ihres Unternehmens, bei dem es sich um die schnellste und kostengünstigste Möglichkeit handelt, um sich neuen geschäftlichen Herausforderungen zu stellen sowie neuen Workloads, Anwendungen und Cloudumgebungen nachzukommen.

### <a name="oms-services"></a>OMS-Dienste

Die Kernfunktionen von OMS werden durch eine Reihe von Diensten bereitgestellt, die in Azure ausgeführt werden. Jeder Dienst bietet eine bestimmte Verwaltungsfunktion, und Sie können Dienste miteinander kombinieren, um verschiedene Verwaltungsszenarien zu bewältigen.

| Dienst  | Beschreibung|
| :------------- | :-------------|
| Log Analytics | Dient zur Überwachung und Analyse der Verfügbarkeit und Leistung verschiedener Ressourcen (einschließlich physischer und virtueller Computer). |
|Automation | Dient zur Automatisierung manueller Prozesse sowie zur Erzwingung von Konfigurationen für physische und virtuelle Computer. |
| Sicherung | Dient zur Sicherung und Wiederherstellung wichtiger Daten. |
| Site Recovery | Dient zur Bereitstellung von hoher Verfügbarkeit für kritische Anwendungen. |

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) sammelt Daten von verwalteten Ressourcen in einem zentralen Repository und stellt dadurch Überwachungsdienste für OMS bereit. Bei diesen Daten kann es sich um Ereignisse, Leistungsdaten oder benutzerdefinierte Daten handeln, die über die API bereitgestellt wurden. Die gesammelten Daten können für Warnungen und Analysen genutzt und exportiert werden.


Mithilfe dieser Methode können Sie Daten aus verschiedenen Quellen zusammenfassen, um Daten aus Ihren Azure-Diensten mit Ihrer vorhandenen lokalen Umgebung zu kombinieren. Die Datensammlung wird außerdem klar von der Aktion getrennt, die für diese Daten ausgeführt wird, sodass alle Aktionen für alle Arten von Daten verfügbar sind.


![Log Analytics](./media/azure-operational-security/azure-operational-security-fig2.png)

Der Log Analytics-Dienst verwaltet Ihre Daten in der Cloud sicher mithilfe der folgenden Methoden:
-   Trennung von Daten
-   Beibehaltung von Daten
-   Physische Sicherheit
-   Incident Management
-   Compliance
-   Sicherheitsstandard-Zertifizierungen

### <a name="azure-backup"></a>Azure Backup

[Azure Backup](http://azure.microsoft.com/documentation/services/backup) bietet Dienste zum Sichern und Wiederherstellen von Daten und ist Teil der OMS-Suite von Produkten und Diensten.
Es schützt Ihre Anwendungsdaten und bewahrt sie jahrelang ohne Kapitaleinsatz und mit minimalen Betriebskosten auf. Mit diesem Dienst können neben Anwendungsworkloads wie SQL Server und SharePoint Daten von physischen und virtuellen Windows-Servern gesichert werden. Zudem kann er von [System Center Data Protection Manager (DPM)](https://en.wikipedia.org/wiki/System_Center_Data_Protection_Manager) verwendet werden, um geschützte Daten zur Redundanz und langfristigen Speicherung in Azure zu replizieren.


Geschützte Daten werden in Azure Backup in einem Sicherungstresor gespeichert, der sich in einer bestimmten geografischen Region befindet. Die Daten werden innerhalb der gleichen Region repliziert und können je nach Art des Tresors auch in eine andere Region repliziert werden, um eine höhere Resilienz zu erzielen.

### <a name="management-solutions"></a>Verwaltungslösungen
[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Ihnen das Verwalten und Schützen Ihrer Infrastruktur lokal und in der Cloud erleichtert.


[Verwaltungslösungen](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) sind vorgefertigte Logiksätze zur Implementierung eines bestimmten Verwaltungsszenarios unter Verwendung mindestens eines OMS-Diensts. Von Microsoft und von Partnern werden verschiedene Lösungen angeboten, die Sie problemlos Ihrem Azure-Abonnement hinzufügen und dadurch den Nutzen Ihrer OMS-Investition erhöhen können. Partner können eigene Lösungen zur Unterstützung ihrer Anwendungen und Dienste erstellen und sie Benutzern über den Azure Marketplace oder über Schnellstartvorlagen zur Verfügung stellen.


![Verwaltungslösungen](./media/azure-operational-security/azure-operational-security-fig4.png)

Die [Lösung für die Updateverwaltung](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management) ist ein gutes Beispiel für eine Lösung, die mehrere Dienste verwendet, um zusätzliche Funktionen bereitzustellen. Diese Lösung verwendet den [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)-Agent für Windows und Linux, um Informationen zu erforderlichen Updates für die einzelnen Agents zu sammeln. Die Daten werden in das Log Analytics-Repository geschrieben, wo Sie sie über ein enthaltenes Dashboard analysieren können.

Wenn Sie eine Bereitstellung erstellen, werden erforderliche Updates mithilfe von Runbooks in [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) installiert. Der gesamte Prozess wird über das Portal verwaltet, und Sie müssen sich keine Gedanken über die zugrunde liegenden Details machen.

## <a name="azure-security-center"></a>Azure Security Center

Azure Security Center unterstützt Sie beim Schützen Ihrer Azure-Ressourcen. Es bietet eine integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements. Innerhalb des Diensts können Sie Richtlinien nicht nur für Ihre Azure-Abonnements definieren, sondern auch für [Ressourcengruppen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups), sodass Sie differenzierter vorgehen können.

### <a name="security-policies-and-recommendations"></a>Sicherheitsrichtlinien und -empfehlungen

In einer Sicherheitsrichtlinie wird der Satz von Sicherheitsmechanismen definiert, die für Ressourcen in dem angegebenen Abonnement oder der angegebenen Ressourcengruppe zu empfehlen sind.

In Security Center definieren Sie Richtlinien auf Grundlage der Sicherheitsanforderungen Ihres Unternehmens sowie auf Grundlage der Art von Anwendungen oder der Vertraulichkeit der Daten.

![Sicherheitsrichtlinien und -empfehlungen](./media/azure-operational-security/azure-operational-security-fig5.png)


Auf Abonnementebene aktivierte Richtlinien werden automatisch an alle Ressourcen innerhalb des Abonnements verteilt. Dies ist im Diagramm auf der rechten Seite dargestellt:


### <a name="data-collection"></a>Datensammlung

Security Center sammelt Daten von Ihren virtuellen Computern (VMs), um den Sicherheitsstatus zu bewerten, Sicherheitsempfehlungen bereitzustellen und vor Bedrohungen zu warnen. Beim ersten Zugriff auf Security Center wird die Datensammlung für alle virtuellen Computer in Ihrem Abonnement aktiviert. Die Datensammlung wird zwar empfohlen, kann aber in der Security Center-Richtlinie deaktiviert werden.

### <a name="data-sources"></a>Datenquellen

- Azure Security Center analysiert Daten aus den folgenden Quellen, um über den Sicherheitsstatus zu informieren, Sicherheitslücken zu identifizieren, Gegenmaßnahmen zu empfehlen und aktive Bedrohungen zu erkennen:

-   Azure Services: Verwendet Informationen zur Konfiguration von Azure-Diensten, die Sie bereitgestellt haben. Hierzu wird mit dem Ressourcenanbieter des Diensts kommuniziert.

- Netzwerkdatenverkehr: Verwendet Metadatenstichproben des Netzwerkdatenverkehrs aus der Infrastruktur von Microsoft wie etwa Quelle/Ziel, IP/Port, Paketgröße und Netzwerkprotokoll.

-   Partnerlösungen: Verwendet Sicherheitswarnungen von integrierten Partnerlösungen (beispielsweise Firewalls und Antischadsoftwarelösungen).

-   Ihre virtuellen Computer: Verwendet Konfigurationsinformationen und Informationen zu Sicherheitsereignissen – beispielsweise Windows-Ereignis- und Überwachungsprotokolle, IIS-Protokolle, Syslog-Nachrichten und Absturzabbilddateien von Ihren virtuellen Computern.

### <a name="data-protection"></a>Datenschutz

Azure Security Center erfasst und verarbeitet sicherheitsbezogene Daten (einschließlich Konfigurationsinformationen, Metadaten, Ereignisprotokolle, Absturzabbilddateien und Ähnliches), um Kunden bei der Vermeidung, Erkennung und Behandlung von Bedrohungen zu unterstützen. Microsoft hält strenge Compliance- und Sicherheitsrichtlinien ein – angefangen bei der Codierung bis hin zum Betreiben von Diensten.

-   **Trennung von Daten:**Daten werden für jede Komponente des Diensts logisch getrennt verwaltet. Sämtliche Daten werden nach Organisation gekennzeichnet. Dieser Kennzeichnung wird während des gesamten Datenlebenszyklus beibehalten und auf jeder Ebene des Diensts erzwungen.

-   **Datenzugriff**: Bei der Bereitstellung von Sicherheitsempfehlungen sowie bei der Untersuchung potenzieller Sicherheitsrisiken greifen Mitarbeiter von Microsoft unter Umständen auf Informationen zu, die von Azure-Diensten erfasst oder analysiert wurden. Hierzu zählen etwa Absturzabbilddateien, Prozesserstellungsereignisse, Momentaufnahmen von VM-Datenträgern und Artefakte. Diese können ggf. Kundendaten oder persönliche Informationen von Ihren virtuellen Computern enthalten. Wir halten uns an die [Microsoft Online Services-Bedingungen und Datenschutzerklärung](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). Darin ist festgelegt, dass Microsoft keine Kundendaten oder daraus abgeleiteten Informationen zu Werbezwecken oder anderen kommerziellen Zwecken verwendet.

-   **Datennutzung:** Microsoft nutzt mandantenübergreifende Muster und Informationen zu Bedrohungen (Threat Intelligence), um die Funktionen für Prävention und Erkennung zu verbessern. Dies erfolgt in Übereinstimmung mit den in unserer [Datenschutzerklärung](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx) beschriebenen Datenschutzzusagen.

### <a name="data-location"></a>Speicherort der Daten

Azure Security Center erfasst kurzlebige Kopien Ihrer Absturzabbilddateien und analysiert sie, um nach Spuren von Exploitversuchen und erfolgreichen Kompromittierungen zu suchen. Azure Security Center führt diese Analyse in dem geografischen Raum durch, in dem sich auch der Arbeitsbereich befindet, und löscht die kurzlebigen Kopien nach Abschluss der Analyse. Computerartefakte werden zentral in der Region gespeichert, in der sich auch der virtuelle Computer befindet.

-   **Ihre Speicherkonten**: Ein Speicherkonto wird für jede Region angegeben, in der virtuelle Computer ausgeführt werden. So können Sie Daten in derselben Region speichern, in der auch der virtuelle Computer angeordnet ist, von dem die Daten erfasst werden.

-   **Azure Security Center-Speicher:** Informationen zu Sicherheitswarnungen (einschließlich Partnerwarnungen, Empfehlungen und Informationen zum Sicherheitsintegritätsstatus) werden zentral gespeichert (aktuell in den USA). Diese Informationen können auch verwandte Konfigurationsinformationen und Sicherheitsereignisse umfassen, die je nach Bedarf von Ihren virtuellen Computern erfasst werden, um für Sie die Sicherheitswarnung, die Empfehlung oder den Sicherheitsintegritätsstatus bereitzustellen.


## <a name="azure-monitor"></a>Azure Monitor

Mit der Sicherheits- und Überwachungslösung von [OMS](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) kann die IT-Abteilung aktiv alle Ressourcen überwachen. Dies kann zur Minimierung der Auswirkungen von Sicherheitsvorfällen beitragen. Die Sicherheits- und Überwachungslösung von OMS verfügt über Sicherheitsdomänen, die zum Überwachen von Ressourcen verwendet werden können. Die Sicherheitsdomäne ermöglicht den schnellen Zugriff auf Optionen. Für die Sicherheitsüberwachung werden die folgenden Domänen ausführlicher beschrieben:

-   Bewertung von Schadsoftware
-   Updatebewertung
-   Identität und Zugriff

Azure Monitor enthält Verweise auf Informationen zu bestimmten Ressourcentypen. Es bietet Visualisierung, Abfrage, Weiterleitung, Warnung, automatische Skalierung und Automatisierung für Daten sowohl aus der Azure-Infrastruktur (Aktivitätsprotokoll) als auch aus jeder einzelnen Azure-Ressource (Diagnoseprotokolle).

![Azure Monitor](./media/azure-operational-security/azure-operational-security-fig6.png)


Cloudanwendungen sind komplexe Systeme mit zahlreichen Variablen. Die Überwachung stellt Daten bereit, auf deren Grundlage die ordnungsgemäße Ausführung der Anwendung sichergestellt werden kann. Sie trägt auch zur Vermeidung potenzieller Probleme bei und hilft bei der Behandlung bereits aufgetretener Probleme.

Darüber hinaus können Sie auf der Grundlage von Überwachungsdaten umfassende Erkenntnisse über Ihre Anwendung gewinnen. Mithilfe dieser Kenntnisse können Sie die Leistung oder Wartungsfreundlichkeit der Anwendung verbessern oder Aktionen automatisieren, die andernfalls manuell ausgeführt werden müssten.

### <a name="azure-activity-log"></a>Azure-Aktivitätsprotokoll


Das Aktivitätsprotokoll bietet Einblicke in Vorgänge, die für Ressourcen Ihres Abonnements durchgeführt wurden. Das Aktivitätsprotokoll wurde bisher als „Überwachungsprotokolle“ oder „Vorgangsprotokolle“ bezeichnet, da es Ereignisse der Steuerungsebene für Ihre Abonnements enthält.

![Azure-Aktivitätsprotokoll](./media/azure-operational-security/azure-operational-security-fig7.png)

Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die für die Ressourcen Ihres Abonnements durchgeführt wurden. Sie können auch den Status des Vorgangs und andere relevante Eigenschaften verstehen. Das Aktivitätsprotokoll umfasst keine Lesevorgänge (GET) oder Vorgänge für Ressourcen, die das klassische Modell verwenden.

### <a name="azure-diagnostic-logs"></a>Azure-Diagnoseprotokolle

Diese Protokolle werden von einer Ressource ausgegeben und stellen umfangreiche, in kurzen Abständen erfasste Betriebsdaten der Ressource bereit. Der Inhalt dieser Protokolle variiert je nach Ressourcentyp.

Windows-Systemereignisprotokolle sind z.B. eine Kategorie des Diagnoseprotokolls für virtuelle Computer und Blob-, Tabellen- und Warteschlangenprotokolle sind Kategorien der Diagnoseprotokolle für Speicherkonten.

Diagnoseprotokolle unterscheiden sich vom [Aktivitätsprotokoll (früher als Überwachungsprotokoll, oder Betriebsprotokoll bezeichnet)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Das Aktivitätsprotokoll bietet Einblicke in Vorgänge, die für Ressourcen Ihres Abonnements durchgeführt wurden. Diagnoseprotokolle bieten Einblick in Vorgänge, die Ihre Ressource selbst ausgeführt hat.

### <a name="metrics"></a>Metriken

Mit Azure Monitor können Sie Telemetriedaten verwenden, um sich einen Überblick über Leistung und Integrität Ihrer Workloads in Azure zu verschaffen. Die wichtigsten Typen von Telemetriedaten sind Metriken (auch Leistungsindikatoren genannt), die von den meisten Azure-Ressourcen ausgegeben werden. Azure Monitor bietet Ihnen verschiedene Möglichkeiten, diese [Metriken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) für die Überwachung und Problembehandlung zu konfigurieren und zu nutzen. Metriken sind eine wertvolle Quelle für Telemetriedaten, mit denen Sie folgende Aufgaben ausführen können:

-   **Verfolgen der Leistung** Ihrer Ressourcen (z.B. virtuelle Computer, Websites oder Logik-Apps) durch Darstellung der entsprechenden Metriken in einem Diagramm im Portal, das Sie an Ihr Dashboard anheften.

-   **Erhalten von Benachrichtigungen über Probleme**, die die Leistung Ihrer Ressourcen beeinträchtigen, wenn ein Metrikwert einen bestimmten Schwellenwert überschreitet.

-   **Konfigurieren automatisierter Aktionen**, z. B. der automatischen Skalierung einer Ressource oder der Auslösung eines Runbooks, wenn ein Metrikwert einen bestimmten Schwellenwert überschreitet.

-   **Ausführen fortgeschrittener Analysen** oder Erstellen von Berichten zu Leistungs- oder Nutzungstrends für Ihre Ressource.

-   **Archivieren** des Leistungs- oder Integritätsverlaufs Ihrer Ressourcen zu Kompatibilitäts- oder Überwachungszwecken.

### <a name="azure-diagnostics"></a>Azure-Diagnose

Sie ist eine Funktion in Azure, mit der Diagnosedaten für eine bereitgestellte Anwendung erfasst werden können. Sie können die Diagnoseerweiterung von einer Reihe verschiedener Quellen aus verwenden. Derzeit werden die [Web- und Workerrollen des Azure-Clouddiensts](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/windows/overview) unter Microsoft Windows und [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) unterstützt. Andere Azure-Dienste verwenden ihre eigenen separaten Diagnosefunktionen.

## <a name="azure-network-watcher"></a>Azure Network Watcher

Die Überwachung der Netzwerksicherheit ist ein entscheidender Faktor zur Erkennung von Sicherheitsrisiken im Netzwerk und zur Compliance mit Ihrem Modell für die IT-Sicherheit und bestimmende Governance. Mithilfe der Ansicht für Sicherheitsgruppen können Sie die konfigurierte Netzwerksicherheitsgruppe und Sicherheitsregeln sowie die effektiven Sicherheitsregeln abrufen. Mit den angewendeten Regeln der Liste können Sie die offenen Ports ermitteln und das Netzwerksicherheitsrisiko bewerten.

[Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) ist ein regionaler Dienst, mit dem Sie Bedingungen auf Netzwerkebene in Azure überwachen und diagnostizieren können. Die Tools zur Netzwerkdiagnose und -visualisierung von Network Watcher helfen Ihnen dabei, Ihr Netzwerk in Azure zu verstehen, Diagnosen durchzuführen und Einblicke zu gewinnen. Dieser Dienst umfasst die Paketerfassung, „Nächster Hop“, die IP-Datenflussüberprüfung, die Sicherheitsgruppenansicht und NSG-Datenflussprotokolle. Die Überwachung auf Szenarioebene bietet im Gegensatz zur Überwachung einzelner Netzwerkressourcen eine End-to-End-Ansicht der Netzwerkressourcen.

![Azure Network Watcher](./media/azure-operational-security/azure-operational-security-fig8.png)

Network Watcher verfügt derzeit über die folgenden Funktionen:

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview">Überwachungsprotokolle</a>**: Im Rahmen der Konfiguration von Netzwerken durchgeführte Vorgänge werden protokolliert. Diese Protokolle können im Azure-Portal angezeigt oder mithilfe von Microsoft-Tools wie Power BI oder Drittanbietertools abgerufen werden. Die Überwachungsprotokolle sind über das Verwaltungsportal, PowerShell, die Befehlszeilenschnittstelle und die Rest-API verfügbar. Weitere Informationen zu Überwachungsprotokollen finden Sie unter „Überwachen von Vorgängen mit Resource Manager“. Überwachungsprotokolle stehen für Vorgänge zur Verfügung, die für alle Netzwerkressourcen durchgeführt werden.


-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview">IP-Datenflussüberprüfung</a>**: Überprüft basierend auf 5-Tupel-Paketparametern (Ziel-IP, Quell-IP, Zielport, Quellport und Protokoll) anhand der Datenflussinformationen, ob ein Paket zugelassen oder verweigert wird. Wenn das Paket durch eine Netzwerksicherheitsgruppe verweigert wird, werden die Namen der Regel und der Netzwerksicherheitsgruppe, die das Paket verweigert haben, zurückgegeben.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview">Nächster Hop:</a>** ermittelt den nächsten Hop für Pakete, die im Azure-Netzwerkfabric weitergeleitet werden, sodass Sie eine Diagnose auf falsch konfigurierte benutzerdefinierte Routen durchführen können.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview">Sicherheitsgruppenansicht:</a>** ruft die geltenden und angewendeten Sicherheitsregeln ab, die auf einen virtuellen Computer angewendet werden.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview">NSG-Datenflussprotokollierung:</a>** Mithilfe von Datenflussprotokollen für Netzwerksicherheitsgruppen können Sie Protokolle zum Datenverkehr erfassen, der von den Sicherheitsregeln in der Gruppe zugelassen oder verweigert wird. Der Datenfluss wird durch 5-Tupel-Informationen definiert: Quell-IP-Adresse, Ziel-IP-Adresse, Quellport, Zielport und Protokoll.

## <a name="azure-storage-analytics"></a>Azure-Speicheranalyse

Von der [Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) können Metriken gespeichert werden, zu denen aggregierte Transaktionsstatistiken und Kapazitätsdaten für die an einen Speicherdienst gesendeten Anforderungen zählen. Berichte zu Transaktionen werden sowohl auf API-Vorgangsebene als auch auf Speicherdienstebene erstellt, während Berichte zur Kapazität nur auf Speicherdienstebene erstellt werden. Mit den Metrikdaten können die Speicherdienstnutzung analysiert, Probleme mit Anforderungen für den Speicherdienst diagnostiziert und die Leistung von Anwendungen verbessert werden, die einen Dienst verwenden.

Die [Azure-Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) führt die Protokollierung aus und stellt Metrikdaten für ein Speicherkonto bereit. Mit diesen Daten können Sie Anforderungen verfolgen, Verwendungstrends analysieren und Probleme mit dem Speicherkonto diagnostizieren. Die Protokollierung der Speicheranalyse ist für [Blob-, Warteschlangen- und Tabellenspeicherdienste](https://docs.microsoft.com/azure/storage/storage-introduction) nicht verfügbar. Die Speicheranalyse protokolliert ausführliche Informationen zu erfolgreichen und nicht erfolgreichen Anforderungen an einen Speicherdienst.

Anhand dieser Informationen können einzelne Anforderungen überwacht und Probleme mit einem Speicherdienst untersucht werden. Anforderungen werden auf Grundlage der besten Leistung protokolliert. Protokolleinträge werden nur erstellt, wenn Anforderungen für den Dienstendpunkt gestellt wurden. Wenn beispielsweise ein Speicherkonto Aktivität im Blob-Endpunkt, jedoch nicht im Tabellen- oder Warteschlangenendpunkt aufweist, werden nur Protokolle für den Blob-Dienst erstellt.

Zum Verwenden der Speicheranalyse müssen Sie sie einzeln für jeden zu überwachenden Dienst aktivieren. Sie können sie im [Azure-Portal](https://portal.azure.com/) aktivieren. Details finden Sie unter [Überwachen eines Speicherkontos im Azure-Portal](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Sie können die Speicheranalyse auch programmgesteuert über die REST-API oder die Clientbibliothek aktivieren. Verwenden Sie den Vorgang „Diensteigenschaften festlegen“, um die Speicheranalyse für jeden Dienst einzeln zu aktivieren.

Die aggregierten Daten werden in einem bekannten BLOB (zur Protokollierung) und in bekannten Tabellen (als Metrik) gespeichert. Der Zugriff erfolgt über APIs für den BLOB-Dienst und Tabellendienst.

Bei der Speicheranalyse ist die Menge der gespeicherten Daten auf 20 TB beschränkt. Diese Beschränkung gilt unabhängig vom Gesamtlimit für Ihr Speicherkonto. Alle Protokolle werden in [Block-Blobs](https://docs.microsoft.com/azure/storage/storage-analytics) in einem Container namens „$logs“ gespeichert, der automatisch erstellt wird, wenn die Speicheranalyse für ein Speicherkonto aktiviert ist.

Die folgenden Aktionen der Speicheranalyse sind gebührenpflichtig:

-   Anforderungen zum Erstellen von Blobs für die Protokollierung
-   Anforderungen zum Erstellen von Tabellenentitäten für Metriken

> [!Note]
> Weitere Informationen zu Abrechnungs- und Datenaufbewahrungsrichtlinien finden Sie unter [Speicheranalyse und Speicheranalysekosten](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> Zur Optimierung der Leistung sollten Sie die Anzahl stark ausgelasteter Datenträger begrenzen, die an den virtuellen Computer angefügt sind, um eine mögliche Drosselung zu vermeiden. Wenn nicht alle Datenträger gleichzeitig hoch ausgelastet sind, kann das Speicherkonto eine höhere Anzahl von Datenträgern unterstützen.

> [!Note]
> Weitere Informationen zu Speicherkontobegrenzungen finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage](https://docs.microsoft.com/azure/storage/storage-scalability-targets).


Die folgenden Typen authentifizierter und anonymer Anforderungen werden protokolliert.

| Authentifiziert  | Anonym|
| :------------- | :-------------|
| Erfolgreiche Anforderungen | Erfolgreiche Anforderungen |
|Fehlerhafte Anforderungen, einschließlich Timeout-, Drosselungs-, Netzwerk- und Autorisierungsfehler sowie anderer Fehler | Anforderungen mithilfe einer SAS (Shared Access Signature), einschließlich fehlerhafter und erfolgreicher Anforderungen |
| Anforderungen mithilfe einer SAS (Shared Access Signature), einschließlich fehlerhafter und erfolgreicher Anforderungen |Timeoutfehler für Client und Server |
|   Anforderungen von Analysedaten |    Mit Fehlercode 304 (Nicht geändert) misslungene GET-Anforderungen |
| Anforderungen, die durch die Speicheranalyse selbst erfolgen, z. B. Protokollerstellung oder -löschung, werden nicht protokolliert. Eine vollständige Liste der protokollierten Daten ist in den Themen [Protokollierte Speicheranalysevorgänge und Statusmeldungen](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) und [Protokollformat der Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) dokumentiert. | Alle anderen misslungenen anonymen Anforderungen werden nicht protokolliert. Eine vollständige Liste der protokollierten Daten ist in den Themen [Protokollierte Speicheranalysevorgänge und Statusmeldungen](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) und [Protokollformat der Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) dokumentiert. |
## <a name="azure-active-directory"></a>Azure Active Directory

Azure AD bietet auch einen vollständigen Satz von Identitätsverwaltungsfunktionen, z. B.: mehrstufige Authentifizierung, Geräteregistrierung, Self-Service-Kennwortverwaltung, Self-Service-Gruppenverwaltung, privilegierte Kontenverwaltung, rollenbasierte Zugriffssteuerung, Überwachung der Anwendungsnutzung, umfassende Überwachungsfunktionen sowie Sicherheitsüberwachung und -warnungen.

-   Erhöhen Sie die Sicherheit von Anwendungen mit der mehrstufigen Authentifizierung und dem bedingten Zugriff von Azure AD.

-   Überwachen Sie die Anwendungsnutzung, und schützen Sie Ihr Unternehmen mit Funktionen für Sicherheitsberichte und -überwachung vor Bedrohungen.

Azure Active Directory (Azure AD) umfasst Sicherheits-, Aktivitäts- und Prüfberichte für Ihr Verzeichnis. Der [Azure Active Directory-Überwachungsbericht](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) hilft Kunden, privilegierte Aktionen zu bestimmen, die in ihrem Azure Active Directory aufgetreten sind. Privilegierte Aktionen umfassen Änderungen zur Rechteerweiterung (z. B. das Erstellen von Rollen oder Zurücksetzen von Kennwörtern), das Ändern von Richtlinienkonfigurationen (z. B. Kennwortrichtlinien) oder Änderungen an der Verzeichniskonfiguration (z. B. Änderungen an Domänenverbundeinstellungen).

Die Berichte enthalten den Überwachungsdatensatz für den Ereignisnamen, den Akteur, der die Aktion ausgeführt hat, die von der Änderung betroffene Zielressource sowie Datum und Uhrzeit (in UTC). Kunden können die Liste mit den Überwachungsereignissen für ihre Azure Active Directory-Instanz über das [Azure-Portal](https://portal.azure.com/) abrufen, wie in [Anzeigen von Überwachungsprotokollen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal) beschrieben. Hier sehen Sie eine Liste der enthaltenen Berichte:

| Sicherheitsberichte  | Aktivitätsberichte| Prüfberichte |
| :------------- | :-------------| :-------------|
|Anmeldungen von unbekannten Quellen | Anwendungsnutzung: Zusammenfassung | Verzeichnisprüfbericht |
|Anmeldungen nach mehreren Fehlern | Anwendungsnutzung: detailliert |   |
|Anmeldungen aus mehreren geografischen Regionen | Anwendungsdashboard |  |
|Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten |Kontobereitstellungsfehler |  |
|Irreguläre Anmeldeaktivitäten |Geräte einzelner Benutzer |  |
|Anmeldungen von möglicherweise infizierten Geräten |Aktivität einzelner Benutzer |   |
|Benutzer mit anomalen Anmeldeaktivitäten |Gruppenaktivitätsbericht |   |
| |Bericht zur Registrierung für die Kennwortzurücksetzung |   |
| |Kennwortzurücksetzungsaktivität |   | |



Die Daten dieser Berichte können für Ihre Anwendungen – beispielsweise SIEM-Systeme, Überwachungs- und Business Intelligence-Tools – nützlich sein. Die Azure AD-Berichterstellungs-[APIs](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) bieten über einen Satz von REST-basierten APIs programmgesteuerten Zugriff auf die Daten. Sie können diese APIs über verschiedene Programmiersprachen und Tools aufrufen.

Ereignisse im Azure AD-Überwachungsbericht werden für 180 Tage beibehalten.

> [!Note]
> Weitere Informationen zur Aufbewahrung von Berichten finden Sie unter [Aufbewahrungsrichtlinien für Azure Active Directory-Berichte](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Kunden, die ihre [Überwachungsereignisse](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) für längere Zeit speichern möchten, können mithilfe der Reporting-API regelmäßig Überwachungsereignisse in einen separaten Datenspeicher abrufen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird das Schützen Ihrer Privatsphäre und Ihrer Daten zusammengefasst, während Software und Dienste bereitgestellt werden, die Sie beim Verwalten der IT-Infrastruktur Ihres Unternehmens unterstützen. Wenn sie ihre Daten anderen anvertrauen, setzt dieses Vertrauen strikte Sicherheit voraus. Dies ist Microsoft bekannt. Microsoft hält strenge Compliance- und Sicherheitsrichtlinien ein – angefangen bei der Codierung bis hin zum Betreiben von Diensten. Das Sichern und Schützen von Daten besitzt bei Microsoft höchste Priorität.

In diesem Artikel wird Folgendes erläutert:

-   Wie Daten in der Operations Management Suite (OMS) gesammelt, verarbeitet und geschützt werden.

-   Schnelles Analysieren von Ereignissen über mehrere Datenquellen hinweg. Ermitteln von Sicherheitsrisiken, und besseres verstehen der Auswirkungen und betroffenen Bereiche von Bedrohungen und Angriffen, um Schäden bei Sicherheitsverletzungen zu minimieren.

-   Erkennen von Angriffsmustern durch die Visualisierung von ausgehendem böswilligen IP-Datenverkehr und von Bedrohungsarten. Verstehen der Sicherheitsstellung Ihrer gesamten Umgebung – unabhängig von der Plattform.

-   Erfassen aller Protokoll- und Ereignisdaten, die für die Sicherheits- oder Complianceüberwachung erforderlich sind. Reduzieren des Zeit- und Ressourcenaufwands für eine Sicherheitsüberwachung durch einen umfassenden, durchsuchbaren und exportierbaren Satz an Protokoll- und Ereignisdaten.

<ul>
<li>Sammeln sicherheitsbezogener Ereignisse und Ausführen von auf Überwachung und Sicherheitsverletzungen bezogener Analysen, um Ihre Ressourcen fest im Blick zu haben:</li>
<ul>
<li>Sicherheitsstatus</li>
<li>Relevantes Problem</li>
<li>Zusammenfassung der Bedrohungen</li>
</ul>
</ul>

## <a name="next-steps"></a>Nächste Schritte

- [Gestaltung und betriebsbedingte Sicherheit](https://www.microsoft.com/trustcenter/security/designopsecurity)

Microsoft entwickelt seine Dienste und Software in Hinblick auf die Sicherheit, um sicherzustellen, dass seine Cloudinfrastruktur resilient und vor Angriffen geschützt ist.

- [Operations Management Suite | Security & Compliance](https://www.microsoft.com/cloud-platform/security-and-compliance)

Verwenden Sie Microsoft-Sicherheitsdaten und -analysen, um eine intelligentere und effektivere Bedrohungserkennung vorzunehmen.

- [Planung und Betrieb für Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide): Eine Reihe von Schritten und Aufgaben, mit denen Sie die Verwendung von Azure Security Center auf Grundlage der Sicherheitsanforderungen und des Cloudverwaltungsmodells Ihres Unternehmens optimieren können.


