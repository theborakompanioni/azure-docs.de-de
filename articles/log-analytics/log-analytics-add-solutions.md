---
title: Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog | Microsoft Docs
description: Log Analytics-Lösungen sind eine Sammlung von Logik-, Visualisierungs- und Datenerfassungsregeln, die Metriken rund um einen bestimmten Problembereich bereitstellen.
services: log-analytics
documentationcenter: ''
author: bandersmsft
manager: jwhit
editor: ''

ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: banders

---
# <a name="add-log-analytics-solutions-from-the-solutions-gallery"></a>Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog
Log Analytics-Lösungen sind eine Sammlung von **Logik-**, **Visualisierungs-** und **Datenerfassungsregeln**, die Metriken rund um einen bestimmten Problembereich bereitstellen. Dieser Artikel beschreibt von Log Analytics unterstützte Lösungen und erläutert, wie Sie diese über den Lösungskatalog hinzufügen und entfernen.

Die detaillierten Einblicke der Lösungen ermöglichen Folgendes:

* Schnelleres Untersuchen und Lösen von Betriebsproblemen
* Sammeln und Korrelieren von verschiedenen Arten von Computerdaten
* Proaktive Umsetzung von Maßnahmen wie Kapazitätsplanung, Patchstatusberichte und Sicherheitsüberwachung.

> [!NOTE]
> Log Analytics umfasst Funktionen für die Protokollsuche, sodass Sie keine separate Lösung dafür installieren müssen. Sie können aber Datenvisualisierungen, Vorschläge für Suchen und Einblicke nutzen, indem Sie Lösungen aus dem Lösungskatalog hinzufügen.
> 
> 

Nachdem Sie eine Lösung hinzugefügt haben, werden Daten von den Servern in Ihrer Infrastruktur erfasst und an den OMS-Dienst gesendet. Die Verarbeitung durch den OMS-Dienst dauert normalerweise von einigen Minuten bis zu einer Stunde. Nach der Verarbeitung der Daten durch den Dienst können Sie diese in OMS anzeigen.

Wenn eine Lösung nicht mehr benötigt wird, können Sie sie problemlos entfernen. Wenn Sie eine Lösung entfernen, werden ihre Daten nicht mehr an OMS gesendet, und die Datenmenge, die Ihr Tageskontingent belastet (sofern vorhanden), verringert sich entsprechend.

## <a name="solutions-supported-by-the-microsoft-monitoring-agent"></a>Von Microsoft Monitoring Agent unterstützte Lösungen
Zurzeit können Server, die über den Microsoft Monitoring Agent direkt mit OMS verbunden sind, die meisten verfügbaren Lösungen nutzen, einschließlich der folgenden:

* Active Directory Assessment
* Alert Management (ohne SCOM-Warnungen)
* Antimalware
* Change Tracking
* Sicherheit
* SQL Assessment
* System Updates

Die folgenden Lösungen werden hingegen *nicht* mit dem Microsoft Monitoring Agent unterstützt, sondern erfordern einen SCOM-Agent (System Center Operations Manager).

* Alert Management (mit SCOM-Warnungen)
* Capacity Management
* Konfigurationsbewertung

Informationen zum Verbinden des SCOM-Agents mit Log Analytics finden Sie unter [Connecting Operations Manager to Log Analytics](log-analytics-om-agents.md) (Verbinden von Operations Manager mit Log Analytics).

### <a name="to-add-a-solution-using-the-solutions-gallery"></a>So fügen Sie eine Lösung mit Solutions Gallery hinzu
1. Klicken Sie in OMS auf der Übersichtsseite auf die Kachel **Lösungskatalog** .    
    ![Lösungskatalog](./media/log-analytics-add-solutions/sol-gallery.png)
2. Auf der Seite des OMS-Lösungskatalogs finden Sie Informationen zu jeder der verfügbaren Lösungen. Klicken Sie auf den Namen der Lösung, die Sie OMS hinzufügen möchten.
3. Auf der Seite für die ausgewählte Lösung werden ausführliche Informationen zur Lösung angezeigt. Klicken Sie auf **Hinzufügen**.
4. Auf der Übersichtsseite in OMS wird eine neue Kachel für die hinzugefügte Lösung angezeigt, und Sie können die Lösung verwenden, nachdem der OMS-Dienst Ihre Daten verarbeitet hat.

