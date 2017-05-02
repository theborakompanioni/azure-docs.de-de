---
title: "Lösung für die Updateverwaltung in OMS | Microsoft Docs"
description: "In diesem Artikel soll vermittelt werden, wie Sie diese Lösung zum Verwalten von Updates für Ihre Windows- und Linux-Computer verwenden."
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: e33ce6f9-d9b0-4a03-b94e-8ddedcc595d2
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/20/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: b0624bde9ab53231768beb0c832a4a49b21a8975
ms.lasthandoff: 04/20/2017


---
# <a name="update-management-solution-in-oms"></a>Lösung für die Updateverwaltung in OMS
Mit der Lösung für die Updateverwaltung in OMS können Sie Updates für Ihre Windows- und Linux-Computer verwalten.  Sie können den Status der verfügbaren Updates auf allen Agent-Computern schnell bewerten und den Installationsvorgang initiieren, der für die Serverupdates erforderlich ist. 

## <a name="solution-components"></a>Lösungskomponenten

Für Computer, die mit OMS verwaltet werden, wird Folgendes verwendet, um Bewertungen und Updatebereitstellungen durchzuführen: 

* OMS-Agent für Windows oder Linux
* PowerShell Desired State Configuration (DSC) für Linux 
* Automation Hybrid Runbook Worker 
* Microsoft Update oder Windows Server Update Services für Windows-Computer

Die folgenden Abbildungen enthalten eine konzeptionelle Darstellung des Verhaltens und des Datenflusses. Es wird gezeigt, wie die Lösung alle verbundenen Windows Server- und Linux-Computer eines Arbeitsbereichs bewertet und Updates darauf anwendet.    

#### <a name="windows-server"></a>Windows Server
![Prozessablauf der Windows Server-Updateverwaltung](media/oms-solution-update-management/update-mgmt-windows-updateworkflow.png)

#### <a name="linux"></a>Linux
![Prozessablauf der Linux-Updateverwaltung](media/oms-solution-update-management/update-mgmt-linux-updateworkflow.png)

Nachdem der Computer einen Scanvorgang durchgeführt hat, um die Konformität für das Update zu überprüfen, leitet der OMS-Agent die Informationen gesammelt an OMS weiter. Auf einem Windows-Computer wird der Konformitätsscan standardmäßig alle zwölf Stunden durchgeführt.  Zusätzlich zum Scanzeitplan wird der Scanvorgang für die Updatekonformität innerhalb von 15 Minuten initiiert, wenn der Microsoft Monitoring Agent (MMA) neu gestartet wird – jeweils vor und nach der Installation des Updates.  Bei einem Linux-Computer wird der Konformitätsscan standardmäßig alle drei Stunden durchgeführt, und bei einem MMA-Neustart erfolgt innerhalb von 15 Minuten ein Konformitätsscan.  

Der Konformitätsscan wird dann verarbeitet und in den Dashboards der Lösung zusammengefasst oder kann mit benutzerdefinierten bzw. vorab definierten Abfragen durchsucht werden.  Die Lösung meldet basierend darauf, welche Quelle für die Synchronisierung konfiguriert ist, wie aktuell der Computer ist.  Wenn der Windows-Computer für das Senden von Meldungen an WSUS konfiguriert ist, können sich die Ergebnisse von den angezeigten Microsoft Update-Ergebnissen unterscheiden. Dies hängt davon ab, wann WSUS zuletzt mit Microsoft Update synchronisiert wurde.  Dasselbe gilt für Linux-Computer, die für das Melden an ein lokales Repository konfiguriert sind, anstatt an ein öffentliches Repository.   

Sie können Softwareupdates auf Computern bereitstellen und installieren, für die die Updates erforderlich sind, indem Sie einen geplante Bereitstellung erstellen.  Updates, die als *Optional* klassifiziert sind, sind im Bereitstellungsumfang von Windows-Computern nicht enthalten, sondern nur erforderliche Updates.  Bei der geplanten Bereitstellung wird definiert, welche Zielcomputer die jeweiligen Updates erhalten – entweder durch das explizite Angeben von Computern oder das Auswählen einer [Computergruppe](../log-analytics/log-analytics-computer-groups.md), die auf Protokollsuchen einer bestimmten Gruppe von Computern basiert.  Außerdem geben Sie einen Zeitplan an, um einen Zeitraum zu genehmigen und festzulegen, in dem Updates installiert werden dürfen.  Updates werden mit Runbooks in Azure Automation installiert.  Sie können diese Runbooks nicht anzeigen, und für die Runbooks ist keine Konfiguration erforderlich.  Bei der Erstellung einer Updatebereitstellung wird ein Zeitplan erstellt, nach dem für die einbezogenen Computer zur angegebenen Zeit ein Masterrunbook für das Update gestartet wird.  Über dieses Masterrunbook wird ein untergeordnetes Runbook auf jedem Agent gestartet, mit dem die Installation von erforderlichen Updates durchgeführt wird.       

