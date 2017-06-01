---
title: "Dashboard, Überwachen, Skalieren, Konfigurieren und Hybridverbindungen in BizTalk Services | Microsoft Docs"
description: "Erfahren Sie mehr über die Steuerelemente und Leistungsüberwachung auf den Registerkarten des klassischen Portals für BizTalk Services: „Dashboard“, „Überwachen“, „Skalieren“, „Konfigurieren“ und „Hybridverbindungen“. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7a1815db-0de2-4274-8be0-198c1b077324
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: ec1c54c8520170cd19095bf8d752c79c2fd812d8
ms.contentlocale: de-de
ms.lasthandoff: 03/15/2017


---
# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Registerkarten "Dashboard", "Überwachen", "Skalieren", "Konfigurieren" und "Hybridverbindungen"

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Nachdem Sie Ihren BizTalk Service erstellt und Ihre Anwendung bereitgestellt haben, können Sie einige der BizTalk Service-Einstellungen ändern und die Anwendungsleistung überwachen. 

Wenn Sie das klassische Azure-Portal zum ersten Mal öffnen, ist automatisch die Registerkarte **ALLE ELEMENTE** geöffnet. Wenn Sie Ihren BizTalk-Dienst anzeigen möchten, wählen Sie diesen auf der Registerkarte **ALLE ELEMENTE** aus. Alternativ können Sie auf der Registerkarte **BIZTALK SERVICES** den Namen des BizTalk-Diensts auswählen.

Dadurch wird ein neues Fenster mit den folgenden Registerkarten geöffnet. In diesem Thema werden diese Registerkarten beschrieben.

## <a name="quickstart-quickstartquickstart"></a>Schnellstart (![Schnellstart][Quickstart])
Eventuell stehen nicht alle aufgeführten Optionen für alle BizTalk Services-Editionen zur Verfügung. 

<table border="1">
    <tr>
        <td><strong>Abrufen der Tools</strong></td>
        <td>Laden Sie das BizTalk Services SDK herunter, um die Visual Studio-Projektvorlagen auf Ihrem lokalen Entwicklungscomputer zu speichern. Mithilfe dieser Vorlagen werden die Visual Studio-Projekte <strong>BizTalk Services</strong> (Brücke) und <strong>BizTalk Service Artifacts</strong> (Transformation) erstellt, die in Ihrem BizTalk-Dienst bereitgestellt werden.
        <br/><br/>Unter 
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Erstellen des Projekts in Visual Studio</a> und <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">Installieren des Azure BizTalk Services-SDKs</a> werden die ersten Schritte erläutert.
        </td>
    </tr>
    <tr>
        <td><strong>Partnervereinbarungen erstellen</strong></td>
        <td>Öffnet das auf Azure gehostete Azure BizTalk Services-Portal, auf dem Sie Partner hinzufügen und X12-, AS2- und EDIFACT-EDI-Verträge erstellen können.
        <br/><br/>Unter 
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurieren von EDI, AS2 und EDIFACT im BizTalk Services-Portal</a> werden die ersten Schritte erläutert.
        </td>
    </tr>

<tr>
        <td><strong>Weitere Informationen über BizTalk Services</strong></td>
        <td>Im <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">Trainingscenter</a> erfahren Sie mehr über Azure BizTalk Services.</td>
</tr>
</table>


In der Taskleiste am unteren Rand stehen folgende Optionen zur Verfügung:

<table border="1">

<tr>
<td><strong>Verwalten</strong> Ihrer Anwendungsbereitstellung</td>
<td>Hiermit öffnen Sie das Azure BizTalk Services-Portal. Das BizTalk Services-Portal ist das Tor zur EDI-Konfiguration. Hier können Sie Partner hinzufügen und X12-, AS2- sowie EDIFACT-Vereinbarungen erstellen.
<br/><br/>
Dies ist funktionsgleich mit <strong>Partnervereinbarungen erstellen</strong> auf der Registerkarte <strong>Schnellstart</strong>.
<br/><br/>Unter 
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurieren von EDI, AS2 und EDIFACT im BizTalk Services-Portal</a> erhalten Sie weitere Informationen zum BizTalk Services-Portal.</td>
</tr>

