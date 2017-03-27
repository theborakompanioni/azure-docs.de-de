---
title: "Nachverfolgen von Änderungen mit Azure Log Analytics | Microsoft Docs"
description: "Mit der Change Tracking-Lösung in Log Analytics können Sie Änderungen an Software und Windows-Diensten in Ihrer Umgebung ganz einfach identifizieren."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: becb179da6bc6b6df629a07d3ddb5d50edbaa577
ms.lasthandoff: 03/14/2017


---
# <a name="track-software-changes-in-your-environment-with-the-change-tracking-solution"></a>Nachverfolgen von Änderungen an der Software in Ihrer Umgebung mit der Change Tracking-Lösung

Dieser Artikel unterstützt Sie bei der einfachen Erkennung von Änderungen an Ihrer Umgebung mithilfe der Änderungsnachverfolgungslösung in Log Analytics. Die Lösung verfolgt Änderungen an Windows- und Linux-Software, an Windows-Dateien, an Windows-Diensten und an Linux-Daemons nach. Durch Ermitteln von Konfigurationsänderungen können Sie Betriebsprobleme präzise bestimmen.

Sie installieren die Lösung, um den Typ des installierten Agents zu aktualisieren. Änderungen an installierter Software, Windows-Diensten und Linux-Daemons auf den überwachten Servern werden gelesen, und di Daten werden Verarbeitung an den Log Analytics-Dienst in der Cloud gesendet. Auf die empfangenen Daten wird Logik angewendet, und der Clouddienst zeichnet die Daten auf. Mithilfe der Informationen im Change Tracking-Dashboard können Sie ganz leicht die Änderungen erkennen, die in Ihrer Serverinfrastruktur vorgenommen wurden.

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

* Sie benötigen einen [Windows](log-analytics-windows-agents.md)-, [Operations Manager](log-analytics-om-agents.md)- oder [Linux](log-analytics-linux-agents.md)-Agent auf jedem Computer, auf dem Änderungen überwacht werden sollen.
* Fügen Sie mithilfe des [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ChangeTrackingOMS?tab=Overview) oder des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem OMS-Arbeitsbereich die Lösung für die Änderungsnachverfolgung hinzu.  Es ist keine weitere Konfiguration erforderlich.

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
| Windows und Linux |![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png) |![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png) |![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png) |![Nein](./media/log-analytics-change-tracking/oms-bullet-red.png) |![Nein](./media/log-analytics-change-tracking/oms-bullet-red.png) |![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png) | Zwischen 5 und 50 Minuten (je nach Art der Änderung) Weitere Informationen finden Sie unten. |


Die folgende Tabelle zeigt die Datensammlungshäufigkeit für die Änderungstypen.

| **Änderungstyp** | **frequency** | **Sendet der** **Agent** **gefundene Änderungen?** |
| --- | --- | --- |
| Windows-Registrierung | 50 Minuten | no |
| Windows-Datei | 30 Minuten | Ja. Wenn es innerhalb von 24 Stunden keine Änderungen gibt, wird eine Momentaufnahme gesendet. |
| Linux-Datei | 15 Minuten | Ja. Wenn es innerhalb von 24 Stunden keine Änderungen gibt, wird eine Momentaufnahme gesendet. |
| Windows-Dienste | 30 Minuten | Ja, alle 30 Minuten, wenn Änderungen gefunden werden. Alle 24 Stunden wird unabhängig von Änderungen eine Momentaufnahme gesendet. Die Momentaufnahme wird also auch gesendet, wenn es keine Änderungen gibt. |
| Linux-Daemons | 5 Minuten | Ja. Wenn es innerhalb von 24 Stunden keine Änderungen gibt, wird eine Momentaufnahme gesendet. |
| Windows-Software | 30 Minuten | Ja, alle 30 Minuten, wenn Änderungen gefunden werden. Alle 24 Stunden wird unabhängig von Änderungen eine Momentaufnahme gesendet. Die Momentaufnahme wird also auch gesendet, wenn es keine Änderungen gibt. |
| Linux-Software | 5 Minuten | Ja. Wenn es innerhalb von 24 Stunden keine Änderungen gibt, wird eine Momentaufnahme gesendet. |


## <a name="use-change-tracking"></a>Verwenden von Change Tracking
Nach der Installation können Sie die Zusammenfassung der Änderungen für die überwachten Server in OMS über die Kachel **Änderungsnachverfolgung** auf der Seite **Übersicht** anzeigen.

![Abbildung der Kachel "Change Tracking"](./media/log-analytics-change-tracking/change-tracking-tile.png)

Sie können Änderungen an Ihrer Infrastruktur anzeigen und dann Einzelheiten für die folgenden Kategorien abrufen:

* Änderungen nach Konfigurationstyp für Software und Windows-Dienste
* Softwareänderungen an Anwendungen und Updates für einzelne Server
* Gesamtanzahl von Softwareänderungen für jede Anwendungen
* Linux-Pakete
* Änderungen am Windows-Dienst für einzelne Server
* Änderungen an Linux-Daemons

![Abbildung des Change Tracking-Dashboards](./media/log-analytics-change-tracking/change-tracking-dash01.png)

![Abbildung des Change Tracking-Dashboards](./media/log-analytics-change-tracking/change-tracking-dash02.png)

### <a name="to-view-changes-for-any-change-type"></a>So zeigen Sie Änderungen für Änderungstypen an
1. Klicken Sie auf der Seite **Übersicht** auf die Kachel **Änderungsnachverfolgung**.
2. Prüfen Sie im Dashboard **Änderungsnachverfolgung** die Zusammenfassungsinformationen in einem der Blätter mit den Änderungstypen, und klicken Sie dann auf einen Eintrag, um detaillierte Informationen auf der Seite **Protokollsuche** anzuzeigen.
3. Sie können auf jeder Seite für die Protokollsuche die Ergebnisse nach Zeit, detaillierte Ergebnisse und Ihren Protokollsuchverlauf anzeigen. Außerdem können Sie nach Facets filtern, um die Ergebnisse einzugrenzen.

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie die [Protokollsuche in Log Analytics](log-analytics-log-searches.md) , um ausführliche Daten zur Änderungsnachverfolgung anzuzeigen.