## <a name="to-configure-solutions"></a>Konfigurieren von Lösungen
1. Sie müssen einige Lösungen konfigurieren. Beispielsweise müssen Sie Automation, Azure Site Recovery und Backup konfigurieren, bevor Sie die Lösungen verwenden können.
2. Klicken Sie auf der Übersichtsseite auf die Kachel für eine dieser Lösungen.  
    ![Lösung konfigurieren](./media/log-analytics-add-solutions/configure-additional.png)
3. Konfigurieren Sie die Lösung mit den notwendigen Informationen, und klicken Sie dann auf **Speichern**.  
    ![Konfigurieren der Lösung](./media/log-analytics-add-solutions/configure.png)

### <a name="to-remove-a-solution-using-the-solutions-gallery"></a>So entfernen Sie eine Lösung mit Solutions Gallery
1. Klicken Sie in OMS auf der Übersichtsseite auf die Kachel **Einstellungen** .
2. Klicken Sie auf der Einstellungsseite auf der Registerkarte mit den Lösungen für die Lösung, die Sie entfernen möchten, auf **Entfernen** .
3. Klicken Sie im Bestätigungsdialogfeld auf **Ja** , um die Lösung zu entfernen.

## <a name="data-collection-details-for-oms-features-and-solutions"></a>Details zur Datensammlung für OMS-Features und -Lösungen
Die folgende Tabelle zeigt die Datensammlungsmethoden und andere Details dazu, wie Daten für OMS-Features und -Lösungen gesammelt werden. Direkt-Agents und SCOM-Agents sind nahezu identisch, Direkt-Agents stellen jedoch zusätzliche Funktionen bereit, die eine Verbindung mit dem OMS-Arbeitsbereich und die Weiterleitung über einen Proxy erlauben. Wenn Sie einen SCOM-Agent verwenden, muss er als Ziel wie ein OMS-Agent konfiguriert werden, damit eine Kommunikation mit OMS möglich ist. Die SCOM-Agents in dieser Tabelle sind mit SCOM verbunden. Weitere Informationen zum Verbinden Ihrer vorhandenen SCOM-Umgebung mit OMS finden Sie unter [Verbinden von Operations Manager mit Log Analytics](log-analytics-om-agents.md).

> [!NOTE]
> Der Typ des verwendeten Agents bestimmt, wie Daten an OMS gesendet werden. Dabei gelten die folgenden Bedingungen:
> 
> 

* Sie verwenden den Direkt-Agent oder einen mit SCOM verbundenen OMS-Agent.
* Wenn SCOM erforderlich ist, werden SCOM-Agent-Daten für die Lösung immer mithilfe der SCOM-Verwaltungsgruppe an OMS gesendet. Wenn SCOM erforderlich ist, verwendet die Lösung darüber hinaus den SCOM-Agent.
* Wenn SCOM nicht erforderlich ist und die Tabelle zeigt, dass Daten vom SCOM-Agent mithilfe der Verwaltungsgruppe an OMS gesendet werden, werden SCOM-Agent-Daten immer mithilfe von Verwaltungsgruppen an OMS gesendet. Direkt-Agents umgehen die Verwaltungsgruppe und senden ihre Daten direkt an OMS.
* Wenn die SCOM-Agent-Daten nicht mit einer Verwaltungsgruppe gesendet werden, erfolgt ein direkter Versand an OMS – unter Umgehung der Verwaltungsgruppe.

