---
title: Erstellen von Ansichten zum Analysieren von Daten in OMS Log Analytics | Microsoft-Dokumentation
description: "Mit dem Ansicht-Designer in Log Analytics können Sie benutzerdefinierte Ansichten erstellen, die im OMS- und im Azure-Portal angezeigt werden und verschiedene Visualisierungen von Daten im OMS-Repository enthalten. Dieser Artikel enthält eine Übersicht über den Ansicht-Designer und die Verfahren zum Erstellen und Bearbeiten von benutzerdefinierten Ansichten."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 4d6ea516717077ffb4b832caa9e8a064905fce09
ms.openlocfilehash: e101972777e45bcf3b49b113c6fd92d220ad43fc


---
# <a name="use-view-designer-to-create-custom-views-in-log-analytics"></a>Erstellen benutzerdefinierter Ansichten in Log Analytics mithilfe des Ansicht-Designers
Mit dem Ansicht-Designer in [Log Analytics]() können Sie benutzerdefinierte Ansichten in der OMS-Konsole erstellen, die verschiedene Visualisierungen von Daten im OMS-Repository enthalten. Dieser Artikel enthält eine Übersicht über den Ansicht-Designer und die Verfahren zum Erstellen und Bearbeiten von benutzerdefinierten Ansichten.

Weitere verfügbare Artikel für den Ansicht-Designer:

* [Referenz der Kacheln](log-analytics-view-designer-tiles.md): Referenz der Einstellungen für die einzelnen Kacheln, die zur Verwendung in Ihren benutzerdefinierten Ansichten verfügbar sind. 
* [Referenz der Visualisierungskomponenten](log-analytics-view-designer-parts.md): Referenz der Einstellungen für die einzelnen Kacheln, die zur Verwendung in Ihren benutzerdefinierten Ansichten verfügbar sind. 

## <a name="concepts"></a>Konzepte
Mit dem Ansicht-Designer erstellte Ansichten enthalten die Elemente der folgenden Tabelle.

| Teil | Beschreibung |
|:--- |:--- |
| Kachel |Wird auf dem Hauptdashboard mit der Log Analytics-Übersicht angezeigt.  Enthält ein visuelle Zusammenfassung der in der benutzerdefinierten Ansicht enthaltenen Informationen.  Verschiedene Kacheltypen bieten unterschiedliche Visualisierungen von Datensätzen im OMS-Repository.  Klicken Sie auf die Kachel, um die benutzerdefinierte Ansicht zu öffnen. |
| Benutzerdefinierte Ansicht |Wird angezeigt, wenn der Benutzer auf die Kachel klickt.  Enthält eine oder mehrere Visualisierungskomponenten. |
| Visualisierungskomponenten |Visualisierung von Daten im OMS-Repository basierend auf einem oder mehreren [Protokollsuchvorgängen](log-analytics-log-searches.md).  Die meisten Komponenten enthalten eine Kopfzeile mit einer allgemeinen Visualisierung und eine Liste der wichtigsten Ergebnisse.  Verschiedene Komponententypen bieten unterschiedliche Visualisierungen von Datensätzen im OMS-Repository.  Klicken Sie auf Elemente in der Komponente, um einen Protokollsuchvorgang auszuführen, der detaillierte Datensätze bereitstellt. |

