---
title: "Kachelreferenz für den Ansicht-Designer in OMS Log Analytics | Microsoft-Dokumentation"
description: "Mit dem Ansicht-Designer in Log Analytics können Sie benutzerdefinierte Ansichten in der OMS-Konsole erstellen, die unterschiedliche Visualisierungen von Daten im OMS-Repository enthalten. Dieser Artikel enthält eine Referenz der Einstellungen für die einzelnen Kacheln, die zur Verwendung in Ihren benutzerdefinierten Ansichten verfügbar sind."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 41787c8f-6c13-4520-b0d3-5d3d84fcf142
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 806aba12d07c10816a106b9b2af0e824ceb25ade
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="log-analytics-view-designer-tile-reference"></a>Kachelreferenz für den Ansicht-Designer in Log Analytics
Mit dem Ansicht-Designer in Log Analytics können Sie benutzerdefinierte Ansichten in der OMS-Konsole erstellen, die unterschiedliche Visualisierungen von Daten im OMS-Repository enthalten. Dieser Artikel enthält eine Referenz der Einstellungen für die einzelnen Kacheln, die zur Verwendung in Ihren benutzerdefinierten Ansichten verfügbar sind.

Weitere verfügbare Artikel für den Ansicht-Designer:

* [Ansicht-Designer](log-analytics-view-designer.md): Übersicht über den Ansicht-Designer und die Verfahren zum Erstellen und Bearbeiten von benutzerdefinierten Ansichten.
* [Referenz der Visualisierungskomponenten](log-analytics-view-designer-parts.md): Referenz der Einstellungen für die einzelnen Kacheln, die zur Verwendung in Ihren benutzerdefinierten Ansichten verfügbar sind.

