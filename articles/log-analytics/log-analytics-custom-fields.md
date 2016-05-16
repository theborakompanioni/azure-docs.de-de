<properties
   pageTitle="Benutzerdefinierte Felder in Log Analytics | Microsoft Azure"
   description="Mit dem Log Analytics-Feature „Benutzerdefinierte Felder“ können Sie auf der Grundlage von OMS-Daten eigene durchsuchbare Felder erstellen, um die Eigenschaften gesammelter Datensätze zu erweitern. Dieser Artikel beschreibt die Erstellung eines benutzerdefinierten Felds und enthält eine ausführliche exemplarische Vorgehensweise mit einem Beispielereignis."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/28/2016"
   ms.author="bwren" />

# Benutzerdefinierte Felder in Log Analytics

Mit dem Feature **Benutzerdefinierte Felder** von Log Analytics können Sie vorhandene Datensätze im OMS-Repository mit eigenen durchsuchbaren Feldern erweitern. Benutzerdefinierte Felder werden automatisch auf der Grundlage von Daten aufgefüllt, die aus anderen Eigenschaften im gleichen Datensatz extrahiert wurden.

![Übersicht über benutzerdefinierte Felder](media/log-analytics-custom-fields/overview.png)

Der folgende Beispieldatensatz enthält beispielsweise weitere hilfreiche Daten in der Ereignisbeschreibung. Die Extraktion dieser Daten in separate Eigenschaften ermöglicht Aktionen wie Sortieren und Filtern.

![Schaltfläche für die Protokollsuche](media/log-analytics-custom-fields/sample-extract.png)

>[AZURE.NOTE] In der Vorschauversion können Sie in Ihrem Arbeitsbereich maximal 100 benutzerdefinierte Felder verwenden. Diese Obergrenze wird erhöht, wenn das Feature allgemein verfügbar wird.

## Erstellen eines benutzerdefinierten Felds

Wenn Sie ein benutzerdefiniertes Feld erstellen, muss Log Analytics wissen, mit welchen Daten dessen Wert aufgefüllt werden soll. Zur schnellen Ermittlung dieser Daten kommt eine von Microsoft Research entwickelte Technologie namens FlashExtract zum Einsatz. Die zu extrahierenden Daten werden dabei von Log Analytics anhand von bereitgestellten Beispielen ermittelt, sodass Sie keine expliziten Anweisungen angeben müssen.

In den folgenden Abschnitten wird die Vorgehensweise zum Erstellen eines benutzerdefinierten Felds erläutert. Am Ende dieses Artikels befindet sich eine exemplarische Vorgehensweise für eine Beispielextraktion.

> [HINWEIS] Das benutzerdefinierte Feld wird aufgefüllt, wenn dem OMS-Datenspeicher Datensätze hinzugefügt werden, die den angegebenen Kriterien entsprechen. Das Feld ist also nur für Datensätze verfügbar, die nach der Erstellung des benutzerdefinierten Felds gesammelt werden. Das benutzerdefinierte Feld wird nicht zu Datensätzen hinzugefügt, die zum Zeitpunkt der Felderstellung bereits im Datenspeicher vorhanden sind.

### Schritt 1: Angeben der Datensätze, die über das benutzerdefinierte Feld verfügen sollen
Als Erstes müssen die Datensätze angegeben werden, die über das benutzerdefinierte Feld verfügen sollen. Hierzu führen Sie zunächst eine [Standard-Protokollsuche](log-analytics-log-searches.md) durch wählen dann einen Datensatz aus, der Log Analytics als Modell für die Ermittlung dient. Wenn Sie angeben, dass Sie Daten in ein benutzerdefiniertes Feld extrahieren möchten, wird der Feldextraktions-Assistent geöffnet, in dem Sie die Kriterien überprüfen und anpassen können.

2. Wechseln Sie zur Protokollsuche, und verwenden Sie eine [Abfrage zum Abrufen der Datensätze](log-analytics-log-searches.md), die über das benutzerdefinierte Feld verfügen sollen.
2. Wählen Sie einen Datensatz aus, der Log Analytics als Modell für die Extraktion von Daten zur Auffüllung des benutzerdefinierten Felds dienen soll. Nachdem Sie die Daten angegeben haben, die aus diesem Datensatz extrahiert werden sollen, ermittelt Log Analytics auf der Grundlage dieser Informationen die Logik, mit der das benutzerdefinierte Felds für alle ähnlichen Datensätze aufgefüllt werden kann.
3. Klicken Sie auf die Schaltfläche links neben einer beliebigen Texteigenschaft des Datensatzes, und wählen Sie **Extract fields from** (Felder extrahieren aus).
4. Der Feldextraktions-Assistent wird geöffnet, und der ausgewählte Datensatz wird in der Spalte **Main Example** (Hauptbeispiel) angezeigt. Das benutzerdefinierte Feld wird für Datensätze mit den gleichen Werten in den ausgewählten Eigenschaften definiert.  
5. Falls die Auswahl noch nicht ganz Ihren Vorstellungen entspricht, können Sie weitere Felder auswählen, um die Kriterien einzugrenzen. Wenn Sie die Feldwerte für die Kriterien ändern möchten, müssen Sie den Vorgang abbrechen und einen anderen Datensatz auswählen, der den gewünschten Kriterien entspricht.

