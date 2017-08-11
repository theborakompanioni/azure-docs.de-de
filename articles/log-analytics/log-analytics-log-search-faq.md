---
title: "Häufig gestellte Fragen zur neuen Protokollsuche von Log Analytics | Microsoft-Dokumentation"
description: "Dieser Artikel behandelt häufig gestellte Fragen im Hinblick auf das Upgrade von Log Analytics auf die neue Abfragesprache."
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
ms.date: 07/25/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: b4031e5ec5ab38863e1e74f5b6ebb117f23a327e
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---

# <a name="log-analytics-new-log-search-frequently-asked-questions"></a>Häufig gestellte Fragen zur neuen Protokollsuche von Log Analytics

Dieser Artikel behandelt häufig gestellte Fragen im Hinblick auf das Upgrade von [Log Analytics auf die neue Abfragesprache](log-analytics-log-search-upgrade.md).



## <a name="upgrade-process"></a>Upgradeprozess

### <a name="i-have-several-workspaces-can-i-upgrade-all-workspaces-at-the-same-time"></a>Ich besitze mehrere Arbeitsbereiche. Kann ich alle Arbeitsbereiche gleichzeitig upgraden?  
A: Nein.  Das Upgrade gilt für jeweils nur einen Arbeitsbereich. Aktuell können Sie nicht mehrere Arbeitsbereiche gleichzeitig upgraden. Beachten Sie, dass andere Benutzer des upgegradeten Arbeitsbereichs ebenfalls betroffen sind.  

### <a name="will-existing-log-data-collected-in-my-workspace-be-modified-if-i-upgrade"></a>Werden vorhandene Protokolldaten, die in meinem Arbeitsbereich gesammelt wurden, verändert, wenn ich ein Upgrade durchführe?  
Nr. Die verfügbaren Protokolldaten, die für Ihre Arbeitsbereich-Suchvorgänge verfügbar sind, sind durch das Upgrade nicht betroffen. Gespeicherte Suchvorgänge, Warnungen und Ansichten, werden automatisch in die neue Suchsprache konvertiert.  

### <a name="what-happens-if-i-dont-upgrade-my-workspace"></a>Was passiert, wenn ich kein Upgrade für meinen Arbeitsbereich durchführe?  
Die ältere Protokollsuche wird in den kommenden Monaten als veraltet gekennzeichnet. Arbeitsbereiche, die zu dieser Zeit noch nicht upgegradet worden sind, erhalten automatisch ein Upgrade.

### <a name="i-didnt-choose-to-upgrade-but-my-workspace-has-been-upgraded-anyway-what-happened"></a>Ich wollte kein Upgrade durchführen, jedoch wurde trotzdem eines für meinen Arbeitsbereich durchgeführt. Was ist passiert?  
Es kann sein, dass ein anderer Administrator dieses Arbeitsbereichs ein Upgrade durchgeführt hat. Beachten Sie, dass ein Arbeitsbereich automatisch upgegradet wird, wenn die neue Sprache allgemein verfügbar wird.  

### <a name="i-have-upgraded-by-mistake-and-now-need-to-cancel-it-and-restore-everything-back-what-should-i-do"></a>Ich habe aus Versehen ein Upgrade durchgeführt, muss es jetzt abbrechen und alles wiederherstellen. Wie soll ich vorgehen?  
Kein Problem.  Wir erstellen vor dem Upgrade eine Momentaufnahme Ihres Arbeitsbereichs, damit Sie ihn wiederherstellen können. Bedenken Sie, dass Suchvorgänge, Warnungen oder Ansichten, die Sie nach dem Upgrade gespeichert haben, trotzdem verloren gehen.  Um Ihre Arbeitsbereichumgebung wiederherzustellen, befolgen Sie die Prozedur unter [Kann ich nach einem Upgrade die vorherige Version wiederherstellen?](log-analytics-log-search-upgrade.md#can-i-go-back-after-i-upgrade).



## <a name="log-searches"></a>Protokollsuchvorgänge

### <a name="i-have-saved-searches-outside-of-my-upgraded-workspace-can-i-convert-them-to-the-new-search-language-automatically"></a>Ich habe Suchen außerhalb meines aktualisierten Arbeitsbereichs gespeichert. Kann ich diese automatisch in die neue Suchsprache konvertieren?
Sie können den das Tool zum Konvertieren der Sprache auf der Seite der Protokollsuche verwenden, um sie zu konvertieren.  Es gibt keine Möglichkeit, mehrere Suchen automatisch zu konvertieren, ohne ein Upgrade auf den Arbeitsbereich durchzuführen.



## <a name="alerts"></a>Warnungen

### <a name="i-have-a-lot-of-alert-rules-do-i-need-to-create-them-again-in-the-new-language-after-i-upgrade"></a>Ich habe viele Warnungsregeln. Muss ich diese erneut in der neuen Sprache erstellen, nachdem ich das Upgrade durchgeführt habe?  
Nein, Ihre Warnungsregeln werden während des Upgrades automatisch in die neue Suchsprache konvertiert.  

## <a name="power-bi"></a>Power BI

### <a name="does-anything-change-with-powerbi-integration"></a>Ändert sich etwas bei der PowerBI-Integration?
Ja.  Sobald für Ihren Arbeitsbereich ein Upgrade durchgeführt wurde, funktioniert dieses Verfahren zum Exportieren von Log Analytics-Daten in Power BI nicht mehr.  Alle vorhandenen Zeitpläne, die Sie vor dem Upgrade erstellt haben, werden deaktiviert.  Nach dem Upgrade verwendet Azure Log Analytics dieselbe Plattform wie Application Insights, und Sie verwenden zum Exportieren von Log Analytics-Abfragen nach Power BI dasselbe Verfahren wie zum [Exportieren von Application Insights-Abfragen nach Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).



## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Upgrade Ihres Arbeitsbereichs auf die neue Log Analytics-Abfragesprache](log-analytics-log-search-upgrade.md).

