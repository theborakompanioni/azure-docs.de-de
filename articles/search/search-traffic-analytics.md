---
title: „Datenverkehrsanalyse durchsuchen“ für Azure Search | Microsoft Docs
description: Aktivieren Sie „Datenverkehrsanalyse durchsuchen“ für Azure Search, einen cloudgehosteten Suchdienst für Microsoft Azure, um Erkenntnisse über Ihre Benutzer und Daten zu gewinnen.
services: search
documentationcenter: ''
author: bernitorres
manager: pablocas
editor: ''

ms.service: search
ms.devlang: multiple
ms.workload: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 09/23/2016
ms.author: betorres

---
# <a name="enabling-and-using-search-traffic-analytics"></a>Aktivieren und Verwenden von „Datenverkehrsanalyse durchsuchen“
„Datenverkehrsanalyse durchsuchen“ ist ein Azure Search-Feature, mit dem Sie Einblick in Ihren Suchdienst und Erkenntnisse über Ihre Benutzer und deren Verhalten erhalten. Wenn Sie dieses Feature aktivieren, werden Ihre Suchdienstdaten auf ein Speicherkonto Ihrer Wahl kopiert. Diese Daten umfassen die Suchdienstprotokolle und die aggregierten operativen Metriken, die Sie zur weiteren Analyse verarbeiten und bearbeiten können.

## <a name="how-to-enable-search-traffic-analytics"></a>Aktivieren von „Datenverkehrsanalyse durchsuchen“
Sie benötigen ein Speicherkonto, das sich in der gleichen Region und dem gleichen Abonnement befindet wie Ihr Suchdienst.

> [!IMPORTANT]
> Für dieses Speicherkonto fallen Standardgebühren an.
> 
> 

Sie können die Datenverkehrsanalyse im Portal oder über PowerShell aktivieren. Nach der Aktivierung beginnt innerhalb von fünf bis zehn Minuten der Datenfluss in Ihr Speicherkonto in die folgenden zwei Blob-Container:

    insights-logs-operationlogs: search traffic logs
    insights-metrics-pt1m: aggregated metrics


