<properties
   pageTitle="Application Dependency Monitor (ADM) in Operations Management Suite (OMS) | Microsoft Azure"
   description="Der Application Dependency Monitor (ADM) ist eine Lösung in der Operations Management Suite (OMS), die automatisch Anwendungskomponenten auf Windows- und Linux-Systemen ermittelt und die Kommunikation zwischen Diensten abbildet.  Dieser Artikel enthält Informationen zum Bereitstellen des ADM in Ihrer Umgebung und zur Verwendung der Lösung in einer Vielzahl von Szenarien."
   services="operations-management-suite"
   documentationCenter=""
   authors="daseidma"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/28/2016"
   ms.author="daseidma;bwren" />


# <a name="using-application-dependency-monitor-solution-in-operations-management-suite-(oms)"></a>Verwenden der Application Dependency Monitor-Lösung in der Operations Management Suite (OMS)
![Symbol „Alert Management“](media/operations-management-suite-application-dependency-monitor/icon.png) Der Application Dependency Monitor (ADM) ermittelt automatisch Anwendungskomponenten auf Windows- und Linux-Systemen und bietet eine Darstellung der Kommunikation zwischen Diensten. In dieser Lösung können Sie die Server ihrer Funktion gemäß anzeigen – als verbundene Systeme, die wichtige Dienste bereitstellen.  Der Application Dependency Monitor zeigt Verbindungen zwischen Servern, Prozessen und Ports über die gesamte TCP-Verbindungsarchitektur an. Außer der Installation eines Agents ist keine weitere Konfiguration erforderlich.

Dieser Artikel bietet eine detaillierte Beschreibung der Nutzung des Application Dependency Monitors.  Weitere Informationen zum Konfigurieren des ADM und zum Onboarding von Agents finden Sie unter [Konfigurieren der Lösung Application Dependency Monitor in der Operations Management Suite (OMS)](operations-management-suite-application-dependency-monitor-configure.md).