Wenn die Datums- bzw. Uhrzeitangabe der Updatebereitstellung erreicht ist, führt der Zielcomputer die Bereitstellung parallel aus.  Zuerst wird ein Scanvorgang durchgeführt, um sicherzustellen, dass die Updates weiterhin erforderlich sind, und anschließend werden sie installiert.  Beachten Sie folgenden wichtigen Hinweis: Für WSUS-Clientcomputer schlägt die Updatebereitstellung fehl, wenn die Updates in WSUS nicht genehmigt sind.  Die Ergebnisse der angewendeten Updates werden an OMS weitergeleitet, damit sie in den Dashboards verarbeitet und zusammengefasst werden können (oder per Ereignissuche).     

## <a name="prerequisites"></a>Voraussetzungen
* Die Lösung unterstützt die Durchführung von Updatebewertungen für Windows Server 2008 und höher und Updatebereitstellungen für Windows Server 2012 und höher.  Server Core- und Nano Server-Installationsoptionen werden nicht unterstützt.
* Windows-Clientbetriebssysteme werden nicht unterstützt.  
* Windows-Agents müssen entweder für die Kommunikation mit einem WSUS-Server (Windows Server Update Services) konfiguriert sein oder über Zugriff auf Microsoft-Update verfügen.  
  
    > [!NOTE]
    > Der Windows-Agent kann nicht gleichzeitig mit System Center Configuration Manager verwaltet werden.  
    >
* CentOS 6 (x86/x64) und 7 (x64)
* Red Hat Enterprise 6 (x86/x64) und 7 (x64)
* SUSE Linux Enterprise Server 11 (x86/x64) und 12 (x64)
* Ubuntu 12.04 LTS und höher (x86/x64)  
* Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.  

    > [!NOTE]
    > Ein OMS-Agent für Linux, der für das Melden an mehrere OMS-Arbeitsbereiche konfiguriert ist, wird für diese Lösung nicht unterstützt.  
    > 

Weitere Informationen dazu, wie Sie den OMS-Agent für Linux installieren und die aktuelle Version herunterladen, finden Sie im Artikel zum [Operations Management Suite-Agent für Linux](https://github.com/microsoft/oms-agent-for-linux).  Informationen zur Installation des OMS-Agents für Windows finden Sie unter [Verbinden von Windows-Computern mit dem Log Analytics-Dienst in Azure](../log-analytics/log-analytics-windows-agents.md).  

## <a name="solution-components"></a>Lösungskomponenten
Diese Lösung besteht aus den folgenden Ressourcen, die Ihrem Automation-Konto hinzugefügt werden, und direkt verbundenen Agents oder mit Operations Manager verbundenen Verwaltungsgruppen. 

### <a name="management-packs"></a>Management Packs
Wenn Ihre System Center Operations Manager-Verwaltungsgruppe mit einem OMS-Arbeitsbereich verbunden ist, werden in Operations Manager die folgenden Management Packs installiert.  Diese Management Packs werden nach dem Hinzufügen dieser Lösung auch auf direkt verbundenen Windows-Computern installiert. Für diese Management Packs fällt kein Konfigurations- oder Verwaltungsaufwand an. 

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Update Deployment MP

Weitere Informationen zur Aktualisierung von Management Packs finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](../log-analytics/log-analytics-om-agents.md).

### <a name="hybrid-worker-groups"></a>Hybrid Worker-Gruppen
Nachdem Sie diese Lösung aktiviert haben, werden alle direkt mit dem OMS-Arbeitsbereich verbundenen Windows-Computer automatisch als Hybrid Runbook Worker konfiguriert, um die in dieser Lösung enthaltenen Runbooks zu unterstützen.  Für jeden von der Lösung verwalteten Windows-Computer wird dieser auf dem Blatt „Hybrid Runbook Worker Groups“ (Hybrid-Runbook-Workergruppen) des Automation-Kontos aufgeführt. Die Benennungskonvention lautet *Hostname FQDN_GUID*.  Es ist nicht möglich, diese Gruppen mit Runbooks in Ihrem Konto zu erreichen, da der Vorgang fehlschlägt. Diese Gruppen sind nur für die Unterstützung der Verwaltungslösung bestimmt.   

Aber Sie können die Windows-Computer einer Hybrid Runbook Worker-Gruppe in Ihrem Automation-Konto hinzufügen, um Automation-Runbooks zu unterstützen, solange Sie sowohl für die Lösung als auch die Mitgliedschaft in der Hybrid Runbook Worker-Gruppe dasselbe Konto verwenden.  Diese Funktionalität wurde Version 7.2.12024.0 des Hybrid Runbook Worker hinzugefügt.  

## <a name="configuration"></a>Konfiguration
Führen Sie die folgenden Schritte aus, um die Lösung für die Updateverwaltung dem OMS-Arbeitsbereich hinzuzufügen und zu bestätigen, dass die Agents Meldungen senden. Windows-Agents werden automatisch ohne weitere Konfiguration hinzugefügt. 

