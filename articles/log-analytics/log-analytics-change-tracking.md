<properties
	pageTitle="Änderungsnachverfolgungslösung in Log Analytics | Microsoft Azure"
	description="Mit der Lösung zur Nachverfolgung von Konfigurationsänderungen in Log Analytics können Sie ganz einfach Änderungen an Software und Windows-Diensten identifizieren, die in Ihrer Umgebung vorgenommen werden. Durch das Feststellen solcher Konfigurationsänderungen können Sie Betriebsprobleme einfacher lokalisieren."
	services="log-analytics"
	documentationCenter=""
	authors="bandersmsft"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="log-analytics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/26/2016"
	ms.author="banders"/>

# Änderungsnachverfolgungslösung in Log Analytics


Mit der Lösung zur Nachverfolgung von Konfigurationsänderungen in Log Analytics können Sie ganz einfach Änderungen an Software und Windows-Diensten identifizieren, die in Ihrer Umgebung vorgenommen werden. Durch das Feststellen solcher Konfigurationsänderungen können Sie Betriebsprobleme einfacher lokalisieren.

Sie installieren die Lösung, um den Typ des installierten Agents zu aktualisieren. Änderungen an installierter Software und Windows-Diensten auf den überwachten Servern werden gelesen, und die Daten werden zur Verarbeitung an den Log Analytics-Dienst in der Cloud gesendet. Auf die empfangenen Daten wird Logik angewendet, und der Clouddienst zeichnet die Daten auf. Wenn Änderungen gefunden werden, werden die Server mit den Änderungen im Change Tracking-Dashboard angezeigt. Mithilfe der Informationen im Change Tracking-Dashboard können Sie ganz leicht die Änderungen erkennen, die in Ihrer Serverinfrastruktur vorgenommen wurden.

## Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Operations Manager ist für die Änderungsnachverfolgungslösung erforderlich.
- Sie benötigen einen Windows- oder Operations Manager-Agent auf jedem Computer, auf dem Änderungen überwacht werden sollen.
- Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem OMS-Arbeitsbereich die Änderungsnachverfolgungslösung hinzu. Es ist keine weitere Konfiguration erforderlich.


## Details zur Datensammlung für die Änderungsnachverfolgung

Die folgende Tabelle zeigt die Datensammlungsmethoden und andere Details dazu, wie Daten für die Änderungsnachverfolgung erfasst werden.


| Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Nein](./media/log-analytics-change-tracking/oms-bullet-red.png)| ![Nein](./media/log-analytics-change-tracking/oms-bullet-red.png)|![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png)| Stündlich|


## Verwenden von Change Tracking

Nach der Installation können Sie die Zusammenfassung der Änderungen für die überwachten Server in OMS über die Kachel **Änderungsnachverfolgung** auf der Seite **Übersicht** anzeigen.

![Abbildung der Kachel "Change Tracking"](./media/log-analytics-change-tracking/oms-changetracking-tile.png)

Sie können Änderungen an Ihrer Infrastruktur anzeigen und dann Einzelheiten für die folgenden Kategorien abrufen:

- Änderungen nach Konfigurationstyp für Software und Windows-Dienste
- Softwareänderungen an Anwendungen und Updates für einzelne Server
- Gesamtanzahl von Softwareänderungen für jede Anwendungen
- Änderungen am Windows-Dienst für einzelne Server

![Abbildung des Change Tracking-Dashboards](./media/log-analytics-change-tracking/oms-changetracking01.png)

![Abbildung des Change Tracking-Dashboards](./media/log-analytics-change-tracking/oms-changetracking02.png)

### So zeigen Sie Änderungen für Änderungstypen an

1. Klicken Sie auf der Seite **Übersicht** auf die Kachel **Change Tracking**.
2. Prüfen Sie im Dashboard **Change Tracking** die Zusammenfassungsinformationen in einem der Blätter mit den Änderungstypen, und klicken Sie dann auf einen Eintrag, um detaillierte Informationen auf der Seite **Protokollsuche** anzuzeigen.
3. Sie können auf jeder Seite für die Protokollsuche die Ergebnisse nach Zeit, detaillierte Ergebnisse und Ihren Protokollsuchverlauf anzeigen. Außerdem können Sie nach Facets filtern, um die Ergebnisse einzugrenzen.

## Nächste Schritte

- Verwenden Sie die [Protokollsuche in Log Analytics](log-analytics-log-searches.md), um ausführliche Daten zur Änderungsnachverfolgung anzuzeigen.

<!---HONumber=AcomDC_0518_2016-->