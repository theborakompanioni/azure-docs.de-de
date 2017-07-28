---
title: Verwenden von Service Map in der Operations Management Suite (OMS) | Microsoft-Dokumentation
description: "Service Map ist eine Lösung in der Operations Management Suite, die automatisch Anwendungskomponenten auf Windows- und Linux-Systemen ermittelt und die Kommunikation zwischen Diensten abbildet. Dieser Artikel enthält Informationen zum Bereitstellen von Service Map in Ihrer Umgebung und zur Verwendung der Lösung in einer Vielzahl von Szenarien."
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
ms.translationtype: Human Translation
ms.sourcegitcommit: fc27849f3309f8a780925e3ceec12f318971872c
ms.openlocfilehash: 2e5475a0563549ddfaa2c146e4acf94c019841ec
ms.contentlocale: de-de
ms.lasthandoff: 06/14/2017


---

# <a name="use-the-service-map-solution-in-operations-management-suite"></a>Verwenden von Service Map in der Operations Management Suite (OMS)
Service Map ermittelt automatisch Anwendungskomponenten auf Windows- und Linux-Systemen und stellt die Kommunikation zwischen Diensten dar. Mit Service Map können Sie die Server Ihrer Vorstellung gemäß anzeigen – als verbundene Systeme, die wichtige Dienste bereitstellen. Service Map zeigt Verbindungen zwischen Servern, Prozessen und Ports über die gesamte TCP-Verbindungsarchitektur an. Außer der Installation eines Agents ist keine weitere Konfiguration erforderlich.

In diesem Artikel werden die Details der Verwendung von Service Map beschrieben. Weitere Informationen zum Konfigurieren von Service Map und zum Onboarding von Agents finden Sie unter [Konfigurieren von Service Map in der Operations Management Suite (OMS)](operations-management-suite-service-map-configure.md).


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Anwendungsfälle: Berücksichtigen von Abhängigkeiten in IT-Prozessen

### <a name="discovery"></a>Ermittlung
Service Map erstellt automatisch eine allgemeine Referenzzuordnung der Abhängigkeiten für all Ihre Server, Prozesse und Drittanbieterdienste. Service Map ermittelt alle TCP-Abhängigkeiten und bildet diese ab. Dabei werden unerwartete Verbindungen, von Ihnen benötigte Remotesysteme von Drittanbietern und Abhängigkeiten von Elementen in traditionell weniger transparenten Netzwerkbereichen wie z.B. Active Directory identifiziert. Service Map ermittelt fehlerhafte Netzwerkverbindungen, die Ihre verwalteten Systeme herzustellen versuchen, sodass Sie mögliche Fehlkonfigurationen der Server, Dienstausfälle und Netzwerkprobleme erkennen können.

### <a name="incident-management"></a>Incident Management
Service Map macht Schluss mit dem Rätselraten bei der Problemermittlung und zeigt Ihnen, wie Ihre Systeme miteinander verbunden sind und sich gegenseitig beeinflussen. Sie können nicht nur fehlerhafte Verbindungen identifizieren, sondern auch verbundenen Clients, sodass Sie falsch konfigurierte Loadbalancer, eine unerwartete oder übermäßige Auslastung kritischer Dienste und eine nicht autorisierte Clientkommunikation – z.B. zwischen Entwicklercomputern und Produktionssystemen – identifizieren können. Dank integrierter Workflows und OMS-Änderungsnachverfolgung können Sie erkennen, können Sie auch sehen, ob die Ursache eines Incidents durch ein Änderungsereignis auf einem Back-End-Computer oder in einem Back-End-Dienst zurückzuführen ist.

### <a name="migration-assurance"></a>Sichere Migrationen
Mithilfe der Dienstzuordnung können Sie Azure-Migrationen effektiv planen, beschleunigen und überprüfen. Damit können Sie sicherstellen, dass nichts zurückbleibt und dass keine überraschenden Ausfälle auftreten. Sie können alle voneinander abhängigen Systeme ermitteln, die gemeinsam migriert werden müssen, die Systemkonfiguration und -kapazität bewerten und herausfinden, ob ein ausgeführtes System noch benötigt wird oder eher stillgelegt als migriert werden sollte. Nach der Migration können Sie die Clientauslastung und -identität überprüfen, um sicherzustellen, dass Testsysteme und Kunden eine Verbindung herstellen können. Wenn bei der Subnetzplanung und den Firewalldefinitionen Probleme auftreten, weisen Verbindungsfehler in den Service Map-Zuordnungen auf die Systeme hin, die Konnektivität benötigen.

