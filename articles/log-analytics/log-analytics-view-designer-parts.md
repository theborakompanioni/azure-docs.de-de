---
title: Ansicht-Designer in Log Analytics | Microsoft Docs
description: "Mit dem Ansicht-Designer in Log Analytics können Sie benutzerdefinierte Ansichten in der OMS-Konsole erstellen, die unterschiedliche Visualisierungen von Daten im OMS-Repository enthalten. Dieser Artikel enthält eine Referenz der Einstellungen für die einzelnen Visualisierungskomponenten, die zur Verwendung in Ihren benutzerdefinierten Ansichten verfügbar sind."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 5718d620-b96e-4d33-8616-e127ee9379c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 58d9f258bbfa582cf4059eeed939a87fb03cd232


---
# <a name="log-analytics-view-designer-visualization-part-reference"></a>Referenz der Visualisierungskomponenten im Ansicht-Designer in Log Analytics
Mit dem Ansicht-Designer in Log Analytics können Sie benutzerdefinierte Ansichten in der OMS-Konsole erstellen, die unterschiedliche Visualisierungen von Daten aus dem OMS-Repository enthalten. Dieser Artikel enthält eine Referenz der Einstellungen für die einzelnen Visualisierungskomponenten, die zur Verwendung in Ihren benutzerdefinierten Ansichten verfügbar sind.

Weitere verfügbare Artikel für den Ansicht-Designer:

* [Ansicht-Designer](log-analytics-view-designer.md): Übersicht über den Ansicht-Designer und die Verfahren zum Erstellen und Bearbeiten von benutzerdefinierten Ansichten.
* [Referenz der Kacheln](log-analytics-view-designer-tiles.md): Referenz der Einstellungen für die einzelnen Kacheln, die zur Verwendung in Ihren benutzerdefinierten Ansichten verfügbar sind. 

Die folgende Tabelle beschreibt die verschiedenen Typen von Kacheln, die im Ansicht-Designer verfügbar sind.  In den folgenden Abschnitten werden jeder Kacheltyp im Detail und dessen Eigenschaften beschrieben.

