---
title: "Verwenden des Portals für die Protokollsuche in Azure Log Analytics | Microsoft-Dokumentation"
description: "Dieser Artikel enthält ein Tutorial, mit dem das Erstellen von Protokollsuchvorgängen und das Analysieren von Daten in Ihrem Log Analytics-Arbeitsbereich mithilfe des Portals für die Protokollsuche beschrieben wird.  Das Tutorial umfasst die Ausführung einiger einfachen Abfragen zum Zurückgeben von verschiedenen Arten von Daten und das Analysieren der Ergebnisse."
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
ms.date: 08/23/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: 6fc556ceb34cde26d5f3789a2397cdaa34b0b84d
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="create-log-searches-in-azure-log-analytics-using-the-log-search-portal"></a>Erstellen von Protokollsuchvorgängen in Azure Log Analytics mit dem Portal für die Protokollsuche

> [!NOTE]
> In diesem Artikel werden das Portal für die Protokollsuche in Azure Log Analytics und die Verwendung der neuen Abfragesprache behandelt.  Weitere Informationen zur neuen Sprache und zum Upgradeverfahren für den Arbeitsbereich finden Sie unter [Upgrade des Azure Log Analytics-Arbeitsbereichs auf die neue Protokollsuche](log-analytics-log-search-upgrade.md).  
>
> Wenn für Ihren Arbeitsbereich noch kein Upgrade auf die neue Abfragesprache durchgeführt wurde, finden Sie unter [Suchen von Daten mithilfe der Protokollsuche in Log Analytics](log-analytics-log-searches.md) Informationen zur aktuellen Version des Portals für die Protokollsuche.

