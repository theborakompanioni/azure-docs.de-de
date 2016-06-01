<properties 
   pageTitle="Alert Management-Lösung in Log Analytics | Microsoft Azure"
   description="Die Alert Management-Lösung in Log Analytics unterstützt Sie beim Analysieren aller Warnungen in Ihrer Umgebung. Zusätzlich zur Konsolidierung von in OMS generierten Warnungen werden Warnungen aus verbundenen Verwaltungsgruppen von System Center Operations Manager (SCOM) in Log Analytics importiert."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/12/2016"
   ms.author="bwren" />

# Alert Management-Lösung in Log Analytics

Die Alert Management-Lösung unterstützt Sie beim Analysieren aller Warnungen in Ihrer Umgebung. Zusätzlich zur Konsolidierung von in OMS generierten Warnungen werden Warnungen aus verbundenen Verwaltungsgruppen von System Center Operations Manager (SCOM) in Log Analytics importiert. In Umgebungen mit mehreren Verwaltungsgruppen bietet die Alert Management-Lösung eine konsolidierte Ansicht der Warnungen über alle Verwaltungsgruppen hinweg.

![Symbol „Alert Management“](media/log-analytics-solution-alert-management/icon.png)

## Voraussetzungen

- Zum Importieren der Warnungen aus SCOM ist für diese Lösung eine Verbindung zwischen Ihrem OMS-Arbeitsbereich und einer SCOM-Verwaltungsgruppe erforderlich. Hierzu wird der unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](log-analytics-om-agents.md) beschriebene Prozess verwendet.  


## Konfiguration

Fügen Sie die Alert Management-Lösung dem OMS-Arbeitsbereich hinzu, indem Sie den unter [Hinzufügen von Lösungen](log-analytics-add-solutions.md) beschriebenen Prozess verwenden. Es ist keine weitere Konfiguration erforderlich.

## Management Packs

Falls Ihre SCOM-Verwaltungsgruppe mit dem OMS-Arbeitsbereich verbunden ist, werden in SCOM die folgenden Management Packs installiert, wenn Sie diese Lösung hinzufügen. Für diese Management Packs ist keine Konfiguration oder Wartung erforderlich.

- Microsoft System Center Advisor Alert Management (Microsoft.IntelligencePacks.AlertManagement)

Weitere Informationen zur Aktualisierung von Management Packs finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](log-analytics-om-agents.md).

## Datensammlung

### Agents

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden.

| Verbundene Quelle | Support | Beschreibung |
|:--|:--|:--|
| [Windows-Agents](log-analytics-windows-agents.md) | Nein | Direkte Windows-Agents generieren keine SCOM-Warnungen. |
| [Linux-Agents](log-analytics-limux-agents.md) | Nein | Direkte Linux-Agents generieren keine SCOM-Warnungen. |
| [SCOM-Verwaltungsgruppe](log-analytics-om-agents.md) | Ja | Warnungen, die auf SCOM-Agents generiert werden, werden für die Verwaltungsgruppe und dann an Log Analytics weitergeleitet.<br><br>Es ist keine direkte Verbindung vom SCOM-Agent mit Log Analytics erforderlich. Warnungsdaten werden von der Verwaltungsgruppe an das OMS-Repository weitergeleitet. |
| [Azure-Speicherkonto](log-analytics-azure-storage.md) | Nein | SCOM-Warnungen werden nicht in Azure-Speicherkonten gespeichert. |

### Sammlungshäufigkeit

In OMS generierte Warnungen stehen sofort für die Lösung zur Verfügung. Warnungsdaten werden alle drei Minuten von der SCOM-Verwaltungsgruppe an Log Analytics gesendet.

## Verwenden der Lösung

Wenn Sie dem OMS-Arbeitsbereich die Alert Management-Lösung hinzufügen, wird Ihrem OMS-Dashboard die Kachel **Alert Management** (Alert Management) hinzugefügt. Auf dieser Kachel werden die Anzahl und eine grafische Darstellung der Anzahl von derzeit aktiven Warnungen angezeigt, die innerhalb der letzten 24 Stunden generiert wurden. Sie können diesen Zeitraum nicht ändern.

