---
title: Ansicht-Designer in Log Analytics | Microsoft Docs
description: Mit dem Ansicht-Designer in Log Analytics können Sie benutzerdefinierte Ansichten in der OMS-Konsole erstellen, die unterschiedliche Visualisierungen von Daten im OMS-Repository enthalten. Dieser Artikel enthält eine Referenz der Einstellungen für die einzelnen Visualisierungskomponenten, die zur Verwendung in Ihren benutzerdefinierten Ansichten verfügbar sind.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: ''

ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: bwren

---
# <a name="log-analytics-view-designer-visualization-part-reference"></a>Referenz der Visualisierungskomponenten im Ansicht-Designer in Log Analytics
Mit dem Ansicht-Designer in Log Analytics können Sie benutzerdefinierte Ansichten in der OMS-Konsole erstellen, die unterschiedliche Visualisierungen von Daten aus dem OMS-Repository enthalten. Dieser Artikel enthält eine Referenz der Einstellungen für die einzelnen Visualisierungskomponenten, die zur Verwendung in Ihren benutzerdefinierten Ansichten verfügbar sind.

Weitere verfügbare Artikel für den Ansicht-Designer:

* [Ansicht-Designer](log-analytics-view-designer.md): Übersicht über den Ansicht-Designer und die Verfahren zum Erstellen und Bearbeiten von benutzerdefinierten Ansichten.
* [Referenz der Kacheln](log-analytics-view-designer-tiles.md): Referenz der Einstellungen für die einzelnen Kacheln, die zur Verwendung in Ihren benutzerdefinierten Ansichten verfügbar sind. 

Die folgende Tabelle beschreibt die verschiedenen Typen von Kacheln, die im Ansicht-Designer verfügbar sind.  In den folgenden Abschnitten werden jeder Kacheltyp im Detail und dessen Eigenschaften beschrieben.