### <a name="business-continuity"></a>Geschäftskontinuität
Wenn Sie Azure Site Recovery verwenden und Hilfe beim Definieren der Wiederherstellungsreihenfolge für Ihre Anwendungsumgebung benötigen, kann der Service Map Ihnen automatisch zeigen, welche Systeme auf welche Weise voneinander abhängig sind, damit Sie sicherstellen können, dass Ihr Wiederherstellungsplan zuverlässig ist. Wenn Sie einen wichtigen Server oder eine Gruppe auswählen und sich die dazugehörigen Clients anschauen, können Sie bestimmen, welche Front-End-Systeme wiederhergestellt werden müssen, nachdem der Server wiederhergestellt wurde und wieder verfügbar ist. Wenn Sie sich umgekehrt die Back-End-Abhängigkeiten eines wichtigen Servers anschauen, können Sie bestimmen, welche Systeme wiederhergestellt werden müssen, bevor Ihre untersuchten Systeme wiederhergestellt werden.

### <a name="patch-management"></a>Patchverwaltung
Service Map erweitert die Verwendung des OMS-Tools zur Bewertung von Systemupdates, indem es anzeigt, welche anderen Teams und Server von Ihrem Dienst abhängig sind. Auf diese Weise können Sie diese benachrichtigen, bevor Sie Ihre Systeme zum Patchen herunterfahren. Service Map erweitert auch die Patchverwaltung in der OMS, indem es anzeigt, ob Ihre Dienste verfügbar und ordnungsgemäß verbunden sind, nachdem die Patches aufgespielt und die Dienste neu gestartet wurden.


## <a name="mapping-overview"></a>Übersicht über die Zuordnung
Service Map-Agents sammeln Informationen über alle über TCP verbundenen Prozesse auf dem Server, auf dem sie installiert sind, und Informationen zu den eingehenden und ausgehenden Verbindungen für jeden Prozess. In der Liste im linken Bereich können Sie Computer oder Gruppen mit Dienstzuordnung-Agents auswählen, um ihre Abhängigkeiten in einem angegebenen Zeitabschnitt visuell darzustellen. Die Abhängigkeitszuordnungen gelten für einen bestimmten Computer und zeigen alle Computer an, die direkte TCP-Clients oder -Server dieses Computers sind.  Computergruppe ordnet Anzeigesets von Servern und deren Abhängigkeiten zu.

![Service Map-Übersicht](media/oms-service-map/service-map-overview.png)

Computer können in der Zuordnung erweitert werden, um die ausgeführten Prozesse mit aktiven Netzwerkverbindungen während des ausgewählten Zeitraums anzuzeigen. Wenn ein Remotecomputer mit einem Service Map-Agent erweitert wird, um Details zu den Prozessen anzuzeigen, werden nur die Prozesse angezeigt, die mit dem untersuchten Computer kommunizieren. Die Anzahl der Front-End-Computer ohne Agents, die eine Verbindung mit dem untersuchten Computer herstellen, wird links neben den Prozessen angegeben, mit denen eine Verbindung hergestellt wird. Wenn der untersuchte Computer eine Verbindung mit einem Back-End-Computer ohne Agent herstellt, wird der Back-End-Server zusammen mit anderen Verbindungen mit derselben Portnummer einer Serverportgruppe hinzugefügt.

Standardmäßig zeigt Service Map Abhängigkeitsinformationen der letzten 30 Minuten an. Mithilfe der Zeitauswahl in der linken oberen Ecke können Sie Zuordnungen nach historischen Zeiträumen (maximal eine Stunde) abfragen, um zu zeigen, wie die Abhängigkeiten in der Vergangenheit aussahen, z.B. während eines Incidents oder vor einer Änderung. Service Map-Daten werden in kostenpflichtigen Arbeitsbereichen 30 Tage lang gespeichert, in kostenlosen Arbeitsbereichen 7 Tage lang.