>[AZURE.NOTE]Der Application Dependency Monitor ist derzeit in der privaten Vorschau verfügbar.  Unter [https://aka.ms/getadm](https://aka.ms/getadm) können Sie den Zugriff auf die private Vorschau des ADM anfordern.
>
>In der privaten Vorschau haben alle OMS-Konten uneingeschränkten Zugriff auf den ADM.  ADM-Knoten sind kostenlos, die Log Analytics-Daten für die Typen AdmComputer_CL und AdmProcess_CL werden jedoch wie bei jeder anderen Lösung gemessen.
>
>In der öffentlichen Vorschau wird die Lösung nur für Benutzer der kostenlosen und kostenpflichtigen Version von Insight & Analytics im OMS-Tarif zur Verfügung stehen.  Im Free-Tarif werden nur 5 ADM-Knoten verfügbar sein.  Wenn Sie die private Vorschau nutzen und beim Wechsel des ADM zur öffentlichen Vorschau nicht für den OMS-Tarif registriert sind, wird der ADM zum Zeitpunkt des Wechsels deaktiviert. 


## <a name="use-cases:-make-your-it-processes-dependency-aware"></a>Anwendungsfälle: Berücksichtigen von Abhängigkeiten in IT-Prozessen

### <a name="discovery"></a>Ermittlung
Der ADM erstellt automatisch eine allgemeine Referenzzuordnung der Abhängigkeiten für all Ihre Server, Prozesse und Drittanbieterdienste.  Der ADM ermittelt alle TCP-Abhängigkeiten und bildet diese ab. Dabei werden unerwartete Verbindungen, von Ihnen benötigte Remotesysteme von Drittanbietern und Abhängigkeiten von Elementen in traditionell weniger transparenten Netzwerkbereichen wie z.B. DNS und AD identifiziert.  Der ADM ermittelt fehlerhafte Netzwerkverbindungen, die Ihre verwalteten Systeme versuchen herzustellen, sodass Sie mögliche Fehlkonfigurationen der Server, Dienstausfälle und Netzwerkprobleme erkennen können.

### <a name="incident-management"></a>Incident Management
Der ADM macht Schluss mit dem Rätselraten bei der Problemermittlung und zeigt Ihnen, wie Ihre Systeme miteinander verbunden sind und sich gegenseitig beeinflussen.  Sie erhalten nicht nur Informationen zu fehlerhaften Verbindungen, sondern auch zu verbundenen Clients, sodass Sie falsch konfigurierte Lastenausgleichsmodule, eine unerwartete oder übermäßige Auslastung kritischer Dienste und eine nicht autorisierte Clientkommunikation z.B. zwischen Entwicklercomputern und Produktionssystemen identifizieren können.  Dank integrierter Workflows und OMS-Änderungsnachverfolgung können Sie erkennen, ob sich die Ursache eines Incidents durch ein Änderungsereignis auf einem Back-End-Computer oder in einem Back-End-Dienst erklären lässt.

### <a name="migration-assurance"></a>Sichere Migrationen
Mit dem ADM können Sie Azure-Migrationen effektiv planen, beschleunigen und überprüfen. So stellen Sie sicher, dass nichts vergessen wird und keine unerwarteten Ausfälle auftreten.  Sie können alle voneinander abhängigen Systeme ermitteln, die gemeinsam migriert werden müssen, die Systemkonfiguration und -kapazität bewerten und herausfinden, ob ein ausgeführtes System noch benötigt wird oder eher stillgelegt als migriert werden sollte.  Nach der Migration können Sie die Clientauslastung und -identität überprüfen, um sicherzustellen, dass Testsysteme und Kunden eine Verbindung herstellen können.  Wenn bei der Subnetzplanung und den Firewalldefinitionen Probleme auftreten, weisen Verbindungsfehler in den ADM-Zuordnungen auf die Systeme hin, die Konnektivität benötigen.

### <a name="business-continuity"></a>Geschäftskontinuität
Wenn Sie Azure Site Recovery verwenden und Hilfe beim Definieren der Wiederherstellungsreihenfolge für Ihre Anwendungsumgebung benötigen, kann der ADM Ihnen automatisch zeigen, welche Systeme auf welche Weise voneinander abhängig sind, damit Sie sicherstellen können, dass Ihr Wiederherstellungsplan zuverlässig ist.  Wählen Sie einen wichtigen Server aus, und zeigen Sie die zugehörigen Clients an, um die Front-End-Systeme zu identifizieren, die erst dann wiederhergestellt werden sollten, wenn der wichtige Server wiederhergestellt und verfügbar ist.  Umgekehrt können Sie sich die Back-End-Abhängigkeiten eines wichtigen Servers ansehen, um die Systeme zu identifizieren, die wiederhergestellt werden müssen, bevor das untersuchte System wiederhergestellt werden kann.

### <a name="patch-management"></a>Patchverwaltung
Der ADM erweitert die Verwendung des OMS-Tools zur Bewertung von Systemupdates, indem er anzeigt, welche anderen Teams und Server von Ihrem Dienst abhängig sind, damit Sie diese benachrichtigen können, bevor Sie Ihre Systeme zum Patchen herunterfahren.  Der ADM erweitert auch die Patchverwaltung in der OMS, indem er zeigt, ob Ihre Dienste verfügbar und ordnungsgemäß verbunden sind, nachdem die Patches aufgespielt und die Dienste neu gestartet wurden. 


## <a name="mapping-overview"></a>Übersicht über die Zuordnung
ADM-Agents sammeln Informationen über alle über TCP verbundenen Prozesse auf dem Server, auf dem sie installiert sind, sowie Informationen zu den eingehenden und ausgehenden Verbindungen für jeden Prozess.  Aus der Computerliste auf der linken Seite der ADM-Lösung können Computer mit ADM-Agents ausgewählt werden, um deren Abhängigkeiten während eines ausgewählten Zeitraums zu visualisieren.  Die Abhängigkeitszuordnungen gelten für einen bestimmten Computer und zeigen alle Computer an, die direkte TCP-Clients oder -Server dieses Computers sind.

![Übersicht über den ADM](media/operations-management-suite-application-dependency-monitor/adm-overview.png)

Computer können in der Zuordnung erweitert werden, um die ausgeführten Prozesse mit aktiven Netzwerkverbindungen während des ausgewählten Zeitraums anzuzeigen.  Wenn ein Remotecomputer mit einem ADM-Agent erweitert wird, um Details zu den Prozessen anzuzeigen, werden nur diejenigen Prozesse angezeigt, die mit dem untersuchten Computer kommunizieren.  Die Anzahl der Front-End-Computer ohne Agents, die eine Verbindung mit dem untersuchten Computer herstellen, wird links neben den Prozessen angegeben, mit denen eine Verbindung hergestellt wird.  Wenn der untersuchte Computer eine Verbindung mit einem Back-End-Computer ohne Agent herstellt, wird dieses Back-End in der Zuordnung durch einen Knoten dargestellt, der die IPv4-Adresse des Back-Ends zeigt und erweitert werden kann, um einzelne Ports und Dienste anzuzeigen, mit denen der untersuchte Computer kommuniziert.

Standardmäßig zeigt der ADM Abhängigkeitsinformationen der letzten 10 Minuten an.  Mithilfe der Zeitauswahl in der linken oberen Ecke können Zuordnungen nach historischen Zeiträumen (max. eine Stunde) abgefragt werden, um zu zeigen, wie die Abhängigkeiten in der Vergangenheit aussahen, z.B. während eines Incidents oder vor einer Änderung.    ADM-Daten werden in kostenpflichtigen Arbeitsbereichen 30 Tage lang gespeichert, in kostenlosen Arbeitsbereichen 7 Tage lang.

![Computerzuordnung mit ausgewählten Computereigenschaften](media/operations-management-suite-application-dependency-monitor/machine-map.png)

## <a name="failed-connections"></a>Verbindungsfehler
Verbindungsfehler werden in ADM-Zuordnungen für Prozesse und Computer angezeigt. Eine gestrichelte rote Linie zeigt, dass ein Clientsystem einen Prozess oder Port nicht erreichen kann.  Verbindungsfehler werden von jedem System mit einem bereitgestellten ADM-Agent berichtet, wenn dieses System dasjenige ist, das versucht, die fehlerhafte Verbindung herzustellen.  Der ADM misst dies durch Überwachen der TCP-Sockets, die keine Verbindung herstellen können.  Der Fehler kann durch eine Firewall, eine fehlerhafte Konfiguration auf einem Client oder Server oder dadurch verursacht werden, dass ein Remotedienst nicht verfügbar ist. 

![Verbindungsfehler](media/operations-management-suite-application-dependency-monitor/failed-connections.png)

Kenntnisse zu Verbindungsfehlern können Ihnen bei der Problembehandlung, der Überprüfung einer Migration, der Sicherheitsanalyse und dem allgemeinen Verständnis der Architektur helfen.  Manchmal sind Verbindungsfehler harmlos, häufig weisen sie aber auch direkt auf ein Problem hin, z.B. wenn eine Failoverumgebung plötzlich nicht mehr erreichbar ist oder zwei Anwendungsebenen nach einer Cloudmigration nicht mehr kommunizieren können.  In der obigen Abbildung werden die Internetinformationsdienste und WebSphere ausgeführt, können aber keine Verbindung herstellen. 

## <a name="computer-and-process-properties"></a>Computer- und Prozesseigenschaften
In einer ADM-Zuordnung können Sie Computer und Prozesse auswählen, um zusätzlichen Kontext zu deren Eigenschaften zu erhalten.  Folgende Informationen zu Computern können angezeigt werden: DNS-Name, IPv4-Adressen, CPU- und Arbeitsspeicherkapazität, VM-Typ, Betriebssystemversion, Uhrzeit des letzten Neustarts und die IDs ihrer OMS- und ADM-Agents.

Prozessdetails werden aus Metadaten des Betriebssystems zu ausgeführten Prozessen erfasst: Prozessname und -beschreibung, Benutzername und Domäne (unter Windows), Firmenname, Produktname, Produktversion, Arbeitsverzeichnis, Befehlszeile und Startzeit des Prozesses.

![Prozesseigenschaften](media/operations-management-suite-application-dependency-monitor/process-properties.png)

Der Übersichtsbereich für einen Prozess bietet zusätzliche Informationen über die Konnektivität dieses Prozesses, einschließlich Bindungsports, eingehender und ausgehender Verbindungen sowie Verbindungsfehler. 

![Prozessübersicht](media/operations-management-suite-application-dependency-monitor/process-summary.png)

## <a name="oms-change-tracking-integration"></a>Integration der OMS-Änderungsnachverfolgung
Die Integration von ADM in die Änderungsnachverfolgung erfolgt automatisch, wenn beide Lösungen im OMS-Arbeitsbereich aktiviert und konfiguriert werden.

Der Übersichtsbereich für den Computer zeigt an, ob während des ausgewählten Zeitraums Änderungsnachverfolgungsereignisse auf dem ausgewählten Computer aufgetreten sind.

![Übersichtsbereich des Computers](media/operations-management-suite-application-dependency-monitor/machine-summary.png)

Der Bereich für die Nachverfolgung von Änderungen auf einem Computer zeigt eine Liste aller Änderungen an, wobei die jüngste Änderung zuerst angezeigt wird. Der Bereich bietet auch einen Link, mit dem Sie die Protokollsuche aufrufen und weitere Details anzeigen können.
![Bereich für die Änderungsnachverfolgung auf einem Computer](media/operations-management-suite-application-dependency-monitor/machine-change-tracking.png)

Im Folgenden sehen Sie die Detailansicht eines Konfigurationsänderungsereignisses nach Auswahl von **In Log Analytics anzeigen**.
![Konfigurationsänderungsereignis](media/operations-management-suite-application-dependency-monitor/configuration-change-event.png)


## <a name="log-analytics-records"></a>Log Analytics-Datensätze
Die Computer- und Prozessbestandsdaten des ADM stehen zur [Suche](../log-analytics/log-analytics-log-searches.md) in Log Analytics zur Verfügung.  Dies kann in verschiedenen Szenarien von Nutzen sein, wie z.B. Migrationsplanung, Kapazitätsanalyse, Ermittlung und Ad-hoc-Behebung von Leistungsproblemen. 

Zusätzlich zu den Datensätzen, die beim Starten eines Prozesses oder Computers oder beim Onboarding in den ADM generiert werden, wird pro Stunde ein Datensatz für jeden eindeutigen Computer und jeden eindeutigen Prozess generiert.  Die Eigenschaften der Datensätze sind in den folgenden Tabellen aufgeführt. 

Es gibt intern generierte Eigenschaften, mit denen Sie eindeutige Prozessen und Computer identifizieren können:

- PersistentKey_s ist eindeutig durch die Prozesskonfiguration definiert, z.B. Befehlszeile und Benutzer-ID.  Die Eigenschaft ist für einen bestimmten Computer eindeutig, kann jedoch auf Computern wiederholt werden.
- ProcessId_s und ComputerId_s sind im ADM-Modell global eindeutig.



### <a name="admcomputer_cl-records"></a>AdmComputer_CL-Datensätze
Datensätze des Typs **AdmComputer_CL** enthalten Bestandsdaten für Server mit ADM-Agents.  Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt.  

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ | *AdmComputer_CL* |
| SourceSystem | *OpsManager* |
| ComputerName_s | Name des Windows- oder Linux-Computers |
| CPUSpeed_d | CPU-Geschwindigkeit in MHz |
| DnsNames_s | Liste aller DNS-Namen für diesen Computer |
| IPv4s_s | Liste aller von diesem Computer verwendeten IPv4-Adressen |
| IPv6s_s | Liste aller von diesem Computer verwendeten IPv6-Adressen  (der ADM identifiziert IPv6-Adressen, ermittelt aber keine IPv6-Abhängigkeiten) |
| Is64Bit_b | „true“ oder „false“, je nach Betriebssystemtyp |
| MachineId_s | Eine interne, über einen OMS-Arbeitsbereich hinweg eindeutige GUID  |
| OperatingSystemFamily_s | Windows oder Linux |
| OperatingSystemVersion_s | Lange Zeichenfolge mit der Betriebssystemversion |
| TimeGenerated | Datum und Uhrzeit der Erstellung des Datensatzes |
| TotalCPUs_d | Anzahl von CPU-Kernen |
| TotalPhysicalMemory_d | Arbeitsspeicherkapazität in MB |
| VirtualMachine_b | „true“ oder „false“, je nachdem, ob das Betriebssystem ein VM-Gastbetriebssystem ist |
| VirtualMachineID_g | ID des virtuellen Hyper-V-Computers |
| VirtualMachineName_g | Name des virtuellen Hyper-V-Computers |
| VirtualMachineType_s | Hyperv, Vmware, Xen, Kvm, Ldom, Lpar, Virtualpc |


### <a name="admprocess_cl-type-records"></a>AdmProcess_CL Type records 
Datensätze des Typs **AdmProcess_CL** enthalten Bestandsdaten für über TCP verbundene Prozesse auf Servern mit ADM-Agents.  Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt.

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ | *AdmProcess_CL* |
| SourceSystem | *OpsManager* |
| CommandLine_s | Vollständige Befehlszeile des Prozesses |
| CompanyName_s | Firmenname (aus Windows PE oder Linux RPM) |
| Description_s | Lange Prozessbeschreibung (aus Windows PE oder Linux RPM) |
| FileVersion_s | Version der ausführbaren Datei (aus Windows PE, nur Windows) |
| FirstPid_d | Betriebssystemprozess-ID |
| InternalName_s | Interner Name der ausführbaren Datei (aus Windows PE, nur Windows) |
| MachineId_s | Interne, über einen OMS-Arbeitsbereich hinweg eindeutige GUID  |
| Name_s | Der Name der ausführbaren Prozessdatei |
| Path_s | Dateisystempfad der ausführbaren Prozessdatei |
| PersistentKey_s | Interne, innerhalb dieses Computers eindeutige GUID |
| PoolId_d | Interne ID zum Aggregieren von Prozessen basierend auf ähnlichen Befehlszeilen |
| ProcessId_s | Interne, über einen OMS-Arbeitsbereich hinweg eindeutige GUID  |
| ProductName_s | Zeichenfolge des Produktnamens (aus Windows PE oder Linux RPM) |
| ProductVersion_s | Zeichenfolge der Produktversion (aus Windows PE oder Linux RPM) |
| StartTime_t | Startzeit des Prozesses gemäß Uhr des lokalen Computers |
| TimeGenerated | Datum und Uhrzeit der Erstellung des Datensatzes |
| UserDomain_s | Domäne des Prozessbesitzers (nur Windows) |
| UserName_s | Name des Prozessbesitzers (nur Windows) |
| WorkingDirectory_s | Arbeitsverzeichnis des Prozesses |


## <a name="sample-log-searches"></a>Beispiele für Protokollsuchen

### <a name="list-the-physical-memory-capacity-of-all-managed-computers."></a>Auflisten der physischen Arbeitsspeicherkapazität aller verwalteten Computer 
Type=AdmComputer_CL | select TotalPhysicalMemory_d, ComputerName_s | Dedup ComputerName_s

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-01.png)