<tr>
<td><strong>Verbindungsinformationen</strong> des Access Control-Namespace</td>
<td>Wenn Sie "Verbindungsinformationen" auswählen, werden der Access Control-Namespace, der Standardaussteller und der Standardschlüssel angezeigt. Diese Werte können kopiert werden.
<br/><br/>
Sie können auch das Access Control-Portal öffnen. Unter <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Erstellen eines Access Control-Namespace</a> finden Sie weitere Informationen zum Access Control-Portal.</td>
</tr>

<tr>
<td><strong>Synchronisierungsschlüssel</strong> im Speicherkonto</td>
<td>Beim Erstellen eines Speicherkontos werden automatisch ein Primärschlüssel und ein Sekundärschlüssel erstellt. Diese Verschlüsselungsschlüssel steuern den Zugriff auf das Speicherkonto. Der BizTalk Service verwendet automatisch den Primärschlüssel. Mit <strong>Synchronisierungsschlüssel</strong> können Benutzer zwischen dem primären und dem sekundären Schlüssel umschalten, ohne den BizTalk-Dienst zu unterbrechen.
<br/><br/>
Angenommen, Sie möchten, dass der BizTalk Service einen neuen Primärschlüssel für das Speicherkonto verwendet. Gehen Sie dazu folgendermaßen vor:
<br/><br/>
<ol>
<li>Wählen Sie Ihren BizTalk-Dienst und die Option <strong>Synchronisierungsschlüssel</strong> aus. Wählen Sie den Sekundärschlüssel aus. Wenn Sie dies tun, startet der BizTalk Service die Verwendung des Sekundärschlüssels.</li>
<li>Wählen Sie im klassischen Azure-Portal Ihr Speicherkonto aus und generieren Sie den Primärschlüssel neu. Denken Sie daran, dass Ihr BizTalk Service den Sekundärschlüssel verwendet.</li>
<li>Wählen Sie Ihren BizTalk-Dienst und die Option <strong>Synchronisierungsschlüssel</strong> aus. Wählen Sie nun den Primärschlüssel. Dies ist der neue Primärschlüssel, den Sie generiert haben.</li>
<li>Wählen Sie im klassischen Azure-Portal Ihr Speicherkonto aus und generieren Sie den Sekundärschlüssel neu.</li>
</ol>
<br/>
Man spricht hier von "Rollover-Schlüsseln". Der Zweck ist, zwischen dem Primär- und dem Sekundärschlüssel umzuschalten, ohne den BizTalk Service zu unterbrechen.</td>
</tr>

<tr>
<td><strong>Löschen</strong> Ihrer Anwendung</td>
<td>Wenn Sie "Löschen" auswählen, werden Ihr BizTalk Service und alle darin bereitgestellten Elemente entfernt.</td>
</tr>
</table>


## <a name="dashboard"></a>Dashboard
Eventuell stehen nicht alle aufgeführten Optionen für alle BizTalk Services-Editionen zur Verfügung. 

Wenn Sie den Namen Ihres BizTalk Service auswählen, wird die Registerkarte "Dashboard" angezeigt. Im Dashboard können Sie folgende Aktionen ausführen:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Verwendungsübersicht: Zeigt die Anzahl der verwendeten Hybridverbindungen an.
Gibt außerdem Aufschluss über die Datennutzung in GB. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Metrikendiagramm: Zeigt eine nicht bearbeitbare Liste von Leistungsmetriken an.
Diese Metriken liefern Echtzeitwerte zum Zustand des BizTalk Service. Sie können auch die **relativen** oder **absoluten** Werte sowie ein **Zeitintervall** für die im Diagramm angezeigten Metriken auswählen. 