Es gibt drei Möglichkeiten, wie Sie diese Lösung hinzufügen können: über den Azure Marketplace im Azure-Portal, indem Sie entweder die Option „Automation + Control“ oder die Lösung für die Updateverwaltung wählen, oder über den OMS-Lösungskatalog in Ihrem OMS-Arbeitsbereich.  Falls Sie in derselben Ressourcengruppe und Region bereits ein Automation-Konto mit einem OMS-Arbeitsbereich verknüpft haben, wird die Konfiguration bei Auswahl von „Automation + Control“ überprüft und nur die Lösung installiert und für beide Dienste konfiguriert.  Für die Auswahl der Lösung für die Updateverwaltung im Azure Marketplace gilt dasselbe Verhalten.  Wenn Sie diese Dienste unter Ihrem Abonnement nicht bereitgestellt haben, können Sie die Schritte auf dem Blatt **Neue Lösung erstellen** ausführen und bestätigen, dass Sie die anderen vorab ausgewählten empfohlenen Lösungen installieren möchten.  Optional können Sie Ihrem OMS-Arbeitsbereich mit den unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](../log-analytics/log-analytics-add-solutions.md) beschriebenen Schritten die Lösung für die Updateverwaltung hinzufügen.  

### <a name="confirm-oms-agents-and-operations-manager-management-group-connected-to-oms"></a>Bestätigen der Verbindung von OMS-Agents und der Operations Manager-Verwaltungsgruppe mit OMS

Nach einigen Minuten können Sie die folgende Protokollsuche durchführen, um zu bestätigen, dass der direkt verbundene OMS-Agent für Linux und Windows jeweils mit OMS kommuniziert:

* Linux – `Type=Heartbeat OSType=Linux | top 500000 | dedup SourceComputerId | Sort Computer | display Table`.  

* Windows – `Type=Heartbeat OSType=Windows | top 500000 | dedup SourceComputerId | Sort Computer | display Table`

Auf einem Windows-Computer können Sie Folgendes überprüfen, um für den Agent die OMS-Konnektivität zu bestätigen:

1.  Öffnen Sie in der Systemsteuerung den Microsoft Monitoring Agent. Auf der Registerkarte **Azure Log Analytics (OMS)** wird vom Agent folgende Meldung angezeigt: **The Microsoft Monitoring Agent has successfully connected to the Microsoft Operations Management Suite service** (Für den Microsoft Monitoring Agent wurde die Verbindung mit dem Microsoft Operations Management Suite-Dienst erfolgreich hergestellt).   
2.  Öffnen Sie das Windows-Ereignisprotokoll, navigieren Sie zu **Anwendungs- und Dienstprotokolle\Operations Manager**, und suchen Sie nach der Ereignis-ID 3000 und 5002 (Service Connector-Quellinstanz).  Mit diesen Ereignissen wird angegeben, dass für den Computer die Registrierung beim OMS-Arbeitsbereich und die Konfiguration durchgeführt wurden.  

Falls der Agent nicht mit dem OMS-Dienst kommunizieren kann und für die Kommunikation mit dem Internet über eine Firewall oder einen Proxyserver konfiguriert ist, sollten Sie für die Firewall bzw. den Proxyserver die Richtigkeit der Konfiguration sicherstellen, indem Sie die Vorgehensweise unter [Konfigurieren von Proxy- und Firewalleinstellungen in Log Analytics](../log-analytics/log-analytics-proxy-firewall.md) verwenden.
  
Für neu hinzugefügte Linux-Agents wird der Status **Aktualisiert** angezeigt, nachdem eine Bewertung durchgeführt wurde.  Dieser Vorgang kann bis zu sechs Stunden dauern. 

Wenn Sie bestätigen möchten, dass eine Operations Manager-Verwaltungsgruppe mit OMS kommuniziert, helfen Ihnen die Informationen unter [Überprüfen der Operations Manager-Integration für OMS](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-oms) weiter.

## <a name="data-collection"></a>Datensammlung
### <a name="supported-agents"></a>Unterstützte Agents
In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden.

| Verbundene Quelle | Unterstützt | Beschreibung |
| --- | --- | --- |
| Windows-Agents |Ja |Die Lösung sammelt Informationen zu Systemupdates aus Windows-Agents und initiiert die Installation von erforderlichen Updates. |
| Linux-Agents |Ja |Die Lösung sammelt Informationen zu Systemupdates von Linux-Agents und initiiert die Installation von erforderlichen Updates für unterstützte Distributionen. |
| Operations Manager-Verwaltungsgruppe |Ja |Die Lösung sammelt Informationen zu Systemupdates von Agents in einer verbundenen Verwaltungsgruppe.<br>Es ist keine direkte Verbindung von Operations Manager mit Log Analytics erforderlich. Daten werden von der Verwaltungsgruppe an das OMS-Repository weitergeleitet. |
| Azure-Speicherkonto |Nein |Azure-Speicher enthält keine Informationen zu Systemupdates. |