### <a name="list-computer-name,-dns,-ip,-and-os-version."></a>Auflisten von Computernamen, DNS, IP und Betriebssystemversion
Type=AdmComputer_CL | select ComputerName_s, OperatingSystemVersion_s, DnsNames_s, IPv4s_s  | dedup ComputerName_s

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-02.png)

### <a name="find-all-processes-with-"sql"-in-the-command-line"></a>Suchen nach allen Prozesse mit „sql“ in der Befehlszeile
Type=AdmProcess_CL CommandLine_s = \*sql\* | dedup ProcessId_s

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-03.png)

### <a name="after-viewing-event-data-for-given-process,-use-its-machine-id-to-retrieve-the-computer’s-name"></a>Verwenden der Computer-ID zum Abrufen des Computernamens nach der Anzeige von Ereignisdaten für einen bestimmten Prozess
Type=AdmComputer_CL "m!m-9bb187fa-e522-5f73-66d2-211164dc4e2b" | Distinct ComputerName_s

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-04.png)

### <a name="list-all-computers-running-sql"></a>Auflisten aller Computer, auf denen SQL ausgeführt wird
Type=AdmComputer_CL MachineId_s IN {Type=AdmProcess_CL \*sql\* | Distinct MachineId_s} | Distinct ComputerName_s

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-05.png)