### Schritt 2: Ausführen der ersten Extraktion
Nach Angabe der Datensätze, die über das benutzerdefinierte Feld verfügen sollen, müssen die zu extrahierenden Daten angegeben werden. Auf der Grundlage dieser Informationen ermittelt Log Analytics dann ähnliche Muster in ähnlichen Datensätzen. Im nächsten Schritt können Sie die Ergebnisse überprüfen und weitere Details angeben, die Log Analytics bei der Analyse berücksichtigen soll.

1. Markieren Sie im Beispieldatensatz den Text, mit dem das benutzerdefinierte Feld aufgefüllt werden soll. Daraufhin erscheint ein Dialogfeld, in dem Sie einen Namen für das Feld angeben und die erste Extraktion durchführen können. Die Zeichen **\_CF** werden automatisch angefügt.
2. Klicken Sie auf **Extrahieren**, um eine Analyse der gesammelten Datensätze durchzuführen.  
3. In den Abschnitten **Zusammenfassung** und **Suchergebnisse** werden die Ergebnisse der Extraktion zur Überprüfung angezeigt. **Zusammenfassung** enthält die Kriterien, die zur Ermittlung der Datensätze verwendet wurden, sowie die Anzahl der einzelnen ermittelten Datenwerte. **Suchergebnisse** enthält eine detaillierte Liste mit Datensätzen, die den Kriterien entsprechen.


### Schritt 3: Überprüfen der Extraktion und Erstellen des benutzerdefinierten Felds

Im Anschluss an die erste Extraktion zeigt Log Analytics die Ergebnisse an, die auf der Grundlage bereits gesammelter Daten ermittelt wurden. Wenn die Ergebnisse Ihren Vorstellungen entsprechen, können Sie das benutzerdefinierte Feld ohne weitere Schritte erstellen. Andernfalls können Sie die Ergebnisse optimieren, damit Log Analytics seine Logik verbessern kann.

2.	Sollte die erste Extraktion falsche Werte enthalten, klicken Sie neben einem falschen Datensatz auf das Bearbeitungssymbol, und wählen Sie **Modify this highlight** (Diese Markierung ändern) aus, um die Auswahl zu ändern.
3.	Der Eintrag wird in den Abschnitt **Additional examples** (Weitere Beispiele) kopiert, der sich unter dem Abschnitt **Main Example** (Hauptbeispiel) befindet. Hier können Sie die Markierung anpassen, um Log Analytics mitzuteilen, wie die Auswahl hätte aussehen sollen.
4.	Klicken Sie auf **Extrahieren**, um alle vorhandenen Datensätze auf der Grundlage dieser neuen Informationen zu untersuchen. Dadurch ändern sich unter Umständen auch die Ergebnisse anderer Datensätze.
5.	Nehmen Sie weitere Korrekturen vor, bis für alle Datensätze in der Extraktion genau die Daten angeben werden, mit denen das neue benutzerdefinierte Feld aufgefüllt werden soll.
6. Klicken Sie auf **Save Extract** (Extraktion speichern), wenn Sie mit den Ergebnissen zufrieden sind. Das benutzerdefinierte Feld ist nun zwar definiert, wird aber noch keinen Datensätzen hinzugefügt.
7.	Warten Sie, bis neue, den angegebenen Kriterien entsprechende Datensätze gesammelt wurden, und führen Sie die Protokollsuche dann erneut aus. Die neuen Datensätze verfügen nun über das benutzerdefinierte Feld.
8.	Das benutzerdefinierte Feld kann wie jede andere Datensatzeigenschaft verwendet werden. Sie können damit Daten aggregieren und gruppieren und sogar neue Insights generieren.


## Anzeigen benutzerdefinierter Felder
Über die Kachel **Einstellungen** des OMS-Dashboards können Sie eine Liste mit allen benutzerdefinierten Feldern in Ihrer Verwaltungsgruppe anzeigen. Wählen Sie **Daten** und anschließend **Benutzerdefinierte Felder** aus, um eine Liste mit allen benutzerdefinierten Feldern in Ihrem Arbeitsbereich anzuzeigen.

![Benutzerdefinierte Felder](media/log-analytics-custom-fields/list.png)

## Entfernen eines benutzerdefinierten Felds
Benutzerdefinierte Felder können auf zwei Arten entfernt werden: Sie können in der weiter oben beschriebenen Liste für jedes Feld die Option **Entfernen** verwenden. Alternativ können Sie einen Datensatz abrufen und auf die Schaltfläche links neben dem Feld klicken. Das Menü enthält eine Option zum Entfernen des benutzerdefinierten Felds.

## Exemplarische Vorgehensweise