![Übersicht über den Ansicht-Designer](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Hinzufügen des Ansicht-Designers zum Arbeitsbereich
Während sich der Ansicht-Designer in der Vorschauphase befindet, müssen Sie ihn zu Ihrem Arbeitsbereich hinzufügen, indem Sie **Vorschaufeatures** im Abschnitt **Einstellungen** des OMS-Portals auswählen.

![Aktivieren der Vorschau](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Erstellen und Bearbeiten von Ansichten
### <a name="create-a-new-view"></a>Erstellen einer neuen Ansicht
Öffnen Sie eine neue Ansicht im **Ansicht-Designer**, indem Sie im OMS-Hauptdashboard auf die Kachel für den Ansicht-Designer klicken.

![Kachel für den Ansicht-Designer](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Bearbeiten einer vorhandenen Ansicht
Um eine vorhandene Ansicht im Ansicht-Designer zu bearbeiten, öffnen Sie die Ansicht, indem Sie im OMS-Hauptdashboard auf deren Kachel klicken.  Klicken Sie dann auf die Schaltfläche **Bearbeiten**, um die Ansicht im Ansicht-Designer zu öffnen.

![Bearbeiten einer Ansicht](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Klonen einer vorhandenen Ansicht
Beim Klonen einer Ansicht wird eine neue Ansicht erstellt und im Ansicht-Designer geöffnet.  Die neue Ansicht weist den gleichen Namen wie das Original auf, allerdings wird „Kopie“ an das Ende angefügt.  Um eine Ansicht zu klonen, öffnen Sie die vorhandene Ansicht, indem Sie im OMS-Hauptdashboard auf deren Kachel klicken.  Klicken Sie dann auf die Schaltfläche **Klonen**, um die Ansicht im Ansicht-Designer zu öffnen.

![Klonen einer Ansicht](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Löschen einer vorhandenen Ansicht
Um eine vorhandene Ansicht zu löschen, öffnen Sie die Ansicht, indem Sie im OMS-Hauptdashboard auf deren Kachel klicken.  Klicken Sie dann auf die Schaltfläche **Bearbeiten**, um die Ansicht im Ansicht-Designer zu öffnen, und klicken Sie auf **Ansicht löschen**.

![Löschen einer Ansicht](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Exportieren einer vorhandenen Ansicht
Sie können eine Ansicht in eine JSON-Datei exportieren, die Sie in einen anderen Arbeitsbereich importieren oder in einer [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-authoring-templates.md) verwenden können.  Um eine vorhandene Ansicht zu exportieren, öffnen Sie die Ansicht, indem Sie im OMS-Hauptdashboard auf deren Kachel klicken.  Klicken Sie dann auf die Schaltfläche **Exportieren**, um eine Datei im Downloadordner des Browsers zu erstellen.  Der Name der Datei wird zum Namen der Ansicht mit der Erweiterung *omsview*.

![Exportieren einer Ansicht](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Importieren einer vorhandenen Ansicht
Sie können eine *omsview*-Datei importieren, die Sie aus einer anderen Verwaltungsgruppe exportiert haben.  Um eine vorhandene Ansicht zu importieren, müssen Sie zuerst eine neue Ansicht erstellen.  Klicken Sie dann auf die Schaltfläche **Importieren**, und wählen Sie die *omsview*-Datei aus.  Die Konfiguration in der Datei wird in die vorhandene Ansicht kopiert.

![Exportieren einer Ansicht](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Arbeiten mit dem Ansicht-Designer
Der Ansicht-Designer verfügt über drei Bereiche.  Der Bereich **Entwurf** stellt die benutzerdefinierte Ansicht dar.  Wenn Sie Kacheln und Komponenten des Bereichs **Steuerung** zum Bereich **Entwurf** hinzufügen, werden sie zur Ansicht hinzugefügt.  Im Bereich **Eigenschaften** werden die Eigenschaften für die Kachel oder die ausgewählte Komponente angezeigt.

![Ansicht-Designer](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigurieren der Kachel für die Ansicht
Eine benutzerdefinierte Ansicht kann nur über eine einzige Kachel verfügen.  Wählen Sie die Registerkarte **Kachel** im Bereich **Steuerung** aus, um die aktuelle Kachel anzuzeigen oder eine andere auszuwählen.  Im Bereich **Eigenschaften** werden die Eigenschaften für die aktuelle Kachel angezeigt.  Konfigurieren Sie die Kacheleigenschaften entsprechend den detaillierten Informationen in der [Referenz der Kacheln](log-analytics-view-designer-tiles.md), und klicken Sie auf **Übernehmen**, um die Änderungen zu speichern.

### <a name="configure-visualization-parts"></a>Konfigurieren von Visualisierungskomponenten
Eine Ansicht kann eine beliebige Anzahl von Visualisierungskomponenten enthalten.  Wählen Sie die Registerkarte **Ansicht** und anschließend eine Visualisierungskomponente aus, die Sie der Ansicht hinzufügen möchten.  Im Bereich **Eigenschaften** werden die Eigenschaften für die ausgewählte Komponente angezeigt.  Konfigurieren Sie die Ansichtseigenschaften entsprechend den detaillierten Informationen in der [Referenz der Visualisierungskomponenten](log-analytics-view-designer-parts.md), und klicken Sie auf **Übernehmen**, um die Änderungen zu speichern.

### <a name="delete-a-visualization-part"></a>Löschen einer Visualisierungskomponente
Sie können eine Visualisierungskomponente aus der Ansicht entfernen, indem Sie auf die Schaltfläche **X** oben rechts in der Komponente klicken.

### <a name="rearrange-visualization-parts"></a>Ändern der Anordnung von Visualisierungskomponenten
Ansichten weisen nur eine Zeile mit Visualisierungskomponenten auf.  Sie können vorhandene Komponenten in einer Ansicht neu anordnen, indem Sie darauf klicken und sie an eine neue Position ziehen.

## <a name="next-steps"></a>Nächste Schritte
* Fügen Sie [Kacheln](log-analytics-view-designer-tiles.md) zu Ihrer benutzerdefinierten Ansicht hinzu.
* Fügen Sie [Visualisierungskomponenten](log-analytics-view-designer-parts.md) zu Ihrer benutzerdefinierten Ansicht hinzu.




<!--HONumber=Jan17_HO4-->