![Kachel „Alert Management“](media/log-analytics-solution-alert-management/tile.png)

Klicken Sie auf die Kachel **Alert Management** (Alert Management), um das Dashboard **Alert Management** (Alert Management) zu öffnen. Das Dashboard enthält die Spalten, die in der folgenden Tabelle angegeben sind. In jeder Spalte sind die zehn wichtigsten Warnungen nach ihrer Anzahl aufgeführt. Dies richtet sich jeweils nach den Spaltenkriterien für den angegebenen Bereich und Zeitraum. Sie können eine Protokollsuche durchführen, mit der die gesamte Liste ausgegeben wird, indem Sie unten in der Spalte auf **Alle anzeigen** oder auf die Spaltenüberschrift klicken.

| Column| Beschreibung |
|:--|:--|
| Kritische Warnungen | Alle Warnungen mit dem Schweregrad „Kritisch“, gruppiert nach Warnungsname. Klicken Sie auf einen Warnungsnamen, um eine Protokollsuche durchzuführen, mit der alle Datensätze für die Warnung zurückgegeben werden. |
| Warnungen | Alle Warnungen mit dem Schweregrad „Warnung“, gruppiert nach Warnungsname. Klicken Sie auf einen Warnungsnamen, um eine Protokollsuche durchzuführen, mit der alle Datensätze für die Warnung zurückgegeben werden. |
| Aktive SCOM-Warnungen | Alle SCOM-Warnungen mit einem anderen Zustand als *Geschlossen*, gruppiert nach der Quelle, die die Warnung generiert hat. |
| Alle aktiven Warnungen | Alle Warnungen mit einem beliebigen Schweregrad, gruppiert nach dem Namen der Warnung. Umfasst nur SCOM-Warnungen mit einem anderen Zustand als *Geschlossen*.|

Wenn Sie einen Bildlauf nach rechts durchführen, werden im Dashboard mehrere allgemeine Abfragen aufgeführt, auf die Sie klicken können, um eine [Protokollsuche](log-analytics-log-searches.md) nach Warnungsdaten durchzuführen.

![Alert Management-Dashboard](media/log-analytics-solution-alert-management/dashboard.png)

## Umfang und Zeitraum

Standardmäßig umfasst der Bereich der in der Alert Management-Lösung analysierten Warnungen alle verbundenen Verwaltungsgruppen, die innerhalb der letzten sieben Tage generiert wurden.

![Alert Management-Umfang](media/log-analytics-solution-alert-management/scope.png)

- Klicken Sie zum Ändern der in der Analyse enthaltenen Verwaltungsgruppen oben im Dashboard auf **Umfang**. Sie können entweder die Option **Global** verwenden, um alle verbundenen Verwaltungsgruppen anzuzeigen, oder **By Management Group** (Nach Verwaltungsgruppe), um eine einzelne Verwaltungsgruppe auszuwählen.

- Um den Zeitraum von Warnungen zu ändern, wählen Sie oben im Dashboard die Option **Data based on** (Daten basierend auf). Sie können Warnungen auswählen, die innerhalb der letzten sieben Tage, eines Tages oder sechs Stunden generiert wurden. Außerdem können Sie **Benutzerdefiniert** auswählen und einen benutzerdefinierten Datumsbereich angeben.

## Log Analytics-Datensätze