>[!NOTE]
> Wenn Ihr Arbeitsbereich auf die [neue Log Analytics-Abfragesprache](log-analytics-log-search-upgrade.md) aktualisiert wurde, müssen die Abfragen in allen Ansichten in der [neuen Abfragesprache](https://docs.loganalytics.io/learn/tutorial_getting_started_with_queries.html) geschrieben werden.  Alle Ansichten, die vor dem Upgrade des Arbeitsbereichs erstellt wurden, werden automatisch konvertiert.

Die folgende Tabelle enthält die verschiedenen Typen von Kacheln, die im Ansicht-Designer verfügbar sind.  In den folgenden Abschnitten werden jeder Kacheltyp im Detail und dessen Eigenschaften beschrieben.

| Kachel | Beschreibung |
|:--- |:--- |
| [Number](#number-tile) |Einzelne Zahl, die die Anzahl der Datensätze aus einer Protokollsuchabfrage angibt. |
| [Zwei Zahlen](#two-numbers-tile) |Zwei einzelne Zahlen, die die Anzahl der Datensätze aus zwei unterschiedlichen Abfragen angeben. |
| [Ring](#donut-tile) |Ringdiagramm, das auf einer Abfrage basiert, mit einem zusammenfassenden Wert in der Mitte. |
| [Liniendiagramm und Legende](#line-chart-amp-callout-tile) |Liniendiagramm, das auf einer Abfrage basiert, und eine Legende mit einem zusammenfassenden Wert. |
| [Liniendiagramm](#line-chart-tile) |Liniendiagramm, das auf einer Abfrage basiert. |
| [Zwei Zeitachsen](#two-timelines-tile) |Säulendiagramm mit zwei Reihen, die jeweils auf einer separaten Abfrage basieren. |

## <a name="number-tile"></a>Kachel „Zahl“
Die Kachel **Zahl** zeigt eine einzelne Zahl, die die Anzahl der Datensätze einer Protokollabfrage angibt, und eine Legende an.

![Kachel „Zahl“](media/log-analytics-view-designer/tile-number.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| Name |Text, der oben auf der Kachel angezeigt wird. |
| Beschreibung |Text, der unter dem Namen der Kachel angezeigt wird. |
| **Kachel** | |
| Legende |Text, der unter dem Wert angezeigt wird. |
| Abfragen |Die auszuführende Abfrage.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze wird angezeigt. |
| **Erweitert** |**> Datenflussüberprüfung** |
| Aktiviert |Wählen Sie diese Option aus, wenn die Datenflussüberprüfung für die Kachel aktiviert werden soll.  Dadurch wird eine andere Meldung angezeigt, wenn keine Daten für die Kachel verfügbar sind.  Dies wird normalerweise verwendet, um eine Meldung während des vorübergehenden Zeitraums anzuzeigen, in dem die Ansicht installiert wird und Daten zur Verfügung gestellt werden. |
| Abfragen |Abfrage, die ausgeführt wird, um zu überprüfen, ob Daten für die Ansicht verfügbar sind.  Wenn die Abfrage keine Ergebnisse zurückgibt, wird eine Meldung anstelle des Werts aus der Hauptabfrage angezeigt. |
| Nachricht |Die Meldung, die angezeigt wird, wenn die Abfrage zur Datenflussüberprüfung keine Daten zurückgibt.  Wenn Sie keine Meldung angeben, wird *Bewertung wird durchgeführt* angezeigt. |


## <a name="two-numbers-tile"></a>Kachel „Zwei Zahlen“
Die Kachel **Zwei Zahlen** zeigt zwei Zahlen, die die Anzahl der Datensätze aus zwei verschiedenen Protokollabfragen angeben, sowie jeweils eine Bezeichnung an.

![Kachel „Zwei Zahlen“](media/log-analytics-view-designer/tile-two-numbers.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| Name |Text, der oben auf der Kachel angezeigt wird. |
| Beschreibung |Text, der unter dem Namen der Kachel angezeigt wird. |
| **Erste Kachel** | |
| Legende |Text, der unter dem Wert angezeigt wird. |
| Abfragen |Die auszuführende Abfrage.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze wird angezeigt. |
| **Zweite Kachel** | |
| Legende |Text, der unter dem Wert angezeigt wird. |
| Abfragen |Die auszuführende Abfrage.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze wird angezeigt. |
| **Erweitert** |**> Datenflussüberprüfung** |
| Aktiviert |Wählen Sie diese Option aus, wenn die Datenflussüberprüfung für die Kachel aktiviert werden soll.  Dadurch wird eine andere Meldung angezeigt, wenn keine Daten für die Kachel verfügbar sind.  Dies wird normalerweise verwendet, um eine Meldung während des vorübergehenden Zeitraums anzuzeigen, in dem die Ansicht installiert wird und Daten zur Verfügung gestellt werden. |
| Abfragen |Abfrage, die ausgeführt wird, um zu überprüfen, ob Daten für die Ansicht verfügbar sind.  Wenn die Abfrage keine Ergebnisse zurückgibt, wird eine Meldung anstelle des Werts aus der Hauptabfrage angezeigt. |
| Nachricht |Die Meldung, die angezeigt wird, wenn die Abfrage zur Datenflussüberprüfung keine Daten zurückgibt.  Wenn Sie keine Meldung angeben, wird *Bewertung wird durchgeführt* angezeigt. |


## <a name="donut-tile"></a>Kachel „Ring“
Die Kachel **Ring** zeigt eine einzelne Zahl an, die eine Spalte mit Werten in einer Protokollabfrage zusammenfasst.  Der Ring stellt die Ergebnisse der ersten drei Datensätze grafisch dar.

![Kachel „Ring“](media/log-analytics-view-designer/tile-donut.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| Name |Text, der oben auf der Kachel angezeigt wird. |
| Beschreibung |Text, der unter dem Namen der Kachel angezeigt wird. |
| **Ring** | |
| Abfragen |Abfrage, die für den Ring ausgeführt wird.  Die erste Eigenschaft sollte ein Textwert und die zweite Eigenschaft ein numerischer Wert sein.  Dies ist normalerweise eine Abfrage, in der das Schlüsselwort für die **Maßeinheit** verwendet wird, um die Ergebnisse zusammenzufassen. |
| **Ring** |**> Mitte** |
| Text |Text, der unter dem Wert im Ring angezeigt wird. |
| Vorgang |Der Vorgang, der für die Werteigenschaft ausgeführt wird, um sie zu einem einzelnen Wert zusammenzufassen.<br><br>– Summe: Addiert die Werte aller Datensätze mit dem Eigenschaftswert.<br>– Prozentsatz: Prozentsatz der summierten Werte aus Datensätzen mit dem Eigenschaftswert verglichen mit den summierten Werten aller Datensätze. |
| Vom Vorgang in der Mitte verwendete Ergebniswerte |Klicken Sie optional auf das Pluszeichen, um einen oder mehrere Werte hinzuzufügen.  Die Ergebnisse der Abfrage werden auf Datensätze mit den von Ihnen angegebenen Eigenschaftswerten beschränkt.  Wenn keine Werte hinzugefügt werden, werden alle Datensätze in die Abfrage aufgenommen. |
| **Ring** |**> Weitere Optionen** |
| Farben |Die Farbe, die für jede der ersten drei Eigenschaften angezeigt wird.  Wenn Sie andere Farben für bestimmte Eigenschaftswerte angeben möchten, verwenden Sie „Erweiterte Farbzuordnung“. |
| Erweiterte Farbzuordnung |Zeigt eine Farbe für bestimmte Eigenschaftswerte an.  Gehört der angegebene Wert zu den ersten drei, wird die alternative Farbe anstelle der Standardfarbe angezeigt.  Wenn die Eigenschaft nicht in den ersten drei enthalten ist, wird die Farbe nicht angezeigt. |
| **Erweitert** |**> Datenflussüberprüfung** |
| Aktiviert |Wählen Sie diese Option aus, wenn die Datenflussüberprüfung für die Kachel aktiviert werden soll.  Dadurch wird eine andere Meldung angezeigt, wenn keine Daten für die Kachel verfügbar sind.  Dies wird normalerweise verwendet, um eine Meldung während des vorübergehenden Zeitraums anzuzeigen, in dem die Ansicht installiert wird und Daten zur Verfügung gestellt werden. |
| Abfragen |Abfrage, die ausgeführt wird, um zu überprüfen, ob Daten für die Ansicht verfügbar sind.  Wenn die Abfrage keine Ergebnisse zurückgibt, wird eine Meldung anstelle des Werts aus der Hauptabfrage angezeigt. |
| Nachricht |Die Meldung, die angezeigt wird, wenn die Abfrage zur Datenflussüberprüfung keine Daten zurückgibt.  Wenn Sie keine Meldung angeben, wird *Bewertung wird durchgeführt* angezeigt. |


## <a name="line-chart-tile"></a>Kachel „Liniendiagramm“
Die Kachel **Liniendiagramm** zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im Verlauf an.  

![Kachel „Liniendiagramm und Legende“](media/log-analytics-view-designer/tile-line-chart.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| Name |Text, der oben auf der Kachel angezeigt wird. |
| Beschreibung |Text, der unter dem Namen der Kachel angezeigt wird. |
| **Liniendiagramm** | |
| Abfragen |Abfrage, die für das Liniendiagramm ausgeführt wird.  Die erste Eigenschaft sollte ein Textwert und die zweite Eigenschaft ein numerischer Wert sein.  Dies ist normalerweise eine Abfrage, in der das Schlüsselwort für die **Maßeinheit** verwendet wird, um die Ergebnisse zusammenzufassen.  Wenn in der Abfrage das Schlüsselwort für das **Intervall** verwendet wird, wird für die X-Achse des Diagramms dieses Intervall verwendet.  Wenn die Abfrage das Schlüsselwort für das **Intervall** nicht enthält, werden stündliche Intervalle für die X-Achse verwendet. |
| **Liniendiagramm** |**> Y-Achse** |
| Logarithmische Skala verwenden |Wählen Sie diese Option aus, um eine logarithmische Skala für die Y-Achse zu verwenden. |
| Units |Geben Sie die Einheiten für die von der Abfrage zurückgegebenen Werte an.  Diese Informationen werden verwendet, um Bezeichnungen im Diagramm anzuzeigen, die Werttypen angeben, und um optional Werte zu konvertieren.  Der **Typ der Einheit** gibt die Kategorie der Einheit an und definiert die verfügbaren Werte für den Typ in **Aktuelle Einheit**.  Bei Auswahl eines Werts in **Konvertieren in** werden die numerischen Werte vom Typ in **Aktuelle Einheit** in den Typ in **Konvertieren in** konvertiert. |
| Benutzerdefinierte Bezeichnung |Text, der für die Y-Achse neben der Bezeichnung für den Typ der Einheit angezeigt wird.  Wenn keine Bezeichnung angegeben ist, wird nur der Typ der Einheit angezeigt. |
| **Erweitert** |**> Datenflussüberprüfung** |
| Aktiviert |Wählen Sie diese Option aus, wenn die Datenflussüberprüfung für die Kachel aktiviert werden soll.  Dadurch wird eine andere Meldung angezeigt, wenn keine Daten für die Kachel verfügbar sind.  Dies wird normalerweise verwendet, um eine Meldung während des vorübergehenden Zeitraums anzuzeigen, in dem die Ansicht installiert wird und Daten zur Verfügung gestellt werden. |
| Abfragen |Abfrage, die ausgeführt wird, um zu überprüfen, ob Daten für die Ansicht verfügbar sind.  Wenn die Abfrage keine Ergebnisse zurückgibt, wird eine Meldung anstelle des Werts aus der Hauptabfrage angezeigt. |
| Nachricht |Die Meldung, die angezeigt wird, wenn die Abfrage zur Datenflussüberprüfung keine Daten zurückgibt.  Wenn Sie keine Meldung angeben, wird *Bewertung wird durchgeführt* angezeigt. |


## <a name="line-chart--callout-tile"></a>Kachel „Liniendiagramm und Legende“
Die Kachel **Liniendiagramm und Legende** zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im Verlauf und eine Legende mit einem zusammengefassten Wert an.  

![Kachel „Liniendiagramm und Legende“](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| Name |Text, der oben auf der Kachel angezeigt wird. |
| Beschreibung |Text, der unter dem Namen der Kachel angezeigt wird. |
| **Liniendiagramm** | |
| Abfragen |Abfrage, die für das Liniendiagramm ausgeführt wird.  Die erste Eigenschaft sollte ein Textwert und die zweite Eigenschaft ein numerischer Wert sein.  Dies ist normalerweise eine Abfrage, in der das Schlüsselwort für die **Maßeinheit** verwendet wird, um die Ergebnisse zusammenzufassen.  Wenn in der Abfrage das Schlüsselwort für das **Intervall** verwendet wird, wird für die X-Achse des Diagramms dieses Intervall verwendet.  Wenn die Abfrage das Schlüsselwort für das **Intervall** nicht enthält, werden stündliche Intervalle für die X-Achse verwendet. |
| **Liniendiagramm** |**> Legende** |
| Legende |Titeltext, der über dem Wert der Legende angezeigt wird. |
| Reihenname |Eigenschaftswert für die Reihe, die für den Wert der Legende verwendet wird.  Wenn keine Reihe angegeben wird, werden alle Datensätze der Abfrage verwendet. |
| Vorgang |Der Vorgang, der für die Werteigenschaft ausgeführt wird, um sie für die Legende zu einem einzelnen Wert zusammenzufassen.<br>– Durchschnitt: Durchschnitt der Werte von allen Datensätzen.<br><br>– Anzahl: Anzahl aller von der Abfrage zurückgegebenen Datensätze.<br>– Letztes Beispiel: Wert aus dem letzten Intervall im Diagramm.<br>– Max: Höchstwert aus den Intervallen im Diagramm.<br>– Min: Mindestwert aus den Intervallen im Diagramm.<br>– Summe: Summe der Werte von allen Datensätzen. |
| **Liniendiagramm** |**> Y-Achse** |
| Logarithmische Skala verwenden |Wählen Sie diese Option aus, um eine logarithmische Skala für die Y-Achse zu verwenden. |
| Units |Geben Sie die Einheiten für die von der Abfrage zurückgegebenen Werte an.  Diese Informationen werden verwendet, um Bezeichnungen im Diagramm anzuzeigen, die Werttypen angeben, und um optional Werte zu konvertieren.  Der **Typ der Einheit** gibt die Kategorie der Einheit an und definiert die verfügbaren Werte für den Typ in **Aktuelle Einheit**.  Bei Auswahl eines Werts in **Konvertieren in** werden die numerischen Werte vom Typ in **Aktuelle Einheit** in den Typ in **Konvertieren in** konvertiert. |
| Benutzerdefinierte Bezeichnung |Text, der für die Y-Achse neben der Bezeichnung für den Typ der Einheit angezeigt wird.  Wenn keine Bezeichnung angegeben ist, wird nur der Typ der Einheit angezeigt. |
| **Erweitert** |**> Datenflussüberprüfung** |
| Aktiviert |Wählen Sie diese Option aus, wenn die Datenflussüberprüfung für die Kachel aktiviert werden soll.  Dadurch wird eine andere Meldung angezeigt, wenn keine Daten für die Kachel verfügbar sind.  Dies wird normalerweise verwendet, um eine Meldung während des vorübergehenden Zeitraums anzuzeigen, in dem die Ansicht installiert wird und Daten zur Verfügung gestellt werden. |
| Abfragen |Abfrage, die ausgeführt wird, um zu überprüfen, ob Daten für die Ansicht verfügbar sind.  Wenn die Abfrage keine Ergebnisse zurückgibt, wird eine Meldung anstelle des Werts aus der Hauptabfrage angezeigt. |
| Nachricht |Die Meldung, die angezeigt wird, wenn die Abfrage zur Datenflussüberprüfung keine Daten zurückgibt.  Wenn Sie keine Meldung angeben, wird *Bewertung wird durchgeführt* angezeigt. |


## <a name="two-timelines-tile"></a>Kachel „Zwei Zeitachsen“
Die Kachel **Zwei Zeitachsen** zeigt die Ergebnisse von zwei Protokollabfragen im Verlauf als Säulendiagramme an.  Für jede Reihe wird eine Legende angezeigt.  

![Kachel „Zwei Zeitachsen“](media/log-analytics-view-designer/tile-two-timelines.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| Name |Text, der oben auf der Kachel angezeigt wird. |
| Beschreibung |Text, der unter dem Namen der Kachel angezeigt wird. |
| Erstes Diagramm | |
| Legende |Text, der unter der Legende für die erste Reihe angezeigt wird. |
| Farbe |Farbe, die für die Säulen in der ersten Reihe verwendet wird. |
| Diagrammabfrage |Abfrage, die für die erste Reihe ausgeführt wird.  Die Angabe der Anzahl von Datensätzen in jedem Zeitintervall wird durch die Säulen im Diagramm dargestellt. |
| Vorgang |Der Vorgang, der für die Werteigenschaft ausgeführt wird, um sie für die Legende zu einem einzelnen Wert zusammenzufassen.<br><br>– Durchschnitt: Durchschnitt der Werte von allen Datensätzen.<br>– Anzahl: Anzahl aller von der Abfrage zurückgegebenen Datensätze.<br>– Letztes Beispiel: Wert aus dem letzten Intervall im Diagramm.<br>– Max: Höchstwert aus den Intervallen im Diagramm. |
| **Zweites Diagramm** | |
| Legende |Text, der unter der Legende für die zweite Reihe angezeigt wird. |
| Farbe |Farbe, die für die Säulen in der zweiten Reihe verwendet wird. |
| Diagrammabfrage |Abfrage, die für die zweite Reihe ausgeführt wird.  Die Angabe der Anzahl von Datensätzen in jedem Zeitintervall wird durch die Säulen im Diagramm dargestellt. |
| Vorgang |Der Vorgang, der für die Werteigenschaft ausgeführt wird, um sie für die Legende zu einem einzelnen Wert zusammenzufassen.<br><br>– Durchschnitt: Durchschnitt der Werte von allen Datensätzen.<br>– Anzahl: Anzahl aller von der Abfrage zurückgegebenen Datensätze.<br>– Letztes Beispiel: Wert aus dem letzten Intervall im Diagramm.<br>– Max: Höchstwert aus den Intervallen im Diagramm. |
| **Erweitert** |**> Datenflussüberprüfung** |
| Aktiviert |Wählen Sie diese Option aus, wenn die Datenflussüberprüfung für die Kachel aktiviert werden soll.  Dadurch wird eine andere Meldung angezeigt, wenn keine Daten für die Kachel verfügbar sind.  Dies wird normalerweise verwendet, um eine Meldung während des vorübergehenden Zeitraums anzuzeigen, in dem die Ansicht installiert wird und Daten zur Verfügung gestellt werden. |
| Abfragen |Abfrage, die ausgeführt wird, um zu überprüfen, ob Daten für die Ansicht verfügbar sind.  Wenn die Abfrage keine Ergebnisse zurückgibt, wird eine Meldung anstelle des Werts aus der Hauptabfrage angezeigt. |
| Nachricht |Die Meldung, die angezeigt wird, wenn die Abfrage zur Datenflussüberprüfung keine Daten zurückgibt.  Wenn Sie keine Meldung angeben, wird *Bewertung wird durchgeführt* angezeigt. |


## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zur Unterstützung der Abfragen in Kacheln.
* Fügen Sie [Visualisierungskomponenten](log-analytics-view-designer-parts.md) zu Ihrer benutzerdefinierten Ansicht hinzu.