| Datentyp | Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AD Assessment |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |7 Tage |
| AD Replication Status |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |5 Tage |
| Warnungen (Nagios) |Linux |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Bei Ankunft |
| Warnungen (Zabbix) |Linux |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |1 Minute |
| Warnungen (Operations Manager) |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |3 Minuten |
| Antimalware |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |Stündlich |
| Capacity Management |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |Stündlich |
| Change Tracking |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |Stündlich |
| Change Tracking |Linux |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Stündlich |
| Configuration Assessment (ehemals Advisor) |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |Zweimal pro Tag |
| ETW |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |5 Minuten |
| IIS-Protokolle |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |5 Minuten |
| Schlüsseltresore |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |10 Minuten |
| Netzwerk-Anwendungsgateways |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |10 Minuten |
| Netzwerksicherheitsgruppen |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |10 Minuten |
| Office 365 |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Bei Benachrichtigung |
| Leistungsindikatoren |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Gemäß Zeitplan, mindestens 10 Sekunden |
| Leistungsindikatoren |Linux |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Gemäß Zeitplan, mindestens 10 Sekunden |
| Service Fabric |Windows |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |5 Minuten |
| SQL Assessment |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |7 Tage |
| SurfaceHub |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Bei Ankunft |
| Syslog |Linux |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Von Azure-Speicher: 10 Minuten; von Agent: bei Ankunft |
| System Updates |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |Mindestens zweimal pro Tag und 15 Minuten nach Installation eines Updates |
| Windows-Sicherheitsereignisprotokolle |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Für Azure-Speicher: 10 Minuten; für Agent: bei Ankunft |
| Windows-Firewallprotokolle |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Bei Ankunft |
| Windows-Ereignisprotokolle |Windows |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |Für Azure-Speicher: 1 Minute; für Agent: bei Ankunft |
| Wire Data |Windows (2012 R2 / 8.1 oder höher) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png) |Jede Minute |

## <a name="log-analytics-preview-solutions-and-features"></a>Lösungen und Features der Vorschauversion von Log Analytics
Indem wir einen Dienst ausführen und DevOps-Methoden anwenden, können wir als Partner mit Kunden zusammenarbeiten, um Features und Lösungen zu entwickeln.

Während der Phase der privaten Vorschau erhält eine kleine Gruppe von Kunden Zugriff auf eine frühe Implementierung des Features bzw. der Lösung, damit wir Feedback erhalten und Verbesserungen einbauen können. Diese frühe Implementierung verfügt nur über minimale Features und betriebsbezogene Funktionen.

Das Ziel besteht darin, Dinge schnell auszuprobieren, um zu ermitteln, was funktioniert und was nicht funktioniert. Dieser Prozess wird durchlaufen, bis wir von den Kunden mit der privaten Vorschauversion das Feedback erhalten, dass alles für eine öffentliche Vorschauversion bereit ist.

Während der öffentlichen Vorschauphase stellen wir das Feature oder die Lösung für alle Benutzer zur Verfügung, um weiteres Feedback zu erhalten und die Skalierung und Effizienz zu überprüfen. Während dieser Phase gilt Folgendes:

* Vorschaufeatures werden auf der Registerkarte „Einstellungen“ angezeigt und können von jedem Benutzer aktiviert werden.
* Vorschaulösungen können über den Katalog oder mit einem veröffentlichten Skript hinzugefügt werden.

### <a name="what-should-i-know-about-preview-features-and-solutions?"></a>Was sollte ich über Features und Lösungen der Vorschauversion wissen?
Wir freuen uns, die neuen Features und Lösungen anbieten zu können, und wir arbeiten bei der Entwicklung gern mit Ihnen zusammen.

Vorschaufeatures und -lösungen sind aber nicht für jeden Benutzer geeignet. Stellen Sie also sicher, dass die Verwendung einer in der Entwicklung befindlichen Anwendung für Sie in Ordnung ist, bevor Sie die Nutzung einer privaten Vorschauversion beantragen oder eine öffentliche Vorschau aktivieren.

Wenn Sie ein Vorschaufeature über das Portal aktivieren, wird eine Warnung mit dem Hinweis angezeigt, dass sich das Feature in der Vorschauphase befindet.

#### <a name="for-both-*private*-and-*public*-preview"></a>Für *private* und *öffentliche* Vorschauversion
Folgendes gilt sowohl für öffentliche als auch für private Vorschauversionen:

* Es kann sein, dass nicht immer alles einwandfrei funktioniert.
  * Probleme können von kleineren Unzulänglichkeiten bis zu Fehlern reichen, bei denen ein Feature gar nicht funktioniert.
* Es besteht die Möglichkeit, dass die Vorschauversion eine negative Auswirkung auf Ihr System bzw. Ihre Umgebung hat.
  * Wir versuchen zu vermeiden, dass sich für die Systeme, die Sie mit OMS verwenden, negative Auswirkungen ergeben, aber es kann sein, dass unerwartete Dinge passieren.
