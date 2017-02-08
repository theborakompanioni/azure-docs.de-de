---
title: Service Map in der Operations Management Suite (OMS) | Microsoft Docs
description: "Service Map ist eine Lösung in der Operations Management Suite (OMS), die automatisch Anwendungskomponenten auf Windows- und Linux-Systemen ermittelt und die Kommunikation zwischen Diensten abbildet.  Dieser Artikel enthält Informationen zum Bereitstellen von Service Map in Ihrer Umgebung und zur Verwendung der Lösung in einer Vielzahl von Szenarien."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: 3ceb84cc-32d7-4a7a-a916-8858ef70c0bd
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/22/2016
ms.author: daseidma;bwren;dairwin
translationtype: Human Translation
ms.sourcegitcommit: 6cc30ace0b57555ea2b5815906d3e6a4f79d8fce
ms.openlocfilehash: 505b4e3b7862657bcfd8eea1755677104a9b68eb


---

# <a name="using-service-map-solution-in-operations-management-suite-oms"></a>Verwenden von Service Map in der Operations Management Suite (OMS)
Service Map ermittelt automatisch Anwendungskomponenten auf Windows- und Linux-Systemen und stellt die Kommunikation zwischen Diensten dar. In dieser Lösung können Sie die Server ihrer Funktion gemäß anzeigen – als verbundene Systeme, die wichtige Dienste bereitstellen.  Service Map zeigt Verbindungen zwischen Servern, Prozessen und Ports über die gesamte TCP-Verbindungsarchitektur an. Außer der Installation eines Agents ist keine weitere Konfiguration erforderlich.

In diesem Artikel werden die Details der Verwendung von Service Map beschrieben.  Weitere Informationen zum Konfigurieren von Service Map und zum Onboarding von Agents finden Sie unter [Konfigurieren von Service Map in der Operations Management Suite (OMS)](operations-management-suite-service-map-configure.md).


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Anwendungsfälle: Berücksichtigen von Abhängigkeiten in IT-Prozessen

### <a name="discovery"></a>Ermittlung
Service Map erstellt automatisch eine allgemeine Referenzzuordnung der Abhängigkeiten für all Ihre Server, Prozesse und Drittanbieterdienste.  Service Map ermittelt alle TCP-Abhängigkeiten und bildet diese ab. Dabei werden unerwartete Verbindungen, von Ihnen benötigte Remotesysteme von Drittanbietern und Abhängigkeiten von Elementen in traditionell weniger transparenten Netzwerkbereichen wie z.B. DNS und AD identifiziert.  Service Map ermittelt fehlerhafte Netzwerkverbindungen, die Ihre verwalteten Systeme herzustellen versuchen, sodass Sie mögliche Fehlkonfigurationen der Server, Dienstausfälle und Netzwerkprobleme erkennen können.

### <a name="incident-management"></a>Incident Management
Service Map macht Schluss mit dem Rätselraten bei der Problemermittlung und zeigt Ihnen, wie Ihre Systeme miteinander verbunden sind und sich gegenseitig beeinflussen.  Sie erhalten nicht nur Informationen zu fehlerhaften Verbindungen, sondern auch zu verbundenen Clients, sodass Sie falsch konfigurierte Lastenausgleichsmodule, eine unerwartete oder übermäßige Auslastung kritischer Dienste und eine nicht autorisierte Clientkommunikation – z.B. zwischen Entwicklercomputern und Produktionssystemen – identifizieren können.  Dank integrierter Workflows und OMS-Änderungsnachverfolgung können Sie erkennen, ob sich die Ursache eines Incidents durch ein Änderungsereignis auf einem Back-End-Computer oder in einem Back-End-Dienst erklären lässt.

### <a name="migration-assurance"></a>Sichere Migrationen
Mit Service Map können Sie Azure-Migrationen effektiv planen, beschleunigen und überprüfen. So stellen Sie sicher, dass nichts vergessen wird, und keine unerwarteten Ausfälle auftreten.  Sie können alle voneinander abhängigen Systeme ermitteln, die gemeinsam migriert werden müssen, die Systemkonfiguration und -kapazität bewerten und herausfinden, ob ein ausgeführtes System noch benötigt wird oder eher stillgelegt als migriert werden sollte.  Nach der Migration können Sie die Clientauslastung und -identität überprüfen, um sicherzustellen, dass Testsysteme und Kunden eine Verbindung herstellen können.  Wenn bei der Subnetzplanung und den Firewalldefinitionen Probleme auftreten, weisen Verbindungsfehler in den Service Map-Zuordnungen auf die Systeme hin, die Konnektivität benötigen.

