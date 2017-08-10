---
title: Untersuchen und Freigeben von Nutzungsdaten mit interaktiven Arbeitsmappen in Azure Application Insights | Microsoft-Dokumentation
description: Demografische Analyse der Benutzer Ihrer Web-App.
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 06/12/2017
ms.author: cfreeman
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: ecd3d86db1d6b3b65015d1d560027cedb4f2df58
ms.contentlocale: de-de
ms.lasthandoff: 08/02/2017

---

# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a>Untersuchen und Freigeben von Nutzungsdaten mit interaktiven Arbeitsmappen in Application Insights

Mit Arbeitsmappen werden [Azure Application Insights](app-insights-overview.md)-Datenvisualisierungen, [Analytics-Abfragen](app-insights-analytics.md) und Text in interaktiven Dokumenten kombiniert. Arbeitsmappen können von anderen Teammitgliedern bearbeitet werden, die Zugriff auf dieselbe Azure-Ressource haben. Dies bedeutet, dass die Abfragen und Steuerelemente zum Erstellen einer Arbeitsmappe für andere Benutzer verfügbar sind, die die Arbeitsmappe lesen. Auf diese Weise können sie leicht untersucht, erweitert und auf Fehler geprüft werden.

Arbeitsmappen sind für folgende Szenarien hilfreich:

* Untersuchen der Nutzung Ihrer App, wenn Ihnen die jeweilige Metrik nicht bereits bekannt ist: Anzahl von Benutzern, Bindungsraten, Konvertierungsraten usw. Im Gegensatz zu anderen Tools für die Analyse der Nutzung in Application Insights können Sie mit Arbeitsmappen mehrere Arten von Visualisierungen und Analysen kombinieren, sodass Sie bei der Untersuchung deutlich weniger stark eingeschränkt sind.
* Erläutern, welche Leistung für ein neu veröffentlichtes Feature erzielt wird, für Ihr Team, indem Sie die Benutzeranzahl für wichtige Interaktionen und andere Metriken anzeigen.
* Freigeben der Ergebnisse eines A/B-Experiments in Ihrer App für andere Mitglieder Ihres Teams. Sie können die Ziele für das Experiment anhand von Text erklären und anschließend die einzelnen Nutzungsmetriken und Analytics-Abfragen anzeigen, die zum Auswerten des Experiments verwendet werden – und zusätzlich eindeutige Anmerkungen dazu, ob eine Metrik jeweils oberhalb oder unterhalb des Zielwerts gelegen hat.
* Melden der Auswirkungen eines Ausfalls auf die Nutzung Ihrer App, indem Daten, eine Erläuterung in Textform und eine Auflistung der nächsten Schritte verwendet wird, um Ausfälle in der Zukunft zu verhindern.

> [!NOTE]
> Ihre Application Insights-Ressource muss Seitenansichten oder benutzerdefinierte Ereignisse enthalten, damit Arbeitsmappen verwendet werden können. [Informieren Sie sich darüber, wie Sie Ihre App so einrichten können, dass Seitenansichten automatisch mit dem Application Insights-JavaScript-SDK erfasst werden](app-insights-javascript.md).
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a>Bearbeiten, Neuanordnen, Klonen und Löschen der Abschnitte von Arbeitsmappen

Eine Arbeitsmappe besteht aus Abschnitten: Visualisierungen, Diagramme, Tabellen, Text oder Analytics-Abfrageergebnisse, die unabhängig voneinander bearbeitet werden können.

Klicken Sie zum Bearbeiten der Inhalte eines Arbeitsmappenabschnitts rechts unterhalb davon auf die Schaltfläche **Bearbeiten**.

![Application Insights – Bearbeitungssteuerelemente für Arbeitsmappenabschnitte](./media/app-insights-usage-workbooks/editing-controls.png)

1. Klicken Sie in der unten linken Ecke des Abschnitts auf **Bearbeitung abgeschlossen**, wenn Sie mit der Bearbeitung fertig sind.

2. Klicken Sie zum Erstellen eines Duplikats eines Abschnitts auf das Symbol **Abschnitt klonen**. Die Erstellung von doppelten Abschnitten ist eine hervorragende Möglichkeit, eine Abfrage zu durchlaufen, ohne vorherige Iterationen zu verlieren.