## <a name="status-badges-and-border-coloring"></a>Statusbadges und Rahmenfarbe
Am unteren Rand jedes Servers in der Zuordnung wird möglicherweise eine Liste der Statusbadges mit Statusinformationen über den Server angezeigt. Die Badges weisen darauf hin, dass relevante Informationen für den Server aus einer der OMS-Lösungsintegrationen vorliegen. Durch Klicken auf einen Badge gelangen Sie direkt zu den Statusdetails im rechten Bereich. Aktuell sind folgende Statusbadges verfügbar: Warnungen, Service Desk, Änderungen, Sicherheit und Updates.

Je nach Schweregrad der Statusbadges können die Computerknotenrahmen rot (kritisch), gelb (Warnung) oder blau (Information) sein. Die Farbe stellt den schwerwiegendsten Status aller Statusbadges dar. Ein grauer Rahmen gibt einen Knoten an, der keine Statusindikatoren an.

![Statusbadges](media/oms-service-map/status-badges.png)

## <a name="machine-groups"></a>Computergruppen
Mit Computergruppen können Sie sich Zuordnungen im Zusammenhang mit einem bestimmten Serverset anschauen, und nicht nur eine, sodass Sie alle Member einer mehrschichtigen Anwendung oder eines Serverclusters in einer Zuordnung sehen.

Benutzer wählen aus, welche Server sich in einer Gruppe befinden. Ebenso bestimmen Sie den Namen der Gruppe.  Dann können Sie die Gruppe mit allen ihren Prozessen und Verbindungen anzeigen oder lediglich mit den Prozessen oder Verbindungen, die in direkter Verbindung mit anderen Membern der Gruppe stehen.

![Computergruppe](media/oms-service-map/machine-group.png)

### <a name="creating-a-machine-group"></a>Erstellen eine Computergruppe
Um eine Gruppe zu erstellen, wählen Sie den Computer oder die Computer aus, die in die Computerliste aufgenommen werden sollen, und klicken Sie anschließend auf **Add to group** (Zu Gruppe hinzufügen).

![Erstellen einer Gruppe](media/oms-service-map/machine-groups-create.png)

Hier können Sie auf **Create new** (Neu erstellen) klicken und die Gruppe benennen.

![Benennen einer Gruppe](media/oms-service-map/machine-groups-name.png)

>[!NOTE]
>Computergruppen sind derzeit auf 10 Server beschränkt, aber wir planen, diese Einschränkung bald zu erhöhen.

### <a name="viewing-a-group"></a>Anzeigen einer Gruppe
Sobald Sie einige Gruppen erstellt haben, können Sie sich diese auf der Registerkarte „Gruppen“ anschauen.

![Registerkarte „Gruppen“](media/oms-service-map/machine-groups-tab.png)

Wählen Sie dann den Gruppennamen, um die Zuordnung für diese Computergruppe anzuzeigen.
![Computergruppe](media/oms-service-map/machine-group.png): Die Computer, die zu dieser Gruppe gehören, sind in der Zuordnung weiß eingerahmt.

Wenn Sie die Gruppe erweitern, werden die Computer aufgelistet, aus denen die Computergruppe besteht.

![Computer der Computergruppe](media/oms-service-map/machine-groups-machines.png)

### <a name="filter-by-processes"></a>Filtern nach Prozessen
Sie können in der Zuordnungsansicht zwischen allen Prozessen und Verbindungen der Gruppe und denjenigen, die in direkter Verbindung mit der Computergruppe stehen, wechseln.  Die Standardansicht ist das Anzeigen aller Prozesse.  Sie können die Ansicht ändern, indem Sie auf das Filtersymbol über der Zuordnung klicken.

![Filtergruppe](media/oms-service-map/machine-groups-filter.png)

Wenn **Alle Prozesse** ausgewählt ist, enthält die Zuordnung alle Prozesse und Verbindungen jedes Computers der Gruppe.

![Alle Prozesse der Computergruppe](media/oms-service-map/machine-groups-all.png)

Wenn Sie die Ansicht ändern, sodass nur die **mit Gruppen verbundenen Prozesse** angezeigt werden, wird die Zuordnung auf die Prozesse beschränkt, die in direkter Verbindung zu anderen Computern in der Gruppe stehen. So erhalten Sie eine einfachere Ansicht.

![Gefilterte Prozesse von Computergruppen](media/oms-service-map/machine-groups-filtered.png)
 