### <a name="business-continuity"></a>Geschäftskontinuität
Wenn Sie Azure Site Recovery verwenden und Hilfe beim Definieren der Wiederherstellungsreihenfolge für Ihre Anwendungsumgebung benötigen, kann der Service Map Ihnen automatisch zeigen, welche Systeme auf welche Weise voneinander abhängig sind, damit Sie sicherstellen können, dass Ihr Wiederherstellungsplan zuverlässig ist.  Wählen Sie einen wichtigen Server aus, und zeigen Sie die zugehörigen Clients an, um die Front-End-Systeme zu identifizieren, die erst dann wiederhergestellt werden sollten, wenn der wichtige Server wiederhergestellt und verfügbar ist.  Umgekehrt können Sie sich die Back-End-Abhängigkeiten eines wichtigen Servers ansehen, um die Systeme zu identifizieren, die wiederhergestellt werden müssen, bevor das untersuchte System wiederhergestellt werden kann.

### <a name="patch-management"></a>Patchverwaltung
Service Map erweitert die Verwendung des OMS-Tools zur Bewertung von Systemupdates durch die Anzeige, welche anderen Teams und Server von Ihrem Dienst abhängig sind, damit Sie diese benachrichtigen können, bevor Sie Ihre Systeme zum Patchen herunterfahren.  Service Map erweitert auch die Patchverwaltung in der OMS durch die Anzeige, ob Ihre Dienste verfügbar und ordnungsgemäß verbunden sind, nachdem die Patches aufgespielt und die Dienste neu gestartet wurden.


## <a name="mapping-overview"></a>Übersicht über die Zuordnung
Service Map-Agents sammeln Informationen über alle über TCP verbundenen Prozesse auf dem Server, auf dem sie installiert sind, sowie Informationen zu den eingehenden und ausgehenden Verbindungen für jeden Prozess.  Aus der Computerliste auf der linken Seite der Service Map-Lösung können Computer mit Service Map-Agents ausgewählt werden, um deren Abhängigkeiten während eines ausgewählten Zeitraums zu visualisieren.  Die Abhängigkeitszuordnungen gelten für einen bestimmten Computer und zeigen alle Computer an, die direkte TCP-Clients oder -Server dieses Computers sind.

![Service Map-Übersicht](media/oms-service-map/service-map-overview.png)

Computer können in der Zuordnung erweitert werden, um die ausgeführten Prozesse mit aktiven Netzwerkverbindungen während des ausgewählten Zeitraums anzuzeigen.  Wenn ein Remotecomputer mit einem Service Map-Agent erweitert wird, um Details zu den Prozessen anzuzeigen, werden nur die Prozesse angezeigt, die mit dem untersuchten Computer kommunizieren.  Die Anzahl der Front-End-Computer ohne Agents, die eine Verbindung mit dem untersuchten Computer herstellen, wird links neben den Prozessen angegeben, mit denen eine Verbindung hergestellt wird.  Wenn der untersuchte Computer eine Verbindung mit einem Back-End-Computer ohne Agent herstellt, wird dieses Back-End in der Zuordnung durch einen Knoten dargestellt, und der Knoten kann erweitert werden, um einzelne Ports und Dienste anzuzeigen, mit denen der untersuchte Computer kommuniziert.

Standardmäßig zeigt Service Map Abhängigkeitsinformationen der letzten 10 Minuten an.  Mithilfe der Zeitauswahl in der linken oberen Ecke können Zuordnungen nach historischen Zeiträumen (max. eine Stunde) abgefragt werden, um zu zeigen, wie die Abhängigkeiten in der Vergangenheit aussahen, z.B. während eines Incidents oder vor einer Änderung.    Service Map-Daten werden in kostenpflichtigen Arbeitsbereichen 30 Tage lang gespeichert, in kostenlosen Arbeitsbereichen 7 Tage lang.

![Computerzuordnung mit ausgewählten Computereigenschaften](media/oms-service-map/machine-map.png)