Der folgende Abschnitt enthält ein vollständiges Beispiel für die Erstellung eines benutzerdefinierten Felds. In diesem Beispiel wird in Windows-Ereignissen, die auf eine Zustandsänderung bei einem Dienst hindeuten, der Dienstname extrahiert. Als Grundlage dienen Ereignisse, die auf Windows-Computern vom Dienststeuerungs-Manager im Systemprotokoll erstellt werden. Wenn Sie dieses Beispiel nachvollziehen möchten, müssen Sie [Informationsereignisse für das Systemprotokoll sammeln](log-analytics-data-sources-windows-events.md).

Wir geben die folgende Abfrage ein, die alle Ereignisse des Dienststeuerungs-Managers mit der Ereignis-ID 7036 zurückgibt. (Dieses Ereignis gibt an, dass ein Dienst gestartet oder beendet wird.)

![Abfrage](media/log-analytics-custom-fields/query.png)

Anschließend wählen wir alle Datensätze mit der Ereignis-ID 7036 aus.

![Quelldatensatz](media/log-analytics-custom-fields/source-record.png)

Da wir den Dienstnamen aus der Eigenschaft **RenderedDescription** benötigen, klicken wir auf die Schaltfläche neben dieser Eigenschaft.

![Felder extrahieren](media/log-analytics-custom-fields/extract-fields.png)

Der Feldextraktions-Assistent wird geöffnet, und in der Spalte **Main Example** (Hauptbeispiel) sind die Felder **EventLog** und **EventID** ausgewählt. Das benutzerdefinierte Feld wird also für Ereignisse aus dem Systemprotokoll mit der Ereignis-ID 7036 definiert. Diese Angaben sind ausreichend, und es müssen keine weiteren Felder ausgewählt werden.

![Hauptbeispiel](media/log-analytics-custom-fields/main-example.png)

Wir markieren den Namen des Diensts in der Eigenschaft **RenderedDescription** und verwenden **Service**, um den Dienstnamen anzugeben. Der Name des benutzerdefinierten Felds lautet **Service\_CF**.

![Feldtitel](media/log-analytics-custom-fields/field-title.png)

Wir stellen fest, dass der Dienstname nicht bei allen Datensätzen korrekt ermittelt wird. In den Suchergebnissen sehen wir, dass bei **WMI Performance Adapter** ein Teil des Namens nicht ausgewählt wurde. Die Zusammenfassung zeigt, dass bei vier Datensätzen mit **DPRMA** fälschlicherweise ein zusätzliches Wort erfasst wurde und zwei Datensätze als **Modules Installer** (anstelle von **Windows Modules Installer**) erfasst wurden.

![Suchergebnisse](media/log-analytics-custom-fields/search-results-01.png)

Kümmern wir uns zunächst um den Datensatz **WMI Performance Adapter**. Wir klicken auf das Bearbeitungssymbol und anschließend auf **Modify this highlight** (Diese Markierung ändern).

![Markierung ändern](media/log-analytics-custom-fields/modify-highlight.png)

Wir erweitern die Markierung um das Wort **WMI** und wiederholen dann den Extraktionsvorgang.

![Zusätzliches Beispiel](media/log-analytics-custom-fields/additional-example-01.png)

Wir sehen, dass Log Analytics auf der Grundlage dieser Information nicht nur die Einträge für **WMI Performance Adapter**, sondern auch die Einträge für **Windows Modules Installer** korrigiert hat. In der Zusammenfassung stellen wir jedoch fest, dass **DPMRA** immer noch nicht korrekt erfasst wird.

![Suchergebnisse](media/log-analytics-custom-fields/search-results-02.png)

Wir navigieren zu einem Datensatz mit dem DPMRA-Dienst und wenden das gleiche Korrekturverfahren an.

![Zusätzliches Beispiel](media/log-analytics-custom-fields/additional-example-02.png)

 Wenn wir nun die Extraktion ausführen, sind alle Ergebnisse korrekt.

![Suchergebnisse](media/log-analytics-custom-fields/search-results-03.png)

Wir sehen, dass **Service\_CF** zwar erstellt, aber noch keinen Datensätzen hinzugefügt wurde.

![Anfängliche Anzahl](media/log-analytics-custom-fields/initial-count.png)

Nachdem etwas Zeit vergangen ist und neue Ereignisse gesammelt wurden, sehen wir, dass, das Feld **Service\_CF** jetzt Datensätzen hinzugefügt wird, die unseren Kriterien entsprechen.

![Endergebnis](media/log-analytics-custom-fields/final-results.png)

Nun können wir das benutzerdefinierte Feld wie jede andere Datensatzeigenschaft verwenden. Zur Veranschaulichung erstellen wir eine Abfrage, die auf der Grundlage des neuen Felds **Service\_CF** eine Gruppierung vornimmt, um zu prüfen, welche Dienste besonders aktiv sind.

![Gruppierungsabfrage](media/log-analytics-custom-fields/query-group.png)

## Nächste Schritte

- Informieren Sie sich ausführlicher über [Protokollsuchvorgänge](log-analytics-log-searches.md), um Abfragen mithilfe von benutzerdefinierten Feldern für Kriterien zu erstellen.

<!---HONumber=AcomDC_0504_2016-->