### <a name="adding-machines-to-a-group"></a>Hinzufügen von Computern zu einer Gruppe
Um Computer zu einer vorhandenen Gruppe hinzuzufügen, aktivieren Sie die Kontrollkästchen neben den gewünschten Computern, und klicken Sie dann auf **Zur Gruppe hinzufügen**.  Wählen Sie dann die Gruppe aus, zu der Sie die Computer hinzugefügt möchten.
 
### <a name="removing-machines-from-a-group"></a>Entfernen von Computern aus einer Gruppe
Erweitern Sie in der Liste der Gruppen den Gruppennamen. Dann werden die Computer der Computergruppe aufgelistet.  Klicken Sie dann auf die Auslassungszeichen neben dem zu entfernenden Computer, und klicken Sie auf **Entfernen**.

![Entfernen eines Computers aus einer Gruppe](media/oms-service-map/machine-groups-remove.png)

### <a name="removing-or-renaming-a-group"></a>Entfernen oder Umbenennen einer Gruppe
Klicken Sie auf die Auslassungszeichen neben dem Gruppennamen in der Gruppenliste.

![Menü der Computergruppe](media/oms-service-map/machine-groups-menu.png)


## <a name="role-icons"></a>Rollensymbole
Bestimmte Prozesse haben bestimmte Rollen auf Computern: Webserver, Anwendungsserver, Datenbank usw. Service Map versieht Prozess- und Computerfelder mit Rollensymbolen, damit die Rolle eines Prozesses oder Servers auf einen Blick erkannt werden kann.

| Rollensymbol | Beschreibung |
|:--|:--|
| ![Webserver](media/oms-service-map/role-web-server.png) | Webserver |
| ![App-Server](media/oms-service-map/role-application-server.png) | Anwendungsserver |
| ![Datenbankserver](media/oms-service-map/role-database.png) | Datenbankserver |
| ![LDAP-Server](media/oms-service-map/role-ldap.png) | LDAP-Server |
| ![SMB-Server](media/oms-service-map/role-smb.png) | SMB-Server |

![Rollensymbole](media/oms-service-map/role-icons.png)


## <a name="failed-connections"></a>Verbindungsfehler
Verbindungsfehler werden in Service Map-Zuordnungen für Prozesse und Computer angezeigt. Eine gestrichelte rote Linie zeigt an, dass ein Clientsystem einen Prozess oder Port nicht erreichen kann. Verbindungsfehler werden von jedem System mit einem bereitgestellten Service Map-Agent berichtet, wenn dieses System dasjenige ist, das versucht, die fehlerhafte Verbindung herzustellen. Service Map misst diesen Prozess durch Überwachen der TCP-Sockets, die keine Verbindung herstellen können. Der Fehler kann durch eine Firewall, eine fehlerhafte Konfiguration auf einem Client oder Server oder dadurch verursacht werden, dass ein Remotedienst nicht verfügbar ist.

![Verbindungsfehler](media/oms-service-map/failed-connections.png)

Kenntnisse zu Verbindungsfehlern können Ihnen bei der Problembehandlung, der Überprüfung einer Migration, der Sicherheitsanalyse und dem allgemeinen Verständnis der Architektur helfen. Manchmal sind Verbindungsfehler harmlos, häufig weisen sie aber auch direkt auf ein Problem hin, z.B. wenn eine Failoverumgebung plötzlich nicht mehr erreichbar ist oder zwei Anwendungsebenen nach einer Cloudmigration nicht mehr kommunizieren können.

## <a name="client-groups"></a>Clientgruppen
Clientgruppen sind die Felder auf der Karte, die Clientcomputer ohne Dependency-Agents darstellen. Eine einzelne Clientgruppe stellt die Clients für einen einzelnen Prozess oder Computer dar.

![Clientgruppen](media/oms-service-map/client-groups.png)

Um die IP-Adressen der Server in einer Clientgruppe anzuzeigen, wählen Sie die Gruppe aus. Der Inhalt der Gruppe wird im Bereich **Eigenschaften von Clientgruppen** aufgelistet.

![Eigenschaften von Clientgruppen](media/oms-service-map/client-group-properties.png)

## <a name="server-port-groups"></a>Serverportgruppen
Serverportgruppen sind Felder, die Serverports auf Servern darstellen, die über keinen Dependency-Agent verfügen. Das Feld enthält den Serverport zusammen mit der Anzahl von Servern mit Verbindungen zu diesem Port. Erweitern Sie das Feld, um die einzelnen Server und Verbindungen anzuzeigen. Wenn das Feld nur einen Server enthält, ist der Name oder die IP-Adresse aufgeführt.