## <a name="status-badges"></a>Statusbadges
Am unteren Rand jedes Servers in der Zuordnung wird möglicherweise eine Liste der Statusbadges mit Statusinformationen über den Server angezeigt.  Die Badges weisen darauf hin, dass relevante Informationen für den Server aus einer der OMS-Lösungsintegrationen vorliegen.  Indem Sie auf einen Badge klicken, gelangen Sie direkt zu den Statusdetails im rechten Bereich.  Aktuell sind folgende Statusbadges verfügbar: Warnungen, Änderungen, Sicherheit und Updates.

![Verbindungsfehler](media/oms-service-map/status-badges.png)

## <a name="failed-connections"></a>Verbindungsfehler
Verbindungsfehler werden in Service Map-Zuordnungen für Prozesse und Computer angezeigt. Eine gestrichelte rote Linie zeigt, dass ein Clientsystem einen Prozess oder Port nicht erreichen kann.  Verbindungsfehler werden von jedem System mit einem bereitgestellten Service Map-Agent berichtet, wenn dieses System dasjenige ist, das versucht, die fehlerhafte Verbindung herzustellen.  Service Map misst dies durch Überwachen der TCP-Sockets, die keine Verbindung herstellen können.  Der Fehler kann durch eine Firewall, eine fehlerhafte Konfiguration auf einem Client oder Server oder dadurch verursacht werden, dass ein Remotedienst nicht verfügbar ist.

![Verbindungsfehler](media/oms-service-map/failed-connections.png)

Kenntnisse zu Verbindungsfehlern können Ihnen bei der Problembehandlung, der Überprüfung einer Migration, der Sicherheitsanalyse und dem allgemeinen Verständnis der Architektur helfen.  Manchmal sind Verbindungsfehler harmlos, häufig weisen sie aber auch direkt auf ein Problem hin, z.B. wenn eine Failoverumgebung plötzlich nicht mehr erreichbar ist oder zwei Anwendungsebenen nach einer Cloudmigration nicht mehr kommunizieren können.

## <a name="context-menu"></a>Kontextmenü
Indem Sie in einem beliebigen Server auf die drei Punkte oben rechts klicken, wird das Kontextmenü für diesen Server angezeigt.

![Verbindungsfehler](media/oms-service-map/context-menu.png)

### <a name="load-server-map"></a>Serverzuordnung laden
Über „Serverzuordnung laden“ gelangen Sie zu einer neuen Zuordnung mit dem ausgewählten Server als neuem untersuchten Computer.

### <a name="showhide-self-links"></a>Selbstverlinkungen ein-/ausblenden
Über „Selbstverlinkungen einblenden“ wird der Serverknoten mit allen Selbstverlinkungen (TCP-Verbindungen, die bei Prozessen auf dem Server starten und enden) neu gezeichnet.  Wenn Selbstverlinkungen angezeigt werden, ändert sich die Option im Menü zu „Selbstverlinkungen ausblenden“, wodurch Benutzer die Anzeige der Selbstverlinkungen ein- bzw. ausschalten können.



## <a name="computer-and-process-properties"></a>Computer- und Prozesseigenschaften
In einer Service Map-Zuordnung können Sie Computer und Prozesse auswählen, um zusätzlichen Kontext zu deren Eigenschaften zu erhalten.  Folgende Informationen zu Computern können angezeigt werden: DNS-Name, IPv4-Adressen, CPU- und Arbeitsspeicherkapazität, VM-Typ, Betriebssystemversion, Uhrzeit des letzten Neustarts und die IDs ihrer OMS- und Service Map-Agents.

![Computereigenschaften](media/oms-service-map/machine-properties.png)

Prozessdetails werden aus Metadaten des Betriebssystems zu ausgeführten Prozessen erfasst: Prozessname und -beschreibung, Benutzername und Domäne (unter Windows), Firmenname, Produktname, Produktversion, Arbeitsverzeichnis, Befehlszeile und Startzeit des Prozesses.

![Prozesseigenschaften](media/oms-service-map/process-properties.png)

Der Übersichtsbereich für einen Prozess bietet zusätzliche Informationen über die Konnektivität dieses Prozesses, einschließlich Bindungsports, eingehender und ausgehender Verbindungen sowie Verbindungsfehler.

![Prozessübersicht](media/oms-service-map/process-summary.png)

