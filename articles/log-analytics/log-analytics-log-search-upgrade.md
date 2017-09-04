---
title: Upgrade von Azure Log Analytics auf die neue Protokollsuche | Microsoft-Dokumentation
description: "Die neue Log Analytics-Abfragesprache ist fast da – Sie können an der öffentlichen Vorschau teilnehmen.  Dieser Artikel beschreibt die Vorteile der neuen Sprache und die Konvertierung Ihres Arbeitsbereichs."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: magoedte;bwren
ms.translationtype: HT
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: 4a3ee3c4d1fa9b626a51f24997603adceed8311f
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---

# <a name="upgrade-your-azure-log-analytics-workspace-to-new-log-search"></a>Upgrade des Azure Log Analytics-Arbeitsbereichs auf die neue Protokollsuche

> [!NOTE]
> Ein Upgrade auf die neue Log Analytics-Abfragesprache ist zurzeit optional, sodass Sie genügend Zeit haben, [sich mit der neuen Sprache vertraut zu machen](https://go.microsoft.com/fwlink/?linkid=856078).  

Die neue Log Analytics-Abfragesprache ist da, und Sie müssen Ihren Arbeitsbereich aktualisieren, um sie nutzen zu können.  Dieser Artikel beschreibt die Vorteile der neuen Sprache und die Konvertierung Ihres Arbeitsbereichs.  Wenn Sie das Upgrade jetzt nicht durchführen, funktioniert Ihr Arbeitsbereich so wie bisher, wird aber zu einem späteren Zeitpunkt automatisch konvertiert.  Sie werden rechtzeitig benachrichtigt, wenn das Datum der Konvertierung feststeht.

Dieser Artikel bietet detaillierte Informationen zur neuen Sprache und zum Upgradeprozess.

## <a name="why-the-new-language"></a>Warum gibt es eine neue Sprache?
Wir wissen, dass jede Neuerung auch immer Arbeit und Mühen mit sich bringt, und wir ändern die Abfragesprache nicht einfach so aus Spaß.  Es gibt mehrere Gründe dafür, dass diese Änderung einen erheblichen Mehrwert für Ihre Log Analytics-Kunden bedeuten wird.

- **Einfach und gleichzeitig leistungsstark.** Die neue Sprache ist einfacher zu verstehen und ähnelt SQL – die Konstrukte entsprechen eher der natürlichen Sprache als bei der bisherigen Abfragesprache.
- **Umfassende Pipingfunktionen.**  Die neue Sprache bietet umfangreichere Pipingfunktionen als die alte.  Praktisch jede Ausgabe kann per Piping an einen anderen Befehl weitergegeben werden. So lassen sich komplexere Abfragen erstellen, die zuvor nicht möglich waren.
- **Feldextraktionen zur Suchzeit.**  Die neue Sprache unterstützt umfassendere zur der Laufzeit berechnete Felder als die alte Sprache.  Sie können komplexe Berechnungen für erweiterte Felder durchführen, und die berechneten Felder dann für weitere Befehle wie z.B. Joins und Aggregationen verwenden.
- **Erweiterte Joins.**  Die neue Sprache bietet im Vergleich zur alten Sprache erweiterte Joins, einschließlich der Fähigkeit, Tabellen in mehreren Feldern zu verknüpfen, innere und äußere Joins zu verwenden und erweiterte Felder zu verknüpfen.
- **Funktionen für Datum und Uhrzeit.**  Die neue Sprache bietet modernere Funktionen für Datum und Uhrzeit als die alte.
- **Intelligente Analysen**  Die neue Sprache arbeitet mit erweiterten Algorithmen, um Muster in Datasets zu bewerten und verschiedene Datasets zu vergleichen.
- **Advanced Analytics-Portal.**  Das Advanced Analytics-Portal bietet Analysefunktionen, die im Log Analytics-Portal nicht verfügbar sind, beispielsweise die Bearbeitung von mehreren Zeilen in Abfragen, zusätzliche Visualisierungen und eine erweiterte Diagnose.
- **Konsistenz mit anderen Anwendungen.**  Die neue Sprache und das Advanced Analytics-Portal werden bereits für Analysen in Application Insights verwendet.  Durch die Implementierung für Log Analytics sorgen wir für Konsistenz über die Azure-Dienste hinweg.
- **Bessere Integration in Power BI.** Abfragen in der neuen Sprache können in Power BI Desktop exportiert werden, damit Sie die umfangreichen Funktionen für die Datentransformation nutzen können.
- **Viele weitere Vorteile.** Auf der Website [Azure Log Analytics Query Language](https://docs.loganalytics.io) finden Sie alle Informationen und Tutorials für die neue Sprache.


## <a name="when-can-i-upgrade"></a>Wann kann ich das Upgrade durchführen?
Das Upgrade wird phasenweise in allen Azure-Regionen eingeführt und wird daher in einigen Regionen früher zur Verfügung stehen als in anderen.  Wenn Sie im oberen Bereich Ihres Arbeitsbereichs ein lilafarbenes Banner mit einer Einladung zum Upgrade sehen, können Sie das Upgrade für Ihren Arbeitsbereich durchführen.

![Upgrade 1](media/log-analytics-log-search-upgrade/upgrade-01a.png)

## <a name="what-happens-when-i-upgrade"></a>Was geschieht, wenn ich das Upgrade durchführe?
Wenn Sie Ihren Arbeitsbereich konvertieren, werden alle gespeicherten Suchvorgänge, Warnungsregeln und Ansichten, die Sie mit dem Ansicht-Designer erstellt haben, automatisch in die neue Sprache konvertiert.  Suchvorgänge, die in Projektmappen eingebunden sind, werden nicht automatisch, sondern beim nächsten Öffnen konvertiert.  Dieser Vorgang ist für Sie vollständig transparent.

## <a name="can-i-go-back-after-i-upgrade"></a>Kann ich nach einem Upgrade die vorherige Version wiederherstellen?
Beim Upgrade wird ein vollständiges Backup Ihres Arbeitsbereichs erstellt, einschließlich einer Momentaufnahme aller gespeicherten Suchvorgänge, Warnungsregeln und Ansichten.  So können Sie bei Bedarf Ihren alten Arbeitsbereich wiederherstellen.

Öffnen Sie zum Wiederherstellen Ihres alten Arbeitsbereichs die **Einstellungen** Ihres Arbeitsbereichs, und klicken Sie auf **Aktualisierungszusammenfassung**.  Wählen Sie die Option zum **Wiederherstellen des Legacyarbeitsbereichs**.  

![Legacyarbeitsbereich wiederherstellen](media/log-analytics-log-search-upgrade/restore-legacy-b.png)

## <a name="how-do-i-perform-the-upgrade"></a>Wie führe ich das Upgrade durch?
Sie können das Upgrade für Ihren Arbeitsbereich durchführen, wenn oben im Portal das lilafarbene Banner angezeigt wird.  

1.  Starten Sie den Upgradeprozess, indem Sie auf das lilafarbene Banner **Weitere Informationen und Upgrade** klicken.<br>![Upgrade 2](media/log-analytics-log-search-upgrade/upgrade-01a.png)<br>
2.  Lesen Sie die zusätzlichen Informationen auf der Seite „Upgradeinformationen“.<br>![Upgrade 2](media/log-analytics-log-search-upgrade/upgrade-03.png)<br>
3.  Klicken Sie auf **Upgrade jetzt auszuführen**, um das Upgrade zu starten.<br>![Upgrade 4](media/log-analytics-log-search-upgrade/upgrade-04.png)<br>Ein Benachrichtigungsfeld in der oberen rechten Ecke zeigt den Status an.<br>![Upgrade 5](media/log-analytics-log-search-upgrade/upgrade-05.png)
4.  Fertig!  Wechseln Sie zur Seite der Protokollsuche, um sich Ihren aktualisierten Arbeitsbereich anzusehen.<br>![Upgrade 6](media/log-analytics-log-search-upgrade/upgrade-06.png)<br>

Wenn ein Fehler auftritt, durch den das Upgrade nicht ausgeführt werden kann, stellen Sie eine Frage im [Diskussionsforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights), oder [erstellen Sie eine Supportanfrage](../azure-supportability/how-to-create-azure-support-request.md) im Azure-Portal.

## <a name="how-do-i-learn-the-new-language"></a>Wie lerne ich die neue Sprache?
Da die neue Sprache von vielen Diensten verwendet wird, haben wir eine [externe Website für die Dokumentation](https://docs.loganalytics.io/) erstellt.  Diese Website enthält Tutorials, Beispiele und eine vollständige Referenz, damit Sie schnell loslegen können. Unter [Getting Started with Queries](https://go.microsoft.com/fwlink/?linkid=856078) (Erste Schritte mit Abfragen) können Sie ein Tutorial zu der neuen Sprache durchlaufen, unter [Log Analytics query language](https://go.microsoft.com/fwlink/?linkid=856079) (Log Analytics-Abfragesprache) finden Sie die Sprachreferenz.  

Wenn Sie jedoch die bisherige Log Analytics-Abfragesprache noch nicht kennen, können Sie den Sprachkonverter verwenden, der Ihrem Arbeitsbereich im Rahmen des Upgrades hinzugefügt wird.

Geben Sie einfach die Abfrage in der alten Sprache ein, und klicken Sie dann auf **Konvertieren**, um die übersetzte Version anzuzeigen.  Sie können entweder auf die Suchschaltfläche klicken, um die Abfrage auszuführen, oder die Abfrage kopieren und an einer anderen Stelle verwenden – z.B. für eine Warnungsregel.

![Sprachkonverter](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="next-steps"></a>Nächste Schritte
- Sehen Sie sich ein [Tutorial zur neuen Sprache](https://go.microsoft.com/fwlink/?linkid=856078) an.
- Durchlaufen Sie ein [Tutorial zur Verwendung des Portals der Protokollsuche](log-analytics-log-search-log-search-portal.md) mit der neuen Abfragesprache.
- Sehen Sie sich eine Einführung in das neue [Advanced Analytics-Portal](https://go.microsoft.com/fwlink/?linkid=856587) an.