### <a name="collection-frequency"></a>Sammlungshäufigkeit
Für jeden verwalteten Windows-Computer wird zweimal pro Tag ein Scanvorgang durchgeführt. Alle 15 Minuten wird die Windows-API aufgerufen, um den letzten Updatezeitpunkt abzufragen und zu ermitteln, ob sich der Status geändert hat. Wenn ja, wird ein Konformitätsscan initiiert.  Für jeden verwalteten Linux-Computer wird alle drei Stunden ein Scanvorgang durchgeführt. 

Es kann zwischen 30 Minuten und sechs Stunden dauern, bis im Dashboard aktualisierte Daten von verwalteten Computern angezeigt werden.   

## <a name="using-the-solution"></a>Verwenden der Lösung
Wenn Sie dem OMS-Arbeitsbereich die Lösung für die Updateverwaltung hinzufügen, wird Ihrem OMS-Dashboard die Kachel **Update Management** (Updateverwaltung) hinzugefügt. Auf dieser Kachel werden ein Zahlenwert und eine grafische Darstellung der Anzahl von Computern in Ihrer Umgebung und jeweils die Updatekonformität angezeigt.<br><br>
![Kachel mit Zusammenfassung zur Updateverwaltung](media/oms-solution-update-management/update-management-summary-tile.png)  


## <a name="viewing-update-assessments"></a>Anzeigen von Updatebewertungen
Klicken Sie auf die Kachel **Updateverwaltung**, um das Dashboard **Update Management** (Updateverwaltung) zu öffnen.<br><br> ![Dashboard mit Zusammenfassung zur Updateverwaltung](./media/oms-solution-update-management/update-management-dashboard.png)<br> 

Dieses Dashboard enthält eine ausführliche Auflistung des Updatestatus, kategorisiert nach Betriebssystemtyp und Updateklassifizierung: kritisch, Sicherheitsupdate oder Sonstiges (z.B. Definitionsupdate). Wenn die Kachel **Updatebereitstellungen** ausgewählt ist, werden Sie auf die Seite „Updatebereitstellungen“ weitergeleitet, auf der Sie Zeitpläne, derzeit ausgeführte Bereitstellungen und abgeschlossene Bereitstellungen anzeigen oder eine neue Bereitstellung planen können.  

Sie können eine Protokollsuche ausführen, mit der alle Datensätze zurückgegeben werden, indem Sie auf die jeweilige Kachel klicken. Um eine Abfrage einer bestimmten Kategorie und mit vordefinierten Kriterien auszuführen, wählen Sie diese in der Liste der Spalte **Häufige Updateabfragen** aus.    

## <a name="installing-updates"></a>Installieren von Updates
Nachdem die Updates für alle Linux- und Windows-Computer des Arbeitsbereichs bewertet wurden, installieren Sie die erforderlichen Updates, indem Sie eine *Updatebereitstellung* erstellen.  Eine Updatebereitstellung ist eine geplante Installation von erforderlichen Updates für mindestens einen Computer.  Sie geben das Datum und die Uhrzeit für die Bereitstellung und einen Computer bzw. eine Gruppe von Computern an, die in den Umfang der Bereitstellung einbezogen werden sollen.  Weitere Informationen zu Computergruppen finden Sie unter [Computergruppen in Log Analytics](../log-analytics/log-analytics-computer-groups.md).  Wenn Sie Computergruppen in Ihre Updatebereitstellung einbinden, wird die Gruppenmitgliedschaft nur einmal beim Erstellen des Zeitplans ausgewertet.  Nachfolgende Änderungen einer Gruppe werden nicht widergespiegelt.  Löschen Sie die geplante Updatebereitstellung, und erstellen Sie sie neu, um dieses Problem zu umgehen. 

> [!NOTE]
> Über den Azure Marketplace bereitgestellte Windows-VMs sind standardmäßig so konfiguriert, dass sie automatisch Updates von Windows Update Service erhalten.  Dieses Verhalten ändert sich nicht, nachdem Sie Ihrem Arbeitsbereich diese Lösung oder Windows-VMs hinzugefügt haben.  Wenn Sie Updates für diese Lösung nicht aktiv verwalten, gilt das Standardverhalten (Updates werden automatisch angewendet).  

Für virtuelle Computer, die basierend auf den über den Azure Marketplace erhältlichen On-Demand-RHEL-Images (Red Hat Enterprise Linux) erstellt werden, werden sie für den Zugriff auf die in Azure bereitgestellte [Red Hat-Updateinfrastruktur (RHUI)](../virtual-machines/linux/update-infrastructure-redhat.md) registriert.  Alle anderen Linux-Distributionen müssen über das Onlinedateirepository der Distributionen gemäß den unterstützten Methoden aktualisiert werden.  