### <a name="a.-using-the-portal"></a>A: Verwenden des Portals
Öffnen Sie Ihren Azure Search-Dienst im [Azure-Portal](http://portal.azure.com). Unter „Einstellungen“ finden Sie die Option „Datenverkehrsanalyse durchsuchen“. 

![][1]

Ändern Sie den Status in **Ein**, wählen Sie das zu verwendende Azure Storage-Konto, und wählen Sie die Daten aus, die Sie kopieren möchten: Protokolle, Metriken oder beides. Sie sollten Protokolle und Metriken kopieren. Die Aufbewahrungsrichtlinie für Ihre Daten kann auf einen Wert zwischen 1 und 365 Tagen festgelegt werden. Wenn die Daten nicht unbegrenzt aufbewahrt werden sollen, legen Sie die Aufbewahrung (Tage) auf 0 fest.

![][2]

### <a name="b.-using-powershell"></a>B. Verwenden von PowerShell
Stellen Sie zunächst sicher, dass Sie die aktuellen [Azure PowerShell-Cmdlets](https://github.com/Azure/azure-powershell/releases) installiert haben.

Rufen Sie anschließend die Ressourcen-IDs für Ihren Suchdienst und Ihr Speicherkonto ab. Sie finden sie im Portal unter „Einstellungen -> Eigenschaften -> Ressourcen-ID“.

![][3]

```PowerShell
Login-AzureRmAccount
$SearchServiceResourceId = "Your Search service resource id"
$StorageAccountResourceId = "Your Storage account resource id"
Set-AzureRmDiagnosticSetting -ResourceId $SearchServiceResourceId StorageAccountId $StorageAccountResourceId -Enabled $true
```

## <a name="understanding-the-data"></a>Grundlegendes zu den Daten
Die Daten werden in JSON-formatierten Azure Storage-Blobs gespeichert.

Es gibt einen Blob pro Stunde pro Container.

Beispiel-Pfad: `resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2015/m=12/d=25/h=01/m=00/name=PT1H.json`

### <a name="logs"></a>Protokolle
Die Protokollblobs enthalten die Datenverkehrprotokolle des Suchdiensts.
Jedes Blob hat ein Stammobjekt namens **records** , das ein Array mit Protokollobjekten enthält.
Jedes Blob enthält Einträge zu allen Vorgängen, die während derselben Stunde erfolgt sind.

#### <a name="log-schema"></a>Protokollschema
| Name | Typ | Beispiel | Hinweise |
| --- | --- | --- | --- |
| in |datetime |„2015-12-07T00:00:43.6872559Z“ |Zeitstempel des Vorgangs |
| resourceId |string |"/SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111/<br/>RESOURCEGROUPS/DEFAULT/PROVIDERS/<br/>  MICROSOFT.SEARCH/SEARCHSERVICES/SEARCHSERVICE" |Ihre Ressourcen-ID |
| operationName |string |„Query.Search“ |Der Name des Vorgangs |
| operationVersion |string |„2015-02-28“ |Die verwendete API-Version |
| category |string |„OperationLogs“ |Konstante |
| resultType |string |„Success“ |Mögliche Werte: „Success“ oder „Failure“ |
| resultSignature |int |200 |HTTP-Ergebniscode |
| durationMS |int |50 |Dauer des Vorgangs in Millisekunden |
| Eigenschaften |objekt |Siehe hierzu die folgende Tabelle. |Objekt, das vorgangsspezifische Daten enthält |

#### <a name="properties-schema"></a>Eigenschaftsschema
| Name | Typ | Beispiel | Hinweise |
| --- | --- | --- | --- |
| Beschreibung |string |„GET-/indexes('content')/docs“ |Endpunkt des Vorgangs |
| Abfrage |string |„?search=AzureSearch&$count=true&api-version=2015-02-28“ |Die Abfrageparameter |
| Dokumente |int |42 |Anzahl von verarbeiteten Dokumenten |
| IndexName |string |„testindex“ |Name des Indexes, der dem Vorgang zugeordnet ist |

### <a name="metrics"></a>Metriken
Die Metrikblobs enthalten aggregierte Werte für Ihren Suchdienst. Jede Datei hat ein Stammobjekt namens **records** , das ein Array von Metrikobjekten enthält. Dieses Stammobjekt enthält Metriken für jede Minute, für die die Daten verfügbar waren. 

Verfügbare Metriken:

* SearchLatency: Die Zeit, die der Suchdienst für die Verarbeitung von Suchabfragen benötigt hat, auf Minutenbasis aggregiert.
* SearchQueriesPerSecond: Anzahl der pro Sekunde empfangenen Suchabfragen, auf Minutenbasis aggregiert.
* ThrottledSearchQueriesPercentage: Prozentsatz der gedrosselten Suchabfragen, auf Minutenbasis aggregiert.

> [!IMPORTANT]
> Eine Drosselung erfolgt, wenn zu viele Abfragen gesendet werden, die die bereitgestellte Ressourcenkapazität des Diensts ausbremsen. Erwägen Sie das Hinzufügen weiterer Replikate zu Ihrem Dienst.
> 
> 

#### <a name="metrics-schema"></a>Metrikenschema
| Name | Typ | Beispiel | Hinweise |
| --- | --- | --- | --- |
| resourceId |string |"/SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111/<br/>RESOURCEGROUPS/DEFAULT/PROVIDERS/<br/> MICROSOFT.SEARCH/SEARCHSERVICES/SEARCHSERVICE" |Ihre Ressourcen-ID |
| metricName |string |„Latency“ |Der Name der Metrik |
| in |datetime |„2015-12-07T00:00:43.6872559Z“ |Der Zeitstempel des Vorgangs |
| average |int |64 |Der Durchschnittswert der unformatierten Beispiele im Metrikzeitintervall |
| minimum |int |37 |Der Mindestwert der unformatierten Beispiele im Metrikzeitintervall |
| maximum |int |78 |Der Höchstwert der unformatierten Beispiele im Metrikzeitintervall |
| total |int |258 |Der Gesamtwert der unformatierten Beispiele im Metrikzeitintervall |
| count |int |4 |Die Anzahl der unformatierten Beispiele, die zum Generieren der Metrik verwendet werden |
| timegrain |string |„PT1M“ |Das Aggregationsintervall der Metrik in ISO 8601 |

Alle Metriken werden in Intervallen von einer Minute gemeldet. Jede Metrik macht Mindest-, Höchst- und Durchschnittswerte pro Minute verfügbar.

Bei der Metrik „SearchQueriesPerSecond“ ist der Mindestwert der niedrigste Wert für Suchabfragen pro Sekunde, der während dieser Minute registriert wurde. Dasselbe gilt für den Höchstwert. Der Durchschnittswert ist das Aggregat der gesamten Minute. Beispiel: Innerhalb einer Minute kann es für eine Sekunde eine sehr hohe Last geben (dies ist der Höchstwert für „SearchQueriesPerSecond“), gefolgt von 58 Sekunden mit mittlerer Last sowie einer Sekunde mit nur einer Abfrage, was der Mindestwert ist.

Für „ThrottledSearchQueriesPercentage“ entsprechen der Mindest-, Höchst-, Durchschnitts- und Gesamtwert demselben Wert, nämlich dem Prozentsatz von Suchabfragen, die gedrosselt wurden, basierend auf der Gesamtanzahl von Suchabfragen während einer Minute.

## <a name="analyzing-your-data"></a>Analysieren Ihrer Daten
Die Daten befinden sich in Ihrem eigenen Speicherkonto, und Sie sollten diese Daten in der für Sie idealen Art und Weise untersuchen.

Zunächst sollten Sie Ihre Daten mit [Power BI](https://powerbi.microsoft.com) untersuchen und visualisieren. Sie können sich ganz einfach mit Ihrem Azure Storage-Konto verbinden und schnell mit der Analyse Ihrer Daten beginnen. 

#### <a name="power-bi-online"></a>Power BI Online
[Power BI-Inhaltspaket](https://app.powerbi.com/getdata/services/azure-search): Erstellen Sie ein Power BI-Dashboard und einen Satz von Power BI-Berichten, die Ihre Daten automatisch anzeigen und visuelle Einblicke in Ihren Suchdienst gestatten. Informationen finden Sie auf der [Hilfeseite zum Inhaltspaket](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-search/).

![][4]

#### <a name="power-bi-desktop"></a>Power BI Desktop
[Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop): Untersuchen Sie Ihre Daten, und erstellen Sie Ihre eigenen Visualisierungen für Ihre Daten. Im folgenden Abschnitt finden Sie eine Abfrage für den Einstieg:

1. Öffnen eines neuen Power BI Desktop-Berichts
2. Wählen Sie „Daten abrufen -> Mehr...“
   
    ![][5]
3. Wählen Sie „Microsoft Azure-Blobspeicher“ und „Verbinden“.
   
    ![][6]
4. Geben Sie Namen und Kontoschlüssel des Speicherkontos ein.
5. Wählen Sie „insight-logs-operationlogs“ und „insights-metrics-pt1m“, und klicken Sie dann auf „Bearbeiten“.
6. Der Abfrage-Editor wird geöffnet. Stellen Sie sicher, dass „insight-logs-operationlogs“ auf der linken Seite ausgewählt ist. Öffnen Sie jetzt den erweiterten Editor durch Auswahl von „Ansicht -> Erweiterter Editor“.
   
    ![][7]
7. Behalten Sie die ersten beiden Zeilen bei, und ersetzen Sie den Rest durch die folgende Abfrage:
   
   > # "insights-logs-operationlogs" = Source{[Name="insights-logs-operationlogs"]}[Data],
   > # "Sorted Rows" = Table.Sort(#"insights-logs-operationlogs",{{"Date modified", Order.Descending}}),
   > # "Kept First Rows" = Table.FirstN(#"Sorted Rows",744),
   > # "Removed Columns" = Table.RemoveColumns(#"Kept First Rows",{"Name", "Extension", "Date accessed", "Date modified", "Date created", "Attributes", "Folder Path"}),
   > # "Parsed JSON" = Table.TransformColumns(#"Removed Columns",{},Json.Document),
   > # "Expanded Content" = Table.ExpandRecordColumn(#"Parsed JSON", "Content", {"records"}, {"records"}),
   > # "Expanded records" = Table.ExpandListColumn(#"Expanded Content", "records"),
   > # "Expanded records1" = Table.ExpandRecordColumn(#"Expanded records", "records", {"time", "resourceId", "operationName", "operationVersion", "category", "resultType", "resultSignature", "durationMS", "properties"}, {"time", "resourceId", "operationName", "operationVersion", "category", "resultType", "resultSignature", "durationMS", "properties"}),
   > # "Expanded properties" = Table.ExpandRecordColumn(#"Expanded records1", "properties", {"Description", "Query", "IndexName", "Documents"}, {"Description", "Query", "IndexName", "Documents"}),
   > # "Renamed Columns" = Table.RenameColumns(#"Expanded properties",{{"time", "Datetime"}, {"resourceId", "ResourceId"}, {"operationName", "OperationName"}, {"operationVersion", "OperationVersion"}, {"category", "Category"}, {"resultType", "ResultType"}, {"resultSignature", "ResultSignature"}, {"durationMS", "Duration"}}),
   > # "Added Custom2" = Table.AddColumn(#"Renamed Columns", "QueryParameters", each Uri.Parts("http://tmp" & [Query])),
   > # "Expanded QueryParameters" = Table.ExpandRecordColumn(#"Added Custom2", "QueryParameters", {"Query"}, {"Query.1"}),
   > # "Expanded Query.1" = Table.ExpandRecordColumn(#"Expanded QueryParameters", "Query.1", {"search", "$skip", "$top", "$count", "api-version", "searchMode", "$filter"}, {"search", "$skip", "$top", "$count", "api-version", "searchMode", "$filter"}),
   > # "Removed Columns1" = Table.RemoveColumns(#"Expanded Query.1",{"OperationVersion"}),
   > # "Changed Type" = Table.TransformColumnTypes(#"Removed Columns1",{{"Datetime", type datetimezone}, {"ResourceId", type text}, {"OperationName", type text}, {"Category", type text}, {"ResultType", type text}, {"ResultSignature", type text}, {"Duration", Int64.Type}, {"Description", type text}, {"Query", type text}, {"IndexName", type text}, {"Documents", Int64.Type}, {"search", type text}, {"$skip", Int64.Type}, {"$top", Int64.Type}, {"$count", type logical}, {"api-version", type text}, {"searchMode", type text}, {"$filter", type text}}),
   > # "Inserted Date" = Table.AddColumn(#"Changed Type", "Date", each DateTime.Date([Datetime]), type date),
   > # "Duplicated Column" = Table.DuplicateColumn(#"Inserted Date", "ResourceId", "Copy of ResourceId"),
   > # "Split Column by Delimiter" = Table.SplitColumn(#"Duplicated Column","Copy of ResourceId",Splitter.SplitTextByEachDelimiter({"/"}, null, true),{"Copy of ResourceId.1", "Copy of ResourceId.2"}),
   > # "Changed Type1" = Table.TransformColumnTypes(#"Split Column by Delimiter",{{"Copy of ResourceId.1", type text}, {"Copy of ResourceId.2", type text}}),
   > # "Removed Columns2" = Table.RemoveColumns(#"Changed Type1",{"Copy of ResourceId.1"}),
   > # "Renamed Columns1" = Table.RenameColumns(#"Removed Columns2",{{"Copy of ResourceId.2", "ServiceName"}}),
   > # "Lowercased Text" = Table.TransformColumns(#"Renamed Columns1",{{"ServiceName", Text.Lower}}),
   > # "Added Custom" = Table.AddColumn(#"Lowercased Text", "DaysFromToday", each Duration.Days(DateTimeZone.UtcNow() - [Datetime])),
   > # "Changed Type2" = Table.TransformColumnTypes(#"Added Custom",{{"DaysFromToday", Int64.Type}})
   > in
   > 
   > # "Changed Type2"
   > 
8. Klicken Sie auf „Done“.
9. Wählen Sie jetzt „insights-metrics-pt1m“ aus der Liste der Abfragen auf der linken Seite und öffnen Sie den erweiterten Editor erneut. Behalten Sie die ersten beiden Zeilen bei, und ersetzen Sie den Rest durch die folgende Abfrage: 
   
   > # "insights-metrics-pt1m1" = Source{[Name="insights-metrics-pt1m"]}[Data],
   > # "Sorted Rows" = Table.Sort(#"insights-metrics-pt1m1",{{"Date modified", Order.Descending}}),
   > # "Kept First Rows" = Table.FirstN(#"Sorted Rows",744),
   > # "Removed Columns" = Table.RemoveColumns(#"Kept First Rows",{"Name", "Extension", "Date accessed", "Date modified", "Date created", "Attributes", "Folder Path"}),
   > # "Parsed JSON" = Table.TransformColumns(#"Removed Columns",{},Json.Document),
   > # "Expanded Content" = Table.ExpandRecordColumn(#"Parsed JSON", "Content", {"records"}, {"records"}),
   > # "Expanded records" = Table.ExpandListColumn(#"Expanded Content", "records"),
   > # "Expanded records1" = Table.ExpandRecordColumn(#"Expanded records", "records", {"resourceId", "metricName", "time", "average", "minimum", "maximum", "total", "count", "timeGrain"}, {"resourceId", "metricName", "time", "average", "minimum", "maximum", "total", "count", "timeGrain"}),
   > # "Filtered Rows" = Table.SelectRows(#"Expanded records1", each ([metricName] = "Latency")),
   > # "Removed Columns1" = Table.RemoveColumns(#"Filtered Rows",{"timeGrain"}),
   > # "Renamed Columns" = Table.RenameColumns(#"Removed Columns1",{{"time", "Datetime"}, {"resourceId", "ResourceId"}, {"metricName", "MetricName"}, {"average", "Average"}, {"minimum", "Minimum"}, {"maximum", "Maximum"}, {"total", "Total"}, {"count", "Count"}}),
   > # "Changed Type" = Table.TransformColumnTypes(#"Renamed Columns",{{"ResourceId", type text}, {"MetricName", type text}, {"Datetime", type datetimezone}, {"Average", type number}, {"Minimum", Int64.Type}, {"Maximum", Int64.Type}, {"Total", Int64.Type}, {"Count", Int64.Type}}),
   > Rounding = Table.TransformColumns(#"Changed Type",{{"Average", each Number.Round(_, 2)}}),
   > 
   > # "Changed Type1" = Table.TransformColumnTypes(Rounding,{{"Average", type number}}),
   > # "Inserted Date" = Table.AddColumn(#"Changed Type1", "Date", each DateTime.Date([Datetime]), type date)
   > in
   > 
   > # "Inserted Date"
   > 
10. Klicken Sie auf „Fertig“, und wählen Sie dann auf der Registerkarte „Startseite“ „Schließen und Anwenden“.
11. Die Daten der letzten 30 Tage können jetzt verwendet werden. Fahren Sie fort, und erstellen Sie einige [Visualisierungen](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-report-view/).

## <a name="next-steps"></a>Nächste Schritte
Erhalten Sie weitere Informationen zu Syntax und Abfrageparametern. Details finden Sie unter [Suchen von Dokumenten (REST-API für den Azure Search-Dienst)](https://msdn.microsoft.com/library/azure/dn798927.aspx) .

Erfahren Sie hier mehr über das Erstellen erstaunlicher Berichte. Weitere Informationen finden Sie unter [Erste Schritte mit Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/).

<!--Image references-->

[1]: ./media/search-traffic-analytics/SettingsBlade.png
[2]: ./media/search-traffic-analytics/DiagnosticsBlade.png
[3]: ./media/search-traffic-analytics/ResourceId.png
[4]: ./media/search-traffic-analytics/Dashboard.png
[5]: ./media/search-traffic-analytics/GetData.png
[6]: ./media/search-traffic-analytics/BlobStorage.png
[7]: ./media/search-traffic-analytics/QueryEditor.png




<!--HONumber=Oct16_HO2-->