![Serverportgruppen](media/oms-service-map/server-port-groups.png)

## <a name="context-menu"></a>Kontextmenü
Wenn Sie rechts oberhalb eines Servers auf die Auslassungszeichen (...) klicken, wird das Kontextmenü für diesen Server angezeigt.

![Verbindungsfehler](media/oms-service-map/context-menu.png)

### <a name="load-server-map"></a>Laden einer Serverzuordnung
Über **Serverzuordnung laden** gelangen Sie zu einer neuen Zuordnung mit dem ausgewählten Server als neuem untersuchten Computer.

### <a name="show-self-links"></a>Anzeigen seiteninterner Links
Über **Seiteninterne Links anzeigen** wird der Serverknoten mit allen seiteninternen Links (TCP-Verbindungen, die bei Prozessen auf dem Server starten und enden) neu gezeichnet. Wenn seiteninterne Links angezeigt werden, ändert sich der Menübefehl in **Seiteninterne Links anzeigen**, sodass Sie diese deaktivieren können.

## <a name="computer-summary"></a>Computerzusammenfassung
Der Bereich **Computerzusammenfassung** enthält eine Übersicht der Betriebssysteme eines Servers, der Zahl der Abhängigkeiten und der Daten anderer OMS-Lösungen. Hierzu zählen Leistungsmetriken, Service Desk-Tickets, Änderungsnachverfolgung, Sicherheit und Updates.

![Bereich „Computerzusammenfassung“](media/oms-service-map/machine-summary.png)

## <a name="computer-and-process-properties"></a>Computer- und Prozesseigenschaften
Beim Navigieren in einer Service Map-Zuordnung können Sie Computer und Prozesse auswählen, um zusätzlichen Kontext zu deren Eigenschaften zu erhalten. Folgende Informationen zu Computern können angezeigt werden: DNS-Name, IPv4-Adressen, CPU- und Arbeitsspeicherkapazität, VM-Typ, Betriebssystemversion, Uhrzeit des letzten Neustarts und die IDs ihrer OMS- und Service Map-Agents.

![Bereich „Computereigenschaften“](media/oms-service-map/machine-properties.png)

Sie können Prozessdetails aus Metadaten des Betriebssystems zu ausgeführten Prozessen erfassen: Prozessname und -beschreibung, Benutzername und -domäne (unter Windows), Firmenname, Produktname, Produktversion, Arbeitsverzeichnis, Befehlszeile und Startzeit des Prozesses.

![Bereich „Prozesseigenschaften“](media/oms-service-map/process-properties.png)

Der Bereich **Prozessübersicht** bietet zusätzliche Informationen über die Konnektivität eines Prozesses, einschließlich Bindungsports, eingehender und ausgehender Verbindungen sowie Verbindungsfehler.

![Bereiche „Prozessübersicht“](media/oms-service-map/process-summary.png)

## <a name="operations-management-suite-alerts-integration"></a>Integration in OMS-Warnungen
Service Map ist in OMS-Warnungen integriert, um Warnungen für den ausgewählten Server anzuzeigen, die im ausgewählten Zeitraum ausgelöst werden. Für den Server wird ein Symbol angezeigt, wenn aktuelle Warnungen vorliegen, und im **Computerwarnungsbereich** werden die Warnungen aufgelistet.

![Bereich „Computerwarnungen“](media/oms-service-map/machine-alerts.png)

Damit Service Map die relevanten Warnungen anzeigt, müssen Sie eine Warnungsregel aufstellen, die für einen bestimmten Computer ausgelöst wird. So erstellen Sie richtige Warnungen:
- Beziehen Sie eine Klausel zur Gruppierung nach Computer ein (z.B. **by Computer interval 1minute**).
- Wählen Sie die Warnung auf Basis der Metrikmessung.

![Warnungskonfiguration](media/oms-service-map/alert-configuration.png)


## <a name="operations-management-suite-log-events-integration"></a>Integration in OMS-Protokollereignisse
Service Map ist in die Protokollsuche integriert, um die Anzahl aller verfügbaren Protokollereignisse für den ausgewählten Server während des ausgewählten Zeitraums anzuzeigen. Sie können auf eine beliebige Zeile in der Liste der Ereignisanzahl klicken, um zur Protokollsuche zu wechseln und die einzelnen Protokollereignisse anzuzeigen.