* Es kann zu Datenverlust oder zur Beschädigung von Daten kommen.
* Unter Umständen bitten wir Sie, Diagnoseprotokolle oder andere Daten als Beitrag zur Problembehandlung zu erfassen.
* Es kann sein, dass das Feature oder die Lösung entfernt wird (entweder vorübergehend oder dauerhaft).
  * Aufgrund der Erkenntnisse während der Vorschauphase kann es sein, dass das Feature bzw. die Lösung nicht veröffentlicht wird.
* Es kann sein, dass Vorschauversionen nicht mit allen Konfigurationen funktionieren bzw. nicht umfassend getestet wurden und dass Folgendes von uns eingeschränkt wird:
  * Zulässige Betriebssysteme (z.B. kann ein Feature in der Vorschauphase nur für Linux gelten).
  * Zulässiger Agent-Typ (MMA, SCOM) (z.B. funktioniert ein Feature in der Vorschauphase nicht mit SCOM).  
* Vorschaulösungen und -features sind nicht durch die Vereinbarung zum Servicelevel abgedeckt.
* Für die Nutzung von Vorschaufeatures fallen Nutzungsgebühren an.
* Features oder Funktionen, die Sie benötigen, damit das Feature bzw. die Lösung nützlich ist, können fehlen oder unvollständig sein.
* Features/Lösungen sind unter Umständen nicht in allen Regionen verfügbar.
* Features/Lösungen werden unter Umständen nicht lokalisiert.
* Für Features/Lösungen gilt ggf. eine Beschränkung bei der Anzahl von Kunden oder Geräten für die Nutzung.
* Unter Umständen müssen Sie Skripts verwenden, um die Konfiguration durchzuführen und die Lösung bzw. das Feature zu aktivieren.
* Die Benutzeroberfläche ist unvollständig und kann sich von Tag zu Tag ändern.
* Öffentliche Vorschauversionen sind für Ihre Produktionssysteme bzw. kritischen Systeme ggf. nicht geeignet.

#### <a name="for-*private*-preview"></a>Für *private* Vorschauversion
Zusätzlich zu den obigen Punkten gilt für private Vorschauversionen Folgendes:

* Wir erwarten, dass Sie uns Feedback zu Ihren Erfahrungen liefern, damit wir das Feature bzw. die Lösung verbessern können.
* Unter Umständen wenden wir uns per Umfrage, Telefon oder E-Mail an Sie, um Feedback zu erhalten.
* Es funktioniert nicht immer alles einwandfrei.
* Unter Umständen ist ein Geheimhaltungsvertrag für die Teilnahme erforderlich, oder Inhalte werden als vertraulich gekennzeichnet.
  * Informieren Sie sich beim Program Manager, der für die Vorschauversion verantwortlich ist, über geltende Beschränkungen zur Offenlegung, bevor Sie Bloggen, Tweeten oder auf andere Art mit Dritten kommunizieren.
* Keine Verwendung für Produktionssysteme oder kritische Systeme.

### <a name="how-do-i-get-access-to-private-preview-features-and-solutions?"></a>Wie erhalte ich Zugriff auf Features und Lösungen der privaten Vorschauversion?
Wir laden Kunden je nach Art der Version über verschiedene Wege zur Nutzung von privaten Vorschauversionen ein.

* Indem Sie an der monatlichen Kundenumfrage teilnehmen und uns die Erlaubnis erteilen, mit Ihnen Kontakt aufzunehmen, erhöhen Sie Ihre Chancen für eine Einladung zur privaten Vorschauversion.
* Sie können von Ihrem Microsoft-Kundenteam nominiert werden.
* Sie können sich über die bei Twitter geposteten Details anmelden ( [msopsmgmt](https://twitter.com/msopsmgmt)
* Sie können sich basierend auf den Details registrieren, die bei Communityveranstaltungen bereitgestellt werden. Halten Sie bei Treffen, Konferenzen und in Onlinecommunitys nach uns Ausschau.

## <a name="next-steps"></a>Nächste Schritte
* [Durchsuchen von Protokollen](log-analytics-log-searches.md) und darüber, wie Sie detaillierte Informationen anzeigen, die von Lösungen gesammelt wurden.

<!--HONumber=Oct16_HO2-->