Mit der Alert Management-Lösung werden alle Datensätze vom Typ **Warnung** analysiert. Außerdem werden Warnungen aus SCOM importiert, und es werden jeweils ein entsprechender Datensatz vom Typ **Warnung** und der SourceSystem-Eintrag **OpsManager** erstellt. Die Eigenschaften der Datensätze sind in der folgenden Tabelle aufgeführt.

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ | *Warnung* |
| SourceSystem | *OpsManager* |
| AlertContext | Details des Datenelements, durch das die Warnung verursacht wurde, im XML-Format. |
| AlertDescription | Detaillierte Beschreibung der Warnung. |
| AlertId | GUID der Warnung. |
| AlertName | Name der Warnung. |
| AlertPriority | Prioritätsstufe der Warnung. |
| AlertSeverity | Schweregrad der Warnung. |
| AlertState | Letzter Lösungszustand der Warnung. |
| LastModifiedBy | Name des Benutzers, der die Warnung zuletzt geändert hat. |
| ManagementGroupName | Name der Verwaltungsgruppe, in der die Warnung generiert wurde. |
| RepeatCount | Angabe, wie oft die gleiche Warnung für dasselbe überwachte Objekt seit der Lösung generiert wurde. |
| ResolvedBy | Name des Benutzers, der die Lösung für die Warnung durchgeführt hat. Ist leer, wenn die Warnung noch nicht gelöst wurde. |
| SourceDisplayName | Anzeigename des Überwachungsobjekts, von dem die Warnung generiert wurde. |
| SourceFullName | Vollständiger Name des Überwachungsobjekts, von dem die Warnung generiert wurde. |
| TicketId | Ticket-ID für die Warnung, wenn die SCOM-Umgebung in einen Prozess zum Zuweisen von Tickets für Warnungen integriert ist. Ist leer, wenn keine Ticket-ID zugewiesen wurde. |
| TimeGenerated | Datum und Uhrzeit der Warnungserstellung. |
| TimeLastModified | Datum und Uhrzeit der letzten Änderung der Warnung. |
| TimeRaised | Datum und Uhrzeit der Warnungsgenerierung. |
| TimeResolved | Datum und Uhrzeit der Warnungslösung. Ist leer, wenn die Warnung noch nicht gelöst wurde. |

## Beispiele für Protokollsuchen

Die folgende Tabelle enthält Beispiele für Protokollsuchen für Warnungsdatensätze, die mit dieser Lösung erfasst wurden.

| Abfrage | Beschreibung |
|:--|:--|
| Type=Alert SourceSystem=OpsManager AlertSeverity=error TimeRaised>NOW-24HOUR | Kritische Warnungen, die innerhalb der letzten 24 Stunden ausgelöst wurden |
| Type=Alert AlertSeverity=warning TimeRaised>NOW-24HOUR | Warnungen vom Typ „Warnung“, die innerhalb der letzten 24 Stunden ausgelöst wurden |
| Type=Alert SourceSystem=OpsManager AlertState!=Closed TimeRaised>NOW-24HOUR | measure count() as Count by SourceDisplayName | Quellen mit aktiven Warnungen, die während der letzten 24 Stunden ausgelöst wurden |
| Type=Alert SourceSystem=OpsManager AlertSeverity=error TimeRaised>NOW-24HOUR AlertState!=Closed | Kritische Warnungen, die während der letzten 24 Stunden ausgelöst wurden und noch aktiv sind |
| Type=Alert SourceSystem=OpsManager TimeRaised>NOW-24HOUR AlertState=Closed | Warnungen, die während der letzten 24 Stunden ausgelöst wurden und jetzt geschlossen wurden |
| Type=Alert SourceSystem=OpsManager TimeRaised>NOW-1DAY | measure count() as Count by AlertSeverity | Warnungen, die während des letzten Tages ausgelöst wurden, gruppiert nach Schweregrad |
| Type=Alert SourceSystem=OpsManager TimeRaised>NOW-1DAY | sort RepeatCount desc | Warnungen, die während des letzten Tages ausgelöst wurden, sortiert nach dem Wert der Wiederholungsanzahl |

## Nächste Schritte

- Lesen Sie sich die Details zum Generieren von Warnungen aus Log Analytics unter [Warnungen in Log Analytics](log-analytics-alerts.md) durch. 

<!---HONumber=AcomDC_0518_2016-->