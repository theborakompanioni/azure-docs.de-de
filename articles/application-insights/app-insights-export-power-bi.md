<properties 
	pageTitle="Datenexport nach Power BI aus Application Insights" 
	description="Artikel" 
	services="application-insights" 
    documentationCenter=""
	authors="noamben" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/05/2016" 
	ms.author="awills"/>

# Datenimport nach Power BI aus Application Insights

[Power BI](http://www.powerbi.com/) ist eine Suite aus Business Analytics-Tools zum Analysieren von Daten und Teilen von Einblicken. Auf jedem Gerät stehen leistungsfähige Dashboards zur Verfügung. Sie können Daten aus vielen Quellen kombinieren, beispielsweise auch aus [Visual Studio Application Insights](app-insights-overview.md).

Informationen für den Einstieg finden Sie unter [Anzeigen von Application Insights-Daten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-application-insights/).

Sie erhalten ein anfängliches Dashboard, das Sie anpassen können, indem Sie die Application Insights-Diagramme mit denen aus anderen Quellen kombinieren. Es gibt einen Katalog mit visuellen Elementen, in dem weitere Diagramme bereitgestellt werden. Jedes dieser Diagramme verfügt über Parameter, die Sie festlegen können.

![](./media/app-insights-export-power-bi/010.png)


Nach dem anfänglichen Import werden das Dashboard und die Berichte täglich aktualisiert. Sie können den Aktualisierungszeitplan im Dataset steuern.


**Stichproben** Wenn Ihre Anwendung eine große Menge von Daten sendet und Sie das Application Insights-SDK für ASP.NET Version 2.0.0-beta3 oder höher verwenden, wird möglicherweise die adaptive Stichprobenerstellung verwendet, bei der nur ein bestimmter Prozentsatz der Telemetriedaten übermittelt wird. Dasselbe gilt, wenn Sie die Stichprobenerstellung entweder im SDK oder bei der Erfassung manuell festgelegt haben. [Erfahren Sie mehr über das Erstellen von Stichproben.](app-insights-sampling.md)

## Alternative Möglichkeiten zur Anzeige von Application Insights-Daten

* [Azure-Dashboards, die Application Insights-Diagramme enthalten](app-insights-dashboards.md): Diese sind möglicherweise besser geeignet, wenn Sie ausschließlich Azure-Daten anzeigen möchten. Wenn Sie beispielsweise ein Dashboard mit Application Insights-Diagrammen einrichten möchten, über die verschiedene Komponenten eines Systems gemeinsam mit einigen Azure-Dienstmonitoren überwacht werden, stellt ein Azure-Dashboard die ideale Wahl dar. Es wird standardmäßig häufiger aktualisiert. 
* [Fortlaufender Export](app-insights-export-telemetry.md): Hierbei werden Ihre eingehenden Daten in Azure-Speicher kopiert. Von dort aus können Sie die Daten dann verschieben und in der beliebiger Weise verarbeiten.
* [Analytics](app-insights-analytics.md): Ermöglicht die Ausführung komplexer Abfragen für die in Application Insights enthaltenen Rohdaten.


## Erstellen eines eigenen Power BI-Adapters mit Stream Analytics

Das Power BI-Inhaltspaket für Application Insights zeigt eine nützliche Teilmenge Ihrer App-Telemetriedaten an, die für Ihre Anforderungen sehr wahrscheinlich ausreicht. Wenn Sie aber umfangreichere Telemetriedaten benötigen oder einige Daten aus den unverarbeiteten Telemetriedaten berechnen möchten, können Sie über den Azure Stream Analytics-Dienst einen eigenen Adapter erstellen.

In diesem Fall werden Daten aus Application Insights in den Azure-Speicher exportiert. [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) ruft die Daten von dort ab, benennt einige Felder um, verarbeitet sie und reicht die Daten an Power BI weiter. Stream Analytics ist ein Dienst, der einen fortlaufenden Datenstrom filtern, aggregrieren und Berechnungen für diesen durchführen kann.

![Blockdiagramm für Export über SA nach PBI](./media/app-insights-export-power-bi/020.png)


>[AZURE.TIP] **Sie müssen das im verbleibenden Teil dieses Artikels beschriebene Verfahren (mit Stream Analytics) nicht ausführen**, um Application Insights-Daten in Power BI anzuzeigen. Es geht viel einfacher! [Verwenden Sie stattdessen den kostenlosen Adapter](https://powerbi.microsoft.com/documentation/powerbi-content-pack-application-insights/). Führen Sie das im weiteren Verlauf dieses Artikels beschriebene Verfahren nur dann aus, wenn dieser Adapter nicht alle benötigten Daten bietet oder Sie eigene Aggregationen oder Funktionen für Ihre Daten definierten möchten.

### Erstellen von Speicher in Azure

Durch fortlaufende Exportaktivitäten werden Daten an ein Azure-Speicherkonto übertragen, daher müssen Sie zuerst Speicher erstellen.

1. Haben Sie das [Power BI-Powerpack für Application Insights](https://powerbi.microsoft.com/documentation/powerbi-content-pack-application-insights/) ausprobiert? Wenn dieses für Ihre Anforderungen ausreicht, benötigen Sie die weiteren Informationen in diesem Artikel nicht.

2.  Erstellen Sie ein „klassisches“ Speicherkonto in Ihrem Abonnement im [Azure-Portal](https://portal.azure.com).

    ![Wählen Sie im Azure-Portal „Neu“ > „Daten“ > „Speicher“ aus.](./media/app-insights-export-power-bi/030.png)

2. Erstellen eines Containers

    ![Wählen Sie im neuen Speicher „Container“ aus, klicken Sie auf die Kachel „Container“ und anschließend auf „Hinzufügen“.](./media/app-insights-export-power-bi/040.png)

3. Kopieren Sie den Speicherzugriffsschlüssel.

    Sie benötigen ihn bald, um die Eingabe für den Stream Analytics-Dienst einzurichten.

    ![Öffnen Sie im Speicher die Optionen „Einstellungen“, „Schlüssel“, und kopieren Sie den primären Zugriffsschlüssel.](./media/app-insights-export-power-bi/045.png)

### Starten des fortlaufenden Exports in den Azure-Speicher

Mit dem [fortlaufenden Export](app-insights-export-telemetry.md) werden Daten aus Application Insights in den Azure-Speicher verschoben.

1. Navigieren Sie im Azure-Portal zu der Application Insights-Ressource, die Sie für Ihre Anwendung erstellt haben.

    ![Wählen Sie „Durchsuchen“, „Application Insights“ und Ihre Anwendung aus.](./media/app-insights-export-power-bi/050.png)

2. Erstellen Sie einen fortlaufenden Export.

    ![Wählen Sie „Einstellungen“ > „Fortlaufender Export“ > „Hinzufügen“.](./media/app-insights-export-power-bi/060.png)


    Wählen Sie das zuvor erstellte Speicherkonto aus:

    ![Legen Sie das Exportziel fest.](./media/app-insights-export-power-bi/070.png)
    
    Legen Sie die Ereignistypen fest, die Sie anzeigen möchten:

    ![Wählen Sie Ereignistypen aus.](./media/app-insights-export-power-bi/080.png)

3. Warten Sie, bis sich einige Daten angesammelt haben. Lehnen Sie sich zurück, und lassen Sie Ihre Benutzer die Anwendung eine Weile lang verwenden. Telemetriedaten gehen ein, und Sie sehen statistische Diagramme im [Metrik-Explorer](app-insights-metrics-explorer.md) sowie einzelne Ereignisse in der [Diagnosesuche](app-insights-diagnostic-search.md).

    Darüber hinaus werden die Daten in Ihren Speicher exportiert.

4. Überprüfen Sie die exportierten Daten. Wählen Sie in Visual Studio **Anzeigen/Cloud Explorer**, und öffnen Sie „Azure/Storage“. (Wenn diese Menüoption nicht verfügbar ist, müssen Sie das Azure SDK installieren: Öffnen Sie das Dialogfeld „Neues Projekt“ und anschließend „Visual C# / Cloud / Microsoft Azure SDK für .NET abrufen“.)

    ![](./media/app-insights-export-power-bi/04-data.png)

    Notieren Sie sich den gemeinsamen Teil des Pfadnamens, der sich vom Anwendungsnamen und vom Instrumentierungsschlüssel ableitet.

Die Ereignisse werden in Blobdateien im JSON-Format geschrieben. Jede Datei kann ein oder mehrere Ereignisse enthalten. Daher möchten wir die Ereignisdaten lesen und die gewünschten Felder herausfiltern. Es gibt viele verschiedene Möglichkeiten zur Nutzung der Daten, aber unser Plan besteht darin, die Daten mit Stream Analytics per Pipe an Power BI weiterzureichen.

### Erstellen einer Azure Stream Analytics-Instanz

Wählen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) den Azure Stream Analytics-Dienst aus, und erstellen Sie einen neuen Stream Analytics-Auftrags:


![](./media/app-insights-export-power-bi/090.png)



![](./media/app-insights-export-power-bi/100.png)

Wenn der neue Auftrag erstellt ist, erweitern Sie die Details:

![](./media/app-insights-export-power-bi/110.png)


#### Festlegen des Blobspeicherorts

Legen Sie den Auftrag so fest, dass er Eingaben vom Blob für den fortlaufenden Export erhält:

![](./media/app-insights-export-power-bi/120.png)

Jetzt benötigen Sie den primären Zugriffsschlüssel aus Ihrem Speicherkonto, den Sie zuvor notiert haben. Legen Sie diesen als Speicherkontoschlüssel fest.

![](./media/app-insights-export-power-bi/130.png)

#### Festlegen des Präfixmusters des Pfads 

![](./media/app-insights-export-power-bi/140.png)


Achten Sie darauf, dass das Datum das Format „JJJJ-MM-TT“ (mit Bindestrichen) aufweist.

Das Präfixmuster des Pfads gibt an, wo Stream Analytics die Eingabedateien im Speicher findet. Sie müssen es so einstellen, dass es der Speicherung der Daten durch den fortlaufenden Export entspricht. Legen Sie ihn wie folgt fest:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

In diesem Beispiel:

* `webapplication27` ist der Name der Application Insights-Ressource in **Kleinbuchstaben**.
* `1234...` ist der Instrumentierungsschlüssel der Application Insights-Ressource **ohne Bindestriche**. 
* `PageViews` ist der zu analysierende Datentyp. Die verfügbaren Typen sind abhängig von dem Filter, den Sie im fortlaufenden Export festlegen. Untersuchen Sie die exportierten Daten, um die anderen verfügbaren Typen anzuzeigen, und sehen Sie sich das [Exportdatenmodell](app-insights-export-data-model.md) an.
* `/{date}/{time}` ist ein als Literal geschriebenes Muster.

> [AZURE.NOTE] Überprüfen Sie den Speicher, um sicherzustellen, dass der Pfad stimmt.

#### Fertig stellen des ersten Setups

Bestätigen Sie das Serialisierungsformat:

![Bestätigen Sie den Vorgang, und schließen Sie den Assistenten.](./media/app-insights-export-power-bi/150.png)

Schließen Sie den Assistenten, und warten Sie, bis das Setup abgeschlossen ist.

> [AZURE.TIP] Verwenden Sie den Beispielbefehl, um einige Daten herunterzuladen. Behalten Sie ihn als Testbeispiel, um Ihre Abfrage zu debuggen.

### Festlegen der Ausgabe

Nun wählen Sie Ihren Auftrag aus und legen die Ausgabe fest.

![Wählen Sie den neuen Kanal aus, klicken Sie auf „Ausgaben“ > „Hinzufügen“ > „Power BI“.](./media/app-insights-export-power-bi/160.png)

Geben Sie Ihr **Geschäfts- oder Schulkonto** an, um Stream Analytics für den Zugriff auf Ihre Power BI-Ressource zu autorisieren. Geben Sie anschließend Namen für die Ausgabe sowie für den Power BI-Zieldatensatz und die Zieltabelle an.

![Erfinden Sie drei Namen.](./media/app-insights-export-power-bi/170.png)

### Festlegen der Abfrage

Die Abfrage bestimmt die Übersetzung von der Eingabe zur Ausgabe.

![Wählen Sie den Auftrag aus, und klicken Sie auf „Abfrage“. Fügen Sie das folgenden Beispiel ein.](./media/app-insights-export-power-bi/180.png)


Überprüfen Sie mithilfe der Testfunktion, ob Sie die richtige Ausgabe erhalten. Geben Sie die Beispieldaten an, die Sie der Eingabeseite entnommen haben.

#### Abfrage zum Anzeigen der Anzahl von Ereignissen

Fügen Sie diese Abfrage ein:

```SQL

    SELECT
      flat.ArrayValue.name,
      count(*)
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.[event]) as flat
    GROUP BY TumblingWindow(minute, 1), flat.ArrayValue.name
```

* „export-input§ ist der Alias, den wir der Datenstromeingabe gegeben haben.
* „pbi-outout“ ist der definierte Ausgabealias.
* Wir verwenden [OUTER APPLY GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx), da sich der Ereignisname in einem geschachtelten JSON-Arrray befindet. Die SELECT-Anweisung wählt dann den Ereignisnamen zusammen mit der Anzahl der Instanzen mit diesem Namen im angegebenen Zeitraum aus. Die [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx)-Klausel gruppiert die Elemente in Zeiträume von einer Minute.


#### Abfrage zum Anzeigen metrischer Werte


```SQL

    SELECT
      A.context.data.eventtime,
      avg(CASE WHEN flat.arrayvalue.myMetric.value IS NULL THEN 0 ELSE  flat.arrayvalue.myMetric.value END) as myValue
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.context.custom.metrics) as flat
    GROUP BY TumblingWindow(minute, 1), A.context.data.eventtime

``` 

* Diese Abfrage führt einen Drilldown in die Telemetrie der Metriken durch, um die Uhrzeit und den metrischen Wert des Ereignisses abzurufen. Die metrischen Werte befinden sich in einem Array, daher verwenden wir das Muster „OUTER APPLY GetElements“ zum Extrahieren der Zeilen. In diesem Fall lautet der Name der Metrik „myMetric“. 

#### Abfrage zum Anzeigen von Dimensionseigenschaften

```SQL

    WITH flat AS (
    SELECT
      MySource.context.data.eventTime as eventTime,
      InstanceId = MyDimension.ArrayValue.InstanceId.value,
      BusinessUnitId = MyDimension.ArrayValue.BusinessUnitId.value
    FROM MySource
    OUTER APPLY GetArrayElements(MySource.context.custom.dimensions) MyDimension
    )
    SELECT
     eventTime,
     InstanceId,
     BusinessUnitId
    INTO AIOutput
    FROM flat

```

* Diese Abfrage enthält Dimensionseigenschaftswerte, ohne dabei davon abhängig zu sein, dass eine bestimmte Dimension in einem festen Index im Dimensionsarray liegt.

### Ausführen des Auftrags

Sie können ein Datum in der Vergangenheit auswählen, ab dem der Auftrag ausgeführt werden soll.

![Wählen Sie den Auftrag aus, und klicken Sie auf „Abfrage“. Fügen Sie das folgenden Beispiel ein.](./media/app-insights-export-power-bi/190.png)

Warten Sie, bis der Auftrag ausgeführt wird.

### Anzeigen der Ergebnisse in Power BI

Öffnen Sie Power BI mit Ihrem Geschäfts- oder Schulkonto, und wählen Sie das Dataset und die Tabelle aus, die Sie als Ausgabe des Stream Analytics-Auftrags definiert haben.

![Wählen Sie in Power BI Ihr Dataset und Felder aus.](./media/app-insights-export-power-bi/200.png)

Jetzt können Sie dieses Dataset in Berichten und Dashboards in [Power BI](https://powerbi.microsoft.com) verwenden.


![Wählen Sie in Power BI Ihr Dataset und Felder aus.](./media/app-insights-export-power-bi/210.png)

### Video

Noam Ben Zeev zeigt, wie nach Power BI exportiert wird.

> [AZURE.VIDEO export-to-power-bi-from-application-insights]

## Verwandte Themen

* [Fortlaufendem Export](app-insights-export-telemetry.md)
* [Detaillierte Datenmodellreferenz für die Eigenschaftstypen und -werte.](app-insights-export-data-model.md)
* [Application Insights](app-insights-overview.md)
* [Weitere Beispiele und exemplarische Vorgehensweisen](app-insights-code-samples.md)
 

<!---HONumber=AcomDC_0413_2016-->