| Ansichtstyp | Beschreibung |
|:--- |:--- |
| [Liste der Abfragen](#list-of-queries-part) |Zeigt eine Liste der Protokollsuchabfragen an.  Die Benutzer kann auf die einzelnen Abfragen klicken, um die Ergebnisse anzuzeigen. |
| [Zahl und Liste](#number-amp-list-part) |Die Kopfzeile weist eine einzelne Zahl auf, die die Anzahl der Datensätze aus einer Protokollsuchabfrage angibt.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Ein Diagramm gibt dabei den relativen Wert einer numerischen Spalte oder die Änderung des Werts im Lauf der Zeit an. |
| [Zwei Zahlen und Liste](#two-numbers-amp-list-part) |Die Kopfzeile weist zwei Zahlen auf, die die Anzahl der Datensätze aus getrennten Protokollsuchabfragen angeben.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Ein Diagramm gibt dabei den relativen Wert einer numerischen Spalte oder die Änderung des Werts im Lauf der Zeit an. |
| [Ring und Liste](#donut-amp-list-part) |Die Kopfzeile zeigt eine einzelne Zahl an, die eine Spalte mit Werten in einer Protokollabfrage zusammenfasst.  Der Ring stellt die Ergebnisse der ersten drei Datensätze grafisch dar. |
| [Zwei Zeitachsen und Liste](#two-timelines-amp-list-part) |Die Kopfzeile zeigt die Ergebnisse von zwei Protokollabfragen im Verlauf als Säulendiagramme an. In einer Legende wird eine einzelne Zahl dargestellt, die eine Wertespalte in einer Protokollabfrage zusammenfasst.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Ein Diagramm gibt dabei den relativen Wert einer numerischen Spalte oder die Änderung des Werts im Lauf der Zeit an. |
| [Informationen](#information-part) |Die Kopfzeile zeigt statischen Text und einen optionalen Link an.  Die Liste zeigt ein oder mehrere Elemente mit statischem Text und Titel an. |
| [Liniendiagramm, Legende und Liste](#line-chart-callout-amp-list-part) |Die Kopfzeile zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im Verlauf und eine Legende mit einem zusammengefassten Wert an.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Ein Diagramm gibt dabei den relativen Wert einer numerischen Spalte oder die Änderung des Werts im Lauf der Zeit an. |
| [Liniendiagramm und Liste](#line-chart-amp-list-part) |Die Kopfzeile zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im Verlauf an.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Ein Diagramm gibt dabei den relativen Wert einer numerischen Spalte oder die Änderung des Werts im Lauf der Zeit an. |
| [Komponente mit Liniendiagrammstapel](#stack-of-line-charts-part) |Zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im zeitlichen Verlauf an. |

## <a name="list-of-queries-part"></a>Komponente mit der Liste der Abfragen
Zeigt eine Liste der Protokollsuchabfragen an.  Die Benutzer kann auf die einzelnen Abfragen klicken, um die Ergebnisse anzuzeigen.  Die Ansicht enthält standardmäßig eine einzelne Abfrage, und Sie können auf **+ Abfrage** klicken, um zusätzliche Abfragen hinzuzufügen.

![Ansicht der Liste der Abfragen](media/log-analytics-view-designer/view-list-queries.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| **Allgemein** | |
| Titel |Text, der oben in der Ansicht angezeigt wird. |
| Neue Gruppe |Wählen Sie diese Option aus, um in der Ansicht auf der Grundlage der aktuellen Ansicht eine neue Gruppe zu erstellen. |
| Vorab ausgewählte Filter |Durch Trennzeichen getrennte Liste von Eigenschaften, die in den linken Filterbereich aufgenommen werden sollen, wenn der Benutzer eine Abfrage auswählt. |
| Rendermodus |Anfängliche Ansicht, die angezeigt wird, wenn die Abfrage ausgewählt wird.  Der Benutzer kann alle verfügbaren Ansichten auswählen, nachdem er die Abfrage geöffnet hat. |
| **Abfragen** | |
| Suchabfrage |Die auszuführende Abfrage. |
| Anzeigename |Beschreibender Name der Abfrage, der für den Benutzer angezeigt wird. |

## <a name="number-list-part"></a>Komponente mit Zahl und Liste
Die Kopfzeile weist eine einzelne Zahl auf, die die Anzahl der Datensätze aus einer Protokollsuchabfrage angibt.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Ein Diagramm gibt dabei den relativen Wert einer numerischen Spalte oder die Änderung des Werts im Lauf der Zeit an.

![Ansicht der Liste der Abfragen](media/log-analytics-view-designer/view-number-list.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| **Allgemein** | |
| Gruppentitel |Text, der oben in der Ansicht angezeigt wird. |
| Neue Gruppe |Wählen Sie diese Option aus, um in der Ansicht auf der Grundlage der aktuellen Ansicht eine neue Gruppe zu erstellen. |
| Symbol |Bilddatei, die neben dem Ergebnis in der Kopfzeile angezeigt wird. |
| Symbol verwenden |Wählen Sie diese Option aus, damit das Symbol angezeigt wird. |
| **Titel** | |
| Legende |Text, der oben in der Kopfzeile angezeigt wird. |
| Abfragen |Abfrage, die für die Kopfzeile ausgeführt wird.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze wird angezeigt. |
| **Liste** | |
| Abfragen |Abfrage, die für die Liste ausgeführt wird.  Die ersten beiden Eigenschaften für die ersten zehn Datensätze in den Ergebnissen werden angezeigt.  Die erste Eigenschaft sollte ein Textwert und die zweite Eigenschaft ein numerischer Wert sein.  Balken werden automatisch basierend auf dem relativen Wert der numerischen Spalte erstellt.<br><br>Verwenden Sie den Befehl zum Sortieren in der Abfrage, um die Datensätze in der Liste zu sortieren.  Der Benutzer kann auf „Alle anzeigen“ klicken, um die Abfrage auszuführen und alle Datensätze zurückzugeben. |
| Diagramm ausblenden |Wählen Sie diese Option, um das Diagramm rechts neben der numerischen Spalte zu deaktivieren. |
| Sparklines aktivieren |Wählen Sie diese Option, um eine Sparkline anstatt eines horizontalen Balkens anzuzeigen.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#sparklines). |
| Farbe |Farbe der Balken oder Sparklines. |
| Trennzeichen für Name und Wert |Ein Trennzeichen, um die Texteigenschaft in mehrere Werte zu zergliedern.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#name-value-separator). |
| Navigationsabfrage |Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#navigation-query). |
| **Liste** |**> Spaltentitel** |
| Name |Text, der oben in der ersten Spalte der Liste angezeigt wird. |
| Wert |Text, der oben in der zweiten Spalte der Liste angezeigt wird. |
| **Liste** |**> Schwellenwerte** |
| Schwellenwerte aktivieren |Wählen Sie diese Option, um Schwellenwerte zu aktivieren.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#thresholds). |

## <a name="two-numbers-list-part"></a>Komponente mit zwei Zahlen und Liste
Die Kopfzeile weist zwei Zahlen auf, die die Anzahl der Datensätze aus getrennten Protokollsuchabfragen angeben.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Ein Diagramm gibt dabei den relativen Wert einer numerischen Spalte oder die Änderung des Werts im Lauf der Zeit an.

![Ansicht mit zwei Zahlen und Liste](media/log-analytics-view-designer/view-two-numbers-list.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| **Allgemein** | |
| Gruppentitel |Text, der oben in der Ansicht angezeigt wird. |
| Neue Gruppe |Wählen Sie diese Option aus, um in der Ansicht auf der Grundlage der aktuellen Ansicht eine neue Gruppe zu erstellen. |
| Symbol |Bilddatei, die neben dem Ergebnis in der Kopfzeile angezeigt wird. |
| Symbol verwenden |Wählen Sie diese Option aus, damit das Symbol angezeigt wird. |
| **Titel** | |
| Legende |Text, der oben in der Kopfzeile angezeigt wird. |
| Abfragen |Abfrage, die für die Kopfzeile ausgeführt wird.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze wird angezeigt. |
| **Liste** | |
| Abfragen |Abfrage, die für die Liste ausgeführt wird.  Die ersten beiden Eigenschaften für die ersten zehn Datensätze in den Ergebnissen werden angezeigt.  Die erste Eigenschaft sollte ein Textwert und die zweite Eigenschaft ein numerischer Wert sein.  Balken werden automatisch basierend auf dem relativen Wert der numerischen Spalte erstellt.<br><br>Verwenden Sie den Befehl zum Sortieren in der Abfrage, um die Datensätze in der Liste zu sortieren.  Der Benutzer kann auf „Alle anzeigen“ klicken, um die Abfrage auszuführen und alle Datensätze zurückzugeben. |
| Diagramm ausblenden |Wählen Sie diese Option, um das Diagramm rechts neben der numerischen Spalte zu deaktivieren. |
| Sparklines aktivieren |Wählen Sie diese Option, um eine Sparkline anstatt eines horizontalen Balkens anzuzeigen.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#sparklines). |
| Farbe |Farbe der Balken oder Sparklines. |
| Vorgang |Vorgang, der für die Sparkline ausgeführt werden soll.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#sparklines). |
| Trennzeichen für Name und Wert |Ein Trennzeichen, um die Texteigenschaft in mehrere Werte zu zergliedern.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#name-value-separator). |
| Navigationsabfrage |Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#navigation-query). |
| **Liste** |**> Spaltentitel** |
| Name |Text, der oben in der ersten Spalte der Liste angezeigt wird. |
| Wert |Text, der oben in der zweiten Spalte der Liste angezeigt wird. |
| **Liste** |**> Schwellenwerte** |
| Schwellenwerte aktivieren |Wählen Sie diese Option, um Schwellenwerte zu aktivieren.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#thresholds). |

## <a name="donut-list-part"></a>Komponente mit Ring und Liste
Die Kopfzeile zeigt eine einzelne Zahl an, die eine Spalte mit Werten in einer Protokollabfrage zusammenfasst.  Der Ring stellt die Ergebnisse der ersten drei Datensätze grafisch dar.

![Ansicht mit Ring und Liste](media/log-analytics-view-designer/view-donut-list.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| **Allgemein** | |
| Gruppentitel |Text, der oben auf der Kachel angezeigt wird. |
| Neue Gruppe |Wählen Sie diese Option aus, um in der Ansicht auf der Grundlage der aktuellen Ansicht eine neue Gruppe zu erstellen. |
| Symbol |Bilddatei, die neben dem Ergebnis in der Kopfzeile angezeigt wird. |
| Symbol verwenden |Wählen Sie diese Option aus, damit das Symbol angezeigt wird. |
| **Kopfzeile** | |
| Titel |Text, der oben in der Kopfzeile angezeigt wird. |
| Untertitel |Text, der unter dem Titel oben in der Kopfzeile angezeigt wird. |
| **Ring** | |
| Abfragen |Abfrage, die für den Ring ausgeführt wird.  Die erste Eigenschaft sollte ein Textwert und die zweite Eigenschaft ein numerischer Wert sein. |
| **Ring** |**> Mitte** |
| Text |Text, der unter dem Wert im Ring angezeigt wird. |
| Vorgang |Der Vorgang, der für die Werteigenschaft ausgeführt wird, um sie zu einem einzelnen Wert zusammenzufassen.<br><br>– Summe: Die Werte aller Datensätze werden addiert.<br>– Prozentsatz: Prozentsatz der Datensätze, die von den Werten in **Vom Vorgang in der Mitte verwendete Ergebniswerte** zurückgegeben werden, von der Gesamtanzahl der Datensätze in der Abfrage. |
| Vom Vorgang in der Mitte verwendete Ergebniswerte |Klicken Sie optional auf das Pluszeichen, um einen oder mehrere Werte hinzuzufügen.  Die Ergebnisse der Abfrage werden auf Datensätze mit den von Ihnen angegebenen Eigenschaftswerten beschränkt.  Wenn keine Werte hinzugefügt werden, werden alle Datensätze in die Abfrage aufgenommen. |
| **Weitere Optionen** |**> Farben** |
| Farbe 1<br>Farbe 2<br>Farbe 3 |Wählen Sie die Farbe für jeden der im Ring angezeigten Werte aus. |
| **Weitere Optionen** |**> Erweiterte Farbzuordnung** |
| Feldwert |Geben Sie den Namen eines Felds ein, um es in einer anderen Farbe anzuzeigen, wenn es im Ring enthalten ist. |
| Farbe |Wählen Sie die Farbe für das eindeutige Feld aus. |
| **Liste** | |
| Abfragen |Abfrage, die für die Liste ausgeführt wird.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze wird angezeigt. |
| Diagramm ausblenden |Wählen Sie diese Option, um das Diagramm rechts neben der numerischen Spalte zu deaktivieren. |
| Sparklines aktivieren |Wählen Sie diese Option, um eine Sparkline anstatt eines horizontalen Balkens anzuzeigen.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#sparklines). |
| Farbe |Farbe der Balken oder Sparklines. |
| Vorgang |Vorgang, der für die Sparkline ausgeführt werden soll.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#sparklines). |
| Trennzeichen für Name und Wert |Ein Trennzeichen, um die Texteigenschaft in mehrere Werte zu zergliedern.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#name-value-separator). |
| Navigationsabfrage |Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#navigation-query). |
| **Liste** |**> Spaltentitel** |
| Name |Text, der oben in der ersten Spalte der Liste angezeigt wird. |
| Wert |Text, der oben in der zweiten Spalte der Liste angezeigt wird. |
| **Liste** |**> Schwellenwerte** |
| Schwellenwerte aktivieren |Wählen Sie diese Option, um Schwellenwerte zu aktivieren.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#thresholds). |

## <a name="two-timelines-list-part"></a>Komponente mit zwei Zeitachsen und Liste
Die Kopfzeile zeigt die Ergebnisse von zwei Protokollabfragen im Verlauf als Säulendiagramme an. In einer Legende wird eine einzelne Zahl dargestellt, die eine Wertespalte in einer Protokollabfrage zusammenfasst.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Ein Diagramm gibt dabei den relativen Wert einer numerischen Spalte oder die Änderung des Werts im Lauf der Zeit an.

![Ansicht mit zwei Zeitachsen und Liste](media/log-analytics-view-designer/view-two-timelines-list.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| **Allgemein** | |
| Gruppentitel |Text, der oben auf der Kachel angezeigt wird. |
| Neue Gruppe |Wählen Sie diese Option aus, um in der Ansicht auf der Grundlage der aktuellen Ansicht eine neue Gruppe zu erstellen. |
| Symbol |Bilddatei, die neben dem Ergebnis in der Kopfzeile angezeigt wird. |
| Symbol verwenden |Wählen Sie diese Option aus, damit das Symbol angezeigt wird. |
| **Erstes Diagramm<br>Zweites Diagramm** | |
| Legende |Text, der unter der Legende für die erste Reihe angezeigt wird. |
| Farbe |Farbe, die für die Säulen in der Reihe verwendet wird. |
| Abfrage |Abfrage, die für die erste Reihe ausgeführt wird.  Die Angabe der Anzahl von Datensätzen in jedem Zeitintervall wird durch die Säulen im Diagramm dargestellt. |
| Vorgang |Der Vorgang, der für die Werteigenschaft ausgeführt wird, um sie für die Legende zu einem einzelnen Wert zusammenzufassen.<br><br>– Summe: Summe der Werte von allen Datensätzen.<br>– Durchschnitt: Durchschnitt der Werte von allen Datensätzen.<br>– Letztes Beispiel: Wert aus dem letzten Intervall im Diagramm.<br>– Erstes Beispiel: Wert aus dem ersten Intervall im Diagramm.<br>– Anzahl: Anzahl aller von der Abfrage zurückgegebenen Datensätze. |
| **Liste** | |
| Abfragen |Abfrage, die für die Liste ausgeführt wird.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze wird angezeigt. |
| Diagramm ausblenden |Wählen Sie diese Option, um das Diagramm rechts neben der numerischen Spalte zu deaktivieren. |
| Sparklines aktivieren |Wählen Sie diese Option, um eine Sparkline anstatt eines horizontalen Balkens anzuzeigen.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#sparklines). |
| Farbe |Farbe der Balken oder Sparklines. |
| Vorgang |Vorgang, der für die Sparkline ausgeführt werden soll.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#sparklines). |
| Navigationsabfrage |Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#navigation-query). |
| **Liste** |**> Spaltentitel** |
| Name |Text, der oben in der ersten Spalte der Liste angezeigt wird. |
| Wert |Text, der oben in der zweiten Spalte der Liste angezeigt wird. |
| **Liste** |**> Schwellenwerte** |
| Schwellenwerte aktivieren |Wählen Sie diese Option, um Schwellenwerte zu aktivieren.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#thresholds). |

## <a name="information-part"></a>Komponente mit Informationen
Die Kopfzeile zeigt statischen Text und einen optionalen Link an.  Die Liste zeigt ein oder mehrere Elemente mit statischem Text und Titel an.

![Ansicht mit Informationen](media/log-analytics-view-designer/view-information.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| **Allgemein** | |
| Gruppentitel |Text, der oben auf der Kachel angezeigt wird. |
| Neue Gruppe |Wählen Sie diese Option aus, um in der Ansicht auf der Grundlage der aktuellen Ansicht eine neue Gruppe zu erstellen. |
| Farbe |Hintergrundfarbe der Kopfzeile. |
| **Kopfzeile** | |
| Image |Bilddatei, die in der Kopfzeile angezeigt wird. |
| Bezeichnung |Text, der in der Kopfzeile angezeigt wird. |
| **Kopfzeile** |**> Link** |
| Bezeichnung |Text des Links. |
| Url |URL für den Link. |
| **Informationselemente** | |
| Titel |Text, der für Titel der einzelnen Elemente angezeigt wird. |
| Inhalt |Text, der für die einzelnen Elemente angezeigt wird. |

## <a name="line-chart-callout-list-part"></a>Komponente mit Liniendiagramm, Legende und Liste
Die Kopfzeile zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im Verlauf und eine Legende mit einem zusammengefassten Wert an.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Ein Diagramm gibt dabei den relativen Wert einer numerischen Spalte oder die Änderung des Werts im Lauf der Zeit an.

![Ansicht mit Liniendiagramm, Legende und Liste](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| **Allgemein** | |
| Gruppentitel |Text, der oben auf der Kachel angezeigt wird. |
| Neue Gruppe |Wählen Sie diese Option aus, um in der Ansicht auf der Grundlage der aktuellen Ansicht eine neue Gruppe zu erstellen. |
| Symbol |Bilddatei, die neben dem Ergebnis in der Kopfzeile angezeigt wird. |
| Symbol verwenden |Wählen Sie diese Option aus, damit das Symbol angezeigt wird. |
| **Kopfzeile** | |
| Titel |Text, der oben in der Kopfzeile angezeigt wird. |
| Untertitel |Text, der unter dem Titel oben in der Kopfzeile angezeigt wird. |
| **Liniendiagramm** | |
| Abfragen |Abfrage, die für das Liniendiagramm ausgeführt wird.  Die erste Eigenschaft sollte ein Textwert und die zweite Eigenschaft ein numerischer Wert sein.  Dies ist normalerweise eine Abfrage, in der das Schlüsselwort für die **Maßeinheit** verwendet wird, um die Ergebnisse zusammenzufassen.  Wenn in der Abfrage das Schlüsselwort für das **Intervall** verwendet wird, wird für die X-Achse des Diagramms dieses Intervall verwendet.  Wenn die Abfrage das Schlüsselwort für das **Intervall** nicht enthält, werden stündliche Intervalle für die X-Achse verwendet. |
| **Liniendiagramm** |**> Legende** |
| Legendentitel |Text, der über dem Wert der Legende angezeigt wird. |
| Reihenname |Eigenschaftswert für die Reihe, die für den Wert der Legende verwendet wird.  Wenn keine Reihe angegeben wird, werden alle Datensätze der Abfrage verwendet. |
| Vorgang |Der Vorgang, der für die Werteigenschaft ausgeführt wird, um sie für die Legende zu einem einzelnen Wert zusammenzufassen.<br><br>– Durchschnitt: Durchschnitt der Werte von allen Datensätzen.<br>– Anzahl: Anzahl aller von der Abfrage zurückgegebenen Datensätze.<br>– Letztes Beispiel: Wert aus dem letzten Intervall im Diagramm.<br>– Max: Höchstwert aus den Intervallen im Diagramm.<br>– Min: Mindestwert aus den Intervallen im Diagramm.<br>– Summe: Summe der Werte von allen Datensätzen. |
| **Liniendiagramm** |**> Y-Achse** |
| Logarithmische Skala verwenden |Wählen Sie diese Option aus, um eine logarithmische Skala für die Y-Achse zu verwenden. |
| Units |Geben Sie die Einheiten für die von der Abfrage zurückgegebenen Werte an.  Diese Informationen werden verwendet, um Bezeichnungen im Diagramm anzuzeigen, die Werttypen angeben, und um optional Werte zu konvertieren.  Der Typ der Einheit gibt die Kategorie der Einheit an und definiert die verfügbaren Werte für den Typ in „Aktuelle Einheit“.  Bei Auswahl eines Werts in „Konvertieren in“ werden die numerischen Werte vom Typ in „Aktuelle Einheit“ in den Typ in „Konvertieren in“ konvertiert. |
| Benutzerdefinierte Bezeichnung |Text, der für die Y-Achse neben der Bezeichnung für den Typ der Einheit angezeigt wird.  Wenn keine Bezeichnung angegeben ist, wird nur der Typ der Einheit angezeigt. |
| **Liste** | |
| Abfragen |Abfrage, die für die Liste ausgeführt wird.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze wird angezeigt. |
| Diagramm ausblenden |Wählen Sie diese Option, um das Diagramm rechts neben der numerischen Spalte zu deaktivieren. |
| Sparklines aktivieren |Wählen Sie diese Option, um eine Sparkline anstatt eines horizontalen Balkens anzuzeigen.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#sparklines). |
| Farbe |Farbe der Balken oder Sparklines. |
| Vorgang |Vorgang, der für die Sparkline ausgeführt werden soll.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#sparklines). |
| Trennzeichen für Name und Wert |Ein Trennzeichen, um die Texteigenschaft in mehrere Werte zu zergliedern.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#name-value-separator). |
| Navigationsabfrage |Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#navigation-query). |
| **Liste** |**> Spaltentitel** |
| Name |Text, der oben in der ersten Spalte der Liste angezeigt wird. |
| Wert |Text, der oben in der zweiten Spalte der Liste angezeigt wird. |
| **Liste** |**> Schwellenwerte** |
| Schwellenwerte aktivieren |Wählen Sie diese Option, um Schwellenwerte zu aktivieren.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#thresholds). |

## <a name="line-chart-list-part"></a>Komponente mit Liniendiagramm und Liste
Die Kopfzeile zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im Verlauf an.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Ein Diagramm gibt dabei den relativen Wert einer numerischen Spalte oder die Änderung des Werts im Lauf der Zeit an.

![Ansicht mit Liniendiagramm und Liste](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| **Allgemein** | |
| Gruppentitel |Text, der oben auf der Kachel angezeigt wird. |
| Neue Gruppe |Wählen Sie diese Option aus, um in der Ansicht auf der Grundlage der aktuellen Ansicht eine neue Gruppe zu erstellen. |
| Symbol |Bilddatei, die neben dem Ergebnis in der Kopfzeile angezeigt wird. |
| Symbol verwenden |Wählen Sie diese Option aus, damit das Symbol angezeigt wird. |
| **Kopfzeile** | |
| Titel |Text, der oben in der Kopfzeile angezeigt wird. |
| Untertitel |Text, der unter dem Titel oben in der Kopfzeile angezeigt wird. |
| **Liniendiagramm** | |
| Abfragen |Abfrage, die für das Liniendiagramm ausgeführt wird.  Die erste Eigenschaft sollte ein Textwert und die zweite Eigenschaft ein numerischer Wert sein.  Dies ist normalerweise eine Abfrage, in der das Schlüsselwort für die **Maßeinheit** verwendet wird, um die Ergebnisse zusammenzufassen.  Wenn in der Abfrage das Schlüsselwort für das **Intervall** verwendet wird, wird für die X-Achse des Diagramms dieses Intervall verwendet.  Wenn die Abfrage das Schlüsselwort für das **Intervall** nicht enthält, werden stündliche Intervalle für die X-Achse verwendet. |
| **Liniendiagramm** |**> Y-Achse** |
| Logarithmische Skala verwenden |Wählen Sie diese Option aus, um eine logarithmische Skala für die Y-Achse zu verwenden. |
| Units |Geben Sie die Einheiten für die von der Abfrage zurückgegebenen Werte an.  Diese Informationen werden verwendet, um Bezeichnungen im Diagramm anzuzeigen, die Werttypen angeben, und um optional Werte zu konvertieren.  Der Typ der Einheit gibt die Kategorie der Einheit an und definiert die verfügbaren Werte für den Typ in „Aktuelle Einheit“.  Bei Auswahl eines Werts in „Konvertieren in“ werden die numerischen Werte vom Typ in „Aktuelle Einheit“ in den Typ in „Konvertieren in“ konvertiert. |
| Benutzerdefinierte Bezeichnung |Text, der für die Y-Achse neben der Bezeichnung für den Typ der Einheit angezeigt wird.  Wenn keine Bezeichnung angegeben ist, wird nur der Typ der Einheit angezeigt. |
| **Liste** | |
| Abfragen |Abfrage, die für die Liste ausgeführt wird.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze wird angezeigt. |
| Diagramm ausblenden |Wählen Sie diese Option, um das Diagramm rechts neben der numerischen Spalte zu deaktivieren. |
| Sparklines aktivieren |Wählen Sie diese Option, um eine Sparkline anstatt eines horizontalen Balkens anzuzeigen.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#sparklines). |
| Farbe |Farbe der Balken oder Sparklines. |
| Vorgang |Vorgang, der für die Sparkline ausgeführt werden soll.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#sparklines). |
| Trennzeichen für Name und Wert |Ein Trennzeichen, um die Texteigenschaft in mehrere Werte zu zergliedern.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#name-value-separator). |
| Navigationsabfrage |Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#navigation-query). |
| **Liste** |**> Spaltentitel** |
| Name |Text, der oben in der ersten Spalte der Liste angezeigt wird. |
| Wert |Text, der oben in der zweiten Spalte der Liste angezeigt wird. |
| **Liste** |**> Schwellenwerte** |
| Schwellenwerte aktivieren |Wählen Sie diese Option, um Schwellenwerte zu aktivieren.  Weitere Informationen finden Sie unter [Allgemeine Einstellungen](#thresholds). |

## <a name="stack-of-line-charts-part"></a>Komponente mit Liniendiagrammstapel
Zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im zeitlichen Verlauf an.

![Liniendiagrammstapel](media/log-analytics-view-designer/view-stack-line-charts.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| **Allgemein** | |
| Gruppentitel |Text, der oben auf der Kachel angezeigt wird. |
| Neue Gruppe |Wählen Sie diese Option aus, um in der Ansicht auf der Grundlage der aktuellen Ansicht eine neue Gruppe zu erstellen. |
| Symbol |Bilddatei, die neben dem Ergebnis in der Kopfzeile angezeigt wird. |
| **Diagramm 1<br>Diagramm 2<br>Diagramm 3** |**> Kopfzeile** |
| Titel |Text, der oben im Diagramm angezeigt wird. |
| Untertitel |Text, der unter dem Titel oben im Diagramm angezeigt wird. |
| **Diagramm 1<br>Diagramm 2<br>Diagramm 3** |**Liniendiagramm** |
| Abfragen |Abfrage, die für das Liniendiagramm ausgeführt wird.  Die erste Eigenschaft sollte ein Textwert und die zweite Eigenschaft ein numerischer Wert sein.  Dies ist normalerweise eine Abfrage, in der das Schlüsselwort für die **Maßeinheit** verwendet wird, um die Ergebnisse zusammenzufassen.  Wenn in der Abfrage das Schlüsselwort für das **Intervall** verwendet wird, wird für die X-Achse des Diagramms dieses Intervall verwendet.  Wenn die Abfrage das Schlüsselwort für das **Intervall** nicht enthält, werden stündliche Intervalle für die X-Achse verwendet. |
| **Diagramm** |**> Y-Achse** |
| Logarithmische Skala verwenden |Wählen Sie diese Option aus, um eine logarithmische Skala für die Y-Achse zu verwenden. |
| Units |Geben Sie die Einheiten für die von der Abfrage zurückgegebenen Werte an.  Diese Informationen werden verwendet, um Bezeichnungen im Diagramm anzuzeigen, die Werttypen angeben, und um optional Werte zu konvertieren.  Der Typ der Einheit gibt die Kategorie der Einheit an und definiert die verfügbaren Werte für den Typ in „Aktuelle Einheit“.  Bei Auswahl eines Werts in „Konvertieren in“ werden die numerischen Werte vom Typ in „Aktuelle Einheit“ in den Typ in „Konvertieren in“ konvertiert. |
| Benutzerdefinierte Bezeichnung |Text, der für die Y-Achse neben der Bezeichnung für den Typ der Einheit angezeigt wird.  Wenn keine Bezeichnung angegeben ist, wird nur der Typ der Einheit angezeigt. |

## <a name="common-settings"></a>Allgemeine Einstellungen
In den folgenden Abschnitten werden die Einstellungen beschrieben, die für verschiedene Visualisierungskomponenten gelten.

### <a name="a-namename-value-separatorname-value-separatora"></a><a name="name-value-separator">Trennzeichen für Name und Wert</a>
Ein Trennzeichen, um die Texteigenschaft aus einer Listenabfrage in mehrere Werte zu zergliedern.  Wenn Sie ein Trennzeichen angeben, können Sie Namen für die einzelnen Felder getrennt durch dieses Trennzeichen im Feld „Name“ bereitstellen.

Beispiel: Eine Eigenschaft namens *Standort* weist Werte wie *Redmond-Gebäude 41* und *Bellevue-Gebäude12* auf.  Sie könnten - als Trennzeichen für Name und Wert und *Ort-Gebäude* als Namen angeben.  Dadurch wird jeder Wert in die beiden Eigenschaften *Ort* und *Gebäude* zergliedert. 

### <a name="a-namenavigation-querynavigation-querya"></a><a name="navigation-query">Navigationsabfrage</a>
Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Verwenden Sie *{ausgewähltes Element}*, um die Syntax für das Element einzubinden, das der Benutzer ausgewählt hat.

Beispiel: Wenn die Abfrage eine Spalte namens *Computer* aufweist und die Navigationsabfrage *{ausgewähltes Element}* ist, wird eine Abfrage wie *Computer="MeinComputer"* ausgeführt, wenn der Benutzer einen Computer auswählt.  Wenn die Navigationsabfrage *Type=Event {ausgewähltes Element}* ist, wird *Type=Event Computer="MeinComputer"* ausgeführt.

### <a name="a-namesparklinessparklinesa"></a><a name="sparklines">Sparklines</a>
Eine Sparkline ist ein kleines Liniendiagramm, das den Wert eines Listeneintrags im Lauf der Zeit veranschaulicht.  Bei Visualisierungskomponenten mit einer Liste können Sie auswählen, ob ein horizontaler Balken mit dem relativen Wert einer numerischen Spalte oder eine Sparkline mit dem Wert im Verlauf der Zeit angezeigt werden soll.

In der folgenden Tabelle sind Einstellungen für Sparklines beschrieben.

| Einstellung | Beschreibung |
|:--- |:--- |
| Sparklines aktivieren |Wählen Sie diese Option, um eine Sparkline anstatt eines horizontalen Balkens anzuzeigen. |
| Vorgang |Wenn Sparklines aktiviert sind, ist dies der Vorgang, der für jede Eigenschaft in der Liste ausgeführt wird, um die Werte für die Sparkline zu berechnen.<br><br>– Letztes Beispiel: Letzter Wert für die Reihe im Zeitintervall.<br>– Max: Höchstwert für die Reihe im Zeitintervall.<br>– Min: Mindestwert für die Reihe im Zeitintervall.<br>– Summe: Summe der Werte für die Reihe im Zeitintervall.<br>– Zusammenfassung: Verwendet den gleichen Measure-Befehl wie die Abfrage in der Kopfzeile. |

### <a name="a-namethresholdsthresholdsa"></a><a name="thresholds">Schwellenwerte</a>
Schwellenwerte ermöglichen die Anzeige eines farbigen Symbols neben jedem Element einer Liste, damit Sie Elemente, die einen bestimmten Wert überschreiten oder innerhalb eines bestimmten Bereichs liegen, auf einen Blick erkennen können.  Sie können beispielsweise folgende Farbgebung festlegen: ein grünes Symbol für Elemente mit akzeptablem Wert, ein gelbes Symbol für Elemente mit einem Wert in einem Bereich, der auf eine Warnung hindeutet, und ein rotes Symbol für Elemente, die einen Fehlerwert überschreiten.

Wenn Sie Schwellenwerte für eine Komponente aktivieren, müssen Sie mindestens einen Schwellenwert angeben.  Wenn der Wert eines Elements größer ist als ein Schwellenwert und kleiner als der nächste Schwellenwert, wird die entsprechende Farbe verwendet.  Wenn das Element größer ist als der höchste Schwellenwert, wird die entsprechende Farbe verwendet.   

Jeder Schwellenwertsatz verfügt über einen Schwellenwert mit dem Wert **Standard**.  Wenn keine anderen Werte überschritten werden, wird die entsprechende Farbe verwendet.  Sie können Schwellenwerte hinzufügen oder entfernen, indem Sie auf die Schaltfläche **+** oder **x** klicken.

In der folgenden Tabelle sind Einstellungen für Schwellenwerte beschrieben.

| Einstellung | Beschreibung |
|:--- |:--- |
| Schwellenwerte aktivieren |Wählen Sie diese Option, um links neben jedem Wert ein Farbsymbol anzuzeigen, das den Integritätsstatus des Werts in Relation zu den festgelegten Schwellenwerten angibt. |
| Name |Name zur Identifikation des Schwellenwerts. |
| Schwellenwert |Der Wert für den Schwellenwert.  Die Farbe der Integrität für jedes Listenelement ist auf die Farbe des höchsten Schwellenwerts festgelegt, den der Wert des Listenelements überschritten hat.  Es gibt einen Standardschwellenwert, dessen Farbe verwendet wird, wenn keine Schwellenwerte überschritten wurden. |
| Farbe |Die Farbe für den Schwellenwert. |

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zur Unterstützung der Abfragen in Visualisierungskomponenten.




<!--HONumber=Nov16_HO3-->