## <a name="computer-summary"></a>Computerzusammenfassung
Der Computerzusammenfassungsbereich enthält eine Übersicht des Betriebssystems des Servers und der Zahl der Abhängigkeiten zusammen mit einer Vielzahl von Daten aus anderen OMS-Lösungen, einschließlich Leistungsmetriken, Änderungsnachverfolgung, Sicherheit, Updates etc.

![Computerzusammenfassung](media/oms-service-map/machine-summary.png)

## <a name="oms-change-tracking-integration"></a>Integration der OMS-Änderungsnachverfolgung
Die Integration von Service Map in die Änderungsnachverfolgung erfolgt automatisch, wenn beide Lösungen im OMS-Arbeitsbereich aktiviert und konfiguriert werden.

Der Bereich für die Nachverfolgung von Änderungen auf einem Computer zeigt eine Liste aller Änderungen an, wobei die jüngste Änderung zuerst angezeigt wird. Der Bereich bietet auch einen Link, mit dem Sie die Protokollsuche aufrufen und weitere Details anzeigen können.
![Bereich für die Änderungsnachverfolgung auf einem Computer](media/oms-service-map/change-tracking.png)

Im Folgenden sehen Sie die Detailansicht eines Konfigurationsänderungsereignisses nach Auswahl von **In Log Analytics anzeigen**.
![Konfigurationsänderungsereignis](media/oms-service-map/configuration-change-event.png)


## <a name="oms-performance-integration"></a>OMS-Leistungsintegration
Im Computerleistungsbereich werden standardmäßige Leistungsmetriken für den ausgewählten Server angezeigt.  Die Metriken umfassen CPU-Auslastung, Speicherauslastung, über das Netzwerk gesendete und empfangene Bytes und eine Liste der wichtigsten Prozesse nach über das Netzwerk gesendeten und empfangenen Bytes.
![Bereich für die Änderungsnachverfolgung auf einem Computer](media/oms-service-map/machine-performance.png)


## <a name="oms-security-integration"></a>OMS-Sicherheitsintegration
Die Integration von Service Map in Sicherheit und Überwachung erfolgt automatisch, wenn beide Lösungen im OMS-Arbeitsbereich aktiviert und konfiguriert werden.

Im Computersicherheitsbereich werden Daten aus der Sicherheits- und Überwachungslösung von OMS für den ausgewählten Server angezeigt.  Im Bereich wird ggf. eine Zusammenfassung herausragender Sicherheitsprobleme für den Server angezeigt, die während des ausgewählten Zeitraums aufgetreten sind.  Beim Klicken auf ein Sicherheitsproblem wird eine Protokollsuche nach ausführlichen Informationen zu den Sicherheitsproblemen durchgeführt.
![Bereich für die Änderungsnachverfolgung auf einem Computer](media/oms-service-map/machine-security.png)


## <a name="oms-updates-integration"></a>Integration von OMS-Updates
Die Integration von Service Map in die Updateverwaltung erfolgt automatisch, wenn beide Lösungen in Ihrem OMS-Arbeitsbereich aktiviert und konfiguriert sind.

Im Computerupdatebereich werden Daten aus der Updateverwaltungslösung von OMS für den ausgewählten Server angezeigt.  Im Bereich wird ggf. für den ausgewählten Zeitraum eine Zusammenfassung fehlender Updates für den Server angezeigt.
![Bereich für die Änderungsnachverfolgung auf einem Computer](media/oms-service-map/machine-updates.png)


## <a name="oms-alerts-integration"></a>Integration von OMS-Warnungen
Service Map ist in OMS-Warnungen integriert, um Warnungen für den ausgewählten Server anzuzeigen, die im ausgewählten Zeitraum ausgelöst werden.  Für den Server wird ein Symbol angezeigt, wenn aktuelle Warnungen vorliegen, und im Computerwarnungenbereich werden die Warnungen aufgelistet.

![Computerwarnungenbereich](media/oms-service-map/machine-alerts.png)

Beachten Sie: Damit Service Map relevante Warnungen anzeigen kann, muss die Warnungsregel so erstellt werden, dass sie für einen bestimmten Computer ausgelöst wird.  So erstellen Sie richtige Warnungen:
- Beziehen Sie eine Klausel zur Gruppierung nach Computer ein: „by Computer interval 1minute“.
- Wählen Sie die Warnung auf Basis der Metrikmessung.

![Warnungskonfiguration](media/oms-service-map/alert-configuration.png)