Dieser Artikel enthält ein Tutorial, mit dem das Erstellen von Protokollsuchvorgängen und das Analysieren von Daten in Ihrem Log Analytics-Arbeitsbereich mithilfe des Portals für die Protokollsuche beschrieben wird.  Das Tutorial umfasst die Ausführung einiger einfachen Abfragen zum Zurückgeben von verschiedenen Arten von Daten und das Analysieren der Ergebnisse.  Im Mittelpunkt stehen die Funktionen im Portal für die Protokollsuche zum Ändern der Abfrage anstelle einer direkten Änderung der Abfrage.  Ausführliche Informationen zum direkten Bearbeiten der Abfrage finden Sie unter [Referenz zur Abfragesprache](https://go.microsoft.com/fwlink/?linkid=856079).

Wenn Sie Suchabfragen statt im Portal für die Protokollsuche im Advanced Analytics-Portal erstellen möchten, finden Sie weitere Informationen unter [Erste Schritte mit dem Analytics-Portal](https://go.microsoft.com/fwlink/?linkid=856587).  Beide Portale verwenden die gleiche Abfragesprache, um auf dieselben Daten im Log Analytics-Arbeitsbereich zuzugreifen.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Tutorial wird vorausgesetzt, dass Sie bereits über einen Log Analytics-Arbeitsbereich mit mindestens einer verbundenen Quelle verfügen, mit der Daten für zu analysierende Abfragen generiert werden können.  

- Wenn Sie noch keinen Arbeitsbereich eingerichtet haben, können Sie diesen kostenlos erstellen, indem Sie der Anleitung in [Erste Schritte mit einem Log Analytics-Arbeitsbereich](log-analytics-get-started.md) folgen.
- Verbinden Sie mindestens einen [Windows-Agent](log-analytics-windows-agents.md) oder einen [Linux-Agent](log-analytics-linux-agents.md) mit dem Arbeitsbereich.  

## <a name="open-the-log-search-portal"></a>Öffnen des Portals für die Protokollsuche
Öffnen Sie zunächst das Portal für die Protokollsuche.  Der Zugriff darauf ist über das Azure-Portal und das OMS-Portal möglich.

1. Öffnen Sie das Azure-Portal.
2. Navigieren Sie zu „Log Analytics“, und wählen Sie Ihren Arbeitsbereich aus.
3. Wählen Sie **Protokollsuche** aus, um den Vorgang im Azure-Portal fortzusetzen, oder starten Sie das OMS-Portal, indem Sie **OMS-Portal** auswählen und auf die Schaltfläche „Protokollsuche“ klicken.

![Schaltfläche für die Protokollsuche](media/log-analytics-log-search-log-search-portal/log-search-button.png)

## <a name="create-a-simple-search"></a>Erstellen einer einfachen Suche
Die schnellste Methode, Daten für die weitere Verwendung abzurufen, besteht in einer einfachen Abfrage, die alle Datensätze aus der Tabelle zurückgibt.  Wenn Windows- oder Linux-Clients mit Ihrem Arbeitsbereich verbunden sind, liegen die Daten in der Ereignistabelle (Windows) bzw. in der Syslog-Tabelle (Linux) vor.

Geben Sie eine der folgenden Abfragen im Suchfeld ein, und klicken Sie auf die Schaltfläche „Suchen“.  

```
Event
```
```
Syslog
```

Die Daten werden in der Standardlistenansicht zurückgegeben. Sie können ablesen, wie viele Datensätze insgesamt zurückgegeben wurden.

![Einfache Abfrage](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

Es werden nur die ersten paar Eigenschaften zu jedem Datensatz angezeigt.  Klicken Sie auf **Mehr anzeigen**, damit alle Eigenschaften für einen bestimmten Datensatz angezeigt werden.

![Datensatzdetails](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-the-time-scope"></a>Festlegen des Zeitbereichs
Jeder von Log Analytics erfasste Datensatz verfügt über eine **TimeGenerated**-Eigenschaft, die das Datum und die Uhrzeit der Erstellung des Datensatzes enthält.  Eine Abfrage im Portal für die Protokollsuche gibt nur Datensätze mit einem **TimeGenerated**-Wert innerhalb des Zeitbereichs zurück, der auf der linken Seite des Bildschirms angezeigt wird.  

Sie können den Zeitfilter durch Auswählen der Dropdownliste oder durch Verschieben des Schiebereglers ändern.  Der Schieberegler stellt ein Balkendiagramm dar, in dem die relative Anzahl der Datensätze für jeden Zeitabschnitt innerhalb des Bereichs angezeigt wird.  Dieser Abschnitt variiert je nach Bereich.

Der Standardzeitbereich ist **1 Tag**.  Ändern Sie diesen Wert in **7 Tage**. Die Gesamtzahl der Datensätze sollte entsprechend erhöht werden.

![Datums-/Zeitbereich](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-the-query"></a>Filtern der Abfrageergebnisse
Auf der linken Seite des Bildschirms befindet sich der Filterbereich, über den Sie Filter zu Abfragen hinzufügen können, ohne die Abfrage direkt zu verändern.  Es werden mehrere Eigenschaften der zurückgegebenen Datensätze mit den entsprechenden ersten zehn Werten und der Datensatzanzahl angezeigt.

Wenn Sie die **Ereignistabelle** verwenden, wählen Sie das Kontrollkästchen neben **Fehler** unter **EVENTLEVELNAME** aus.   Wenn Sie **Syslog** verwenden, wählen Sie das Kontrollkästchen neben **err** unter **SEVERITYLEVEL** aus.  Dadurch wird die Abfrage wie folgt geändert, um die Ergebnisse auf Fehlerereignisse zu beschränken:

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filter](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

Fügen Sie dem Filterbereich Eigenschaften hinzu, indem Sie im Eigenschaftenmenü eines der Datensätze **Zu Filtern hinzufügen** auswählen.

![Menü „Zu Filtern hinzufügen“](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

Sie können den gleichen Filter festlegen, indem Sie im Eigenschaftenmenü eines Datensatzes mit dem Wert, nach dem Sie filtern möchten, **Filter** auswählen.  

Die Option **Filter** ist nur für Eigenschaften verfügbar, deren Name in Blau angezeigt wird.  Dies sind *durchsuchbare* Felder, die für Suchbedingungen indiziert sind.  Graue Felder sind *Freitextsuchfelder*, die nur über die Option **Verweise anzeigen** verfügen.  Mit dieser Option werden Datensätze zurückgegeben, bei denen eine beliebige Eigenschaft diesen Wert besitzt.

![Menü „Filter“](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

Sie können die Ergebnisse nach einer einzelnen Eigenschaft gruppieren, indem Sie im Datensatzmenü die Option **Gruppieren nach** auswählen.  Dadurch wird der Abfrage ein [summarize](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator)-Operator hinzugefügt, um die Ergebnisse in einem Diagramm anzuzeigen.  Sie können auch nach mehreren Eigenschaften gruppieren, dazu müssen Sie die Abfrage jedoch direkt bearbeiten.  Wählen Sie im Datensatzmenü neben der **Computer**-Eigenschaft **Gruppieren nach „Computer“** aus.  

![Gruppieren nach „Computer“](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a>Arbeiten mit Ergebnissen
Das Portal für die Protokollsuche verfügt über eine Reihe von Funktionen zum Arbeiten mit den Ergebnissen einer Abfrage.  Sie können Ergebnisse sortieren, filtern und gruppieren, um die Daten zu analysieren, ohne die Abfrage selbst zu ändern.  Die Ergebnisse einer Abfrage werden standardmäßig nicht sortiert.

Klicken Sie zum Anzeigen der Daten in Tabellenform, die über zusätzliche Optionen zum Filtern und Sortieren verfügt, auf **Tabelle**.  

![Tabellenansicht](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

Klicken Sie auf den Pfeil neben einem Datensatz, um die Details für diesen Datensatz anzuzeigen.

![Ergebnisse sortieren](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

Sortieren Sie nach einem Feld, indem Sie auf den entsprechenden Spaltenkopf klicken.

![Ergebnisse sortieren](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

Filtern Sie die Ergebnisse nach einem bestimmten Wert in der Spalte, indem Sie auf die Schaltfläche „Filter“ klicken und eine Filterbedingung angeben.

![Ergebnisse filtern](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

Gruppieren Sie nach einer Spalte, indem Sie den entsprechenden Spaltenkopf an den Anfang der Ergebnisse ziehen.  Sie können nach mehreren Feldern gruppieren, indem Sie mehrere Spalten nach oben ziehen.

![Ergebnisse gruppieren](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a>Arbeiten mit Leistungsdaten
Leistungsdaten für Windows und Linux-Agents werden im Log Analytics-Arbeitsbereich in der Tabelle **Perf** gespeichert.  Leistungsdatensätze sehen wie andere Datensätze aus, und Sie können eine einfache Abfrage verfassen, mit der wie bei Ereignissen alle Leistungsdatensätze zurückgegeben werden.

```
Perf
```

![Leistungsdaten](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

Millionen Datensätze für alle Leistungsobjekte und -indikatoren zurückzugeben, ist jedoch nicht sehr nützlich.  Sie können die gleichen Verfahren wie oben verwenden, um die Daten zu filtern, oder einfach die folgende Abfrage direkt in das Protokollsuchfeld eingeben.  Dadurch werden nur die Datensätze zur Prozessorauslastung für Windows- und Linux-Computer zurückgegeben.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Prozessorauslastung](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

Dies schränkt die Daten zwar auf einen bestimmten Leistungsindikator ein, damit liegen sie jedoch immer noch nicht in einem nützlichen Format vor.  Sie können die Daten in einem Liniendiagramm anzeigen, dazu müssen Sie sie jedoch zunächst nach „Computer“ und „TimeGenerated“ gruppieren.  Um nach mehreren Feldern zu gruppieren, müssen Sie die Abfrage direkt ändern, ändern Sie die Abfrage daher wie folgt:  Hierbei wird die [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg())-Funktion für die **CounterValue**-Eigenschaft verwendet, um für jede Stunde den Durchschnittswert zu berechnen.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Leistungsdatendiagramm](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

Nachdem die Daten eine geeignete Gruppierung aufweisen, können Sie sie in einem visuellen Diagramm anzeigen, indem Sie den [render](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator)-Operator hinzufügen.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Liniendiagramm](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Log Analytics-Abfragesprache finden Sie unter [Erste Schritte mit dem Analytics-Portal](https://go.microsoft.com/fwlink/?linkid=856079).
- Arbeiten Sie das Tutorial für das [Advanced Analytics-Portal](https://go.microsoft.com/fwlink/?linkid=856587) durch, mit dem Sie die gleichen Abfragen ausführen und die gleichen Daten wie im Portal für die Protokollsuche abrufen können.