![Bereich „Computerprotokollereignisse“](media/oms-service-map/log-events.png)

## <a name="operations-management-suite-service-desk-integration"></a>Integration in OMS-Service Desk
Die Integration von Service Map in den IT Service Management Connector erfolgt automatisch, wenn beide Lösungen in Ihrem OMS-Arbeitsbereich aktiviert und konfiguriert sind. Die Integration in Service Map wird als „Service Desk“ bezeichnet. Weitere Informationen finden Sie unter [Centrally manage ITSM work items using IT Service Management Connector (Zentrales Verwalten von ITSM-Arbeitselementen mit dem IT Service Management Connector)](https://docs.microsoft.com/azure/log-analytics/log-analytics-itsmc-overview).

Im Bereich **Computer-Service Desk** wird eine Liste aller IT Service Management-Ereignisse für den ausgewählten Server im ausgewählten Zeitraum angezeigt. Der Server zeigt ein Symbol an, wenn es aktuelle Elemente gibt. Diese werden im Bereich „Computer-Service Desk“ angezeigt.

![Bereich „Computer-Service Desk“](media/oms-service-map/service-desk.png)

Klicken Sie auf **Arbeitselement anzeigen**, um das Element in der verbundenen ITSM-Lösung zu öffnen.

Klicken Sie auf **In Protokollsuche anzeigen**, um die Details des Elements in der Protokollsuche anzuzeigen.


## <a name="operations-management-suite-change-tracking-integration"></a>Integration in OMS-Änderungsnachverfolgung
Die Integration von Service Map in die Änderungsnachverfolgung erfolgt automatisch, wenn beide Lösungen im OMS-Arbeitsbereich aktiviert und konfiguriert werden.

Der Bereich für die **Nachverfolgung von Änderungen auf einem Computer** zeigt eine Liste aller Änderungen an, wobei die jüngste Änderung zuerst angezeigt wird. Der Bereich bietet auch einen Link, mit dem Sie die Protokollsuche aufrufen und weitere Details anzeigen können.

![Bereich für die Änderungsnachverfolgung auf einem Computer](media/oms-service-map/change-tracking.png)

Die folgende Abbildung ist eine Detailansicht eines ConfigurationChange-Ereignisses, dass Ihnen möglicherweise angezeigt wird, wenn Sie auf **In Log Analytics anzeigen** klicken.

![Ereignis „ConfigurationChange“](media/oms-service-map/configuration-change-event.png)


## <a name="operations-management-suite-performance-integration"></a>Integration in die OMS-Leistung
Im **Computerleistungsbereich** werden Standardleistungsmetriken für den ausgewählten Server angezeigt. Die Metriken umfassen CPU-Auslastung, Speicherauslastung, über das Netzwerk gesendete und empfangene Bytes und eine Liste der wichtigsten Prozesse nach über das Netzwerk gesendeten und empfangenen Bytes. Um die Leistungsdaten für das Netzwerk abzurufen, muss auch die Wire Data 2.0-Lösung in der OMS aktiviert sein.

![Computerleistungsbereich](media/oms-service-map/machine-performance.png)


## <a name="operations-management-suite-security-integration"></a>Integration in OMS-Sicherheit
Die Integration von Service Map in Sicherheit und Überwachung erfolgt automatisch, wenn beide Lösungen im OMS-Arbeitsbereich aktiviert und konfiguriert werden.

Im **Computersicherheitsbereich** werden Daten aus der Sicherheits- und Überwachungslösung von OMS für den ausgewählten Server angezeigt. Im Bereich wird ggf. eine Zusammenfassung herausragender Sicherheitsprobleme für den Server angezeigt, die während des ausgewählten Zeitraums aufgetreten sind. Beim Klicken auf ein Sicherheitsproblem wird eine Protokollsuche nach ausführlichen Informationen zu diesen durchgeführt.

![Bereich für Computersicherheit](media/oms-service-map/machine-security.png)


## <a name="operations-management-suite-updates-integration"></a>Integration in OMS-Updates
Die Integration von Service Map in die Updateverwaltung erfolgt automatisch, wenn beide Lösungen in Ihrem OMS-Arbeitsbereich aktiviert und konfiguriert sind.

Der Bereich **Computerupdates** zeigt die Daten der OMS-Updateverwaltungslösung des ausgewählten Servers an. Im Bereich wird ggf. für den ausgewählten Zeitraum eine Zusammenfassung fehlender Updates für den Server angezeigt.

![Bereich für die Änderungsnachverfolgung auf einem Computer](media/oms-service-map/machine-updates.png)

## <a name="log-analytics-records"></a>Log Analytics-Datensätze
Die Computer- und Prozessbestandsdaten von Service Map stehen in Log Analytics zur [Suche](../log-analytics/log-analytics-log-searches.md) zur Verfügung. Diese Daten können in verschiedenen Szenarios von Nutzen sein, z.B. bei der Migrationsplanung, Kapazitätsanalyse, Ermittlung und Ad-hoc-Behebung von Leistungsproblemen.

Zusätzlich zu den Datensätzen, die beim Starten eines Prozesses oder Computers oder beim Onboarding in Service Map generiert werden, wird pro Stunde ein Datensatz für jeden eindeutigen Computer und jeden eindeutigen Prozess generiert. Die Eigenschaften der Datensätze sind in den folgenden Tabellen aufgeführt. Die Felder und Werte in den ServiceMapComputer_CL-Ereignissen sind Feldern der Computerressource in der ServiceMap ARM-API (Azure Resource Manager) zugeordnet. Die Felder und Werte in den ServiceMapProcess_CL-Ereignissen sind Feldern der Prozessressource in der ServiceMap ARM-API zugeordnet. Das Feld „ResourceName_s“ entspricht dem Namensfeld in der entsprechenden ARM-Ressource. 

>[!NOTE]
>Mit dem Anwachsen des Service Map-Features können sich diese Felder ändern.

Es gibt intern generierte Eigenschaften, mit denen Sie eindeutige Prozessen und Computer identifizieren können:

- Computer: Verwenden Sie „ResourceId“ oder „ResourceName_s“ zur eindeutigen Identifizierung eines Computers in einem OMS-Arbeitsbereich.
- Prozess: Verwenden Sie „ResourceId“ zur eindeutigen Identifizierung eines Prozesses in einem OMS-Arbeitsbereich. „ResourceName_s“ ist innerhalb des Kontexts des Computers, auf dem der Prozess ausgeführt wird (MachineResourceName_s), eindeutig. 

Da für einen angegebenen Prozess und Computer in einem angegebenen Zeitraum möglicherweise mehrere Datensätze vorhanden sind, können Abfragen mehrere Datensätze für denselben Computer oder Prozess zurückgeben. Wenn nur der aktuellste Datensatz zurückgegeben werden soll, fügen Sie der Abfrage „| dedup ResourceId“ hinzu.

### <a name="servicemapcomputercl-records"></a>ServiceMapComputer_CL-Datensätze
Datensätze des Typs *ServiceMapComputer_CL* enthalten Bestandsdaten für Server mit Service Map-Agents. Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt:

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ | *ServiceMapComputer_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | Der eindeutige Bezeichner für den Computer innerhalb des Arbeitsbereichs |
| ResourceName_s | Der eindeutige Bezeichner für den Computer innerhalb des Arbeitsbereichs |
| ComputerName_s | Der vollqualifizierte Domänenname des Computers |
| Ipv4Addresses_s | Eine Liste der IPv4-Adressen des Servers |
| Ipv6Addresses_s | Eine Liste der IPv6-Adressen des Servers |
| DnsNames_s | Ein Array von DNS-Namen |
| OperatingSystemFamily_s | Windows oder Linux |
| OperatingSystemFullName_s | Der vollständige Name des Betriebssystems  |
| Bitness_s | Die Bitanzahl des Computers (32-Bit oder 64-Bit)  |
| PhysicalMemory_d | Der physischer Speicher in MB |
| Cpus_d | Die Anzahl der CPUs |
| CpuSpeed_d | Die CPU-Geschwindigkeit in MHz|
| VirtualizationState_s | *unknown*, *physical*, *virtual*, *hypervisor* |
| VirtualMachineType_s | *hyperv*, *vmware* usw. |
| VirtualMachineNativeMachineId_g | Die VM-ID, die vom entsprechenden Hypervisor zugewiesen wurde |
| VirtualMachineName_s | Der Name der VM |
| BootTime_t | Die Startzeit |



### <a name="servicemapprocesscl-type-records"></a>Datensätze des ServiceMapProcess_CL-Typs
Datensätze des Typs *ServiceMapProcess_CL* enthalten Bestandsdaten für über TCP verbundene Prozesse auf Servern mit Service Map-Agents. Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt:

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ | *ServiceMapProcess_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | Der eindeutige Bezeichner für den Prozess innerhalb des Arbeitsbereichs |
| ResourceName_s | Der eindeutige Bezeichner für den Prozess auf dem Computer, auf dem er ausgeführt wird|
| MachineResourceName_s | Der Ressourcenname des Computers |
| ExecutableName_s | Der Name der ausführbaren Prozessdatei |
| StartTime_t | Die Startzeit des Prozesspools |
| FirstPid_d | Die erste PID im Prozesspool |
| Description_s | Die Beschreibung des Prozesses |
| CompanyName_s | Der Name des Unternehmens |
| InternalName_s | Der interne Name |
| ProductName_s | Der Name des Produkts |
| ProductVersion_s | Die Produktversion |
| FileVersion_s | Die Dateiversion |
| CommandLine_s | Die Befehlszeile |
| ExecutablePath _s | Der Pfad zur ausführbaren Datei |
| WorkingDirectory_s | Das Arbeitsverzeichnis |
| UserName | Das Konto, unter dem der Prozess ausgeführt wird |
| UserDomain | Die Domäne, unter der der Prozess ausgeführt wird |


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

### <a name="list-all-known-processes-on-a-specified-machine"></a>Auflisten aller bekannten Prozesse auf einem angegebenen Computer auf
Type=ServiceMapProcess_CL MachineResourceName_s="m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | dedup ResourceId

### <a name="list-all-computers-running-sql"></a>Auflisten aller Computer, auf denen SQL ausgeführt wird
Type=ServiceMapComputer_CL ResourceName_s IN {Type=ServiceMapProcess_CL \*sql\* | Distinct MachineResourceName_s} | dedup ResourceId | Distinct ComputerName_s

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Auflisten aller eindeutigen Produktversionen von „curl“ im eigenen Rechenzentrum
Type=ServiceMapProcess_CL ExecutableName_s=curl | Distinct ProductVersion_s

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Erstellen einer Computergruppe mit allen Computern, auf denen CentOS ausgeführt wird
Type=ServiceMapComputer_CL OperatingSystemFullName_s = \*CentOS\* | Distinct ComputerName_s


## <a name="rest-api"></a>REST-API
Alle Server-, Prozess- und Abhängigkeitsdaten in Service Map stehen über die [Service Map-REST-API](https://docs.microsoft.com/rest/api/servicemap/) zur Verfügung.


## <a name="diagnostic-and-usage-data"></a>Diagnose- und Nutzungsdaten
Wenn Sie den Service Map-Dienst verwenden, sammelt Microsoft automatisch Nutzungs- und Leistungsdaten. Microsoft verwendet diese Daten, um die Qualität, Sicherheit und Integrität des Service Map-Diensts sicherzustellen und zu verbessern. Zu den Daten gehören Informationen zur Konfiguration Ihrer Software, z.B. Betriebssystem und Betriebssystemversion sowie IP-Adresse, DNS-Name und Name der Arbeitsstation, um exakte und effiziente Funktionen für die Problembehandlung bereitzustellen. Microsoft erfasst weder Namen noch Adressen oder andere Kontaktinformationen.

Weitere Informationen zur Sammlung und Nutzung von Daten finden Sie in den [Datenschutzbestimmungen für Onlinedienste von Microsoft](https://go.microsoft.com/fwlink/?LinkId=512132).


## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [Protokollsuchvorgänge](../log-analytics/log-analytics-log-searches.md) in Log Analytics, um Daten abzurufen, die von Service Map gesammelt wurden.


## <a name="troubleshooting"></a>Problembehandlung
Weitere Informationen finden Sie im [Abschnitt zur Problembehandlung der Konfigurationsdokumentation für Service Map](operations-management-suite-service-map-configure.md#troubleshooting).


## <a name="feedback"></a>Feedback
Haben Sie Feedback für uns zu Service Map oder dieser Dokumentation?  Besuchen Sie unsere [User Voice-Webseite](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), auf der Sie Funktionen vorschlagen oder vorhandene Vorschläge unterstützen können.