3. Klicken Sie auf das Symbol für **Nach oben** bzw. **Nach unten**, um in einer Arbeitsmappe zum nächsten Abschnitt zu springen.

4. Klicken Sie zum dauerhaften Entfernen eines Abschnitts auf das Symbol **Entfernen**.

## <a name="adding-usage-data-visualization-sections"></a>Hinzufügen von Visualisierungsabschnitten für Nutzungsdaten

Arbeitsmappen enthalten vier Arten von integrierten Visualisierungen für die Nutzungsanalyse. Mit jeder Visualisierung wird eine häufig gestellte Frage zur Nutzung Ihrer App beantwortet. Fügen Sie Abschnitte mit Analytics-Abfragen hinzu (siehe unten), um andere Tabellen und Diagramme als diese vier Abschnitte hinzuzufügen.

Verwenden Sie unten in der Arbeitsmappe bzw. unten in einem Abschnitt die Schaltfläche **Benutzer hinzufügen** oder eine entsprechende andere Schaltfläche, um Ihrer Arbeitsmappe Abschnitte für „Benutzer“, „Sitzungen“, „Ereignisse“ oder „Bindung“ hinzuzufügen.

![Abschnitt „Benutzer“ in Arbeitsmappen](./media/app-insights-usage-workbooks/users-section.png)

In Abschnitten vom Typ **Benutzer** wird die Frage „Wie viele Benutzer haben auf meiner Website eine Seite angezeigt oder ein Feature genutzt?“ beantwortet.

In Abschnitten vom Typ **Sitzungen** wird die Frage „Wie viele Sitzungen haben Benutzer mit dem Anzeigen einer Seite oder Nutzen eines Features meiner Website verbracht?“ beantwortet.

In Abschnitten vom Typ **Ereignisse** wird die Frage „Wie oft haben Benutzer auf meiner Website eine Seite angezeigt oder ein Feature genutzt?“ beantwortet.

Jeder dieser drei Abschnittstypen verfügt über die gleichen Steuerelemente und Visualisierungen:

* [Informieren Sie sich über die Bearbeitung in den Abschnitten „Benutzer“, „Sitzungen“ und „Ereignisse“.](app-insights-usage-segmentation.md)
* Wechseln Sie zwischen den Visualisierungen für das Hauptdiagramm, das Histogrammraster, die automatischen Einblicke und die Benutzerstichproben, indem Sie oben in einem Abschnitt jeweils die Kontrollkästchen **Diagramm anzeigen**, **Raster anzeigen**, **Einblicke anzeigen** und **Stichprobe dieser Benutzer** verwenden.

![Abschnitt vom Typ „Bindung“ in Arbeitsmappen](./media/app-insights-usage-workbooks/retention-section.png)

In Abschnitten vom Typ **Bindung** wird die Frage „Wie viele Personen, die an einem Tag oder in einer Woche eine Seite angezeigt oder ein Feature genutzt haben, sind am folgenden Tag bzw. in der darauffolgenden Woche zurückgekehrt?“ beantwortet.

* [Informieren Sie sich über die Bearbeitung in Abschnitten vom Typ „Bindung“](app-insights-usage-retention.md)
* Schalten Sie die Anzeige des optionalen Diagramms für die gesamten Kundenbindungsinformationen um, indem Sie oben im Abschnitt das Kontrollkästchen **Show overall retention chart** (Diagramm mit der gesamten Aufbewahrungsdauer anzeigen) verwenden.

## <a name="adding-application-insights-analytics-sections"></a>Hinzufügen von Application Insights Analytics-Abschnitten

![Analytics-Abschnitt in Arbeitsmappen](./media/app-insights-usage-workbooks/analytics-section.png)

Verwenden Sie zum Hinzufügen eines Abschnitts für eine Application Insights Analytics-Abfrage zu Ihrer Arbeitsmappe die Schaltfläche **Analyseabfrage hinzufügen** unten in der Arbeitsmappe (bzw. unten in einem Abschnitt).