## <a name="log-analytics-records"></a>Log Analytics-Datensätze
Die Computer- und Prozessbestandsdaten von Service Map stehen in Log Analytics zur [Suche](../log-analytics/log-analytics-log-searches.md) zur Verfügung.  Dies kann in verschiedenen Szenarien von Nutzen sein, wie z.B. Migrationsplanung, Kapazitätsanalyse, Ermittlung und Ad-hoc-Behebung von Leistungsproblemen.

Zusätzlich zu den Datensätzen, die beim Starten eines Prozesses oder Computers oder beim Onboarding in Service Map generiert werden, wird pro Stunde ein Datensatz für jeden eindeutigen Computer und jeden eindeutigen Prozess generiert.  Die Eigenschaften der Datensätze sind in den folgenden Tabellen aufgeführt.  Die Felder und Werte in den ServiceMapComputer_CL-Ereignissen sind Feldern der Computerressource in der ServiceMap ARM-API zugeordnet.  Die Felder und Werte in den ServiceMapProcess_CL-Ereignissen sind Feldern der Prozessressource in der ServiceMap ARM-API zugeordnet.  Das Feld „ResourceName_s“ entspricht dem Namensfeld in der entsprechenden ARM-Ressource. Hinweis: Mit dem Anwachsen des Service Map-Features können sich diese Felder ändern.


Es gibt intern generierte Eigenschaften, mit denen Sie eindeutige Prozessen und Computer identifizieren können:

- Computer: Verwenden Sie „ResourceId“ oder „ResourceName_s“ zur eindeutigen Identifizierung eines Computers in einem OMS-Arbeitsbereich.
- Process – Verwenden Sie „ResourceId“ zur eindeutigen Identifizierung eines Prozesses in einem OMS-Arbeitsbereich. „ResourceName_s“ ist innerhalb des Kontexts des Computers, auf dem der Prozess ausgeführt wird (MachineResourceName_s), eindeutig. 

Da für einen bestimmten Prozess und Computer in einem bestimmten Zeitraum möglicherweise mehrere Datensätze vorhanden sind, können Abfragen mehrere Datensätze für denselben Computer oder Prozess zurückgeben. Wenn nur der aktuellste Datensatz zurückgegeben werden soll, fügen Sie „| dedup ResourceId“ zur Abfrage hinzu.

### <a name="servicemapcomputercl-records"></a>ServiceMapComputer_CL-Datensätze
Datensätze des Typs **ServiceMapComputer_CL** enthalten Bestandsdaten für Server mit Service Map-Agents.  Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt:

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ | *ServiceMapComputer_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | eindeutiger Bezeichner für den Computer innerhalb des Arbeitsbereichs |
| ResourceName_s | eindeutiger Bezeichner für den Computer innerhalb des Arbeitsbereichs |
| ComputerName_s | FQDN des Computers |
| Ipv4Addresses_s | eine Liste der IPv4-Adressen des Servers |
| Ipv6Addresses_s | eine Liste der IPv6-Adressen des Servers |
| DnsNames_s | Array von DNS-Namen |
| OperatingSystemFamily_s | Windows oder Linux |
| OperatingSystemFullName_s | vollständiger Name des Betriebssystems  |
| Bitness_s | Bitanzahl des Computers (32 Bit oder 64 Bit) |
| PhysicalMemory_d | Physischer Speicher in MB |
| Cpus_d | Anzahl der CPUs |
| CpuSpeed_d | CPU-Geschwindigkeit in MHz|
| VirtualizationState_s | „unknown“, „physical“, „virtual“, „hypervisor“ |
| VirtualMachineType_s | „hyperv“, „vmware“, usw. |
| VirtualMachineNativeMachineId_g | VM-ID, die vom entsprechenden Hypervisor zugewiesen wurde |
| VirtualMachineName_s | Name des virtuellen Computers |
| BootTime_t | Startzeit |



