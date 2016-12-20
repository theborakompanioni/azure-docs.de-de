---
title: "Änderungsnachverfolgungslösung in Log Analytics | Microsoft Docs"
description: "Mit der Lösung zur Nachverfolgung von Konfigurationsänderungen in Log Analytics können Sie ganz einfach Änderungen an Software und Windows-Diensten identifizieren, die in Ihrer Umgebung vorgenommen werden. Durch das Feststellen solcher Konfigurationsänderungen können Sie Betriebsprobleme einfacher lokalisieren."
services: operations-management-suite
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: operations-management-suite
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 56faeccbb25d2f0e3dbe6b104b5e9bc95a06ca36


---
# <a name="change-tracking-solution-in-log-analytics"></a>Änderungsnachverfolgungslösung in Log Analytics
Dieser Artikel hilft Ihnen bei der Nutzung der Änderungsnachverfolgungslösung in Log Analytics, um Änderungen an Ihrer Umgebung einfach zu bestimmen. Die Lösung verfolgt Änderungen an Software, Windows-Diensten, Linux-Daemons und Linux-Paketen nach. Durch Ermitteln von Konfigurationsänderungen können Sie Betriebsprobleme präzise bestimmen. Sie können die Lösung auch so konfigurieren, dass Änderungen an bestimmten Windows-Dateien nachverfolgt werden.

Sie installieren die Lösung, um den Typ des installierten Agents zu aktualisieren. Änderungen an installierter Software, Windows-Diensten und Linux-Daemons auf den überwachten Servern werden gelesen, und di Daten werden Verarbeitung an den Log Analytics-Dienst in der Cloud gesendet. Auf die empfangenen Daten wird Logik angewendet, und der Clouddienst zeichnet die Daten auf. Wenn Änderungen gefunden werden, werden die Server mit den Änderungen im Change Tracking-Dashboard angezeigt. Mithilfe der Informationen im Change Tracking-Dashboard können Sie ganz leicht die Änderungen erkennen, die in Ihrer Serverinfrastruktur vorgenommen wurden.

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

* Sie benötigen einen [Windows](log-analytics-windows-agents.md)-, [Operations Manager](log-analytics-om-agents.md)- oder [Linux](log-analytics-linux-agents.md)-Agent auf jedem Computer, auf dem Änderungen überwacht werden sollen.
* Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md)beschriebenen Prozesses Ihrem OMS-Arbeitsbereich die Änderungsnachverfolgungslösung hinzu.  Es ist keine weitere Konfiguration erforderlich.

### <a name="configure-windows-files-to-track"></a>Konfigurieren der nachzuverfolgenden Windows-Dateien
Führen Sie zum Konfigurieren der nachzuverfolgenden Dateien auf Windows-Computern die folgenden Schritte aus.

1. Klicken Sie im OMS-Portal auf **Einstellungen** (das Zahnradsymbol).
2. Klicken Sie auf der Seite **Einstellungen** auf **Daten** und dann auf **Nachverfolgung von Windows-Dateien**.
3. Geben Sie unter „Nachverfolgung von Windows-Dateien“ den vollständigen Pfad ein, und zwar einschließlich des Dateinamens der Datei, die Sie nachverfolgen möchten. Klicken Sie dann auf das Symbol **Hinzufügen**. Beispiel: „C:\Program Files (x86)\Internet Explorer\iexplore.exe“ oder „C:\Windows\System32\drivers\etc\hosts“.
4. Klicken Sie auf **Speichern**.  
   ![Änderungsnachverfolgung von Windows-Dateien](./media/log-analytics-change-tracking/windows-file-change-tracking.png)

### <a name="limitations"></a>Einschränkungen
Die Änderungsnachverfolgungslösung unterstützt derzeit Folgendes nicht:

* Ordner (Verzeichnisse)
* Rekursion
* Platzhalter
* Pfadvariablen
* Netzwerkdateisysteme

Weitere Einschränkungen:

* Die Spalte **Maximale Dateigröße** und ihre Werte werden in der aktuellen Implementierung nicht genutzt.
* Wenn Sie im 30-minütigen Sammlungszyklus mehr als 2.500 Dateien erfassen, wird die Leistung der Lösung möglicherweise beeinträchtigt.
* Wenn der Netzwerkverkehr hoch ist, kann es bis zu sechs Stunden dauern, bis Änderungsdatensätze angezeigt werden.
* Wenn Sie die Konfiguration ändern, während ein Computer heruntergefahren ist, kann der Computer Dateiänderungen bereitstellen, die zur vorherigen Konfiguration gehören.

## <a name="change-tracking-data-collection-details"></a>Details zur Datensammlung für die Änderungsnachverfolgung
Die Änderungsnachverfolgung sammelt Softwareinventur- und Windows-Dienst-Metadaten mit den Agents, die Sie aktiviert haben.

Die folgende Tabelle zeigt die Datensammlungsmethoden und andere Details dazu, wie Daten für die Änderungsnachverfolgung erfasst werden.

| Plattform | Direkt-Agent | SCOM-Agent | Linux-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Windows und Linux |![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png) |![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png) |![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png) |![Nein](./media/log-analytics-change-tracking/oms-bullet-red.png) |![Nein](./media/log-analytics-change-tracking/oms-bullet-red.png) |![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png) |Stündlich |

## <a name="use-change-tracking"></a>Verwenden von Change Tracking
Nach der Installation können Sie die Zusammenfassung der Änderungen für die überwachten Server in OMS über die Kachel **Änderungsnachverfolgung** auf der Seite **Übersicht** anzeigen.

![Abbildung der Kachel "Change Tracking"](./media/log-analytics-change-tracking/oms-changetracking-tile.png)

Sie können Änderungen an Ihrer Infrastruktur anzeigen und dann Einzelheiten für die folgenden Kategorien abrufen:

* Änderungen nach Konfigurationstyp für Software und Windows-Dienste
* Softwareänderungen an Anwendungen und Updates für einzelne Server
* Gesamtanzahl von Softwareänderungen für jede Anwendungen
* Linux-Pakete
* Änderungen am Windows-Dienst für einzelne Server
* Änderungen an Linux-Daemons

![Abbildung des Change Tracking-Dashboards](./media/log-analytics-change-tracking/oms-changetracking01.png)

![Abbildung des Change Tracking-Dashboards](./media/log-analytics-change-tracking/oms-changetracking02.png)

### <a name="to-view-changes-for-any-change-type"></a>So zeigen Sie Änderungen für Änderungstypen an
1. Klicken Sie auf der Seite **Übersicht** auf die Kachel **Änderungsnachverfolgung**.
2. Prüfen Sie im Dashboard **Änderungsnachverfolgung** die Zusammenfassungsinformationen in einem der Blätter mit den Änderungstypen, und klicken Sie dann auf einen Eintrag, um detaillierte Informationen auf der Seite **Protokollsuche** anzuzeigen.
3. Sie können auf jeder Seite für die Protokollsuche die Ergebnisse nach Zeit, detaillierte Ergebnisse und Ihren Protokollsuchverlauf anzeigen. Außerdem können Sie nach Facets filtern, um die Ergebnisse einzugrenzen.

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie die [Protokollsuche in Log Analytics](log-analytics-log-searches.md) , um ausführliche Daten zur Änderungsnachverfolgung anzuzeigen.




<!--HONumber=Nov16_HO3-->