Mit Abschnitten für Analyseabfragen können Sie Arbeitsmappen beliebige Abfragen für Ihre Application Insights-Daten hinzufügen. Diese Flexibilität bedeutet, dass Abschnitte mit Analyseabfragen Ihr Mittel zum Beantworten von anderen Fragen zu Ihrer Website als die obigen Fragen für „Benutzer“, „Sitzungen“, „Ereignisse“ und „Bindung“ sein sollten. Beispiele:

* Wie viele Ausnahmen wurden von Ihrer Website während des Zeitraums ausgelöst, in dem sich die Nutzung verringert hat?
* Wie waren die Seiteladezeiten für Benutzer verteilt, die eine Seite angezeigt haben?
* Wie viele Benutzer haben eine Gruppe von Seiten auf Ihrer Website angezeigt, eine andere Gruppe von Seiten aber nicht? Anhand dieser Information können Sie nachvollziehen, ob bestimmte Benutzergruppen unterschiedliche Teilfunktionen Ihrer Website nutzen. (Verwenden Sie den Operator `join` mit dem Modifizierer `kind=leftanti` in der Log Analytics-Abfragesprache.)

Weitere Informationen zum Schreiben von Abfragen finden Sie in der [Referenz zur Log Analytics-Abfragesprache](https://docs.loganalytics.io/).

## <a name="adding-text-and-markdown-sections"></a>Hinzufügen von Text und Markdown-Abschnitten

Das Hinzufügen von Überschriften, Erklärungen und Kommentaren zu Ihren Arbeitsmappen ermöglicht es Ihnen, aus einer Gruppe von Tabellen und Diagrammen eine Geschichte zu formen. Für Textabschnitte in Arbeitsmappen wird die [Markdownsyntax](https://daringfireball.net/projects/markdown/) zum Formatieren von Text unterstützt, z.B. Überschriften, Fettdruck, Kursivdruck und Listen mit Aufzählungszeichen.

Verwenden Sie die Schaltfläche **Text hinzufügen** unten in der Arbeitsmappe bzw. unten in einem Abschnitt, um Ihrer Arbeitsmappe einen Textabschnitt hinzuzufügen.

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Speichern und Freigeben von Arbeitsmappen für Ihr Team

Arbeitsmappen werden in einer Application Insights-Ressource gespeichert, und zwar entweder im für Sie privaten Abschnitt **Meine Berichte** oder im Abschnitt **Freigegebene Berichte**, der für alle Benutzer mit Zugriff auf die Application Insights-Ressource zugänglich ist. Klicken Sie zum Anzeigen aller Arbeitsmappen in der Ressource in der Aktionsleiste auf die Schaltfläche **Öffnen**.

Gehen Sie wie folgt vor, um eine Arbeitsmappe freizugeben, die derzeit unter **Meine Berichte** vorhanden ist:

1. Klicken Sie in der Aktionsleiste auf **Öffnen**.
2. Klicken Sie neben der Arbeitsmappe, die Sie freigeben möchten, auf die Schaltfläche „...“.
3. Klicken Sie auf **Move to Shared Reports** (In „Freigegebene Berichte“ verschieben).

Klicken Sie in der Aktionsleiste auf **Freigeben**, um eine Arbeitsmappe per Link bzw. E-Mail freizugeben. Beachten Sie, dass Empfänger des Links im Azure-Portal Zugriff auf diese Ressource benötigen, um die Arbeitsmappe anzuzeigen. Zum Durchführen von Bearbeitungen benötigen Empfänger mindestens Berechtigungen vom Typ „Mitwirkender“ für die Ressource.

Gehen Sie wie folgt vor, um in einem Azure-Dashboard einen Link zu einer Arbeitsmappe anzuheften:

1. Klicken Sie in der Aktionsleiste auf **Öffnen**.
2. Klicken Sie neben der Arbeitsmappe, die Sie anheften möchten, auf die Schaltfläche „...“.
3. Klicken Sie auf **An Dashboard anheften**.

## <a name="next-steps"></a>Nächste Schritte

* [Nutzungsübersicht](app-insights-usage-overview.md)
* [Benutzer-, Sitzungs- und Ereignisanalyse in Azure Application Insights](app-insights-usage-segmentation.md)
* [Bindung](app-insights-usage-retention.md)
* [Application Insights-API für benutzerdefinierte Ereignisse und Metriken](app-insights-api-custom-events-metrics.md)