| Ansichtstyp | Beschreibung |
|:--- |:--- |
| [Liste der Abfragen](#list-of-queries-part) |Zeigt eine Liste der Protokollsuchabfragen an.  Die Benutzer kann auf die einzelnen Abfragen klicken, um die Ergebnisse anzuzeigen.  Die Ansicht enthält standardmäßig eine einzelne Abfrage, und Sie können auf **+ Abfrage** klicken, um zusätzliche Abfragen hinzuzufügen. |
| [Zahl und Liste](#number-amp-list-part) |Die Kopfzeile weist eine einzelne Zahl auf, die die Anzahl der Datensätze aus einer Protokollsuchabfrage angibt.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Horizontale Balken geben dabei den relativen Wert einer numerischen Spalte an. |
| [Zwei Zahlen und Liste](#two-numbers-amp-list-part) |Die Kopfzeile weist zwei Zahlen auf, die die Anzahl der Datensätze aus getrennten Protokollsuchabfragen angeben.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Horizontale Balken geben dabei den relativen Wert einer numerischen Spalte an. |
| [Ring und Liste](#donut-amp-list-part) |Die Kopfzeile zeigt eine einzelne Zahl an, die eine Spalte mit Werten in einer Protokollabfrage zusammenfasst.  Der Ring stellt die Ergebnisse der ersten drei Datensätze grafisch dar. |
| [Zwei Zeitachsen und Liste](#two-timelines-amp-list-part) |Die Kopfzeile zeigt die Ergebnisse von zwei Protokollabfragen im Verlauf als Säulendiagramme an. In einer Legende wird eine einzelne Zahl dargestellt, die eine Wertespalte in einer Protokollabfrage zusammenfasst.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Horizontale Balken geben dabei den relativen Wert einer numerischen Spalte an. |
| [Informationen](#information-part) |Die Kopfzeile zeigt statischen Text und einen optionalen Link an.  Die Liste zeigt ein oder mehrere Elemente mit statischem Text und Titel an. |
| [Liniendiagramm, Legende und Liste](#line-chart-callout-amp-list-part) |Die Kopfzeile zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im Verlauf und eine Legende mit einem zusammengefassten Wert an.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Horizontale Balken geben dabei den relativen Wert einer numerischen Spalte an. |
| [Liniendiagramm und Liste](#line-chart-amp-list-part) |Die Kopfzeile zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im Verlauf an.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Horizontale Balken geben dabei den relativen Wert einer numerischen Spalte an. |
| [Liniendiagramm und Sparkline](#line-chart-amp-sparklines-part) |Die Kopfzeile zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im Verlauf an.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Eine Sparkline visualisiert dabei den Wert einer numerischen Spalte im ausgewählten Zeitrahmen. |

## <a name="list-of-queries-part"></a>Komponente mit der Liste der Abfragen
Zeigt eine Liste der Protokollsuchabfragen an.  Die Benutzer kann auf die einzelnen Abfragen klicken, um die Ergebnisse anzuzeigen.  

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

## <a name="number-&-list-part"></a>Komponente mit Zahl und Liste
Die Kopfzeile weist eine einzelne Zahl auf, die die Anzahl der Datensätze aus einer Protokollsuchabfrage angibt.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Horizontale Balken geben dabei den relativen Wert einer numerischen Spalte an.

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
| Farbe |Die Farbe der Balken. |
| Trennzeichen für Name und Wert |Ein Trennzeichen, um die Texteigenschaft in mehrere Werte zu zergliedern.  Wenn Sie ein Trennzeichen angeben, können Sie Namen für die einzelnen Felder getrennt durch dieses Trennzeichen im Feld „Name“ bereitstellen.<br><br>Beispiel: Eine Eigenschaft namens Standort weist Werte wie *Redmond-Gebäude 41* und *Bellevue-Gebäude12* auf.  Sie könnten - als Trennzeichen für Name und Wert und *Ort-Gebäude* als Namen angeben.  Dadurch wird jeder Wert in die beiden Eigenschaften *Ort* und *Gebäude* zergliedert. |
| Navigationsabfrage |Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Verwenden Sie {ausgewähltes Element}, um die Syntax für das Element einzubinden, das der Benutzer ausgewählt hat. <br><br>Beispiel: Wenn die Abfrage eine Spalte namens Computer aufweist und die Navigationsabfrage {ausgewähltes Element} ist, wird eine Abfrage wie Computer="MeinComputer" ausgeführt, wenn der Benutzer einen Computer auswählt.  Wenn die Navigationsabfrage Type=Event {ausgewähltes Element} ist, wird Type=Event Computer="MeinComputer" ausgeführt. |
| **Liste** |**> Spaltentitel** |
| Name |Text, der oben in der ersten Spalte der Liste angezeigt wird. |
| Wert |Text, der oben in der zweiten Spalte der Liste angezeigt wird. |

## <a name="two-numbers-&-list-part"></a>Komponente mit zwei Zahlen und Liste
Die Kopfzeile weist zwei Zahlen auf, die die Anzahl der Datensätze aus getrennten Protokollsuchabfragen angeben.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Horizontale Balken geben dabei den relativen Wert einer numerischen Spalte an.

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
| **Zweiter Titel** | |
| Legende |Text, der oben in der Kopfzeile angezeigt wird. |
| Abfragen |Abfrage, die für die Kopfzeile ausgeführt wird.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze wird angezeigt. |
| **Liste** | |
| Abfragen |Abfrage, die für die Liste ausgeführt wird.  Die ersten beiden Eigenschaften für die ersten zehn Datensätze in den Ergebnissen werden angezeigt.  Die erste Eigenschaft sollte ein Textwert und die zweite Eigenschaft ein numerischer Wert sein.  Balken werden automatisch basierend auf dem relativen Wert der numerischen Spalte erstellt.<br><br>Verwenden Sie den Befehl zum Sortieren in der Abfrage, um die Datensätze in der Liste zu sortieren.  Der Benutzer kann auf „Alle anzeigen“ klicken, um die Abfrage auszuführen und alle Datensätze zurückzugeben. |
| Farbe |Die Farbe der Balken. |
| Trennzeichen für Name und Wert |Ein Trennzeichen, um die Texteigenschaft in mehrere Werte zu zergliedern.  Wenn Sie ein Trennzeichen angeben, können Sie Namen für die einzelnen Felder getrennt durch dieses Trennzeichen im Feld „Name“ bereitstellen.<br><br>Beispiel: Eine Eigenschaft namens Standort weist Werte wie „Redmond-Gebäude 41“ und „Bellevue-Gebäude12“ auf.  Sie könnten - als Trennzeichen für Name und Wert und „Ort-Gebäude“ als Namen angeben.  Dadurch wird jeder Wert in die beiden Eigenschaften „Ort“ und „Gebäude“ zergliedert. |
| Navigationsabfrage |Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Verwenden Sie {ausgewähltes Element}, um die Syntax für das Element einzubinden, das der Benutzer ausgewählt hat. <br><br>Beispiel: Wenn die Abfrage eine Spalte namens Computer aufweist und die Navigationsabfrage {ausgewähltes Element} ist, wird eine Abfrage wie Computer="MeinComputer" ausgeführt, wenn der Benutzer einen Computer auswählt.  Wenn die Navigationsabfrage Type=Event {ausgewähltes Element} ist, wird Type=Event Computer="MeinComputer" ausgeführt. |
| **Liste** |**> Spaltentitel** |
| Name |Text, der oben in der ersten Spalte der Liste angezeigt wird. |
| Wert |Text, der oben in der zweiten Spalte der Liste angezeigt wird. |

## <a name="donut-&-list-part"></a>Komponente mit Ring und Liste
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
| **Liste** | |
| Abfragen |Abfrage, die für die Liste ausgeführt wird.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze wird angezeigt. |
| Farbe |Die Farbe der Balken. |
| Trennzeichen für Name und Wert |Ein Trennzeichen, um die Texteigenschaft in mehrere Werte zu zergliedern.  Wenn Sie ein Trennzeichen angeben, können Sie Namen für die einzelnen Felder getrennt durch dieses Trennzeichen im Feld „Name“ bereitstellen.<br><br>Beispiel: Eine Eigenschaft namens Standort weist Werte wie *Redmond-Gebäude 41* und *Bellevue-Gebäude12* auf.  Sie könnten - als Trennzeichen für Name und Wert und *Ort-Gebäude* als Namen angeben.  Dadurch wird jeder Wert in die beiden Eigenschaften *Ort* und *Gebäude* zergliedert. |
| Navigationsabfrage |Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Verwenden Sie *{ausgewähltes Element}*, um die Syntax für das Element einzubinden, das der Benutzer ausgewählt hat.<br><br>Beispiel: Wenn die Abfrage eine Spalte namens *Computer* aufweist und die Navigationsabfrage *{ausgewähltes Element}* ist, wird eine Abfrage wie *Computer="MeinComputer"* ausgeführt, wenn der Benutzer einen Computer auswählt.  Wenn die Navigationsabfrage *Type=Event {ausgewähltes Element}* ist, wird *Type=Event Computer="MeinComputer"* ausgeführt. |
| **Liste** |**> Spaltentitel** |
| Name |Text, der oben in der ersten Spalte der Liste angezeigt wird. |
| Wert |Text, der oben in der zweiten Spalte der Liste angezeigt wird. |

## <a name="two-timelines-&-list-part"></a>Komponente mit zwei Zeitachsen und Liste
Die Kopfzeile zeigt die Ergebnisse von zwei Protokollabfragen im Verlauf als Säulendiagramme an. In einer Legende wird eine einzelne Zahl dargestellt, die eine Wertespalte in einer Protokollabfrage zusammenfasst.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Horizontale Balken geben dabei den relativen Wert einer numerischen Spalte an.   

![Ansicht mit zwei Zeitachsen und Liste](media/log-analytics-view-designer/view-two-timelines-list.png)

| Einstellung | Beschreibung |
|:--- |:--- |
| **Allgemein** | |
| Gruppentitel |Text, der oben auf der Kachel angezeigt wird. |
| Neue Gruppe |Wählen Sie diese Option aus, um in der Ansicht auf der Grundlage der aktuellen Ansicht eine neue Gruppe zu erstellen. |
| Symbol |Bilddatei, die neben dem Ergebnis in der Kopfzeile angezeigt wird. |
| Symbol verwenden |Wählen Sie diese Option aus, damit das Symbol angezeigt wird. |
| **Erstes Diagramm** | |
| Legende |Text, der unter der Legende für die erste Reihe angezeigt wird. |
| Farbe |Farbe, die für die Säulen in der ersten Reihe verwendet wird. |
| Abfragen |Abfrage, die für die erste Reihe ausgeführt wird.  Die Angabe der Anzahl von Datensätzen in jedem Zeitintervall wird durch die Säulen im Diagramm dargestellt. |
| Vorgang |Der Vorgang, der für die Werteigenschaft ausgeführt wird, um sie für die Legende zu einem einzelnen Wert zusammenzufassen.<br><br>– Durchschnitt: Durchschnitt der Werte von allen Datensätzen.<br>– Anzahl: Anzahl aller von der Abfrage zurückgegebenen Datensätze.<br>– Erstes Beispiel: Wert aus dem ersten Intervall im Diagramm.<br>– Letztes Beispiel: Wert aus dem letzten Intervall im Diagramm.<br>– Max: Höchstwert aus den Intervallen im Diagramm. |
| Abfragen |Abfrage, die für das Liniendiagramm ausgeführt wird.  Die erste Eigenschaft sollte ein Textwert und die zweite Eigenschaft ein numerischer Wert sein.  Dies ist normalerweise eine Abfrage, in der das Schlüsselwort für die **Maßeinheit** verwendet wird, um die Ergebnisse zusammenzufassen.  Wenn in der Abfrage das Schlüsselwort für das **Intervall** verwendet wird, wird für die X-Achse des Diagramms dieses Intervall verwendet.  Wenn die Abfrage das Schlüsselwort für das **Intervall** nicht enthält, werden stündliche Intervalle für die X-Achse verwendet. |
| **Liniendiagramm** |**> Y-Achse** |
| Logarithmische Skala verwenden |Wählen Sie diese Option aus, um eine logarithmische Skala für die Y-Achse zu verwenden. |
| Units |Geben Sie die Einheiten für die von der Abfrage zurückgegebenen Werte an.  Diese Informationen werden verwendet, um Bezeichnungen im Diagramm anzuzeigen, die Werttypen angeben, und um optional Werte zu konvertieren.  Der Typ der Einheit gibt die Kategorie der Einheit an und definiert die verfügbaren Werte für den Typ in „Aktuelle Einheit“.  Bei Auswahl eines Werts in „Konvertieren in“ werden die numerischen Werte vom Typ in „Aktuelle Einheit“ in den Typ in „Konvertieren in“ konvertiert. |
| Benutzerdefinierte Bezeichnung |Text, der für die Y-Achse neben der Bezeichnung für den Typ der Einheit angezeigt wird.  Wenn keine Bezeichnung angegeben ist, wird nur der Typ der Einheit angezeigt. |
| **Liste** | |
| Abfragen |Abfrage, die für die Liste ausgeführt wird.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze wird angezeigt. |
| Farbe |Die Farbe der Balken. |
| Trennzeichen für Name und Wert |Ein Trennzeichen, um die Texteigenschaft in mehrere Werte zu zergliedern.  Wenn Sie ein Trennzeichen angeben, können Sie Namen für die einzelnen Felder getrennt durch dieses Trennzeichen im Feld „Name“ bereitstellen.<br><br>Beispiel: Eine Eigenschaft namens Standort weist Werte wie *Redmond-Gebäude 41* und *Bellevue-Gebäude12* auf.  Sie könnten - als Trennzeichen für Name und Wert und *Ort-Gebäude* als Namen angeben.  Dadurch wird jeder Wert in die beiden Eigenschaften *Ort* und *Gebäude* zergliedert. |
| Navigationsabfrage |Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Verwenden Sie *{ausgewähltes Element}*, um die Syntax für das Element einzubinden, das der Benutzer ausgewählt hat.<br><br>Beispiel: Wenn die Abfrage eine Spalte namens *Computer* aufweist und die Navigationsabfrage *{ausgewähltes Element}* ist, wird eine Abfrage wie *Computer="MeinComputer"* ausgeführt, wenn der Benutzer einen Computer auswählt.  Wenn die Navigationsabfrage *Type=Event {ausgewähltes Element}* ist, wird *Type=Event Computer="MeinComputer"* ausgeführt. |
| **Liste** |**> Spaltentitel** |
| Name |Text, der oben in der ersten Spalte der Liste angezeigt wird. |
| Wert |Text, der oben in der zweiten Spalte der Liste angezeigt wird. |

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

## <a name="line-chart,-callout,-&-list-part"></a>Komponente mit Liniendiagramm, Legende und Liste
Die Kopfzeile zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im Verlauf und eine Legende mit einem zusammengefassten Wert an.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Horizontale Balken geben dabei den relativen Wert einer numerischen Spalte an.

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
| Farbe |Die Farbe der Balken. |
| Trennzeichen für Name und Wert |Ein Trennzeichen, um die Texteigenschaft in mehrere Werte zu zergliedern.  Wenn Sie ein Trennzeichen angeben, können Sie Namen für die einzelnen Felder getrennt durch dieses Trennzeichen im Feld „Name“ bereitstellen.<br><br>Beispiel: Eine Eigenschaft namens Standort weist Werte wie *Redmond-Gebäude 41* und *Bellevue-Gebäude12* auf.  Sie könnten - als Trennzeichen für Name und Wert und *Ort-Gebäude* als Namen angeben.  Dadurch wird jeder Wert in die beiden Eigenschaften *Ort* und *Gebäude* zergliedert. |
| Navigationsabfrage |Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Verwenden Sie *{ausgewähltes Element}*, um die Syntax für das Element einzubinden, das der Benutzer ausgewählt hat.<br><br>Beispiel: Wenn die Abfrage eine Spalte namens *Computer* aufweist und die Navigationsabfrage *{ausgewähltes Element}* ist, wird eine Abfrage wie *Computer="MeinComputer"* ausgeführt, wenn der Benutzer einen Computer auswählt.  Wenn die Navigationsabfrage *Type=Event {ausgewähltes Element}* ist, wird *Type=Event Computer="MeinComputer"* ausgeführt. |
| **Liste** |**> Spaltentitel** |
| Name |Text, der oben in der ersten Spalte der Liste angezeigt wird. |
| Wert |Text, der oben in der zweiten Spalte der Liste angezeigt wird. |

## <a name="line-chart-&-list-part"></a>Komponente mit Liniendiagramm und Liste
Die Kopfzeile zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im Verlauf an.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Horizontale Balken geben dabei den relativen Wert einer numerischen Spalte an.

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
| Farbe |Die Farbe der Balken. |
| Trennzeichen für Name und Wert |Ein Trennzeichen, um die Texteigenschaft in mehrere Werte zu zergliedern.  Wenn Sie ein Trennzeichen angeben, können Sie Namen für die einzelnen Felder getrennt durch dieses Trennzeichen im Feld „Name“ bereitstellen.<br><br>Beispiel: Eine Eigenschaft namens Standort weist Werte wie *Redmond-Gebäude 41* und *Bellevue-Gebäude12* auf.  Sie könnten - als Trennzeichen für Name und Wert und *Ort-Gebäude* als Namen angeben.  Dadurch wird jeder Wert in die beiden Eigenschaften *Ort* und *Gebäude* zergliedert. |
| Navigationsabfrage |Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Verwenden Sie *{ausgewähltes Element}*, um die Syntax für das Element einzubinden, das der Benutzer ausgewählt hat.<br><br>Beispiel: Wenn die Abfrage eine Spalte namens *Computer* aufweist und die Navigationsabfrage *{ausgewähltes Element}* ist, wird eine Abfrage wie *Computer="MeinComputer"* ausgeführt, wenn der Benutzer einen Computer auswählt.  Wenn die Navigationsabfrage *Type=Event {ausgewähltes Element}* ist, wird *Type=Event Computer="MeinComputer"* ausgeführt. |
| **Liste** |**> Spaltentitel** |
| Name |Text, der oben in der ersten Spalte der Liste angezeigt wird. |
| Wert |Text, der oben in der zweiten Spalte der Liste angezeigt wird. |

## <a name="line-chart-&-sparklines-part"></a>Komponente mit Liniendiagramm und Sparklines
Die Kopfzeile zeigt ein Liniendiagramm mit mehreren Reihen aus einer Protokollabfrage im Verlauf an.  Die Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage an. Eine Sparkline visualisiert dabei den Wert einer numerischen Spalte im ausgewählten Zeitrahmen.

![Liniendiagramm und Sparklines](media/log-analytics-view-designer/view-line-chart-sparklines.png)

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
| Vorgang |Der Vorgang, der für jede Eigenschaft in der Liste ausgeführt wird, um die Werte für die Sparkline zu berechnen.<br><br>– Letztes Beispiel: Letzter Wert für die Reihe im Zeitintervall.<br>– Max: Höchstwert für die Reihe im Zeitintervall.<br>– Min: Mindestwert für die Reihe im Zeitintervall.<br>– Summe: Summe der Werte für die Reihe im Zeitintervall.<br>– Zusammenfassung: Verwendet den gleichen Measure-Befehl wie die Abfrage in der Kopfzeile. |
| Navigationsabfrage |Die Abfrage, die ausgeführt wird, wenn der Benutzer ein Element in der Liste auswählt.  Verwenden Sie *{ausgewähltes Element}*, um die Syntax für das Element einzubinden, das der Benutzer ausgewählt hat.<br><br>Beispiel: Wenn die Abfrage eine Spalte namens *Computer* aufweist und die Navigationsabfrage *{ausgewähltes Element}* ist, wird eine Abfrage wie *Computer="MeinComputer"* ausgeführt, wenn der Benutzer einen Computer auswählt.  Wenn die Navigationsabfrage *Type=Event {ausgewähltes Element}* ist, wird *Type=Event Computer="MeinComputer"* ausgeführt. |
| **Liste** |**> Spaltentitel** |
| Name |Text, der oben in der ersten Spalte der Liste angezeigt wird. |
| Wert |Text, der oben in der zweiten Spalte der Liste angezeigt wird. |

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zur Unterstützung der Abfragen in Visualisierungskomponenten.

<!--HONumber=Oct16_HO2-->


