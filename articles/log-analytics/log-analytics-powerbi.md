---
title: Exportieren von Log Analytics-Daten nach Power BI | Microsoft Docs
description: Power BI ist ein cloudbasierter Business Analytics-Dienst von Microsoft, der umfassende Visualisierungen und Berichte zur Analyse verschiedener Datensätze bietet.  Log Analytics kann Daten kontinuierlich aus dem OMS-Repository nach Power BI exportieren, sodass Sie die Visualisierungsfunktionen und Analysetools von Power BI nutzen können.  Dieser Artikel beschreibt, wie Sie Abfragen in Log Analytics konfigurieren, die Daten automatisch in regelmäßigen Intervallen nach Power BI exportieren.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn

ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/18/2016
ms.author: bwren

---
# <a name="export-log-analytics-data-to-power-bi"></a>Exportieren von Log Analytics-Daten nach Power BI
[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) ist ein cloudbasierter Business Analytics-Dienst von Microsoft, der umfassende Visualisierungen und Berichte zur Analyse verschiedener Datensätze bietet.  Log Analytics kann Daten automatisch aus dem OMS-Repository nach Power BI exportieren, sodass Sie die Visualisierungsfunktionen und Analysetools von Power BI nutzen können.

Wenn Sie Power BI für Log Analytics konfigurieren, können Sie Protokollabfragen erstellen, die ihre Ergebnisse in die entsprechenden Datasets in Power BI exportieren.  Der Abfrage- und Exportvorgang wird weiterhin automatisch nach einem von Ihnen definieren Zeitplan ausgeführt, damit das Dataset jederzeit mit den neuesten von Log Analytics gesammelten Daten aktualisiert wird.