Eine Beschreibung dieser Leistungsmetriken finden Sie in diesem Thema unter [Verfügbare Metriken](#Metrics) .

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Auf einen Blick: Führt die Eigenschaften Ihres BizTalk Service auf.
<table border="1">

<tr>
<td><strong>Anmeldeinformationen für die Nachverfolgungsdatenbank aktualisieren</strong></td>
<td>Hiermit können Sie den Benutzernamen und das Kennwort zum Anmelden bei der Nachverfolgungsdatenbank ändern.</td>
</tr>
<tr>
<td><strong>SSL-Zertifikat aktualisieren</strong></td>
<td>Hiermit können Sie Ihren BizTalk Service aktualisieren, sodass ein anderes SSL-Zertifikat verwendet wird. Wenn Sie den <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">BizTalk-Dienst erstellen</a>, wird automatisch ein selbstsigniertes SSL-Zertifikat erstellt.</td>
</tr>
<tr>
<td><strong>Zertifikat herunterladen</strong></td>
<td>Sie können das von Ihrem BizTalk Service verwendete SSL-Zertifikat auf einen lokalen Rechner herunterladen.</td>
</tr>
<tr>
<td><strong>Status</strong></td>
<td>Zeigt den aktuellen Status Ihres BizTalk Service an. Siehe <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Dienststatusübersicht</a>. </td>
</tr>
<tr>
<td><strong>Dienst-URL</strong></td>
<td>Die URL für Ihren BizTalk Service. Dies ist dieselbe URL wie die <strong>Domänen-URL</strong>, die Sie bei der Erstellung Ihres BizTalk-Diensts eingegeben haben.</td>
</tr>
<tr>
<td><strong>Öffentliche virtuelle IP-Adresse (VIP)</strong></td>
<td>Die IP-Adresse, die Ihrem BizTalk Service zugewiesen wurde. Sie wird für alle Eingabeendpunkte verwendet und ist die Quelladresse für ausgehenden Datenverkehr. Diese IP-Adresse gehört so lange zu Ihrem BizTalk Service, wie dieser besteht. Wenn Sie den BizTalk Service löschen, wird die IP-Adresse einem anderen BizTalk Service zugewiesen.</td>
</tr>
<tr>
<td><strong>ACS-Namespace</strong></td>
<td>Zur Authentifizierung im BizTalk Service.</td>
</tr>
<tr>
<td><strong>Edition</strong></td>
<td>Führt die Edition auf, die bei der Erstellung des BizTalk Service eingegeben wurde.</td>
</tr>
<tr>
<td><strong>Standort</strong></td>
<td>Zeigt die geografische Region an, in der Ihr BizTalk Service gehostet wird.</td>
</tr>
<tr>
<td><strong>Erstellt</strong></td>
<td>Zeigt das Datum und den Zeitpunkt der Erstellung Ihres BizTalk Service an.</td>
</tr>
<tr>
<td><strong>Nachverfolgungsdatenbank</strong></td>
<td>Der Name der Azure SQL-Datenbank, in der die von Ihrem BizTalk Service verwendeten Nachverfolgungstabellen gespeichert sind. 
<br/><br/>Unter 
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Erläuterung der Anforderungen</a> finden Sie Einzelheiten zur Nachverfolgungsdatenbank.</td>
</tr>
<tr>
<td><strong>Überwachen/Archivieren des Speicherkontos</strong></td>
<td>Der Name des Azure-Speicherkontos, in dem die Überwachungsergebnisse Ihres BizTalk Service gespeichert sind.
<br/><br/>Unter 
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Erläuterung der Anforderungen</a> finden Sie Einzelheiten zum Speicherkonto.</td>
</tr>
<tr>
<td><strong>Abonnementname</strong></td>
<td>Führt das Abonnement auf, in dem Ihr BizTalk Service gehostet wird. Das Abonnement regelt den Zugriff auf das klassische Azure-Portal.</td>
</tr>
<tr>
<td><strong>Abonnement-ID</strong></td>
<td>Wenn ein Abonnement erstellt wird, wird automatisch eine Abonnement-ID generiert. Wenn Sie REST-APIs verwenden, müssen Sie eventuell die Abonnement-ID eingeben.</td>
</tr>
</table>

[Konfigurieren von BizTalk Services im klassischen Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) enthält die Schritte zur Erstellung eines BizTalk Service.

##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>"Verwalten", "Verbindungsinformationen", "Synchronisierungsschlüssel" und "Löschen" in der Taskleiste:
<table border="1">

<tr>
<td><strong>Verwalten</strong> Ihrer Anwendungsbereitstellung</td>
<td>Hiermit öffnen Sie das Azure BizTalk Services-Portal. Das BizTalk Services-Portal ist das Tor zur EDI-Konfiguration. Hier können Sie Partner hinzufügen und X12-, AS2- sowie EDIFACT-Vereinbarungen erstellen.
<br/><br/>
Dies ist funktionsgleich mit <strong>Partnervereinbarungen erstellen</strong> auf der Registerkarte <strong>Schnellstart</strong>.
<br/><br/>Unter 
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurieren von EDI, AS2 und EDIFACT im BizTalk Services-Portal</a> erhalten Sie weitere Informationen zum BizTalk Services-Portal.</td>
</tr>
<tr>
<td><strong>Verbindungsinformationen</strong> des Access Control-Namespace</td>
<td>Hier finden Sie die Werte für den Access Control-Namespace, den Standardaussteller und den Standardschlüssel. Diese lassen sich kopieren.
<br/><br/>
Sie können auch das Access Control-Portal öffnen. Bei diesem Access Control-Portal handelt es sich um dasselbe Tool wie die Option „Active Directory“ im linken Navigationsbereich.
<br/><br/>Unter 
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Verwalten Ihres ACS-Namespace</a> finden Sie weitere Informationen zum Access Control-Portal.</td>
</tr>
<tr>
<td><strong>Synchronisierungsschlüssel</strong> im Speicherkonto</td>
<td>Beim Erstellen eines Speicherkontos werden automatisch ein Primärschlüssel und ein Sekundärschlüssel erstellt. Diese Verschlüsselungsschlüssel steuern den Zugriff auf das Speicherkonto. Der BizTalk Service verwendet automatisch den Primärschlüssel. Mit <strong>Synchronisierungsschlüssel</strong> können Benutzer zwischen dem primären und dem sekundären Schlüssel umschalten, ohne den BizTalk-Dienst zu unterbrechen.
<br/><br/>
Angenommen, Sie möchten, dass der BizTalk Service einen neuen Primärschlüssel für das Speicherkonto verwendet. Gehen Sie dazu folgendermaßen vor:
<br/><br/>
<ol>
<li>Wählen Sie Ihren BizTalk-Dienst und die Option <strong>Synchronisierungsschlüssel</strong> aus. Wählen Sie den Sekundärschlüssel aus. Wenn Sie dies tun, startet der BizTalk Service die Verwendung des Sekundärschlüssels.</li>
<li>Wählen Sie im klassischen Azure-Portal Ihr Speicherkonto aus und generieren Sie den Primärschlüssel neu. Denken Sie daran, dass Ihr BizTalk Service den Sekundärschlüssel verwendet.</li>
<li>Wählen Sie Ihren BizTalk-Dienst und die Option <strong>Synchronisierungsschlüssel</strong> aus. Wählen Sie nun den Primärschlüssel. Dies ist der neue Primärschlüssel, den Sie generiert haben.</li>
<li>Wählen Sie im klassischen Azure-Portal Ihr Speicherkonto aus und generieren Sie den Sekundärschlüssel neu.</li>
</ol>
<br/>
Man spricht hier von "Rollover-Schlüsseln". Der Zweck ist, zwischen dem Primär- und dem Sekundärschlüssel umzuschalten, ohne den BizTalk Service zu unterbrechen.</td>
</tr>

<tr>
<td><strong>Löschen</strong> Ihrer Anwendung</td>
<td>Ihr BizTalk Service und alle darin bereitgestellten Elemente werden entfernt.</td>
</tr>
</table>


## <a name="monitor"></a>Monitor
Gilt nicht für die Free Edition.

Wenn Sie den Namen Ihres BizTalk Service auswählen, steht die Registerkarte "Überwachen" mit folgendem Inhalt zur Verfügung:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Metrikendiagramm: Zeigt die ausgewählten Leistungsmetriken an.
Diese Metriken liefern Echtzeitwerte zum Zustand des BizTalk Service. Sie wählen aus, welche Leistungsmetriken angezeigt werden. Bis zu sechs Leistungsmetriken können gleichzeitig angezeigt werden. 

Sie können auch die **relativen** oder **absoluten** Werte sowie ein **Zeitintervall** für die angezeigten Metriken auswählen. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>So zeigen Sie Metriken im Diagramm an oder entfernen diese daraus:
1. Wählen Sie die Registerkarte **Überwachen** aus.
2. Klicken Sie auf der Taskleiste auf **Metriken hinzufügen**:  
   !["Metriken hinzufügen" auswählen][AddMetrics]
3. Prüfen Sie die Leistungsmetriken, die Sie anzeigen möchten.
4. Wählen Sie das Häkchen aus, um zur Registerkarte **Überwachen** zurückzukehren.
5. Wählen Sie den Kreis neben der Metrik aus, um den Wert dieser Metrik im Diagramm anzuzeigen.  
   
    Die Metrik **CPU-Auslastung** ist ausgegraut, d.h., ihr Wert wird im Diagramm nicht angezeigt:  
   ![Metrik "CPU-.Auslastung" ist deaktiviert][GrayedMetric]  
   
    Wählen Sie den ausgegrauten Kreis, sodass die Metrik **CPU-Auslastung** ihre Ausgabe im Diagramm anzeigt:  
   ![Metrik "CPU-.Auslastung" ist aktiviert][EnabledMetric]
6. Wenn Sie eine Metrik aus dem Diagramm und der Liste entfernen möchten, wählen Sie in der Taskleiste **Metrik löschen** aus. Um die Metrik der Liste wieder hinzuzufügen, klicken Sie in der Taskleiste auf **Metriken hinzufügen**, markieren die Metrik und wählen das Häkchen aus, um zur Registerkarte **Überwachen** zurückzukehren. Wählen Sie den grau unterlegten Kreis aus, um die Metrik zu aktivieren.

## <a name="Metrics"></a>Verfügbare Metriken
Die folgenden Leistungsindikatoren/Metriken stehen zur Verfügung:

<table border="1">

<tr>
<td><strong>Roundtriplatenz</strong></td>
<td>Diese Leistungsmetrik zeigt die durchschnittliche Zeit in Millisekunden (ms) vom Eingang einer Nachricht bis zu ihrer vollständigen Verarbeitung durch den BizTalk Service über alle Brücken hinweg an. Es werden nur erfolgreich verarbeitete Nachrichten gezählt.<br/><br/>
Bei folgenden Ereignissen wird ein Zeitstempel erstellt:
<ul>
<li>Eintritt der Nachricht in das Gateway</li>
<li>Weiterleitung der Nachricht an das Ziel</li>
<li>Übermittlung der Zielbestätigung</li>
<li>Senden der Zielbestätigung an das Gateway</li>
</ul>
<br/>
Diese Metrik zeigt das Ergebnis der folgenden Berechnung:
<br/><br/>
[Senden der Zielbestätigung an das Gateway] - [Eintritt der Nachricht in das Gateway]</td>
</tr>
<tr>
<td><strong>Fehler an Quelle</strong></td>
<td>Zeigt die Gesamtzahl der Nachrichten des BizTalk Service an, die beim Abruf von den Quellendpunkten fehlgeschlagen sind.</td>
</tr>
<tr>
<td><strong>CPU-Auslastung</strong></td>
<td>Listet die durchschnittliche Prozessorzeit aller Rolleninstanzen in % auf.</td>
</tr>
<tr>
<td><strong>Verarbeitungslatenz</strong></td>
<td>Zeigt die durchschnittliche Zeit in Millisekunden (ms) für die Verarbeitung einer Nachricht durch den BizTalk Service über alle Brücken hinweg an, ausgenommen die an den Zielen verbrachte Zeit. Es werden nur erfolgreich verarbeitete Nachrichten gezählt.<br/><br/>
Bei folgenden Ereignissen wird ein Zeitstempel erstellt:

<ul>
<li>Eintritt der Nachricht in das Gateway</li>
<li>Weiterleitung der Nachricht an das Ziel</li>
<li>Übermittlung der Zielbestätigung</li>
<li>Senden der Zielbestätigung an das Gateway</li>
</ul>
<br/>Diese Metrik zeigt das Ergebnis der folgenden Berechnung:<br/><br/>
[Senden der Zielbestätigung an das Gateway] - [Eintritt der Nachricht in das Gateway] - [Empfang der Zielbestätigung] + [Weiterleitung der Nachricht an das Ziel]</td>
</tr>
<tr>
<td><strong>Fehler in Vorgang</strong></td>
<td>Zeigt die Gesamtanzahl von Nachrichten an, bei denen innerhalb eines bestimmten Zeitintervalls während der Verarbeitung durch den BizTalk Service über alle Brücken hinweg ein Fehler aufgetreten ist.</td>
</tr>
<tr>
<td><strong>Gesendete Nachrichten</strong></td>
<td>Zeigt die Gesamtzahl der Nachrichten an, die innerhalb eines bestimmten Zeitintervalls vom BizTalk Service zwischen allen Brücken gesendet worden sind. Diese Metrik wird erhöht, wenn eine aus einer Pipeline gesendete Nachricht das Routenziel erreicht. Diese Metrik zeigt nicht die erfolgreiche Verarbeitung einer Nachricht an.<br/><br/>
In einem Abfrage-Antwort-Szenario wird die Metrik erhöht, wenn das Routenziel eine Empfangsbestätigung an die Pipeline zurücksendet.</td>
</tr>
<tr>
<td><strong>Empfangene Nachrichten</strong></td>
<td>Zeigt die Gesamtanzahl von Nachrichten an, die innerhalb eines bestimmten Zeitintervalls vom BizTalk Service über alle allen Brücken hinweg empfangen wurden. Diese Metrik wird erhöht, wenn eine Nachricht aus der Pipeline empfangen wird.</td>
</tr>
<tr>
<td><strong>Nachrichten in Bearbeitung</strong></td>
<td>Zeigt die Gesamtzahl der Nachrichten an, die aktuell innerhalb eines bestimmten Zeitintervalls vom BizTalk Service verarbeitet werden.</td>
</tr>
<tr>
<td><strong>Verarbeitete Nachrichten</strong></td>
<td>Zeigt die Gesamtanzahl von Nachrichten an, die innerhalb eines bestimmten Zeitintervalls vom BizTalk Service über alle Brücken hinweg erfolgreich verarbeitet wurden. Diese Metrik wird erhöht, wenn eine Nachricht erfolgreich von der Pipeline empfangen und erfolgreich an das Ziel weitergeleitet wird.</td>
</tr>
</table>


## <a name="scale"></a>Skalieren
Auf der Registerkarte "Skalieren" können Sie die Anzahl der von Ihrem BizTalk Service verwendeten Einheiten addieren oder subtrahieren. Standardmäßig ist eine Einheit konfiguriert. Zur Skalierung Ihres BizTalk Service können zusätzliche Einheiten hinzugefügt werden. Wenn Sie die Skalierung erhöhen, wird auch der Durchsatz erhöht. Auch die Menge der Ressourcen, also der bereitgestellten Brücken, Verträge und lokalen Branchenverbindungen, sowie die Verarbeitungsleistung werden erhöht. Beispiel: Sie erhöhen die Skalierung von 1 auf 2 Einheiten. In dieser Lage können Sie nun die doppelte Anzahl von Brücken bereitstellen sowie die Verträge, die lokalen Branchenverbindungen und die Verarbeitungsleistung verdoppeln.

Einige BizTalk-Editionen bieten keine Skalierungsoption. In diesen Fällen ist nur eine Einheit erlaubt. Informationen darüber, auf wie viele Einheiten Ihre Edition skaliert werden kann, finden Sie unter [BizTalk Services: Editionsübersicht](biztalk-editions-feature-chart.md).

Eine Erhöhung der Anzahl der Einheiten kann sich auf den Preis auswirken. Wenn Sie die Anzahl der Einheiten erhöhen, erhalten Sie nach Auswahl von **Speichern** die Meldung, dass dies Einfluss auf Ihre Rechnung hat. Sie können dann wählen, ob Sie fortfahren möchten. Wenn Sie die Anzahl der Einheiten erhöhen, ändert sich der Status des BizTalk Service von "Active" auf "Updating". Im Status "Updating" läuft Ihr BizTalk Service weiter.

[BizTalk Services: Editionsübersicht](biztalk-editions-feature-chart.md) definiert eine "Einheit".

## <a name="configure"></a>Konfigurieren
Gilt nicht für Hybridverbindungen.

Stellt den Sicherungsstatus auf "Keine" oder "Automatisch" ein. Bei Einstellung auf "Keine" werden keine Sicherungen automatisch erstellt. Bei Einstellung auf "Automatisch" konfigurieren Sie den Speicherort für die Sicherung, die Häufigkeit der Sicherung und die Aufbewahrungszeit für die Sicherungsdateien. 

[BizTalk Services: Sichern und Wiederherstellen](biztalk-backup-restore.md) enthält Einzelheiten dazu. 

## <a name="HybridConnections"></a>Hybridverbindungen
Über Hybridverbindungen werden Azure-Anwendungen wie Web-Apps oder Mobile Apps in Azure App Service mit einer lokalen Ressource (wie SQL Server, MySQL, HTTP Web-APIs und die meisten benutzerdefinierten Webdienste) verbunden, die einen statischen TCP-Port verwendet. Hybridverbindungen werden in BizTalk Services über das klassische Azure-Portal verwaltet.

Informationen zum Erstellen von Hybridverbindungen in Azure App Service finden Sie unter [Zugreifen auf lokale Ressourcen über Hybridverbindungen in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).

Informationen zum Erstellen oder Verwalten von Hybridverbindungen in Azure BizTalk Services finden Sie unter [Hybridverbindungen](integration-hybrid-connection-overview.md).

## <a name="next"></a>Weiter
Nun, da Sie mit den verschiedenen Registerkarten vertraut sind, können Sie mehr über die Funktionen von Azure BizTalk Services erfahren:

* [BizTalk Services: Drosselung](biztalk-throttling-thresholds.md)  
* [BizTalk Services: Name und Schlüssel des Ausstellers](biztalk-issuer-name-issuer-key.md)  
* [BizTalk Services: Sichern und Wiederherstellen](biztalk-backup-restore.md)

## <a name="see-also"></a>Weitere Informationen
* [Hybridverbindungen](integration-hybrid-connection-overview.md)  
* [BizTalk Services: Editionsübersicht](biztalk-editions-feature-chart.md)  
* [BizTalk Services: Bereitstellen mithilfe des klassischen Azure-Portals](biztalk-provision-services.md)  
* [BizTalk Services: BizTalk-Dienststatusübersicht](biztalk-service-state-chart.md)  
* [Wie verwende ich das Azure BizTalk Services SDK?](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png


