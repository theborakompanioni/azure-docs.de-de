---
title: Log Analytics-Datensicherheit | Microsoft-Dokumentation
description: "Erfahren Sie, wie Log Analytics Ihre Privatsphäre und Daten schützt."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: a33bb05d-b310-4f2c-8f76-f627e600c8e7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 9ae1629462d375c1061d9c7b25975c789ee661c9
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="log-analytics-data-security"></a>Log Analytics-Datensicherheit
Microsoft ist bestrebt, bei der Bereitstellung von Software und Diensten, mit denen Sie die IT-Infrastruktur Ihres Unternehmens verwalten, Ihre Privatsphäre und Ihre Daten zu schützen. Wenn Sie Ihre Daten anderen anvertrauen, setzt dieses Vertrauen strikte Sicherheit voraus. Das wissen wir. Microsoft hält strenge Compliance- und Sicherheitsrichtlinien ein – angefangen bei der Codierung bis hin zum Betreiben von Diensten.

Das Sichern und Schützen von Daten hat bei Microsoft oberste Priorität. Sollten Sie Fragen, Vorschläge oder Probleme im Zusammenhang mit den folgenden Informationen (einschließlich unserer Sicherheitsrichtlinien) haben, können Sie sich über die [Azure-Supportoptionen](http://azure.microsoft.com/support/options/) mit uns in Verbindung setzen.

In diesem Artikel wird erläutert, wie Daten von Log Analytics in der Operations Management Suite (OMS) gesammelt, verarbeitet und geschützt werden. Sie können Agents verwenden, um die Verbindung mit dem Webdienst herzustellen, mit System Center Operations Manager betriebliche Daten sammeln oder Daten aus Azure-Diagnosen für die Verwendung durch Log Analytics abrufen. Die gesammelten Daten werden über das Internet mit zertifikatbasierter Authentifizierung und SSL 3 an den Log Analytics-Dienst gesendet, der in Microsoft Azure gehostet wird. Daten werden vom Agent komprimiert, bevor sie gesendet werden.

Der Log Analytics-Dienst verwaltet Ihre Daten in der Cloud sicher mithilfe der folgenden Methoden:

* Trennung von Daten
* Beibehaltung von Daten
* Physische Sicherheit
* Incident Management
* Compliance
* Sicherheitsstandard-Zertifizierungen

## <a name="data-segregation"></a>Trennung von Daten
Kundendaten werden für jede Komponente des OMS-Diensts logisch getrennt verwaltet. Sämtliche Daten werden nach Organisation gekennzeichnet. Dieser Kennzeichnung wird während des gesamten Datenlebenszyklus beibehalten und auf jeder Ebene des Diensts erzwungen. Jeder Kunde verfügt über einen dedizierten Azure-Blob (Speicher), der die langfristigen Daten enthält.

## <a name="data-retention"></a>Beibehaltung von Daten
Die Daten der indizierten Protokollsuche werden gemäß des von Ihnen gewählten Tarifs gespeichert und aufbewahrt. Weitere Informationen finden Sie unter [Log Analytics-Preise](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft löscht Kundendaten 30 Tage nachdem der OMS-Arbeitsbereich geschlossen wird. Microsoft löscht auch das Azure-Speicherkonto, in dem sich die Daten befinden. Wenn Kundendaten entfernt werden, werden keine physischen Laufwerke zerstört.

Die folgende Tabelle enthält einige der verfügbaren Lösungen in OMS und Beispiele der Typen der von ihnen gesammelten Daten.

| **Lösung** | **Datentypen** |
| --- | --- |
| Konfigurationsbewertung |Konfigurationsdaten, Metadaten und Statusdaten |
| Capacity Planning |Leistungsdaten und Metadaten |
| Antimalware |Konfigurationsdaten und Metadaten |
| System Update Assessment |Metadaten und Statusdaten |
| Log Management |Benutzerdefinierte Ereignisprotokolle, Windows-Ereignisprotokolle und/oder IIS-Protokolle |
| Change Tracking |Softwarebestand und Windows-Dienstmetadaten |
| SQL und Active Directory Assessment |WMI-Daten, Registrierungsdaten, Leistungsdaten und Ergebnisse der dynamischen SQL Server-Verwaltungssichten |

Die folgende Tabelle zeigt Beispiele für Datentypen:

| **Datentyp** | **Felder** |
| --- | --- |
| Warnung |AlertName, AlertDescription, BaseManagedEntityId, ProblemId, IsMonitorAlert, RuleId, ResolutionState, Priority, Severity, Category, Owner, ResolvedBy, TimeRaised, TimeAdded, LastModified, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Konfiguration |CustomerID, AgentID, EntityID, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Ereignis |EventId, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, Number, Category, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Hinweis:** Wenn Sie Ereignisse mit benutzerdefinierten Feldern in das Windows-Ereignisprotokoll schreiben, werden diese von OMS gesammelt. |
| Metadaten |BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IPAddress, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, NetbiosDomainName, LogicalProcessors, DNSName, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Leistung |ObjectName, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Zustand |StateChangeEventId, StateId, NewHealthState, OldHealthState, Context, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Physische Sicherheit
Log Analytics im OMS-Dienst ist mit Microsoft-Mitarbeitern besetzt. Alle Aktivitäten werden protokolliert und können überwacht werden. Der Dienst wird vollständig in Azure ausgeführt und erfüllt die allgemeinen technischen Kriterien für Azure. Ausführliche Informationen über die physische Sicherheit der Azure-Ressourcen finden Sie auf Seite 18 des Dokuments [Microsoft Azure Security Overview (Microsoft Azure-Sicherheitsübersicht)](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Physische Zugriffsrechte auf sichere Bereiche werden innerhalb eines Geschäftstags für jeden Benutzer geändert, der keine Verantwortung mehr für den OMS-Dienst hat, einschließlich der Übertragung und Beendigung. Informieren Sie sich über die globale physische Infrastruktur, die wir in [Microsoft-Rechenzentren](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx) verwenden.

## <a name="incident-management"></a>Incident Management
OMS verfügt über einen Incident-Management-Prozess, dem alle Microsoft-Dienste entsprechen. Zusammenfassung:

* Wir verwenden ein Modell für gemeinsame Verantwortung, bei dem Microsoft einen Teil der Verantwortung für die Sicherheit trägt und der Kunde für den anderen Teil zuständig ist.
* Wir verwalten Azure-Sicherheitsincidents
  * Wir leiten bei Erkennung eines Vorfalls eine Untersuchung ein.
  * Wir bewerten die Auswirkung und den Schweregrad eines Vorfalls durch ein Teammitglied für Sicherheitsvorfälle auf Abruf. Je nach Situation kann diese Bewertung zu einer weiteren Eskalation an das Security Response-Team führen.
  * Wir diagnostizieren einen Vorfall durch Sicherheitsexperten im Rahmen einer technischen oder forensischen Untersuchung, und wir versuchen dieses Problem zu umgehen, einzudämmen und Lösungsstrategien zu identifizieren. Wenn das Sicherheitsteam davon ausgeht, dass Kundendaten möglicherweise einer rechtswidrigen oder nicht autorisierten Person ausgesetzt worden sind, beginnt die parallele Ausführung des Kundenvorfall-Benachrichtigungsvorgangs.  
  * Stabilisieren und Wiederherstellen nach dem Vorfall. Das Sicherheitsteam erstellt einen Wiederherstellungsplan, um das Problem zu beheben. Schritte zur Schadensbegrenzung, z.B. Isolieren betroffener Systeme, können sofort und parallel zur Diagnose erfolgen. Längerfristige Maßnahmen können geplant werden, die ergriffen werden, nachdem die unmittelbare Gefahr vorüber ist.  
  * Schließen des Vorfalls und Durchführen einer Nachbereitung. Das Sicherheitsteam erstellt eine Nachbereitung mit einer Beschreibung der Details des Vorfalls und der Absicht, Richtlinien, Verfahren und Prozesse zu überarbeiten, um eine Wiederholung des Ereignisses zu verhindern.
* Wir benachrichtigen Kunden über Sicherheitsvorfälle
  * Festlegen des Umfangs der betroffenen Kunden und möglichst detaillierte Benachrichtigung aller Betroffenen
  * Erstellen einer Benachrichtigung mit genügend Informationen für Kunden, damit sie ihrerseits eine genauere Untersuchung durchführen und Versprechen halten können, die sie gegenüber ihren Endbenutzern gegeben haben, ohne den Benachrichtigungsvorgang unangemessen zu verzögern.
  * Bestätigung und Meldung des Vorfalls nach Bedarf.
  * Benachrichtigung von Kunden mit einer Vorfallmeldung ohne unangemessene Verzögerung und in Übereinstimmung mit allen gesetzlichen oder vertraglichen Verpflichtungen. Benachrichtigung mindestens eines Administrators des Kunden über Sicherheitsvorfälle mittels von Microsoft ausgewählter Kommunikationsmittel (einschließlich E-Mail).
* Wir führen Team-Bereitschaft und Schulung durch
  * Microsoft-Mitarbeiter müssen eine Sicherheits- und Sensibilisierungs-Schulung absolvieren, um verdächtige Sicherheitsprobleme identifizieren und melden zu können.  
  * Operatoren, die mit dem Microsoft Azure-Dienst arbeiten, haben zusätzliche Schulungspflichten hinsichtlich ihres Zugriffs auf vertrauliche Systeme mit Kundendaten.
  * Microsoft Security Response-Mitarbeiter erhalten spezielle Schulungen für ihre Rollen

Im Falle des Verlusts von Kundendaten benachrichtigen wir jeden Kunden innerhalb eines Tages. Ein Verlust von Kundendaten ist jedoch nie mit OMS aufgetreten. Darüber hinaus verwalten wir Kopien der Daten an verschiedenen Orten.

Weitere Informationen über die Reaktion von Microsoft auf Sicherheitsvorfälle finden Sie unter [Microsoft Azure Security Response in the Cloud (Microsoft Azure Security Response in der Cloud)](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Compliance
Das Informationssicherheits- und Governance-Programm des OMS-Softwareentwicklungs- und Service-Teams unterstützt die geschäftlichen Anforderungen und entspricht Gesetzen und Vorschriften, wie unter [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) und [Microsoft Trust Center Compliance](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx) beschrieben. Außerdem wird beschrieben, wie OMS Sicherheitsanforderungen einrichtet, Sicherheitskontrollen identifiziert und Risiken verwaltet und überwacht. Richtlinien, Standards, Verfahren und Leitlinien werden jährlich überprüft.

Jedes Mitglied des OMS-Entwicklungsteams erhält eine formale Anwendungssicherheitsschulung. Intern verwenden wir ein Versionskontrollsystem für die Softwareentwicklung. Jedes Softwareprojekt wird durch das Versionskontrollsystem geschützt.

Microsoft hat ein Sicherheits- und Compliance-Team, das alle Microsoft-Dienste überwacht und bewertet. In diesem Team sind Information Security Officer, die nicht mit den Entwicklungsabteilungen verbunden sind, die OMS entwickeln. Die Sicherheitsbeauftragten verfügen über eine eigene Managementkette und führen unabhängige Beurteilungen von Produkten und Dienstleistungen durch, um Sicherheit und Compliance zu gewährleisten.

Der Microsoft-Vorstand wird in einem Jahresbericht über alle IT-Sicherheitsprogramme bei Microsoft informiert.

Das Softwareentwicklungs- und Serviceteam für OMS arbeitet aktiv mit den Legal- und Compliance-Teams von Microsoft sowie mit anderen Branchenpartnern zusammen, um verschiedenste Zertifizierungen zu erlangen.

## <a name="certifications-and-attestations"></a>Zertifizierungen und Nachweise
OMS Log Analytics erfüllt folgende Anforderungen:

* [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm)
* [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
* [ISO 22301](https://azure.microsoft.com/en-us/blog/iso22301/)
* [Payment Card Industry (PCI-Compliance) Data Security Standard (PCI DSS)](https://www.microsoft.com/en-us/TrustCenter/Compliance/PCI) des PCI Security Standards Council
* [Service Organization Controls (SOC) 1 Typ 1 und SOC 2 Typ 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2)
* [HIPAA und HITECH](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) für Unternehmen mit einem HIPAA-Geschäftspartnervertrag
* Windows Common Engineering Criteria
* Vertrauenswürdige Datenverarbeitung von Microsoft
* Als Azure-Dienst müssen die Komponenten, die von OMS verwendet werden, die Azure-Bestimmungen einhalten. Weitere Informationen erhalten Sie unter [Microsoft Trust Center – Compliance](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).

> [!NOTE]
> In einigen Zertifizierungen/Nachweisen ist Log Analytics unter dem ehemaligen Namen *Operational Insights* angegeben.
>
>


## <a name="cloud-computing-security-data-flow"></a>Datenfluss beim sicheren Cloud Computing
Das folgende Diagramm zeigt eine Cloudsicherheitsarchitektur sowie den Fluss von Informationen von Ihrem Unternehmen und deren Schutz auf dem Weg zum Log Analytics-Dienst, wo Sie sie letztlich in OMS-Portal anzeigen können. Weitere Informationen zu den einzelnen Schritten finden Sie nach dem Diagramm.

![Bild der OMS-Datensammlung und -Sicherheit](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Registrieren für Log Analytics und Sammeln von Daten
Damit Ihre Organisation Daten an Log Analytics senden kann, müssen Sie Windows-Agents, Agents auf Azure-VMs oder OMS-Agents für Linux konfigurieren. Wenn Sie Operations Manager-Agents verwenden, konfigurieren Sie diese mithilfe eines Konfigurations-Assistenten in der Betriebskonsole. Benutzer (Sie, andere Einzelbenutzer oder eine Gruppe von Personen) erstellen OMS-Konten (OMS-Arbeitsbereiche) und registrieren Agents mithilfe eines der folgenden Konten:

* [Organisations-ID](../active-directory/sign-up-organization.md)
* [Microsoft-Konto – Outlook, Office Live, MSN](http://www.microsoft.com/account/default.aspx)

In einem OMS-Arbeitsbereich werden Daten gesammelt, aggregiert, analysiert und präsentiert. Ein Arbeitsbereich wird hauptsächlich zum Partitionieren von Daten verwendet, wobei jeder Arbeitsbereich eindeutig ist. Beispielsweise empfiehlt es sich, Produktionsdaten mit einem OMS-Arbeitsbereich zu verwalten und Testdaten mit einem anderen Arbeitsbereich. Arbeitsbereiche helfen Administratoren außerdem dabei, den Benutzerzugriff auf Daten zu steuern. Jedem Arbeitsbereich können mehrere Benutzerkonten zugeordnet werden, und jedes Benutzerkonto kann auf mehrere OMS-Arbeitsbereiche zugreifen. Sie erstellen Arbeitsbereiche auf Grundlage der Rechenzentrumsregion. Jeder Arbeitsbereich wird an andere Rechenzentren in der Region repliziert, in erster Linie für OMS-Dienst-Verfügbarkeit.

Wenn für Operations Manager der Konfigurations-Assistent abgeschlossen ist, stellt jede Operations Manager-Verwaltungsgruppe eine Verbindung mit dem Log Analytics-Dienst her. Sie wählen dann mit dem Assistenten zum Hinzufügen von Computern aus, welche Computer in der Verwaltungsgruppe Daten an den Dienst senden können. Alle anderen Agent-Typen verbinden sich sicher mit dem OMS-Dienst.

Die gesamte Kommunikation zwischen verbundenen Systemen und dem Log Analytics-Dienst ist verschlüsselt.  Das TLS (HTTPS)-Protokoll wird für die Verschlüsselung verwendet.  Das Microsoft SDL-Verfahren stellt sicher, dass Log Analytics nach den neuesten Fortschritten bei kryptografischen Protokollen aktualisiert wird.

Jeder Agent-Typ sammelt Daten für Log Analytics. Die Art der gesammelten Daten ist abhängig von der Art der verwendeten Lösungen. Eine Zusammenfassung der Datensammlung finden Sie unter [Add Log Analytics solutions from the Solutions Gallery (Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog)](log-analytics-add-solutions.md). Darüber hinaus stehen ausführlichere Sammlungs-Informationen für die meisten Lösungen zur Verfügung. Eine Lösung ist ein Bündel von vordefinierten Ansichten, Protokollsuchabfragen, Datensammlungsregeln und Verarbeitungslogik. Nur Administratoren können Log Analytics zum Importieren einer Lösung verwenden. Nach dem Importieren der Lösung wird diese auf die Operations Manager-Verwaltungsserver (sofern verwendet) und dann in die ausgewählten Agents verschoben. Anschließend sammeln die Agents die Daten.

## <a name="2-send-data-from-agents"></a>2. Senden von Daten von Agents
Sie registrieren alle Agent-Typen mit einem Registrierungsschlüssel. Eine sichere Verbindung zwischen dem Agent und Log Analytics-Dienst wird mithilfe der zertifikatbasierten Authentifizierung und SSL an Port 443 hergestellt. OMS verwendet einen geheimen Speicher zum Generieren und Verwalten von Schlüsseln. Private Schlüssel werden alle 90 Tage rotiert und in Azure gespeichert. Sie werden von Azure-Operatoren verwaltet, die Gesetze und Compliance-Vorschriften strikt einhalten.

In Operations Manager registrieren Sie einen Arbeitsbereich für den Log Analytics-Dienst. Eine sichere HTTPS-Verbindung mit dem Operations Manager-Verwaltungsserver wird hergestellt.

Für Windows-Agents, die auf virtuellen Computern in Azure ausgeführt werden, wird ein schreibgeschützter Speicherschlüssel verwendet, um Diagnose-Ereignisse in Azure-Tabellen zu lesen.

Wenn ein Agent nicht mit dem Dienst kommunizieren kann, werden die gesammelten Daten in einem temporären Cache lokal gespeichert, und der Verwaltungsserver versucht zwei Stunden lang alle acht Minuten, die Daten erneut zu senden. Zwischengespeicherte Daten des Agents sind durch den Anmeldeinformationsspeicher des Betriebssystems geschützt. Wenn der Dienst die Daten innerhalb von zwei Stunden nicht verarbeiten kann, platzieren die Agents die Daten in einer Warteschlange. Wenn die Warteschlange voll ist, beginnt OMS Datentypen zu löschen, beginnend mit Leistungsdaten. Die Begrenzung für die Agent-Warteschlange ist ein Registrierungsschlüssel, den Sie bei Bedarf ändern können. Gesammelte Daten werden komprimiert und unter Umgehung von lokalen Datenbanken an den Dienst gesendet, sodass keine weitere Last hinzugefügt wird. Nach dem Senden werden die gesammelten Daten aus dem Cache entfernt.

Wie oben beschrieben, werden die Daten von den Agents über SSL an Microsoft Azure-Rechenzentren gesendet. Optional können Sie ExpressRoute verwenden, um zusätzliche Sicherheit für die Daten bereitzustellen. ExpressRoute ist eine Möglichkeit, direkt aus Ihrem vorhandenen WAN mit Azure zu verbinden, z.B. ein Multi-Protocol Label Switching-VPN (MPLS), das von einem Netzwerkdienstanbieter bereitgestellt wird. Weitere Informationen finden Sie unter [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. Empfangen und Verarbeiten von Daten durch den Log Analytics-Dienst
Der Log Analytics-Dienst stellt sicher, dass eingehende Daten aus einer vertrauenswürdigen Quelle stammen, indem Zertifikate und die Integrität der Daten mittels Azure-Authentifizierung überprüft werden. Die unverarbeiteten Rohdaten werden dann als Blob in [Microsoft Azure Storage](../storage/common/storage-introduction.md) unverschlüsselt gespeichert. Jedes Azure Storage-Blob besitzt jedoch einen eindeutigen Satz von Schlüsseln, auf die nur dieser Benutzer zugreifen kann. Die Art der gespeicherten Daten ist abhängig von der Art der importierten und zum Sammeln von Daten verwendeten Lösungen. Der Log Analytics-Dienst verarbeitet dann die Rohdaten für das Azure Storage-Blob.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Verwenden von Log Analytics für den Datenzugriff
Sie können sich im OMS-Portal bei Log Analytics anmelden, indem Sie ein zuvor eingerichtetes Unternehmenskonto oder Microsoft-Konto verwenden. Der gesamte Datenverkehr zwischen dem OMS-Portal und Log Analytics in OMS wird über einen sicheren HTTPS-Kanal gesendet. Wenn das OMS-Portal verwendet wird, wird eine ID für eine Sitzung auf dem Client des Benutzers (Webbrowser) generiert, und Daten werden in einem lokalen Cache gespeichert, bis die Sitzung beendet wird. Wenn die Sitzung beendet ist, wird der Cache gelöscht. Clientseitige Cookies enthalten keine persönlich identifizierbaren Informationen und werden nicht automatisch entfernt. Sitzungscookies sind „HTTPOnly“ markiert und gesichert. Nach einer vorher festgelegten Zeit im Leerlauf wird die OMS-Portal-Sitzung beendet.

Über das OMS-Portal können Sie Daten in eine CSV-Datei exportieren, und Sie können mithilfe von Search-APIs auf Daten zugreifen. Der Export von CSV-Dateien ist auf 50.000 Zeilen pro Export beschränkt und API-Daten sind auf 5.000 Zeilen pro Suche beschränkt.

## <a name="next-steps"></a>Nächste Schritte
* [Erste Schritte mit Log Analytics](log-analytics-get-started.md). Hier erfahren Sie mehr über Log Analytics und wie Sie binnen Minuten loslegen können.