### <a name="viewing-update-deployments"></a>Anzeigen von Updatebereitstellungen
Klicken Sie auf die Kachel **Bereitstellung aktualisieren**, um die Liste mit den vorhandenen Updatebereitstellungen anzuzeigen.  Sie sind nach dem Status gruppiert: **Geplant**, **Wird ausgeführt** und **Abgeschlossen**.<br><br> ![Seite mit Zeitplan für Updatebereitstellungen](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

Die Eigenschaften, die für die Updatebereitstellung angezeigt werden, sind in der folgenden Tabelle beschrieben.

| Eigenschaft | Beschreibung |
| --- | --- |
| Name |Name der Updatebereitstellung |
| Zeitplan |Typ des Zeitplans  Die verfügbaren Optionen sind *Einmal*, *Jede Woche* und *Jeden Monat*. |
| Startzeit |Datum und Uhrzeit des Starts der Updatebereitstellung |
| Dauer |Zulässige Ausführungsdauer der Updatebereitstellung in Minuten.  Wenn innerhalb dieses Zeitraums nicht alle Updates installiert werden, muss für die verbleibenden Updates bis zur nächsten Updatebereitstellung gewartet werden. |
| Server |Anzahl von Computern, die von der Updatebereitstellung betroffen sind  |
| Status |Aktueller Status der Updatebereitstellung<br><br>Mögliche Werte:<br>- Nicht gestartet<br>- Wird ausgeführt<br>- Abgeschlossen |

Wählen Sie eine abgeschlossene Updatebereitstellung aus, um den Detailbildschirm anzuzeigen, der die Spalten in der folgenden Tabelle enthält.  Diese Spalten werden nicht aufgefüllt, wenn die Updatebereitstellung noch nicht gestartet wurde.<br><br> ![Übersicht über Ergebnisse der Updatebereitstellung](./media/oms-solution-update-management/update-management-deploymentresults-dashboard.png)

| Column | Beschreibung |
| --- | --- |
| **Ansicht „Computer“** | |
| Windows-Computer |Listet die Anzahl von Windows-Computern der Updatebereitstellung nach Status auf.  Klicken Sie auf einen Status, um eine Protokollsuche auszuführen, bei der alle Updatedatensätze mit diesem Status für die Updatebereitstellung zurückgegeben werden. |
| Linux-Computer |Listet die Anzahl von Linux-Computern der Updatebereitstellung nach Status auf.  Klicken Sie auf einen Status, um eine Protokollsuche auszuführen, bei der alle Updatedatensätze mit diesem Status für die Updatebereitstellung zurückgegeben werden. |
| Installationsstatus des Computers |Listet die Computer auf, die an der Updatebereitstellung beteiligt sind, und den Prozentsatz der Updates, die erfolgreich installiert wurden. Klicken Sie auf einen der Einträge, um eine Protokollsuche durchzuführen, bei der alle fehlenden und kritischen Updates zurückgegeben werden. |
| **Ansicht „Updates“** | |
| Windows-Updates |Listet Windows-Updates, die Teil der Updatebereitstellung sind, und den Installationsstatus für jedes Update auf.  Wählen Sie ein Update aus, um eine Protokollsuche durchzuführen, bei der alle Updatedatensätze für das jeweilige Update zurückgegeben werden. Klicken Sie auf den Status, um eine Protokollsuche durchzuführen, bei der alle Updatedatensätze für die Bereitstellung zurückgegeben werden. | 
| Linux-Updates |Listet Linux-Updates, die Teil der Updatebereitstellung sind, und den Installationsstatus für jedes Update auf.  Wählen Sie ein Update aus, um eine Protokollsuche durchzuführen, bei der alle Updatedatensätze für das jeweilige Update zurückgegeben werden. Klicken Sie auf den Status, um eine Protokollsuche durchzuführen, bei der alle Updatedatensätze für die Bereitstellung zurückgegeben werden. | 

### <a name="creating-an-update-deployment"></a>Erstellen einer Updatebereitstellung
Erstellen Sie eine neue Updatebereitstellung, indem Sie oben auf dem Bildschirm auf die Schaltfläche **Hinzufügen** klicken, um die Seite **New Update Deployment** (Neue Updatebereitstellung) zu öffnen.  Sie müssen Werte für die Eigenschaften in der folgenden Tabelle angeben.

| Eigenschaft | Beschreibung |
| --- | --- |
| Name |Eindeutiger Name zum Identifizieren der Updatebereitstellung |
| Zeitzone |Zeitzone für die Startzeit |
| Zeitplantyp | Typ des Zeitplans  Die verfügbaren Optionen sind *Einmal*, *Jede Woche* und *Jeden Monat*.  
| Startzeit |Datum und Uhrzeit für den Start der Updatebereitstellung **Hinweis:** Der frühestmögliche Zeitpunkt für die Durchführung einer Bereitstellung ist 30 Minuten nach dem jetzigen Zeitpunkt, wenn die Bereitstellung sofort erfolgen soll. |
| Duration |Zulässige Ausführungsdauer der Updatebereitstellung in Minuten.  Wenn innerhalb dieses Zeitraums nicht alle Updates installiert werden, muss für die verbleibenden Updates bis zur nächsten Updatebereitstellung gewartet werden. |
| Computer |Namen von Computern oder Computergruppen, die in die Updatebereitstellung einbezogen und als Ziel verwendet werden sollen.  Wählen Sie in der Dropdownliste mindestens einen Eintrag aus. |

<br><br> ![Seite „New Update Deployment“ (Neue Updatebereitstellung)](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Zeitbereich
Standardmäßig umfasst der Bereich der Daten, die mit der Lösung für die Updateverwaltung analysiert werden, alle verbundenen Verwaltungsgruppen, die innerhalb des letzten Tags generiert wurden. 

Um den Zeitraum der Daten zu ändern, wählen Sie oben im Dashboard die Option **Data based on** (Daten basierend auf). Sie können Einträge auswählen, die innerhalb der letzten sieben Tage, eines Tages oder der letzten sechs Stunden erstellt oder aktualisiert wurden. Außerdem können Sie **Benutzerdefiniert** auswählen und einen benutzerdefinierten Datumsbereich angeben.

## <a name="log-analytics-records"></a>Log Analytics-Datensätze
Mit der Lösung für die Updateverwaltung werden zwei Arten von Datensätzen im OMS-Repository erstellt.

### <a name="update-records"></a>Updatedatensätze
Ein Datensatz vom Typ **Update** wird für jedes Update erstellt, dass auf jedem Computer entweder installiert ist oder benötigt wird. Die Eigenschaften der Updatedatensätze sind in der folgenden Tabelle aufgeführt.

| Eigenschaft | Beschreibung |
| --- | --- |
| Typ |*Update* |
| SourceSystem |Die Quelle, von der die Installation des Updates genehmigt wurde.<br>Mögliche Werte:<br>- Microsoft Update<br>- Windows Update<br>- SCCM<br>- Linux-Server (von Paket-Managern) |
| Genehmigt |Gibt an, ob die Installation des Updates genehmigt wurde.<br> Für Linux-Server ist dies derzeit optional, da das Patchen nicht über OMS verwaltet wird. |
| Klassifizierung für Windows |Klassifizierung des Updates<br>Mögliche Werte:<br>-    Anwendungen<br>- Kritische Updates<br>- Definitionsupdate<br>- Feature Packs<br>- Sicherheitsupdates<br>- Service Packs<br>- Updaterollups<br>- Updates |
| Klassifizierung für Linux |Klassifizierung des Updates<br>Mögliche Werte:<br>- Kritische Updates<br>- Sicherheitsupdates<br>- Andere Updates |
| Computer |Name des Computers |
| InstallTimeAvailable |Gibt an, ob die Installationsdauer über andere Agents verfügbar ist, für die das gleiche Update installiert wurde. |
| InstallTimePredictionSeconds |Geschätzte Installationsdauer in Sekunden, basierend auf anderen Agents, für die das gleiche Update installiert wurde. |
| KBID |ID des KB-Artikels, in dem das Update beschrieben wird. |
| ManagementGroupName |Bei SCOM-Agents: Der Name der Verwaltungsgruppe.  Bei anderen Agents lautet diese „AOI-<workspace ID>“. |
| MSRCBulletinID |ID des Microsoft-Sicherheitsbulletins, in dem das Update beschrieben wird. |
| MSRCSeverity |Schweregrad des Microsoft-Sicherheitsbulletins<br>Mögliche Werte:<br>- Kritisch<br>- Wichtig<br>- Mittel |
| Optional |Gibt an, ob das Update optional ist. |
| Produkt |Name des Produkts, zu dem das Update gehört.  Klicken Sie auf die Schaltfläche zum **Anzeigen**, um den Artikel im Browser zu öffnen. |
| PackageSeverity |Schweregrad des Sicherheitsrisikos, das mit diesem Update behoben wird, wie von den Anbietern der Linux-Distribution gemeldet. |
| PublishDate |Datum und Uhrzeit der Installation des Updates |
| RebootBehavior |Gibt an, ob beim Updatevorgang ein Neustart erzwungen wird.<br>Mögliche Werte:<br>- canrequestreboot<br>- neverreboots |
| RevisionNumber |Revisionsnummer des Updates |
| SourceComputerId |GUID zur eindeutigen Identifizierung des Computers |
| TimeGenerated |Datum und Uhrzeit der letzten Aktualisierung des Datensatzes |
| Titel |Titel des Updates |
| UpdateID |GUID zur eindeutigen Identifizierung des Updates |
| UpdateState |Gibt an, ob das Update auf diesem Computer installiert ist.<br>Mögliche Werte:<br>- Installiert: Das Update ist auf diesem Computer installiert.<br>- Erforderlich: Das Update ist nicht installiert und wird auf diesem Computer benötigt. |

Wenn Sie eine Protokollsuche durchführen, bei der Datensätze mit dem Typ **Update** zurückgegeben werden, können Sie die Ansicht **Updates** wählen. Darin werden mehrere Kacheln angezeigt, auf denen die von der Suche zurückgegebenen Updates zusammengefasst sind. Sie können auf die Einträge der Kacheln **Fehlende und angewendete Updates** und **Erforderliche und optionale Updates** klicken, um die Ansicht auf die jeweiligen Updates zu begrenzen. Wählen Sie die Ansicht **Liste** oder **Tabelle**, um die einzelnen Datensätze zurückzugeben.<br> 

![Ansicht „Update“ der Protokollsuche mit Datensatztyp „Update“](./media/oms-solution-update-management/update-la-view-updates.png)  

In der Ansicht **Tabelle** können Sie für einen Datensatz jeweils auf die **KBID** klicken, um den KB-Artikel im Browser zu öffnen. So können Sie die Details des jeweiligen Updates schnell anzeigen und lesen.<br> 

![Ansicht „Tabelle“ der Protokollsuche mit Kacheln – Datensatztyp „Updates“](./media/oms-solution-update-management/update-la-view-table.png)

Klicken Sie in der Ansicht **Liste** auf den Link **Ansicht** neben der KBID, um den KB-Artikel zu öffnen.<br>

![Ansicht „Liste“ der Protokollsuche mit Kacheln – Datensatztyp „Updates“](./media/oms-solution-update-management/update-la-view-list.png)

### <a name="updatesummary-records"></a>UpdateSummary-Datensätze
Ein Datensatz vom Typ **UpdateSummary** wird für jeden Windows-Agent-Computer erstellt. Dieser Datensatz wird jedes Mal aktualisiert, wenn der Computer auf erforderliche Updates untersucht wird. Die Eigenschaften der **UpdateSummary**-Datensätze sind in der folgenden Tabelle aufgeführt.

| Eigenschaft | Beschreibung |
| --- | --- |
| Typ |UpdateSummary |
| SourceSystem |OpsManager |
| Computer |Name des Computers |
| CriticalUpdatesMissing |Anzahl von wichtigen Updates, die auf dem Computer nicht vorhanden sind |
| ManagementGroupName |Bei SCOM-Agents: Der Name der Verwaltungsgruppe. Bei anderen Agents lautet diese „AOI-<workspace ID>“. |
| NETRuntimeVersion |Version der .NET-Laufzeit, die auf dem Computer installiert ist |
| OldestMissingSecurityUpdateBucket |Bucket zum Kategorisieren des Zeitraums seit der Veröffentlichung des ältesten fehlenden Sicherheitsupdates auf diesem Computer<br>Mögliche Werte:<br>- Älter<br>-    Vor 180 Tagen<br>- Vor 150 Tagen<br>-    Vor 120 Tagen<br>- Vor 90 Tagen<br>- Vor 60 Tagen<br>-    Vor 30 Tagen<br>-    Aktuell |
| OldestMissingSecurityUpdateInDays |Anzahl von Tagen seit der Veröffentlichung des ältesten fehlenden Sicherheitsupdates auf diesem Computer |
| OsVersion |Version des Betriebssystems, das auf dem Computer installiert ist |
| OtherUpdatesMissing |Anzahl von anderen Updates, die auf dem Computer nicht vorhanden sind |
| SecurityUpdatesMissing |Anzahl von Sicherheitsupdates, die auf dem Computer nicht vorhanden sind |
| SourceComputerId |GUID zur eindeutigen Identifizierung des Computers |
| TimeGenerated |Datum und Uhrzeit der letzten Aktualisierung des Datensatzes |
| TotalUpdatesMissing |Gesamtzahl von Updates, die auf dem Computer nicht vorhanden sind |
| WindowsUpdateAgentVersion |Versionsnummer des Windows Update-Agents auf dem Computer |
| WindowsUpdateSetting |Einstellung für die Vorgehensweise des Computers beim Installieren wichtiger Updates<br>Mögliche Werte:<br>- Deaktiviert<br>- Vor der Installation benachrichtigen<br>- Geplante Installation |
| WSUSServer |URL des WSUS-Servers, wenn der Computer für dessen Nutzung konfiguriert ist |

## <a name="sample-log-searches"></a>Beispiele für Protokollsuchen
Die folgende Tabelle enthält Beispiele für Protokollsuchen für Updatedatensätze, die mit dieser Lösung erfasst wurden. 

| Abfrage | Beschreibung |
| --- | --- |
|Windows-basierte Servercomputer, für die Updates erforderlich sind |`Type:Update OSType!=Linux UpdateState=Needed Optional=false Approved!=false | measure count() by Computer` |
|Linux-Server, für die Updates erforderlich sind | `Type:Update OSType=Linux UpdateState!="Not needed" | measure count() by Computer` |
| Alle Computer, auf denen Updates fehlen |`Type=Update UpdateState=Needed Optional=false | select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate` |
| Fehlende Updates für einen bestimmten Computer (Ersetzen Sie den Wert durch den Namen Ihres eigenen Computers.) |`Type=Update UpdateState=Needed Optional=false Computer="COMPUTER01.contoso.com" | select Computer,Title,KBID,Product,UpdateSeverity,PublishedDate` |
| Alle Computer, auf denen kritische Updates oder Sicherheitsupdates fehlen |`Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates"`) |
| Für Computer erforderliche kritische Updates oder Sicherheitsupdates, die manuell angewendet wurden |`Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | Distinct Computer} | Distinct KBID` |
| Fehlerereignisse für Computer, auf denen erforderliche kritische oder Sicherheitsupdates fehlen |`Type=Event EventLevelName=error Computer IN {Type=Update (Classification="Security Updates" OR Classification="Critical Updates") UpdateState=Needed Optional=false | Distinct Computer}` |
| Alle Computer, auf denen Updaterollups fehlen |`Type=Update Optional=false Classification="Update Rollups" UpdateState=Needed| select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate` |
| Eindeutig identifizierbare fehlende Updates auf allen Computern |`Type=Update UpdateState=Needed Optional=false | Distinct Title` |
| Windows-basierter Servercomputer mit Updates, für die bei einer Updateausführung ein Fehler aufgetreten ist | `Type:UpdateRunProgress InstallationStatus=failed | measure count() by Computer, Title, UpdateRunName` |
| Linux-Server mit Updates, für die bei einer Updateausführung ein Fehler aufgetreten ist |`Type:UpdateRunProgress InstallationStatus=failed | measure count() by Computer, Product, UpdateRunName` |
| WSUS-Computermitgliedschaft |`Type=UpdateSummary | measure count() by WSUSServer` |
| Konfiguration automatischer Updates |`Type=UpdateSummary | measure count() by WindowsUpdateSetting` |
| Computer, auf denen automatische Updates deaktiviert wurden |`Type=UpdateSummary WindowsUpdateSetting=Manual` |
| Liste mit allen Linux-Computern, für die ein Paketupdate verfügbar ist |`Type=Update and OSType=Linux and UpdateState!="Not needed" | measure count() by Computer` |
| Liste mit allen Linux-Computern, für die ein Paketupdate zur Behebung von kritischen oder sicherheitsrelevanten Sicherheitsrisiken verfügbar ist |`Type=Update and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") | measure count() by Computer` |
| Liste mit allen Paketen, für die ein Update verfügbar ist |Type=Update and OSType=Linux and UpdateState!="Not needed" |
| Liste mit allen Paketen, für die ein Update zur Behebung von kritischen oder sicherheitsrelevanten Sicherheitsrisiken verfügbar ist |`Type=Update  and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates")` |
| Liste mit den Updatebereitstellungen, bei denen Computer geändert wurden |`Type:UpdateRunProgress | measure Count() by UpdateRunName` |
|Computer, die bei dieser Updateausführung aktualisiert wurden (Ersetzen Sie den Wert durch den Namen Ihrer Updatebereitstellung.) |`Type:UpdateRunProgress UpdateRunName="DeploymentName" | measure Count() by Computer` |
| Liste mit allen „Ubuntu“-Computern mit verfügbaren Updates |`Type=Update and OSType=Linux and OSName = Ubuntu &| measure count() by Computer` |

## <a name="troubleshooting"></a>Problembehandlung 

Dieser Abschnitt enthält Informationen zum Durchführen der Problembehandlung mit der Lösung für die Updateverwaltung.  

### <a name="how-do-i-troubleshoot-update-deployments"></a>Wie kann ich die Problembehandlung für Updatebereitstellungen durchführen?
Sie können die Ergebnisse des Runbooks, das für die Bereitstellung von Updates der geplanten Updatebereitstellung zuständig ist, über das Blatt „Aufträge“ Ihres Automation-Kontos anzeigen, das mit dem OMS-Arbeitsbereich für diese Lösung verknüpft ist.  Das Runbook **Patch-MicrosoftOMSComputer** ist ein untergeordnetes Runbook, das auf einen bestimmten verwalteten Computer ausgerichtet ist. Beim Überprüfen des ausführlichen Datenstroms werden detaillierte Informationen zur Bereitstellung angezeigt.  In der Ausgabe wird angegeben, welche erforderlichen Updates zutreffen, und der Downloadstatus, der Installationsstatus und weitere Details werden aufgeführt.<br><br> ![Auftragsstatus der Updatebereitstellung](media/oms-solution-update-management/update-la-patchrunbook-outputstream.png)<br>

Weitere Informationen finden Sie unter [Runbookausgabe und -meldungen in Azure Automation](../automation/automation-runbook-output-and-messages.md).   
  
## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie die Protokollsuche in [Log Analytics](../log-analytics/log-analytics-log-searches.md), um ausführliche Daten zu Updates anzuzeigen.
* [Erstellen Sie eigene Dashboards](../log-analytics/log-analytics-dashboards.md), um die Einhaltung der erforderlichen Updates für Ihre verwalteten Computer anzuzeigen.
* [Erstellen Sie Warnungen](../log-analytics/log-analytics-alerts.md), wenn kritische Updates für Computer als fehlend erkannt werden oder für einen Computer die automatischen Updates deaktiviert sind.  