### <a name="servicemapprocesscl-type-records"></a>Datensätze des ServiceMapProcess_CL-Typs
Datensätze des Typs **ServiceMapProcess_CL** enthalten Bestandsdaten für über TCP verbundene Prozesse auf Servern mit Service Map-Agents.  Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt:

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ | *ServiceMapProcess_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | eindeutiger Bezeichner für den Prozess innerhalb des Arbeitsbereichs |
| ResourceName_s | Eindeutiger Bezeichner für den Prozess innerhalb des Computers, auf dem er ausgeführt wird|
| MachineResourceName_s | Computerressourcenname |
| ExecutableName_s | Name der ausführbaren Prozessdatei |
| StartTime_t | Prozesspool-Startzeit |
| FirstPid_d | erste PID im Prozesspool |
| Description_s | Beschreibung des Prozesses |
| CompanyName_s | Name des Unternehmens |
| InternalName_s | interner Name |
| ProductName_s | Produktname |
| ProductVersion_s | Produktversion |
| FileVersion_s | Dateiversion |
| CommandLine_s | Befehlszeile |
| ExecutablePath _s | Pfad zur ausführbaren Datei |
| WorkingDirectory_s | Arbeitsverzeichnis |
| UserName | Konto, unter dem der Prozess ausgeführt wird |
| UserDomain | Domäne, unter der der Prozess ausgeführt wird |


## <a name="sample-log-searches"></a>Beispiele für Protokollsuchen

### <a name="list-all-known-machines"></a>Auflisten aller bekannten Computer
Type=ServiceMapComputer_CL | dedup ResourceId

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Auflisten der physischen Arbeitsspeicherkapazität aller verwalteten Computer
Type=ServiceMapComputer_CL | select PhysicalMemory_d, ComputerName_s | Dedup ResourceId

### <a name="list-computer-name-dns-ip-and-os"></a>Auflisten von Computernamen, DNS, IP und Betriebssystem
Type=ServiceMapComputer_CL | select ComputerName_s, OperatingSystemFullName_s, DnsNames_s, IPv4Addresses_s | dedup ResourceId

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Suchen nach allen Prozesse mit „sql“ in der Befehlszeile
Type=ServiceMapProcess_CL CommandLine_s = \*sql\* | dedup ResourceId

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Suchen eines Computers (aktuellster Datensatz) anhand des Ressourcennamens
Type=ServiceMapComputer_CL "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | dedup ResourceId

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>Suchen eines Computers (aktuellster Datensatz) anhand der IP-Adresse
Type=ServiceMapComputer_CL "10.229.243.232" | dedup ResourceId

### <a name="list-all-known-processes-on-a-given-machine"></a>Auflisten aller bekannten Prozesse auf einem bestimmten Computer
Type=ServiceMapProcess_CL MachineResourceName_s="m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | dedup ResourceId

### <a name="list-all-computers-running-sql"></a>Auflisten aller Computer, auf denen SQL ausgeführt wird
Type=ServiceMapComputer_CL ResourceName_s IN {Type=ServiceMapProcess_CL \*sql\* | Distinct MachineResourceName_s} | dedup ResourceId | Distinct ComputerName_s

### <a name="list-of-all-unique-product-versions-of-curl-in-my-datacenter"></a>Auflisten aller eindeutigen Produktversionen von „curl“ im eigenen Rechenzentrum
Type=ServiceMapProcess_CL ExecutableName_s=curl | Distinct ProductVersion_s

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Erstellen einer Computergruppe mit allen Computern, auf denen CentOS ausgeführt wird
Type=ServiceMapComputer_CL OperatingSystemFullName_s = \*CentOS\* | Distinct ComputerName_s



## <a name="diagnostic-and-usage-data"></a>Diagnose- und Nutzungsdaten
Wenn Sie den Service Map-Dienst verwenden, sammelt Microsoft automatisch Nutzungs- und Leistungsdaten. Microsoft verwendet diese Daten, um die Qualität, Sicherheit und Integrität des Service Map-Diensts sicherzustellen und zu verbessern. Zu den Daten gehören Informationen zur Konfiguration Ihrer Software, z.B. Betriebssystem und Betriebssystemversion, sowie IP-Adresse, DNS-Name und Name der Arbeitsstation, um exakte und effiziente Funktionen für die Problembehandlung bereitzustellen. Wir erfassen weder Namen noch Adressen oder andere Kontaktinformationen.

Weitere Informationen zur Sammlung und Nutzung von Daten finden Sie in den [Datenschutzbestimmungen für Onlinedienste von Microsoft](hhttps://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Protokollsuchvorgänge](../log-analytics/log-analytics-log-searches.md) in Log Analytics, um Daten abzurufen, die von Service Map gesammelt wurden.



<!--HONumber=Dec16_HO1-->


