<properties
	pageTitle="Logik-App-Grenzwerte und -Konfiguration | Microsoft Azure"
	description="Enthält eine Übersicht über die Dienstgrenzwerte und die Konfigurationswerte, die für Logik-Apps verfügbar sind."
	services="logic-apps"
	documentationCenter=".net,nodejs,java"
	authors="jeffhollan"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="logic-apps"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/22/2016"
	ms.author="jehollan"/>

# Logik-App-Grenzwerte und -Konfiguration

Unten sind die aktuellen Grenzwerte und Konfigurationsdetails für Azure Logic Apps angegeben.

## Grenzen

### Grenzwerte für HTTP-Anforderungen

Dies sind die Grenzwerte für eine einzelne HTTP-Anforderung und/oder einen Connectoraufruf.

#### Timeout

|Name|Begrenzung|Hinweise|
|----|----|----|
|Anforderungstimeout|1 Minute|Bei Bedarf können ein [asynchrones Muster](app-service-logic-create-api-app.md) oder eine [Until-Schleife](app-service-logic-loops-and-scopes.md) zur Kompensation verwendet werden.|

#### Nachrichtengröße

|Name|Begrenzung|Hinweise|
|----|----|----|
|Nachrichtengröße|50 MB|Einige Connectors und APIs verfügen unter Umständen nicht über eine Unterstützung von 50 MB. Ein Anforderungstrigger unterstützt bis zu 25 MB.|
|Grenzwert für die Auswertung von Ausdrücken|131\.072 Zeichen|`@concat()`, `@base64()`, `string` dürfen nicht länger sein.|

#### Wiederholungsrichtlinie

|Name|Begrenzung|Hinweise|
|----|----|----|
|Wiederholungsversuche|4|Konfiguration mit dem [Parameter für die Wiederholungsrichtlinie](https://msdn.microsoft.com/de-DE/library/azure/mt643939.aspx) möglich.|
|Maximale Wiederholungsverzögerung|1 Stunde|Konfiguration mit dem [Parameter für die Wiederholungsrichtlinie](https://msdn.microsoft.com/de-DE/library/azure/mt643939.aspx) möglich.|
|Minimale Wiederholungsverzögerung|20 Min.|Konfiguration mit dem [Parameter für die Wiederholungsrichtlinie](https://msdn.microsoft.com/de-DE/library/azure/mt643939.aspx) möglich.|

### Ausführungsdauer und Aufbewahrung

Dies sind die Grenzwerte für eine einzelne Ausführung der Logik-App.

|Name|Begrenzung|Hinweise|
|----|----|----|
|Ausführungsdauer|90 Tage||
|Aufbewahrungsdauer im Speicher|90 Tage|Gilt ab der Startzeit der Ausführung.|
|Min. Wiederholungsintervall|15 Sekunden||
|Max. Wiederholungsintervall|500 Tage||


### Grenzwerte für Schleifen und Auflösen von Batches

Dies sind die Grenzwerte für eine einzelne Ausführung der Logik-App.

|Name|Begrenzung|Hinweise|
|----|----|----|
|ForEach-Elemente|5\.000|Bei Bedarf können Sie die [Abfrageaktion](../connectors/connectors-native-query.md) verwenden, um größere Arrays zu filtern.|
|Until-Iterationen|10\.000||
|SplitOn-Elemente|10\.000||
|ForEach-Parallelität|20|Sie können ein sequenzielles ForEach-Element festlegen, indem Sie `"operationOptions": "Sequential"` der `foreach`-Aktion hinzufügen.|


### Durchsatzlimits

Dies sind die Grenzwerte für eine einzelne Instanz der Logik-App.

|Name|Begrenzung|Hinweise|
|----|----|----|
|Trigger pro Sekunde|100|Bei Bedarf können Workflows auf mehrere Apps verteilt werden.|

### Definitionslimits

Dies sind die Grenzwerte für eine einzelne Logik-App-Definition.

|Name|Begrenzung|Hinweise|
|----|----|----|
|Aktionen in ForEach|1|Bei Bedarf können Sie geschachtelte Workflows als Erweiterung hinzufügen.|
|Aktionen pro Workflow|60|Bei Bedarf können Sie geschachtelte Workflows als Erweiterung hinzufügen.|
|Zulässige Schachtelungstiefe für Aktion|5|Bei Bedarf können Sie geschachtelte Workflows als Erweiterung hinzufügen.|
|Flüsse pro Region und Abonnement|1000||
|Trigger pro Workflow|10||
|Maximale Anzahl von Zeichen pro Ausdruck|8\.192||
|Max. `trackedProperties`-Größe in Zeichen|16\.000|
|`action`/`trigger`-Namensbeschränkung|80||
|Maximale Länge von `description`|256||
|Limit für `parameters`|50||
|Limit für `outputs`|10||

## Konfiguration

### IP-Adresse

Aufrufe, die über einen [Connector](../connectors/apis-list.md) durchgeführt werden, stammen von der unten angegebenen IP-Adresse.

Aufrufe, die direkt über eine Logik-App durchgeführt werden (also per [HTTP](../connectors/connectors-native-http.md) oder [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)), können aus jedem [IP-Bereich für ein Azure-Rechenzentrum](https://www.microsoft.com/de-DE/download/details.aspx?id=41653) stammen.

|Logik-App-Region|Ausgehende IP-Adresse|
|-----|----|
|Australien (Osten)|40\.126.251.213|
|Australien (Südosten)|40\.127.80.34|
|Brasilien Süd|191\.232.38.129|
|Indien (Mitte)|104\.211.98.164|
|USA (Mitte)|40\.122.49.51|
|Ostasien|23\.99.116.181|
|USA (Ost)|191\.237.41.52|
|USA (Ost) 2|104\.208.233.100|
|Japan Ost|40\.115.186.96|
|Japan (Westen)|40\.74.130.77|
|USA (Mitte/Norden)|65\.52.218.230|
|Nordeuropa|104\.45.93.9|
|USA (Mitte/Süden)|104\.214.70.191|
|Südostasien|13\.76.231.68|
|Indien, Süden|104\.211.227.225|
|Westeuropa|40\.115.50.13|
|Indien, Westen|104\.211.161.203|
|USA (West)|104\.40.51.248|


## Nächste Schritte  

- Befolgen Sie zum Einstieg in Logik-Apps das Lernprogramm [Erstellen einer Logik-App](app-service-logic-create-a-logic-app.md).
- [Anzeigen allgemeiner Beispiele und Szenarien](app-service-logic-examples-and-scenarios.md)
- [Sie können Geschäftsprozesse mit Logik-Apps automatisieren.](http://channel9.msdn.com/Events/Build/2016/T694)
- [Erfahren Sie, wie Sie Ihre Systeme in Logik-Apps integrieren.](http://channel9.msdn.com/Events/Build/2016/P462)

<!---HONumber=AcomDC_0831_2016-->