![Von Log Analytics nach Power BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Power BI-Zeitpläne
Ein *Power BI-Zeitplan* enthält eine Protokollsuche, die einen Satz von Daten aus dem OMS-Repository in ein entsprechendes Dataset in Power BI exportiert. In einem Zeitplan ist definiert, wie häufig diese Suche ausgeführt wird, damit das Dataset aktuell bleibt.

Die Felder im Dataset entsprechen den Eigenschaften der von der Protokollsuche zurückgegebenen Datensätze.  Wenn eine Suche Datensätze verschiedener Typen zurückgibt, schließt das Dataset alle Eigenschaften aller enthalten Datensatztypen ein.  

> [!NOTE]
> Es empfiehlt sich, eine Protokollsuchabfrage zu verwenden, die Rohdaten zurückgibt, anstatt mithilfe von Befehlen wie [Measure](log-analytics-search-reference.md#measure)eine Konsolidierung durchzuführen.  Sie können alle notwendigen Aggregierungen und Berechnungen für die Rohdaten in Power BI durchführen.
> 
> 

## <a name="connecting-oms-workspace-to-power-bi"></a>Verbinden des OMS-Arbeitsbereichs mit Power BI
Bevor Sie Daten aus Log Analytics nach Power BI exportieren können, müssen Sie Ihren OMS-Arbeitsbereich mit Ihrem Power BI-Konto verbinden. Führen Sie dazu die folgenden Schritte aus.  

1. Klicken Sie in der OMS-Konsole auf die Kachel **Einstellungen** .
2. Wählen Sie **Konten**aus.
3. Klicken Sie im Abschnitt **Arbeitsbereichsinformationen** auf **Mit Power BI-Konto verbinden**.
4. Geben Sie die Anmeldeinformationen für Ihr Power BI-Konto ein.

## <a name="create-a-power-bi-schedule"></a>Erstellen eines Power BI-Zeitplans
Erstellen Sie einen Power BI-Zeitplan für jedes Dataset. Führen Sie dazu die folgenden Schritte aus.

1. Klicken Sie in der OMS-Konsole auf die Kachel **Protokollsuche** .
2. Geben Sie eine neue Abfrage ein, oder wählen Sie eine gespeicherte Suche aus, die die Daten zurückgibt, die Sie nach **Power BI**exportieren möchten.  
3. Klicken Sie oben auf der Seite auf die Schaltfläche **Power BI**, um das Dialogfeld **Power BI** zu öffnen.
4. Stellen Sie die Informationen aus der folgenden Tabelle bereit, und klicken Sie auf **Speichern**.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Name |Der Name, anhand dessen der Zeitplan in der Liste der Power BI-Zeitpläne identifiziert werden kann. |
| Gespeicherte Suche |Die auszuführende Protokollsuche.  Sie können entweder die aktuelle Abfrage auswählen oder aus dem Dropdownfeld eine vorhandene gespeicherte Suche auswählen. |
| Zeitplan |Die Häufigkeit der Ausführung der gespeicherten Suche und des Exports in das Power BI-Dataset.  Der Wert muss zwischen 15 Minuten und 24 Stunden liegen. |
| Datasetname |Der Name des Datasets in Power BI.  Der Name wird erstellt, wenn noch keiner vorhanden ist. Ist bereits ein Name vorhanden, wird dieser aktualisiert. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Anzeigen und Entfernen von Power BI-Zeitplänen
Führen Sie die folgenden Schritte aus, um die Liste der vorhandenen Power BI-Zeitpläne anzuzeigen.

1. Klicken Sie in der OMS-Konsole auf die Kachel **Einstellungen** .
2. Wählen Sie **Power BI**aus.

Zusätzlich zu den Details zum Zeitplan werden die Anzahl der Ausführungen des Zeitplans in der letzten Woche und der Status der letzten Synchronisierung angezeigt.  Wenn bei der Synchronisierung Fehler auftreten, können Sie auf den Link klicken, um eine Protokollsuche nach Datensätzen mit Details zum Fehler auszuführen.

Sie können einen Zeitplan entfernen, indem Sie in der Spalte **Entfernen** auf das **X** klicken.  Sie können einen Zeitplan deaktivieren, indem Sie **Aus**auswählen.  Um einen Zeitplan zu ändern, müssen Sie ihn entfernen und mit den neuen Einstellungen neu erstellen.

![Power BI-Zeitpläne](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Exemplarische Vorgehensweise
Der folgende Abschnitt zeigt anhand eines Beispiels, wie Sie einen Power BI-Zeitplan erstellen und das Dataset dieses Zeitplans zum Erstellen eines einfachen Berichts verwenden.  In diesem Beispiel werden alle Leistungsdaten für eine Gruppe von Computern nach Power BI exportiert. Anschließend wird ein Liniendiagramm erstellt, um die CPU-Nutzung anzuzeigen.

### <a name="create-log-search"></a>Erstellen einer Protokollsuche
Zunächst erstellen Sie eine Protokollsuche für die Daten, die Sie an das Dataset senden möchten.  In diesem Beispiel wird eine Abfrage verwendet, die alle Leistungsdaten für Computer zurückgibt, deren Name mit *srv*beginnt.  

![Power BI-Zeitpläne](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Erstellen einer Power BI-Suche
Klicken Sie auf die Schaltfläche **Power BI** , um das Power BI-Dialogfeld zu öffnen und die erforderlichen Informationen bereitzustellen.  Diese Suche soll einmal pro Stunde ausgeführt werden und ein Dataset mit der Bezeichnung *Contoso Perf*erstellen.  Da die Suche, mit der die gewünschten Daten erstellt werden, bereits geöffnet ist, behalten Sie die Standardeinstellung *Aktuelle Suchabfrage verwenden* für die Option **Gespeicherte Suche**bei.

![Power BI-Suche](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Überprüfen einer Power BI-Suche
Um sicherzustellen, dass der Zeitplan ordnungsgemäß erstellt wurde, zeigen Sie auf der Kachel **Einstellungen** des OMS-Dashboards die Liste der Power BI-Suchvorgänge an.  Warten Sie einige Minuten, und aktualisieren Sie diese Ansicht solange, bis gemeldet wird, dass die Synchronisierung ausgeführt wurde.

![Power BI-Suche](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Überprüfen des Datasets in Power BI
Melden Sie sich bei Ihrem Konto für [powerbi.microsoft.com](http://powerbi.microsoft.com/) an, und scrollen Sie im linken Bereich nach unten zu **Datasets** .  Sie sehen, dass das Dataset *Contoso Perf* aufgeführt wird – dies bedeutet, dass der Export erfolgreich ausgeführt wurde.

![Power BI-Dataset](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Erstellen eines Berichts anhand eines Datasets
Wählen Sie das Dataset **Contoso Perf** aus, und klicken Sie rechts im Bereich **Felder** auf **Ergebnisse**, um die Felder anzuzeigen, die zu diesem Dataset gehören.  Um ein Liniendiagramm zu erstellen, das die CPU-Nutzung für jeden Computer anzeigt, führen Sie folgende Aktionen aus.

1. Wählen Sie den Visualisierungstyp „Liniendiagramm“.
2. Ziehen Sie **ObjectName** auf **Berichtsstufenfilter**, und aktivieren Sie das Kontrollkästchen **Prozessor**.
3. Ziehen Sie **CounterName** auf **Berichtsstufenfilter**, und aktivieren Sie das Kontrollkästchen **% Prozessorzeit**.
4. Ziehen Sie **CounterValue** auf **Werte**.
5. Ziehen Sie **Computer** auf **Legende**.
6. Ziehen Sie **TimeGenerated** auf **Achse**.

Wie Sie sehen, wird das resultierende Liniendiagramm mit den Daten aus dem Dataset angezeigt.

![Power BI-Liniendiagramm](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>Speichern des Berichts
Speichern Sie den Bericht, indem Sie am oberen Bildschirmrand auf die Schaltfläche „Speichern“ klicken, und überprüfen, ob er jetzt auf der linken Seite im Abschnitt „Berichte“ aufgeführt wird.

![Power BI-Berichte](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr zu [Protokollsuchvorgängen](log-analytics-log-searches.md) zum Erstellen von Abfragen, die nach Power BI exportiert werden können.
* Erfahren Sie mehr über [Power BI](http://powerbi.microsoft.com), um Visualisierungen anhand von Log Analytics-Exporten zu erstellen.

<!--HONumber=Oct16_HO2-->