### <a name="list-of-all-unique-product-versions-of-curl-in-my-datacenter"></a>Auflisten aller eindeutigen Produktversionen von „curl“ im eigenen Rechenzentrum
Type=AdmProcess_CL Name_s=curl | Distinct ProductVersion_s

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-06.png)

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Erstellen einer Computergruppe mit allen Computern, auf denen CentOS ausgeführt wird

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-07.png)



## <a name="diagnostic-and-usage-data"></a>Diagnose- und Nutzungsdaten
Wenn Sie den Application Dependency Monitor-Dienst verwenden, sammelt Microsoft automatisch Nutzungs- und Leistungsdaten. Microsoft verwendet diese Daten, um die Qualität, Sicherheit und Integrität des Application Dependency Monitor-Diensts sicherzustellen und zu verbessern. Zu den Daten gehören Informationen zur Konfiguration Ihrer Software, z.B. Betriebssystem und Betriebssystemversion, sowie IP-Adresse, DNS-Name und Name der Arbeitsstation, um exakte und effiziente Funktionen für die Problembehandlung bereitzustellen. Wir erfassen weder Namen noch Adressen noch andere Kontaktinformationen.

Weitere Informationen zur Sammlung und Nutzung von Daten finden Sie in den [Datenschutzbestimmungen für Onlinedienste von Microsoft](hhttps://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Protokollsuchvorgänge](../log-analytics/log-analytics-log-searches.md] in Log Analytics to retrieve data collected by Application Dependency Monitor.).



<!--HONumber=Oct16_HO2-->


